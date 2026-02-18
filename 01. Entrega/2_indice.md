# Índice

Este arquivo serve como referência para cada um dos pontos de avaliação do Tech Challenge. Cada termo está linkado para o respectivo arquivo de documentação.

Se for sua primeira leitura, siga a navegação "Próximo" ao final de cada arquivo.

## Pontos de Avaliação

1. Refatoração em 3 microsserviços independentes
    1. Repositórios:
        - [fiap-12soat-projeto-fase-4-cadastro](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro)
        - [fiap-12soat-projeto-fase-4-estoque](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque)
        - [fiap-12soat-projeto-fase-4-ordem-servico](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico)
        - [fiap-12soat-projeto-fase-4-infra](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra)
        - [fiap-12soat-projeto-fase-4-auth-lambda](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda)
        - [fiap-12soat-projeto-fase-4-documentacao](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-documentacao)
    2. Veja [ADR 0001 - Divisão em Microsserviços](../09.%20ADRs/0001_adr_divisao_em_microsservicos.md)
2. Bancos de dados isolados e ao menos um banco relacional e um não relacional
    1. Veja [ADR 0004 - Banco de Dados por Serviço](../09.%20ADRs/0004_adr_banco_de_dados_por_servico.md)
    2. [Banco de dados - Cadastro (PostgreSQL)](../03.%20Banco%20de%20dados/1_banco_de_dados_cadastro.md)
    3. [Banco de dados - Estoque (PostgreSQL)](../03.%20Banco%20de%20dados/2_banco_de_dados_estoque.md)
    4. [Banco de dados - Ordem de Serviço (MongoDB/DocumentDB)](../03.%20Banco%20de%20dados/3_banco_de_dados_ordem_servico.md)
    5. Terraform dos bancos: [Cadastro](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/tree/main/terraform), [Estoque](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/tree/main/terraform), [Ordem de Serviço](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/tree/main/terraform)
3. [Diagrama de Componentes](../02.%20Diagrama%20de%20componentes%20%28C4%29/1_diagrama_de_componentes.md)
4. Comunicação síncrona e assíncrona
    1. [Comunicação síncrona](../07.%20Comunicação%20entre%20serviços/1_comunicacao_sincrona.md)
    2. [Comunicação assíncrona e SAGA](../07.%20Comunicação%20entre%20serviços/2_comunicacao_assincrona_saga.md)
    3. Veja [ADR 0002 - Comunicação entre Serviços via NLB](../09.%20ADRs/0002_adr_comunicacao_entre_servicos_via_nlb.md)
5. SAGA Pattern
    1. [Comunicação assíncrona e SAGA](../07.%20Comunicação%20entre%20serviços/2_comunicacao_assincrona_saga.md)
    2. Veja [ADR 0003 - SAGA Coreografada](../09.%20ADRs/0003_adr_saga_coreografada.md) - justificativa de coreografia ao invés de orquestração
    3. [Custom events e monitoramento da compensação](../06.%20Plano%20de%20monitoramento/1_plano_de_monitoramento.md)
6. Qualidade e Testes
    1. [Qualidade - Cadastro](../08.%20Testes%20e%20qualidade/1_qualidade_cadastro.md)
    2. [Qualidade - Estoque](../08.%20Testes%20e%20qualidade/2_qualidade_estoque.md)
    3. [Qualidade - Ordem de Serviço](../08.%20Testes%20e%20qualidade/3_qualidade_ordem_servico.md)
    4. [Qualidade - Auth Lambda](../08.%20Testes%20e%20qualidade/4_qualidade_auth_lambda.md)
    5. [Qualidade - Infraestrutura](../08.%20Testes%20e%20qualidade/5_qualidade_infra.md)
    6. Veja [ADR 0009 - CI Obrigatória com Test Coverage e SonarCloud](../09.%20ADRs/0009_adr_ci_obrigatoria_test_coverage_e_sonar.md)
    7. Veja [CI/CD](../05.%20CI%20%26%20CD/1_ci_cd.md)
7. [BDD](../08.%20Testes%20e%20qualidade/6_BDD.md)
8. CI/CD automatizado e proteção das branchs
    1. Veja [CI/CD](../05.%20CI%20%26%20CD/1_ci_cd.md)
    2. Veja [ADR 0009 - CI Obrigatória com Test Coverage e SonarCloud](../09.%20ADRs/0009_adr_ci_obrigatoria_test_coverage_e_sonar.md)
    3. Pipelines:
        - Ordem de Serviço - [CI](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/blob/main/.github/workflows/pr-validation.yaml) / [CD](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/blob/main/.github/workflows/deploy.yaml)
        - Cadastro - [CI](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/blob/main/.github/workflows/pr-validation.yaml) / [CD](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/blob/main/.github/workflows/deploy.yaml)
        - Estoque - [CI](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/blob/main/.github/workflows/pr-validation.yaml) / [CD](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/blob/main/.github/workflows/deploy.yaml)
        - Auth Lambda - [CI](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda/blob/main/.github/workflows/pr-validation.yaml) / [CD](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda/blob/main/.github/workflows/deploy.yaml)
        - Infraestrutura - [CI](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra/blob/main/.github/workflows/pr-validation.yaml) / [CD](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra/blob/main/.github/workflows/deploy.yaml)
9. Kubernetes
    1. Pastas k8s de cada repo: [Ordem de Serviço](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/tree/main/k8s), [Cadastro](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/tree/main/k8s), [Estoque](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/tree/main/k8s)
10. [Observabilidade e monitoramento](../06.%20Plano%20de%20monitoramento/1_plano_de_monitoramento.md)
11. [RFCs e ADRs](../09.%20ADRs/0001_adr_divisao_em_microsservicos.md)
12. [Autenticação](../04.%20Autenticação/1_autenticacao.md)
13. Documentação da API
    1. [Endpoints - Ordem de Serviço](../10.%20Endpoints/1_endpoints_ordem_servico.md)
    2. [Endpoints - Cadastro](../10.%20Endpoints/2_endpoints_cadastro.md)
    3. [Endpoints - Estoque](../10.%20Endpoints/3_endpoints_estoque.md)

---
Anterior: [Identificação](1_introducao.md)  
Próximo: [Diagrama de componentes](../02.%20Diagrama%20de%20componentes%20%28C4%29/1_diagrama_de_componentes.md)
