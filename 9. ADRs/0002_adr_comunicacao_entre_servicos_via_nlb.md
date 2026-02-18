# ADR 0002 - Comunicação entre Serviços via NLB

## Status

Aceito

## Contexto

Com a divisão em microsserviços, era necessário definir como os serviços se comunicariam entre si. Cada microsserviço roda em pods separados dentro do mesmo cluster EKS, e precisam se encontrar para chamadas HTTP síncronas.

## Discussão e possibilidades

Foram avaliadas as formas de expor serviços no Kubernetes:

1. **ClusterIP + Service Discovery interno:** Funciona, mas exigiria configuração adicional de DNS interno para resolução de nomes entre namespaces.
2. **Network Load Balancer (NLB):** Cada serviço expõe um Kubernetes Service do tipo `LoadBalancer`, que provisiona um NLB na AWS. Os serviços se comunicam usando a URL do NLB.
3. **Service Mesh (Istio, Linkerd):** Seria o overkill para o projeto, adicionando complexidade operacional sem benefícios proporcionais.

O NLB foi a forma mais direta e simples de fazer. Cada serviço tem seu NLB, e o Ordem de Serviço conhece as URLs dos serviços de Cadastro e Estoque via variáveis de ambiente configuradas no ConfigMap do Kubernetes.

## Decisão

Foi decidido utilizar Network Load Balancer (NLB) da AWS para comunicação entre os microsserviços.

## Consequências

**Positivas:**

* Configuração simples via Kubernetes Service `type: LoadBalancer`.
* Funciona nativamente com o EKS sem configurações adicionais.
* Load balancing automático entre réplicas do serviço destino.

**Negativas:**

* Cada NLB tem um custo na AWS.
* O tráfego entre serviços sai e volta pela rede da AWS ao invés de ficar apenas dentro do cluster.

---
Anterior: [ADR 0001 - Divisão em Microsserviços](0001_adr_divisao_em_microsservicos.md)
Próximo: [ADR 0003 - SAGA Coreografada](0003_adr_saga_coreografada.md)
