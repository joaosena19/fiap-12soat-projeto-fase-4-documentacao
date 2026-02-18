# ADR 0004 - Banco de Dados por Serviço

## Status

Aceito

## Contexto

Com a divisão em microsserviços, cada serviço precisa ter seu próprio banco de dados isolado, garantindo independência e autonomia. Era necessário escolher qual banco utilizar para cada um.

O Tech Challenge exige ao menos um banco relacional e um não relacional.

## Discussão e possibilidades

A escolha foi feita de acordo com a natureza dos dados de cada serviço:

1. **Cadastro (PostgreSQL/RDS):** Dados de clientes, veículos, serviços e usuários são naturalmente relacionais. PostgreSQL com Entity Framework Core funciona muito bem com .NET e já era a stack da fase 3.

2. **Estoque (PostgreSQL/RDS):** Embora o estoque tenha apenas uma tabela (`itens_estoque`), num cenário tão pequeno não há diferença de performance significativa entre SQL e NoSQL. Como o PostgreSQL com EF Core funciona muito bem com .NET, e tenho afinidade com SQL, foi a escolha do banco.

3. **Ordem de Serviço (MongoDB/DocumentDB):** A Ordem de Serviço encaixa muito bem com um banco documental NoSQL. As associações com a OS são em maioria associações filhas (itens incluídos, serviços incluídos, orçamento), o que mapeia naturalmente para um documento embarcado. Além disso, o documento resolve naturalmente o problema de imutabilidade temporal que tínhamos na fase 3, onde era necessário ter tabelas separadas de `itens_incluidos` e `servicos_incluidos` para manter snapshots fixados em um ponto no tempo. Com MongoDB, esses dados já ficam embarcados no documento da OS.

Veja mais em [Banco de dados - Cadastro](../03.%20Banco%20de%20dados/1_banco_de_dados_cadastro.md), [Banco de dados - Estoque](../03.%20Banco%20de%20dados/2_banco_de_dados_estoque.md) e [Banco de dados - Ordem de Serviço](../03.%20Banco%20de%20dados/3_banco_de_dados_ordem_servico.md).

## Decisão

Foi decidido:
- **Cadastro:** PostgreSQL no Amazon RDS
- **Estoque:** PostgreSQL no Amazon RDS
- **Ordem de Serviço:** MongoDB no Amazon DocumentDB

## Consequências

**Positivas:**

* Atende o requisito de ao menos um banco relacional e um não relacional.
* Cada banco é otimizado para a natureza dos dados do respectivo serviço.
* Bancos isolados garantem que falha em um não afeta os outros.

**Negativas:**

* Três bancos para gerenciar, com provisionamento e monitoramento independentes.
* Necessidade de manter dois ORMs diferentes (Entity Framework Core e MongoDB Driver).

---
Anterior: [ADR 0003 - SAGA Coreografada](0003_adr_saga_coreografada.md)  
Próximo: [ADR 0005 - Auth Lambda Conectado ao Cadastro](0005_adr_auth_lambda_conectado_ao_cadastro.md)
