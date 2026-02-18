# ADR 0003 - SAGA Coreografada

## Status

Aceito

## Contexto

Com a separação em microsserviços, surgiu a necessidade de manter consistência entre serviços em operações que afetam mais de um banco de dados. Era necessário decidir entre SAGA orquestrada ou coreografada.

## Discussão e possibilidades

Analisando todos os fluxos do sistema, o único cenário que exige uma SAGA é a **redução de estoque ao iniciar a execução de uma Ordem de Serviço**. Nesse fluxo, o serviço de Ordem de Serviço precisa solicitar ao serviço de Estoque que reduza as quantidades dos itens, e caso o estoque seja insuficiente ou ocorra um erro, a OS deve ser compensada (retornando ao status anterior).

Para um único cenário de SAGA, a **orquestração** seria desnecessária. Um orquestrador central adicionaria complexidade de infraestrutura, sem benefícios neste momento.

A **coreografia** resolve o problema de forma mais simples e adequada para o escopo atual.

## Decisão

Foi decidido utilizar SAGA coreografada para o fluxo de redução de estoque.

Veja mais em [Comunicação assíncrona e SAGA](../7.%20Comunicação%20entre%20serviços/2_comunicacao_assincrona_saga.md).

## Consequências

**Positivas:**

* Sem ponto único de falha de orquestração.
* Implementação mais simples para um único caso.
* Cada serviço é autônomo e reage a mensagens.

**Negativas:**

* Se no futuro surgirem muitos cenários de SAGA, a coreografia pode ficar difícil de rastrear sem um orquestrador. Para o momento, com apenas um cenário, isso não é um problema.

---
Anterior: [ADR 0002 - Comunicação entre Serviços via NLB](0002_adr_comunicacao_entre_servicos_via_nlb.md)
Próximo: [ADR 0004 - Banco de Dados por Serviço](0004_adr_banco_de_dados_por_servico.md)
