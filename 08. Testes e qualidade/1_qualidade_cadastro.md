# Qualidade - Cadastro

## SonarCloud

Veja o [relatório completo no SonarCloud](https://sonarcloud.io/project/overview?id=joaosena19_fiap-12soat-projeto-fase-4-cadastro).

![SonarCloud - Cadastro](Anexos/sonar_cadastro.png)

Todas as issues de severidade **Major** foram resolvidas. Algumas issues foram tratadas da seguinte forma:

- **Falsos positivos:** Issues relacionadas a keys/secrets supostamente expostas foram marcadas como falso positivo, pois os valores são injetados via **variáveis de ambiente** e não estão hardcoded no código.
- **Issues aceitas (preferência de estilo):** Foram aceitas issues como o uso do sufixo `Enum` em tipos enum e métodos com mais de 7 parâmetros, uma vez que o projeto utiliza **Builders** e **Factories** que justificam essa abordagem.

## Test Coverage

Veja o [relatório completo de cobertura](Anexos/cadastro_test_coverage_completo.zip) (download do HTML).

![Test Coverage - Cadastro](Anexos/cadastro_test_coverage_resumo.png)

## Proteção de Branch

A branch `main` está protegida contra push direto. Toda alteração precisa ser feita via Pull Request, e o CI Gate deve passar com sucesso antes do merge.

![Proteção de Branches](../05.%20CI%20%26%20CD/Anexos/protecao_branch.png)

---
Anterior: [Comunicação assíncrona e SAGA](../07.%20Comunicação%20entre%20serviços/2_comunicacao_assincrona_saga.md)  
Próximo: [Qualidade - Estoque](2_qualidade_estoque.md)
