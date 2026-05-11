# Adele Goldstine — Avaliação da Qualidade de Software

**Disciplina:** Qualidade de Software 1 (FCTE — UnB) — 2026.1 — Turma T01
**Objeto de avaliação:** [SuaGradeUnB](https://suagradeunb.com.br/) ([código-fonte](https://github.com/unb-mds/2023-2-SuaGradeUnB))
**Modelo de qualidade:** ISO/IEC 25010 (manutenibilidade e segurança)

Site da avaliação: **https://FCTE-Qualidade-de-Software-1.github.io/2026-1_T01_ADELE_GOLDSTINE/**

## Rodando localmente

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Depois abra `http://127.0.0.1:8000`.

Para gerar a versão estática:

```bash
mkdocs build --strict
```

## Deploy

Todo push para `main` dispara o workflow [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml), que roda `mkdocs gh-deploy --force` e publica o site na branch `gh-pages`.

Após o primeiro deploy, habilite GitHub Pages em **Settings → Pages → Source = Deploy from a branch → `gh-pages` / `/ (root)`**.

## Estrutura

```
.
├── .github/workflows/deploy.yml  # pipeline de deploy
├── docs/                         # conteúdo do site
│   ├── index.md
│   ├── assets/                   # logo, favicon
│   ├── stylesheets/extra.css     # ajuste fino da paleta
│   └── fase{1..4}/               # páginas das fases da avaliação
├── mkdocs.yml                    # configuração do MkDocs
└── requirements.txt              # mkdocs + mkdocs-material
```
