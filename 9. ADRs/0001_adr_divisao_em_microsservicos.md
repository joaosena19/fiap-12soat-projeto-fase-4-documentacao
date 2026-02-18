# ADR 0001 - Divisão em Microsserviços

## Status

Aceito

## Contexto

Na fase 3, o sistema era um monolito. O Tech Challenge da fase 4 exige a refatoração para microsserviços independentes, cada um com seu próprio banco de dados e ciclo de deploy.

Era necessário decidir como dividir o sistema e qual critério guiaria essa divisão.

## Discussão e possibilidades

A divisão foi guiada pelos bounded contexts definidos no Event Storming da fase 1 do projeto, em anexo:

![Event Storming - Bounded Contexts (Fase 1)](Anexos/event_storming_fase_1.jpg)

Os três bounded contexts identificados foram:

1. **Cadastro**: responsável por clientes, veículos, serviços e usuários.
2. **Estoque**: responsável pelos itens de estoque e suas quantidades.
3. **Ordem de Serviço**: responsável pelo ciclo de vida da OS, orçamentos e execução.

Esses contextos já eram bem divididos no monolito da fase 3, onde cada módulo interno tinha suas próprias entidades e regras e não se comunicavam diretamente. A separação em microsserviços seguiu essa divisão natural.

## Decisão

Foi decidido dividir o sistema em três microsserviços independentes, um para cada bounded context:

- [fiap-12soat-projeto-fase-4-cadastro](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro)
- [fiap-12soat-projeto-fase-4-estoque](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque)
- [fiap-12soat-projeto-fase-4-ordem-servico](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico)

Além deles, foram mantidos os repositórios de infraestrutura, autenticação Lambda e documentação:

- [fiap-12soat-projeto-fase-4-infra](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra)
- [fiap-12soat-projeto-fase-4-auth-lambda](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda)
- [fiap-12soat-projeto-fase-4-documentacao](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-documentacao)

## Consequências

**Positivas:**

* Cada serviço pode escalar independentemente.
* Deploy, testes e manutenção isolados por contexto.
* Falha em um serviço não derruba todo o sistema.

**Negativas:**

* Complexidade operacional significativamente maior (6 repositórios, 3 bancos, mensageria, etc).
* Comunicação entre serviços exige tratamento de falhas e latência de rede.

---
Anterior: [Comunicação assíncrona e SAGA](../7.%20Comunicação%20entre%20serviços/2_comunicacao_assincrona_saga.md)
Próximo: [ADR 0002 - Comunicação entre Serviços via NLB](0002_adr_comunicacao_entre_servicos_via_nlb.md)
