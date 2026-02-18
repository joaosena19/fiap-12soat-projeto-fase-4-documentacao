# Qualidade - Auth Lambda

## SonarCloud

Veja o [relatório completo no SonarCloud](https://sonarcloud.io/project/overview?id=joaosena19_fiap-12soat-projeto-fase-4-auth-lambda).

![SonarCloud - Auth Lambda](Anexos/sonar_auth_lambda.png)

Todas as issues de severidade **Major** foram resolvidas. Algumas issues foram tratadas da seguinte forma:

- **Falsos positivos:** Issues relacionadas a keys/secrets supostamente expostas foram marcadas como falso positivo, pois os valores são injetados via **variáveis de ambiente** e não estão hardcoded no código.
- **Issues aceitas (preferência de estilo):** Foram aceitas issues como o uso do sufixo `Enum` em tipos enum e métodos com mais de 7 parâmetros, uma vez que o projeto utiliza **Builders** e **Factories** que justificam essa abordagem.

## Test Coverage

Veja o [relatório completo de cobertura](Anexos/auth_lambda_test_coverage_completo.zip) (download do HTML).

![Test Coverage - Auth Lambda](Anexos/auth_lambda_test_coverage_resumo.png)

## Proteção de Branch

A branch `main` está protegida contra push direto. Toda alteração precisa ser feita via Pull Request, e o CI Gate deve passar com sucesso antes do merge.

![Proteção de Branches](../05.%20CI%20%26%20CD/Anexos/protecao_branch.png)

---
Anterior: [Qualidade - Ordem de Serviço](3_qualidade_ordem_servico.md)  
Próximo: [Qualidade - Infraestrutura](5_qualidade_infra.md)
