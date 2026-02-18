# Comunicação Assíncrona e SAGA

## Visão geral

O único ponto do sistema que exige SAGA é a **redução de estoque ao iniciar a execução de uma Ordem de Serviço**. Quando o orçamento de uma OS é aprovado e a execução inicia, o serviço de Ordem de Serviço precisa solicitar ao serviço de Estoque que reduza as quantidades dos itens incluídos.

Essa operação é feita via **mensageria** (Amazon SQS com MassTransit) usando o padrão de **SAGA coreografada**.

## Por que coreografia e não orquestração?

Como temos **apenas um cenário de SAGA**, a orquestração seria desnecessária. Um orquestrador central adicionaria complexidade de infraestrutura sem benefícios neste cenário.

Veja mais em [ADR 0003 - SAGA Coreografada](../09.%20ADRs/0003_adr_saga_coreografada.md).

## Fluxo da SAGA

### 1. Aprovação do orçamento

Quando o `AprovarOrcamentoUseCase` é executado, ele muda o status da OS para `Aprovada` e publica uma mensagem `ReducaoEstoqueSolicitacao` na fila SQS com os itens e quantidades a reduzir.

A OS é marcada internamente com `InteracaoEstoque.AguardandoReducao()` e o status avança para `EmExecucao`.

### 2. Processamento no serviço de Estoque

O `ReducaoEstoqueSolicitacaoConsumer` no serviço de Estoque recebe a mensagem e:

1. **Valida** que todos os itens existem e têm estoque suficiente
2. **Reduz** o estoque de cada item
3. **Publica** uma mensagem `ReducaoEstoqueResultado` com `Sucesso = true`

Se qualquer item não existir ou não tiver estoque suficiente, publica `ReducaoEstoqueResultado` com `Sucesso = false` e o motivo da falha (ex: `"estoque_insuficiente"`).

### 3. Processamento do resultado na Ordem de Serviço

O `ReducaoEstoqueResultadoConsumer` no serviço de Ordem de Serviço recebe o resultado:

- **Sucesso:** Chama `ConfirmarReducaoEstoque()` na OS, marcando que o estoque foi reduzido e a execução pode prosseguir normalmente.
- **Falha (compensação):** Chama `RegistrarFalhaReducaoEstoque()` na OS, que retorna o status para `Aprovada` (compensação). Registra um custom event `SagaCompensacaoFalhaEstoque` no New Relic.

## InteracaoEstoque

O value object `InteracaoEstoque` controla o estado da interação com o estoque dentro da OS:

- `SemInteracao()` — OS sem itens de estoque, não precisa de SAGA
- `AguardandoReducao()` — Aguardando resposta do serviço de Estoque
- `ConfirmarReducao()` — Estoque reduzido com sucesso
- `MarcarFalha()` — Redução falhou, compensação executada

Propriedades computadas:
- `EstaAguardandoRemocaoEstoque` — `true` se está aguardando resultado
- `SemPendenciasEstoque` — `true` se não há interação ou se já foi confirmada

## Status Aprovada e retry

O status `Aprovada` foi criado especificamente para suportar o cenário de retry da SAGA. Em um fluxo normal, a OS transita de `AguardandoAprovacao` diretamente para `EmExecucao`. Porém, se a redução de estoque falhar, a compensação retorna a OS para `Aprovada`, permitindo que o usuário tente aprovar novamente (re-disparando a solicitação de redução).

## Compensação

Existem duas formas de compensação:

### 1. Falha no estoque

Se o serviço de Estoque responder que a redução falhou (estoque insuficiente ou erro interno), o `ReducaoEstoqueResultadoConsumer` executa a compensação imediatamente, retornando a OS para `Aprovada`.

### 2. Timeout (Background Service)

O `SagaTimeoutBackgroundService` roda a cada 30 segundos e verifica se existem OSs que estão aguardando resposta do estoque há mais de 90 segundos. Se encontrar, executa a compensação automaticamente, retornando a OS para `Aprovada` e registrando um custom event `SagaCompensacaoTimeout` no New Relic.

Isso cobre o cenário onde a mensagem de resultado se perdeu ou o serviço de Estoque ficou indisponível.

## Mensagens

### ReducaoEstoqueSolicitacao

```json
{
  "CorrelationId": "guid",
  "OrdemServicoId": "guid",
  "Itens": [
    { "ItemEstoqueId": "guid", "Quantidade": 2 }
  ]
}
```

### ReducaoEstoqueResultado

```json
{
  "CorrelationId": "guid",
  "OrdemServicoId": "guid",
  "Sucesso": true,
  "MotivoFalha": null
}
```

## Monitoramento

Toda a SAGA é monitorada através de custom events no New Relic e CorrelationId propagado entre as mensagens. Veja mais em [Plano de monitoramento](../06.%20Plano%20de%20monitoramento/1_plano_de_monitoramento.md).

---
Anterior: [Comunicação síncrona](1_comunicacao_sincrona.md)  
Próximo: [Qualidade - Cadastro](../08.%20Testes%20e%20qualidade/1_qualidade_cadastro.md)
