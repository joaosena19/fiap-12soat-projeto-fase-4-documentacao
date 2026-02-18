# ADR 0009 - CI Obrigatória com Test Coverage e SonarCloud

## Status

Aceito

## Contexto

O Tech Challenge exige como requisito obrigatório que as pipelines de CI validem a qualidade do código com cobertura de testes acima de 80% e análise estática via SonarCloud. Dessa forma, não houve discussão ou avaliação de alternativas.

## Discussão e possibilidades

Não houve discussão, pois é um requisito obrigatório do Tech Challenge.

A implementação foi feita em cada repositório com uma pipeline `pr-validation.yaml` que executa:

1. **Build** do projeto
2. **Testes** com coleta de cobertura (OpenCover)
3. **SonarCloud** para análise estática de qualidade
4. **Validação de cobertura mínima** de 80% via script bash
5. **CI Gate** como job final que agrega os resultados

O CI Gate é o job que a proteção de branch referencia. O PR só pode ser mergeado se o CI Gate passar com sucesso, garantindo que tanto os testes quanto o Sonar foram aprovados.

Veja mais em [CI/CD](../5.%20CI%20%26%20CD/1_ci_cd.md).

## Decisão

Foi decidido implementar validação obrigatória de test coverage ≥ 80% e SonarCloud em todas as pipelines de CI.

## Consequências

**Positivas:**

* Garante qualidade mínima do código antes do merge.
* Impede que código com baixa cobertura ou problemas de qualidade entre na main.

**Negativas:**

* Tempo de execução da pipeline maior.
* Atraso em hotfixes urgentes caso diminuam a qualidade, mas é um trade-off aceitável para garantir a saúde do código a longo prazo.

---
Anterior: [ADR 0008 - Idempotência na Criação de OS Completa](0008_adr_idempotencia_criacao_os_completa.md)
Próximo: [Banco de dados - Cadastro](../3.%20Banco%20de%20dados/1_banco_de_dados_cadastro.md)
