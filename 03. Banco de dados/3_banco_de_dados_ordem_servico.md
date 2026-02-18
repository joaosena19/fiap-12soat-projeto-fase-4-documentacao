# Banco de dados - Ordem de Serviço

## Escolha do banco de dados
- **Sistema**: MongoDB
- **Hospedagem**: Amazon DocumentDB
- **Driver**: MongoDB.Driver para .NET
- **Terraform**: [fiap-12soat-projeto-fase-4-ordem-servico/terraform](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-ordem-servico/tree/main/terraform)

A Ordem de Serviço encaixa muito bem com um banco documental NoSQL. As associações com a OS são em maioria associações filhas (itens incluídos, serviços incluídos, orçamento), o que mapeia naturalmente para um documento embarcado.

Além disso, o MongoDB resolve naturalmente o problema de **imutabilidade temporal** que tínhamos na fase 3. Na fase 3, era necessário ter tabelas separadas de `itens_incluidos` e `servicos_incluidos` que eram cópias fixadas em um ponto no tempo, para garantir que alterações no catálogo não afetassem OSs históricas. Com MongoDB, esses dados já ficam embarcados no documento da OS, e qualquer alteração no catálogo original não afeta o documento salvo.

## Collection

O banco possui uma única collection: `ordens_servico`.

## Exemplo de documento

```json
{
  "_id": "683a1b2c-4d5e-6f78-9a0b-1c2d3e4f5678",
  "codigo": "OS-A1B2C3",
  "status": "EmExecucao",
  "clienteId": "11111111-1111-1111-1111-111111111111",
  "clienteNome": "João Silva",
  "veiculo": {
    "veiculoId": "22222222-2222-2222-2222-222222222222",
    "placa": "ABC-1234",
    "marca": "Toyota",
    "modelo": "Corolla",
    "ano": 2022,
    "cor": "Prata"
  },
  "servicosIncluidos": [
    {
      "servicoId": "33333333-3333-3333-3333-333333333333",
      "nome": "Troca de óleo",
      "descricao": "Troca de óleo do motor com filtro",
      "preco": 150.00
    }
  ],
  "itensIncluidos": [
    {
      "itemEstoqueId": "44444444-4444-4444-4444-444444444444",
      "nome": "Óleo 5W30 Sintético",
      "descricao": "Óleo sintético para motor",
      "preco": 45.00,
      "quantidade": 4
    }
  ],
  "orcamento": {
    "valorServicos": 150.00,
    "valorItens": 180.00,
    "valorTotal": 330.00,
    "geradoEm": "2026-02-10T14:00:00Z"
  },
  "interacaoEstoque": {
    "deveRemoverEstoque": true,
    "estoqueRemovidoComSucesso": true
  },
  "dataInicioDiagnostico": "2026-02-10T10:00:00Z",
  "dataInicioExecucao": "2026-02-10T15:00:00Z",
  "criadoEm": "2026-02-10T09:00:00Z",
  "atualizadoEm": "2026-02-10T15:00:00Z"
}
```

---
Anterior: [Banco de dados - Estoque](2_banco_de_dados_estoque.md)  
Próximo: [CI/CD](../05.%20CI%20%26%20CD/1_ci_cd.md)
