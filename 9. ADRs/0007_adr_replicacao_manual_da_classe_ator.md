# ADR 0007 - Replicação Manual da Classe Ator

## Status

Aceito

## Contexto

A classe `Ator` é responsável por representar o usuário autenticado e validar permissões e ownership em todos os Use Cases. Com a divisão em microsserviços, cada repositório precisava ter sua própria implementação dessa classe.

## Discussão e possibilidades

A classe `Ator` contém `UsuarioId`, `ClienteId` e uma lista de `Roles`. Ela é usada em todos os Use Cases para validar se o usuário tem permissão para executar a ação (RBAC) e se o recurso pertence a ele (ownership).

A solução ideal seria abstrair a classe `Ator` em uma **biblioteca compartilhada (NuGet package privado)**, que seria referenciada por todos os repositórios. Isso garantiria uma única fonte da verdade para a lógica de autorização.

Contudo, por limitação de tempo, optei por replicar manualmente a classe em cada repositório. Cada microsserviço tem sua própria implementação de `Ator`, com os mesmos atributos base mas extension methods específicos para seus Use Cases.

## Decisão

Foi decidido replicar manualmente a classe `Ator` em cada repositório.

## Consequências

**Positivas:**

* Agilidade na entrega.

**Negativas:**

* Código duplicado. Alterações na lógica base de `Ator` precisam ser replicadas manualmente em todos os repositórios.
* Risco de divergência entre as implementações se não houver atenção na manutenção.

---
Anterior: [ADR 0006 - Geração de Token no Lambda e Forward Token](0006_adr_geracao_token_lambda_e_forward_token.md)
Próximo: [ADR 0008 - Idempotência na Criação de OS Completa](0008_adr_idempotencia_criacao_os_completa.md)
