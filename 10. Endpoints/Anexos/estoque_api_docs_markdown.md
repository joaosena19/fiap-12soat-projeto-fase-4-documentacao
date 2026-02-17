# Oficina Mecânica API
API para gerenciamento de oficina mecânica

## Version: v1

**Contact information:**  
João Dainese  
joaosenadainese@gmail.com  

### /api/estoque/itens

#### GET
##### Summary:

Buscar todos os itens de estoque

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Lista de itens de estoque retornada com sucesso |
| 500 | Erro interno do servidor |

#### POST
##### Summary:

Criar um novo item de estoque

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | Item de estoque criado com sucesso |
| 400 | Dados inválidos fornecidos |
| 500 | Erro interno do servidor |

### /api/estoque/itens/{id}

#### GET
##### Summary:

Buscar item de estoque por ID

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do item de estoque | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Item de estoque encontrado com sucesso |
| 404 | Item de estoque não encontrado |
| 500 | Erro interno do servidor |

#### PUT
##### Summary:

Atualizar um item de estoque existente

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do item de estoque a ser atualizado | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Item de estoque atualizado com sucesso |
| 400 | Dados inválidos fornecidos |
| 404 | Item de estoque não encontrado |
| 500 | Erro interno do servidor |

### /api/estoque/itens/{id}/quantidade

#### PATCH
##### Summary:

Atualizar apenas a quantidade de um item de estoque

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do item de estoque | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Quantidade atualizada com sucesso |
| 400 | Dados inválidos fornecidos |
| 404 | Item de estoque não encontrado |
| 500 | Erro interno do servidor |

### /api/estoque/itens/{id}/disponibilidade

#### GET
##### Summary:

Verificar disponibilidade de um item de estoque

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do item de estoque | Yes | string (uuid) |
| quantidadeRequisitada | query | Quantidade necessária para verificação | No | integer |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Verificação realizada com sucesso |
| 400 | Dados inválidos fornecidos |
| 404 | Item de estoque não encontrado |
| 500 | Erro interno do servidor |
