# Entrega — Prova do Segundo Bimestre (Parte Prática)

**Aluno:** Carina Goncalves dos Santos Dalpino  
**RA:** 6325109  
**Data de entrega:** 28/11/2026  

## Fork do Desafio

- URL do fork: https://github.com/CarinaDalpino/implement_Moodle
- Branch principal com a solução: main
- Commit final: https://github.com/CarinaDalpino/implement_Moodle/commit/62e8cb98bed74bb058687c8ae6800322297560c8

## Evidências

> Todas as evidências (screenshots, código, documentos) estão no fork acima.

### Entregável 1 — Docker Local
- [x] Screenshot do Moodle em localhost:8080: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/print-moodle.png
- [x] Screenshot do status dos contêineres (`docker compose ps`): https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/docker.png

### Entregável 2 — Terraform
- [x] Módulo `networking`: https://github.com/CarinaDalpino/implement_Moodle/tree/main/terraform/modules/networking
- [x] Módulo `compute` (com `user_data.sh`): https://github.com/CarinaDalpino/implement_Moodle/tree/main/terraform/modules/compute
- [x] Módulo `database` (RDS privado): https://github.com/CarinaDalpino/implement_Moodle/tree/main/terraform/modules/database
- [x] Screenshot `terraform fmt` e `validate`: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/terraform-validate.png
- [x] Screenshot `terraform plan` sem erros (19 to add): https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/terraform-plan.png

### Entregável 3 — Documento de Arquitetura
- [x] `docs/architecture-design.md`: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/architecture-design.md

### Entregável 4 — CI/CD
- [x] `terraform-plan.yml`: https://github.com/CarinaDalpino/implement_Moodle/blob/main/.github/workflows/terraform-plan.yml
- [x] Execução no GitHub Actions: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/github-actions-plan.png
- [x] Comentário do plano na PR: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/github-pr-comment.png
- [x] PR do fork com os 3 checks aprovados: https://github.com/CarinaDalpino/implement_Moodle/pull/1

### Entregável 5 — Kiro e Kiro Workflows
- [x] Specs do Kiro em `.kiro/specs/`: https://github.com/CarinaDalpino/implement_Moodle/tree/main/.kiro/specs
  - `networking` (módulo Terraform), `docker-env-fix` e `pricing-calculator-link`
- [x] Screenshot do Kiro Workflow: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/kiro-workflow.png
- [x] Relatório gerado pelo workflow: https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/kiro-investigate-report.md
- [x] Descrição: Executei o workflow `investigate` do Kiro para diagnosticar por que a senha do `docker/.env` não tinha efeito no Docker local. Ele analisou o `docker-compose.yml` e concluiu que os serviços `db` e `moodle` liam o `.env.example` no `env_file`, então o PostgreSQL nunca recebia a senha do `.env`. Também apontou que o volume `db_data` mantém a senha antiga. Com esse diagnóstico criei a spec `docker-env-fix` e corrigi o `env_file` e o workflow `docker-lint`. Depois recriei os volumes e refiz a instalação do Moodle.

### Entregável 6 — Organização
- [x] Screenshot `pytest -v` passando (64 testes): https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/pytest.png
- [x] Commits semânticos: https://github.com/CarinaDalpino/implement_Moodle/commits/main

## Observações

- O Moodle também foi implantado e testado na AWS (EC2 + RDS): https://github.com/CarinaDalpino/implement_Moodle/blob/main/docs/img/moodle-aws.png
- O `docker/.env` não está no repositório. O `docker-compose.yml` lê `docker/.env`, e o workflow `docker-lint` o cria a partir do `.env.example`.
- O RDS usa `rds.force_ssl = 0`, porque o instalador web do Moodle não configura SSL no cliente. O banco fica em subnet privada e o `sg-db` aceita só o `sg-app`. A senha também passa pelo `user_data` e fica no estado do Terraform. As duas limitações estão descritas no documento de arquitetura.
- A EC2 usa `t3.micro`, como o enunciado da prova pede. O `user_data.sh` cria 2 GB de swap por causa da memória de 1 GB.
- A estimativa de custo (US$ 28,28 por mês) foi feita na AWS Pricing Calculator, e o link está no documento de arquitetura.

## Ferramentas de IA utilizadas

Kiro (specs e workflow `investigate`).

**Gerado pelo Kiro (rascunho):**
- os módulos Terraform `networking`, `compute` e `database`, com o `user_data.sh`;
- o workflow `terraform-plan.yml`;
- o documento `docs/architecture-design.md`;
- as specs `networking`, `docker-env-fix` e `pricing-calculator-link`;
- o relatório do workflow `investigate`, que levou à correção do `env_file` e do `docker-lint`.

**Feito e ajustado por mim:**
- revisei o código e testei com as credenciais do Learner Lab: `terraform fmt`, `validate`, `plan` e `apply`;
- corrigi o que falhou, como registram os commits `fix`: a extensão `pgsql` no Dockerfile, a variável `ssh_allowed_cidr` como `sensitive`, o `hashFiles` do `terraform-validate.yml` e a regra DL3008 do hadolint;
- defini o `rds.force_ssl = 0`, o swap de 2 GB e o `t3.micro`, e os documentei no documento de arquitetura;
- configurei os secrets do GitHub e atualizei as credenciais do Lab quando expiraram;
- recriei os volumes do Docker, refiz a instalação do Moodle e tirei os prints;
- montei a estimativa na AWS Pricing Calculator;
- rodei o `pytest` (64 testes) e fiz o merge da PR #1 depois dos 3 checks aprovados;
- fiz o `terraform apply` na AWS, abri o Moodle pelo DNS público da EC2 e testei o `health-check.yml`;
- escrevi o `README.md` com pré-requisitos, Docker, Terraform e a URL pública do Moodle;

