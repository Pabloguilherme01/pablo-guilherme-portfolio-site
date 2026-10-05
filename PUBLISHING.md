# Contrato de publicação

Este repositório é um **artefato gerado**. O código-fonte vive em `Pabloguilherme01/pablo-guilherme-portfolio`.

## Fluxo esperado

1. CI do repositório-fonte passa.
2. O workflow de publicação gera `dist/public`.
3. O conteúdo estático é sincronizado para este repositório.
4. O arquivo `SOURCE_COMMIT` registra o SHA exato do commit-fonte.

A publicação cross-repo requer o segredo administrativo `PORTFOLIO_PUBLISH_TOKEN` no repositório-fonte.

## Regra de manutenção

Não corrija HTML ou assets compilados manualmente. Corrija no repositório-fonte e publique novamente.
