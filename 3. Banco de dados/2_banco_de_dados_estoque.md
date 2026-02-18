# Banco de dados - Estoque

## Escolha do banco de dados
- **Sistema**: PostgreSQL
- **Hospedagem**: Amazon RDS
- **ORM**: Entity Framework Core
- **Terraform**: [fiap-12soat-projeto-fase-4-estoque/terraform](https://github.com/joaosena19/fiap-12soat-projeto-fase-4-estoque/tree/main/terraform)

O estoque possui apenas uma tabela (`itens_estoque`). Num cenário tão pequeno, não há diferença de performance significativa entre SQL e NoSQL. Como o PostgreSQL com Entity Framework Core funciona muito bem com .NET, e tenho afinidade com SQL, foi a escolha natural.

Foi adotada uma abordagem code-first, assim como nos outros serviços.

## Diagrama de Entidade e Relacionamento

![Diagrama ER Estoque](Anexos/diagrama_er_estoque.png)

## Tabela

### Itens de Estoque

`itens_estoque` é a única tabela do banco. Armazena o catálogo de todos os itens disponíveis com nome, descrição, preço, quantidade disponível e tipo (Peça ou Insumo).

---
Anterior: [Banco de dados - Cadastro](1_banco_de_dados_cadastro.md)
Próximo: [Banco de dados - Ordem de Serviço](3_banco_de_dados_ordem_servico.md)