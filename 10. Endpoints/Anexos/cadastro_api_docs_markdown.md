# Oficina Mecânica API
API para gerenciamento de oficina mecânica

## Version: v1

**Contact information:**  
João Dainese  
joaosenadainese@gmail.com  

### /api/clientes

#### GET
##### Summary:

Buscar todos os clientes

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Lista de clientes retornada com sucesso |
| 403 | Acesso negado |
| 500 | Erro interno do servidor |

#### POST
##### Summary:

Criar um novo cliente

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | Cliente criado com sucesso |
| 400 | Dados inválidos fornecidos |
| 403 | Acesso negado - Usuário só pode criar cliente com o mesmo documento |
| 409 | Conflito - Cliente já existe |
| 500 | Erro interno do servidor |

### /api/clientes/{id}

#### GET
##### Summary:

Buscar cliente por ID

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do cliente | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Cliente encontrado com sucesso |
| 403 | Acesso negado |
| 404 | Cliente não encontrado |
| 500 | Erro interno do servidor |

#### PUT
##### Summary:

Atualizar um cliente existente

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do cliente a ser atualizado | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Cliente atualizado com sucesso |
| 400 | Dados inválidos fornecidos |
| 403 | Acesso negado |
| 404 | Cliente não encontrado |
| 500 | Erro interno do servidor |

### /api/clientes/documento/{documento}

#### GET
##### Summary:

Buscar cliente por documento (CPF ou CNPJ)

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| documento | path | CPF ou CNPJ, com ou sem formatação | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Cliente encontrado com sucesso |
| 403 | Acesso negado |
| 404 | Cliente não encontrado |
| 500 | Erro interno do servidor |

### /api/servicos

#### GET
##### Summary:

Buscar todos os serviços

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Lista de serviços retornada com sucesso |
| 500 | Erro interno do servidor |

#### POST
##### Summary:

Criar um novo serviço

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | Serviço criado com sucesso |
| 400 | Dados inválidos fornecidos |
| 409 | Conflito - Serviço já existe |
| 500 | Erro interno do servidor |

### /api/servicos/{id}

#### GET
##### Summary:

Buscar serviço por ID

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do serviço | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Serviço encontrado com sucesso |
| 404 | Serviço não encontrado |
| 500 | Erro interno do servidor |

#### PUT
##### Summary:

Atualizar um serviço existente

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do serviço a ser atualizado | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Serviço atualizado com sucesso |
| 400 | Dados inválidos fornecidos |
| 404 | Serviço não encontrado |
| 500 | Erro interno do servidor |

### /api/usuarios/documento/{documento}

#### GET
##### Summary:

Buscar usuário por documento (CPF ou CNPJ)

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| documento | path | CPF ou CNPJ, com ou sem formatação | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Usuário encontrado com sucesso |
| 404 | Usuário não encontrado |
| 500 | Erro interno do servidor |

### /api/usuarios

#### POST
##### Summary:

Criar um novo usuário

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | Usuário criado com sucesso |
| 400 | Dados inválidos fornecidos |
| 409 | Conflito - Usuário já existe |
| 500 | Erro interno do servidor |

### /api/veiculos

#### GET
##### Summary:

Buscar todos os veículos

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Lista de veículos retornada com sucesso |
| 403 | Acesso negado |
| 500 | Erro interno do servidor |

#### POST
##### Summary:

Criar um novo veículo

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | Veículo criado com sucesso |
| 400 | Dados inválidos fornecidos |
| 403 | Acesso negado |
| 409 | Placa já cadastrada |
| 500 | Erro interno do servidor |

### /api/veiculos/{id}

#### GET
##### Summary:

Buscar veículo por ID

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do veículo | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Veículo encontrado com sucesso |
| 403 | Acesso negado |
| 404 | Veículo não encontrado |
| 500 | Erro interno do servidor |

#### PUT
##### Summary:

Atualizar um veículo existente

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | ID do veículo | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Veículo atualizado com sucesso |
| 400 | Dados inválidos fornecidos |
| 403 | Acesso negado |
| 404 | Veículo não encontrado |
| 500 | Erro interno do servidor |

### /api/veiculos/placa/{placa}

#### GET
##### Summary:

Buscar veículo por placa

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| placa | path | Placa do veículo | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Veículo encontrado com sucesso |
| 403 | Acesso negado |
| 404 | Veículo não encontrado |
| 500 | Erro interno do servidor |

### /api/veiculos/cliente/{clienteId}

#### GET
##### Summary:

Buscar veículos por ID do cliente

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| clienteId | path | ID do cliente | Yes | string (uuid) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Veículos encontrados com sucesso |
| 403 | Acesso negado |
| 422 | Cliente não encontrado |
| 500 | Erro interno do servidor |
