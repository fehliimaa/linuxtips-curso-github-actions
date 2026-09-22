# linuxtips-curso-github-actions

Repositório de estudos do curso de **GitHub Actions** da [LINUXtips](https://linuxtips.io).

Todos os workflows usam `workflow_dispatch`, ou seja, são executados **manualmente** pela aba **Actions** do repositório (botão **Run workflow**).

## Workflows

| Arquivo | O que pratica |
| --- | --- |
| [`meu-primeiro-workflows.yml`](.github/workflows/meu-primeiro-workflows.yml) | Primeiro workflow: um job com steps simples usando `run`. |
| [`trabalho-entre-jsteps.yml`](.github/workflows/trabalho-entre-jsteps.yml) | Compartilhamento de arquivos entre **steps** do mesmo job (mesmo runner/workspace). |
| [`trabalho-entre-jobs.yml`](.github/workflows/trabalho-entre-jobs.yml) | Compartilhamento de arquivos entre **jobs** com `actions/upload-artifact` e `actions/download-artifact`, e dependência entre jobs com `needs`. |
| [`docker-scout-imagem.yml`](.github/workflows/docker-scout-imagem.yml) | Uso de `inputs` no `workflow_dispatch`, pull de imagem Docker e relatório de vulnerabilidades com o Docker Scout. |

## Desafio 1: Docker Scout

O workflow `docker-scout-imagem.yml` recebe o nome de uma imagem Docker via input (padrão: `alpine`) e executa:

1. `docker pull` da imagem informada;
2. `docker image ls` para listar as imagens baixadas;
3. Login no Docker Hub com `docker/login-action`;
4. Relatório de CVEs com [`docker/scout-action`](https://github.com/docker/scout-action) (`command: cves`).

### Pré-requisitos

O Docker Scout exige autenticação no Docker Hub. Cadastre os secrets em **Settings → Secrets and variables → Actions → New repository secret**:

| Secret | Valor |
| --- | --- |
| `DOCKERHUB_USERNAME` | Usuário do Docker Hub |
| `DOCKERHUB_TOKEN` | Personal Access Token do Docker Hub (Account Settings → Personal access tokens) |

### Como executar

1. Acesse a aba **Actions**;
2. Selecione **Pull e analise de imagem Docker com Scout**;
3. Clique em **Run workflow**, informe a imagem (ex: `alpine`, `alpine:3.20`) e confirme;
4. O relatório de vulnerabilidades aparece no log do step do Scout.

## Conceitos praticados

- `on: workflow_dispatch` e `inputs`
- `jobs`, `steps`, `runs-on` e `needs`
- `run` x `uses` (comandos shell x actions compartilhadas)
- Contexts: `${{ github.workspace }}`, `${{ inputs.* }}`, `${{ secrets.* }}`
- Artifacts entre jobs
- Secrets de repositório
