# CI/CD

## Proteção de Branches

Todos os repositórios possuem proteção de branch configurada. Não é possível fazer push direto na main, toda alteração precisa ser feita por Pull Request a partir de outra branch.

A aprovação e o merge de um Pull Request só são permitidos caso a pipeline de CI (`pr-validation.yaml`) seja executada com sucesso.

![Proteção de Branches](Anexos/protecao_branch.png)

## Integração Contínua (CI)

Cada repositório possui uma pipeline `pr-validation.yaml` em `.github/workflows/` que é executada automaticamente em Pull Requests para a branch `main`.

### Estrutura comum

Todos os repositórios de aplicação (.NET) seguem a mesma estrutura:

1. **Build** do projeto
2. **Testes** com coleta de cobertura (OpenCover)
3. **SonarCloud** para análise estática
4. **Validação de cobertura mínima de 80%**
5. **CI Gate** como job final

### CI Gate

O `ci-gate` é um job que agrega todos os resultados anteriores. A proteção de branch está configurada para exigir que o `ci-gate` passe com sucesso antes de permitir o merge do PR. Isso garante que tanto os testes quanto a análise do SonarCloud foram aprovados.

```yaml
ci-gate:
    name: CI Gate
    runs-on: ubuntu-latest
    needs: [build-test-sonar]
    if: always()
    steps:
      - name: Verificar resultados
        run: |
          if [ "${{ needs.build-test-sonar.result }}" != "success" ]; then
            echo "❌ CI Gate falhou: build-test-sonar=${{ needs.build-test-sonar.result }}"
            exit 1
          fi
          echo "✅ CI Gate aprovado"
```

### Validação de cobertura ≥ 80%

A validação de cobertura é feita por um script bash que extrai a porcentagem de cobertura do relatório OpenCover e falha a pipeline caso esteja abaixo de 80%:

```yaml
- name: Validar Cobertura Mínima (80%)
  run: |
    COVERAGE_FILE=$(find ./TestResults -name "coverage.opencover.xml" -print -quit)
    LINE_RATE=$(grep -oP 'sequenceCoverage="\K[^"]+' "$COVERAGE_FILE" | head -1)
    echo "Cobertura de código: ${LINE_RATE}%"
    PASS=$(python3 -c "print('true' if float('${LINE_RATE}') >= 80.0 else 'false')")
    if [ "$PASS" = "false" ]; then
      echo "❌ Cobertura abaixo do mínimo: ${LINE_RATE}% (mínimo: 80%)"
      exit 1
    fi
```

### Validação SonarCloud

A análise do SonarCloud é iniciada antes do build e finalizada após os testes, enviando os relatórios de cobertura:

```yaml
- name: SonarCloud - Iniciar Análise
  run: |
    dotnet sonarscanner begin \
      /k:"joaosena19_fiap-12soat-projeto-fase-4-ordem-servico" \
      /o:"joaosena19" \
      /d:sonar.token="${{ secrets.SONAR_TOKEN }}" \
      /d:sonar.host.url="https://sonarcloud.io" \
      /d:sonar.cs.opencover.reportsPaths="**/TestResults/**/coverage.opencover.xml"
```

### Pipelines por repositório

#### Ordem de Serviço
- CI: [pr-validation.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/blob/main/.github/workflows/pr-validation.yaml)
- CD: [deploy.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/blob/main/.github/workflows/deploy.yaml)
- Build .NET 9.0, testes com Mongo2Go, publicação Docker, provisionamento DocumentDB via Terraform, deploy no EKS.

#### Cadastro
- CI: [pr-validation.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/blob/main/.github/workflows/pr-validation.yaml)
- CD: [deploy.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-cadastro/blob/main/.github/workflows/deploy.yaml)
- Build .NET 9.0, testes, publicação Docker, provisionamento RDS PostgreSQL via Terraform, deploy no EKS.

#### Estoque
- CI: [pr-validation.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/blob/main/.github/workflows/pr-validation.yaml)
- CD: [deploy.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/blob/main/.github/workflows/deploy.yaml)
- Build .NET 9.0, testes, publicação Docker, provisionamento RDS PostgreSQL via Terraform, deploy no EKS.

#### Auth Lambda
- CI: [pr-validation.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda/blob/main/.github/workflows/pr-validation.yaml)
- CD: [deploy.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-auth-lambda/blob/main/.github/workflows/deploy.yaml)
- Build .NET 8.0, testes, CI inclui validação do Terraform. CD publica Lambda e provisiona API Gateway.

#### Infraestrutura
- CI: [pr-validation.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra/blob/main/.github/workflows/pr-validation.yaml)
- CD: [deploy.yaml](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-infra/blob/main/.github/workflows/deploy.yaml)
- CI valida Terraform e executa SonarCloud. CD provisiona EKS, VPC, IAM, SQS, New Relic via Terraform.

## Deploy Contínuo (CD)

Os deploys são realizados via `workflow_dispatch` (gatilho manual no GitHub Actions). A automação do processo é completa (build, push de imagem, provisionamento de infra, deploy no cluster), mas o gatilho é manual para evitar deploys prematuros durante o desenvolvimento.

---
Anterior: [Banco de dados - Ordem de Serviço](../03.%20Banco%20de%20dados/3_banco_de_dados_ordem_servico.md)  
Próximo: [Plano de monitoramento](../06.%20Plano%20de%20monitoramento/1_plano_de_monitoramento.md)