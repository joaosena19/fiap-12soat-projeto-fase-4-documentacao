# ADR 0010 - Infraestrutura Geral Compartilhada e Banco Isolado por Serviço

## Status

Aceito

## Contexto

Com a divisão em microsserviços, foi necessário decidir como organizar a infraestrutura Terraform. Existem recursos que são naturalmente compartilhados entre todos os serviços (cluster EKS, VPC, subnets, permissões IAM, NLB, filas SQS, New Relic) e recursos que pertencem exclusivamente a um serviço (banco de dados).

## Discussão e possibilidades

Foram consideradas duas abordagens:

1. **Tudo centralizado em um único repositório de infra:** Simples de gerenciar, mas acopla o ciclo de vida dos bancos à infraestrutura geral.
2. **Tudo descentralizado, cada microsserviço gerencia sua própria infra completa:** Máxima independência, mas duplicação de configurações de rede e cluster.
3. **Abordagem híbrida:** Um repositório central para a infraestrutura compartilhada (EKS, VPC, subnets, NLB, SQS, IAM, New Relic) e o banco de dados provisionado no repositório de cada microsserviço.

A abordagem híbrida foi escolhida por refletir melhor a realidade: o cluster EKS é compartilhado entre todos os serviços, assim como a VPC, as subnets, as permissões e o NLB. Não faz sentido duplicar esses recursos. Já o banco de dados é o recurso que pode — e deve — ficar realmente isolado em cada microsserviço, pois cada um tem seu próprio banco com schema, engine e configuração independentes.

Na prática, a organização ficou:

| Repositório | Responsabilidade Terraform |
|---|---|
| `fiap-12soat-projeto-fase-4-infra` | VPC, subnets, EKS, NLB, SQS, IAM, New Relic |
| `fiap-12soat-projeto-fase-4-cadastro` | RDS PostgreSQL do Cadastro |
| `fiap-12soat-projeto-fase-4-estoque` | RDS PostgreSQL do Estoque |
| `fiap-12soat-projeto-fase-4-ordem-servico` | DocumentDB MongoDB da Ordem de Serviço |
| `fiap-12soat-projeto-fase-4-auth-lambda` | Lambda, API Gateway, VPC Links |

## Decisão

Foi decidido manter um repositório de infraestrutura geral (`fiap-12soat-projeto-fase-4-infra`) para recursos compartilhados e delegar o provisionamento do banco de dados ao repositório de cada microsserviço.

## Consequências

**Positivas:**

* Recursos compartilhados (EKS, VPC, NLB) ficam centralizados sem duplicação.
* Cada microsserviço tem autonomia total sobre seu banco de dados.
* O pipeline de deploy de cada serviço pode provisionar seu próprio banco sem depender de outros repositórios.

**Negativas:**

* Mudanças em rede ou permissões impactam todos os serviços simultaneamente.

---
Anterior: [ADR 0009 - CI Obrigatória com Test Coverage e SonarCloud](0009_adr_ci_obrigatoria_test_coverage_e_sonar.md)  
Próximo: [Banco de dados - Cadastro](../03.%20Banco%20de%20dados/1_banco_de_dados_cadastro.md)
