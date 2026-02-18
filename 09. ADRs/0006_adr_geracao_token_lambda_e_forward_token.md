# ADR 0006 - Geração de Token no Lambda e Forward Token

## Status

Aceito

## Contexto

Com a migração para microsserviços, era necessário decidir como o token JWT seria gerado e como ele seria propagado entre os serviços nas chamadas internas.

## Discussão e possibilidades

### Geração do Token

A geração do token JWT continua sendo feita pelo Lambda, exatamente como na fase 3. O fluxo funciona perfeitamente, o Lambda já cria tokens com as informações de `UsuarioId`, `ClienteId` e `Roles`, e não há motivo para mudar algo que funciona bem.

### Propagação entre serviços

Quando o serviço de Ordem de Serviço precisa chamar o serviço de Cadastro ou Estoque, ele precisa se autenticar. Ao invés de gerar um token novo (service-to-service), optei pelo padrão de **Forward Token**, que é um padrão de mercado.

O Forward Token consiste em propagar o token JWT do usuário original para os serviços downstream. Isso é feito automaticamente pelo `PropagateHeadersHandler`, um `DelegatingHandler` que copia o header `Authorization` e `X-Correlation-ID` para todas as chamadas HTTP de saída.

As duas principais vantagens são a simplicidade de implementação, e a rastreabilidade completa, pois todos os serviços envolvidos sabem quem é o usuário do token e suas claims, o que é fundamental para a forma como fazemos autorização através de RBAC e ownership.

## Decisão

Foi decidido manter a geração de token no Lambda e adotar Forward Token para comunicação entre serviços.

## Consequências

**Positivas:**

* Rastreabilidade completa: o serviço destino sabe quem é o usuário original.
* Cada serviço pode aplicar suas próprias regras de RBAC e ownership.
* Nenhuma infraestrutura adicional para autenticação service-to-service.

**Negativas:**

* Se o token expirar durante uma cadeia de chamadas longa, a requisição pode falhar no meio. Na prática, isso não é um problema pois o tempo de vida do token é muito maior que o tempo das chamadas.

---
Anterior: [ADR 0005 - Auth Lambda Conectado ao Cadastro](0005_adr_auth_lambda_conectado_ao_cadastro.md)  
Próximo: [ADR 0007 - Replicação Manual da Classe Ator](0007_adr_replicacao_manual_da_classe_ator.md)
