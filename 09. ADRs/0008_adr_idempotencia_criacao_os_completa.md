# ADR 0008 - Idempotência na Criação de OS Completa

## Status

Aceito

## Contexto

O `CriarOrdemServicoCompletaUseCase` é um caso de uso que cria uma OS completa, incluindo cadastro de cliente, veículo, serviços e itens. Para isso, ele faz chamadas REST para os microsserviços de Cadastro e Estoque. Essas chamadas são síncronas e dependem de respostas em tempo real para continuar o processo.

Era necessário definir como tratar falhas parciais nesse fluxo.

## Discussão e possibilidades

O fluxo do use case é:

1. Buscar ou criar o cliente no serviço de Cadastro
2. Buscar ou criar o veículo no serviço de Cadastro
3. Buscar serviços válidos no serviço de Cadastro
4. Buscar itens de estoque válidos no serviço de Estoque
5. Criar a OS localmente

Se, por exemplo, o cliente for criado com sucesso mas a criação do veículo falhar por erro de rede, o processo é interrompido. Não há necessidade de compensar a criação do cliente, pois:

- O cliente criado é um registro válido no banco de dados de Cadastro. Não é um dado "sujo" ou inconsistente.
- Se a requisição for refeita, o use case é esperto para não duplicar cadastros: ele primeiro busca pelo documento do cliente, e se já existir, reutiliza o registro encontrado.
- O mesmo vale para o veículo: a busca é feita pela placa, e se já existir, reutiliza.

Isso garante **idempotência** natural do fluxo. Não importa quantas vezes a requisição seja repetida, o resultado final será o mesmo.

As chamadas precisam ser síncronas porque o use case precisa das respostas em tempo real (IDs do cliente e veículo criados) para compor a OS.

## Decisão

Foi decidido que falhas parciais na criação de OS completa são aceitáveis e não exigem compensação, pois o fluxo é naturalmente idempotente.

## Consequências

**Positivas:**

* Sem necessidade de SAGA ou transação distribuída para esse fluxo.
* Registros parciais (cliente já criado) não são desperdício, são dados válidos.
* Idempotência garante que re-tentativas são seguras.

**Negativas:**

* Nenhum ponto negativo identificado. O que poderia acontecer é o cliente ou veículo ficar cadastrado sem uma OS associada ainda, o que é um estado perfeitamente válido.

---
Anterior: [ADR 0007 - Replicação Manual da Classe Ator](0007_adr_replicacao_manual_da_classe_ator.md)  
Próximo: [ADR 0009 - CI Obrigatória com Test Coverage e SonarCloud](0009_adr_ci_obrigatoria_test_coverage_e_sonar.md)
