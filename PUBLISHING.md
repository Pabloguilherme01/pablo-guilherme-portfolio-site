# Contrato de publicação

O repositório é **somente de artefatos gerados**. A fonte continua em [pablo-guilherme-portfolio](https://github.com/Pabloguilherme01/pablo-guilherme-portfolio), na branch `master`.

## Fluxo sem PAT entre repositórios

O workflow [Sync portfolio from validated source](.github/workflows/sync-validated-source.yml) executa diariamente ou por acionamento manual (opcionalmente com `force_rebuild=true` para testar o build mesmo que o SHA seja igual):

1. Descobre o commit de `master` e compara ao arquivo `SOURCE_COMMIT` já publicado.
2. Se mudou, exige um resultado **CI de push aprovado para o SHA exato** no repositório público de origem.
3. Clona a fonte pública, confirma novamente o SHA, instala usando lockfile e gera `dist/public`.
4. Registra o SHA, verifica caminhos/assets via script do próprio repositório-fonte.
5. Sincroniza apenas o build, preservando `.github/`, `README.md` e `PUBLISHING.md`.
6. Usa exclusivamente o `GITHUB_TOKEN` deste repositório para gravar o artefato e acionar o workflow [Deploy portfolio artifact to GitHub Pages](.github/workflows/deploy-pages.yml), que confere a integridade antes da publicação.

O modo manual forçado refaz as verificações de CI e build; se o conteúdo não mudar, ele solicita uma nova verificação e publicação pelo Pages sem gerar um commit desnecessário.

Não há token pessoal compartilhado entre repositórios. Sem CI aprovado ou com mudança de HEAD durante a verificação, a execução falha sem atualizar os arquivos.

**Pré-requisito:** GitHub Actions deste repositório deve ter permissão de gravação em Contents e permissão de acionamento de Actions. Regras da `main` também precisam autorizar esse bot por um caminho aprovado. Não enfraqueça a proteção geral da branch por conveniência.

**Migração:** o workflow antigo da fonte ainda exige `PORTFOLIO_PUBLISH_TOKEN`. Só desativá-lo após um primeiro ciclo bem-sucedido com o novo fluxo, para evitar duplicidade. Não adicione tokens a commits, arquivos, issues ou logs.

## Verificação de prontidão

- CI da fonte aprovado para o mesmo SHA.
- `SOURCE_COMMIT` corresponde ao build.
- Integridade do artefato e deploy Pages aprovados.
- Navegador real e 320 px validados após a publicação.

Não altere HTML e assets compilados manualmente: corrija a fonte e gere o build novamente.
