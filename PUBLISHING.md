# Publicação do portfólio — fonte privada

**Situação verificada em 24/10/2026:** o código-fonte [pablo-guilherme-portfolio](https://github.com/Pabloguilherme01/pablo-guilherme-portfolio) tem visibilidade **privada**, e este repositório de artefatos é **público**. O token padrão (`GITHUB_TOKEN`) deste repositório não possui automaticamente permissão para ler a fonte privada. Uma tentativa de clonagem anônima falhou em [GitHub Actions](https://github.com/Pabloguilherme01/pablo-guilherme-portfolio-site/actions/runs/40128565505). Por isso, o workflow de sincronização sem credenciais foi **retirado**; ele não deve consumir minutos de CI com erros diários.

## Caminho oficial, sem publicar o código-fonte

1. Na **fonte privada** (`Pabloguilherme01/pablo-guilherme-portfolio`), a CI deve aprovar o commit de `master` que será publicado.
2. Criar uma credencial de **escopo mínimo** com escrita em Contents **somente** para `pablo-guilherme-portfolio-site` (por exemplo, token pessoal fine-grained) e guardá-la em `Settings > Secrets and variables > Actions` do **repositório-fonte**, com nome `PORTFOLIO_PUBLISH_TOKEN`. Não compartilhar valor por chat, issue ou commit.
3. Executar o workflow **Publish static portfolio artifact** na fonte, após confirmar o commit. Esse workflow valida o SHA-fonte, gera `dist/public`, escreve `SOURCE_COMMIT` e atualiza somente os arquivos gerados neste repositório.
4. Este repositório executa **Deploy portfolio artifact to GitHub Pages** ao receber alterações do build. O deploy depende da validação [Artifact integrity](.github/workflows/artifact-integrity.yml) e usa o mesmo build com proveniência. O workflow também aceita acionamento manual por uma pessoa com permissão.
5. Após publicar, confirmar GitHub Pages concluído com sucesso, conferir `SOURCE_COMMIT` com o SHA da fonte e testar mobile 320 px e desktop.

Se preferir uma credencial de GitHub App instalada nos dois repositórios, a segurança e a complexidade precisam ser avaliadas antes de substituir o PAT. Não tornar a fonte pública apenas para contornar a autenticação.

## Publicação do artefato atual

Um redeploy por atualização deste documento ou por execução manual do Pages **republica o artefato que já está neste repositório**; isso não representa um novo build da fonte privada. Verifique `SOURCE_COMMIT`. Uma publicação GitHub Pages aprovada confirma distribuição do artefato, **não** a qualidade da loja, APIs ou testes em aparelhos físicos.

## Contrato e recuperação

- Repositório-fonte privado é a única origem de código.
- Destino público mantém apenas `index.html`, `assets/`, `404.html`, `.nojekyll`, `SOURCE_COMMIT` e arquivos administrativos.
- O SHA escrito em `SOURCE_COMMIT` deve coincidir com o commit realmente construído.
- Não alterar manualmente HTML/assets compilados, não relaxar proteção da `main` nem expor token em URL de clone ou nos logs.
- Em caso de falha, consultar a etapa exata dos workflows antes de alterar credenciais ou fazer novos deploys.
