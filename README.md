# infra-live

Onde os módulos viram infraestrutura de verdade: composição por ambiente, state, identidade e o **Atlantis**, que é quem aplica tudo.

📋 **Kanban:** https://github.com/orgs/TIPS-2026/projects/1 · 🏠 **Visão geral:** [giropops-senhas](https://github.com/TIPS-2026/giropops-senhas)

## Donos

| Time | Responsabilidade |
|---|---|
| **Runtime** | Bootstrap do state, identidade (OIDC), AMI e Atlantis, detecção de drift |
| **Redes** | Rede de cada ambiente |
| **Compute** | Cluster, load balancer, registry e serviço de cada ambiente |

## O que esperamos que seja entregue

### Runtime
- **Bootstrap do backend de state**: remoto, versionado, criptografado e com lock, e documentação de como foi criado
- **Identidade via OIDC** entre GitHub Actions e AWS: roles separadas para build/push e para deploy por ambiente, com confiança restrita a repositório, branch e environment
- **Imagem própria (Packer)** para o Atlantis, com versões fixadas
- **Atlantis** no ar, restrito à org, sem credenciais estáticas, com plan comentado no PR e apply só com PR aprovado
- **Detecção de drift** somente leitura, abrindo issue quando algo mudar fora do código
- `docs/contracts/runtime.md` com as convenções de state e roles

### Redes e Compute
- Ambientes **`dev/`** e **`prod/`** consumindo os módulos do `terraform-modules` **por tag**
- Diferenças entre dev e prod explícitas e justificadas (custo × disponibilidade)
- Outputs necessários para o CD/Push fazer deploy repassados ao time

## Estrutura esperada

```
bootstrap/   # state (criado uma única vez)
runtime/     # identidade, Packer, Atlantis
dev/         # composição de dev (rede, compute...)
prod/        # composição de prod
docs/
```

## Regras

- **Apply só pelo Atlantis.** Ninguém aplica da própria máquina (exceto o bootstrap, documentado).
- **Módulos sempre por tag**, nunca por branch.
- **Nenhuma credencial estática** em código, variável ou secret.
- **Custo importa:** documente o custo estimado dos componentes caros (NAT, load balancer) e destrua o que não estiver em uso.
