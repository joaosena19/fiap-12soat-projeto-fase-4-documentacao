# Comunicação Síncrona

## Visão geral

A comunicação síncrona entre os microsserviços é feita via chamadas HTTP REST. O serviço de **Ordem de Serviço** é o principal consumidor, pois precisa buscar e criar dados nos serviços de **Cadastro** e **Estoque**.

## Serviços e endpoints chamados

### Ordem de Serviço → Cadastro

| Use Case | Método | Endpoint chamado | Motivo |
|----------|--------|------------------|--------|
| `CriarOrdemServicoCompletaUseCase` | GET | `/api/clientes/documento/{documento}` | Buscar cliente existente por documento |
| `CriarOrdemServicoCompletaUseCase` | POST | `/api/clientes` | Criar cliente quando não existe |
| `CriarOrdemServicoCompletaUseCase` | GET | `/api/veiculos/placa/{placa}` | Buscar veículo existente por placa |
| `CriarOrdemServicoCompletaUseCase` | POST | `/api/veiculos` | Criar veículo quando não existe |
| `CriarOrdemServicoCompletaUseCase` | GET | `/api/servicos/{id}` | Buscar serviço do catálogo por ID |

### Ordem de Serviço → Estoque

| Use Case | Método | Endpoint chamado | Motivo |
|----------|--------|------------------|--------|
| `CriarOrdemServicoCompletaUseCase` | GET | `/api/estoque/itens/{id}` | Buscar item de estoque por ID |

## Por que síncrono?

Todas essas chamadas são **GETs** que precisam da resposta no momento exato para compor a OS, ou **POSTs** que são idempotentes (busca antes de criar). Não faz sentido torná-las assíncronas, pois o use case depende do retorno imediato para continuar o processamento.

Os POSTs que o `CriarOrdemServicoCompletaUseCase` faz para criar clientes e veículos são idempotentes por natureza: antes de criar, o use case busca pelo documento/placa, e se já existir, reutiliza o registro. Veja mais em [ADR 0008 - Idempotência na Criação de OS Completa](../9.%20ADRs/0008_adr_idempotencia_criacao_os_completa.md).

## Nota sobre redução de estoque

A **redução de estoque** ao iniciar a execução de uma OS **não** é feita por comunicação síncrona. É o único cenário que exige consistência eventual entre serviços, e por isso é tratado via comunicação assíncrona (SAGA coreografada). Veja mais em [Comunicação assíncrona e SAGA](2_comunicacao_assincrona_saga.md).

---
Anterior: [Autenticação](../4.%20Autenticação/1_autenticacao.md)  
Próximo: [Comunicação assíncrona e SAGA](2_comunicacao_assincrona_saga.md)
