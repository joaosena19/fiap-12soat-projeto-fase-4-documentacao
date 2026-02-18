# ADR 0005 - Auth Lambda Conectado ao Cadastro

## Status

Aceito

## Contexto

Na fase 3, a autenticação era feita por um Lambda que acessava diretamente o banco de dados compartilhado do monolito. Com a divisão em microsserviços, o banco foi separado, e era necessário decidir onde ficaria a tabela de usuários que o Lambda consulta para login.

## Discussão e possibilidades

O ideal seria que a autenticação fosse um microsserviço próprio, com seu banco de dados exclusivo para usuários e credenciais. Isso garantiria completa independência.

Contudo, na fase 3 esse fluxo já funcionava perfeitamente com o Lambda acessando o banco diretamente. Criar um microsserviço de identidade só para separar a tabela de usuários seria um retrabalho sem benefício concreto neste momento, dado que o Lambda já funciona e a entrega tem prazo.

Por limitações de tempo, optei por manter o Lambda conectado ao banco do microsserviço de Cadastro, já que é onde ficam as tabelas de `usuarios` e `roles`.

## Decisão

Foi decidido manter o Auth Lambda acessando diretamente o banco de dados do microsserviço de Cadastro.

## Consequências

**Positivas:**

* Reutilização do código da fase 3, sem retrabalho.
* Funciona perfeitamente para o escopo atual.

**Negativas:**

* O Lambda tem acoplamento direto com o banco do Cadastro. Se o schema de usuários mudar, o Lambda precisa ser atualizado.
* Em um cenário ideal, a identidade seria um serviço independente.

---
Anterior: [ADR 0004 - Banco de Dados por Serviço](0004_adr_banco_de_dados_por_servico.md)
Próximo: [ADR 0006 - Geração de Token no Lambda e Forward Token](0006_adr_geracao_token_lambda_e_forward_token.md)
