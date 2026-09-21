# Claude Code / Cursor Plugins — Uniube

Marketplace interno de plugins (Claude Code **e** Cursor) com os padrões de desenvolvimento da empresa, derivados dos guias de padronização (`PADRONIZACAO-FRONTEND.md` e `PADRONIZACAO-BACKEND.md`). Serve as 3 equipes de desenvolvimento, testando IAs diferentes — o conteúdo aqui é **genérico de propósito**: nenhuma porta, credencial de dev, código de tela específico ou nome de projeto entra neste repositório.

> **Nota sobre o suporte a Cursor**: o Cursor só ganhou marketplace de plugins recentemente (Cursor 2.5, fev/2026) e a parte de manifesto (`.cursor-plugin/`) deste repositório foi montada com base na documentação pública, sem um Cursor real pra testar. Valide na prática antes de confiar 100% — veja a seção "Cursor" abaixo.

## Por que isso existe

Cada projeto acumula, com o tempo, uma configuração rica de `.claude/` (skills, agents, rules) — mas até agora isso vivia só dentro do repositório de cada projeto. Resultado: quando um time criava um projeto novo, ou reaprendia do zero, ou copiava e colava arquivos de outro repositório, que iam ficando desatualizados com o tempo (drift). Este marketplace resolve isso dando **uma fonte única e versionada** do que é genérico o suficiente pra valer pras 3 equipes, instalável em qualquer projeto sem copiar arquivo manualmente.

## O que tem aqui

| Plugin | Pra quando o projeto é |
|---|---|
| `uniube-frontend-standards` | Next.js (App Router) + `componentes-uniube` |
| `uniube-backend-standards` | Node.js + Express + Oracle |

Cada plugin contém:
- **Rules** — convenções sempre ativas ou por tipo de arquivo (`paths`).
- **Skills** — passo a passo pra fluxos recorrentes (nova tela, novo modal, nova rota, etc.).
- **Agents** — papéis especializados (revisor de padrão, test-runner, buscador de rota similar, etc.).

## O que **não** vai entrar aqui

Qualquer coisa específica de um projeto: portas de serviço, credenciais de desenvolvimento, códigos de tela/programa, nomes de módulo do negócio, cores de marca em hex, connection strings. Isso continua vivendo no `.claude/rules/` e no `CLAUDE.md` **do próprio projeto**, ao lado do que vem do plugin — os dois coexistem.

Um jeito prático de decidir "isso é genérico ou específico?": se a frase ainda faz sentido trocando o nome do projeto por outro qualquer da empresa, é genérico. Se cita uma porta, uma credencial ou um código de tela, é específico e fica no projeto.

## Como usar num projeto — Claude Code

1. Adicionar este marketplace (uma vez por projeto — dá pra deixar isso versionado em `.claude/settings.json` do projeto, assim quem clona já recebe):
   ```
   /plugin marketplace add <url-ou-caminho-deste-repositorio>
   ```
2. Instalar o plugin correspondente:
   ```
   /plugin install uniube-frontend-standards@uniube-claude-plugins
   /plugin install uniube-backend-standards@uniube-claude-plugins
   ```
3. No próprio projeto, criar (se ainda não existir) as rules locais que os agents deste plugin esperam encontrar — principalmente a de **ambiente de desenvolvimento** (portas, como subir cada serviço, fluxo de autenticação local) que o agent `test-runner` do plugin de frontend e o skill `testar-rota` do plugin de backend precisam pra funcionar sem inventar credencial.

## Como usar num projeto — Cursor

O Cursor tem dois jeitos de importar um marketplace de plugin, dependendo do plano:

- **Plano Teams/Enterprise**: `Dashboard → Plugins & MCPs → Add Marketplace → Import from Repo`, colando a URL deste repositório.
- **Plano individual/Pro** (nosso caso hoje): via `cursor-agent` (CLI do Cursor), rodando dentro dele:
  ```
  cursor-agent
  ```
  e, no prompt interativo:
  ```
  /plugin marketplace add <url-deste-repositorio>
  ```
  Depois, ainda dentro do `/plugin`, escolher o marketplace `uniube-claude-plugins` e instalar `uniube-frontend-standards`/`uniube-backend-standards` pela interface.

Isso **não foi testado num Cursor real** — se o comando/caminho acima não bater com o que aparecer na tela de vocês, é sinal de que a documentação pública mudou ou que o plano individual não dá esse acesso; nesse caso, cai no plano B: copiar manualmente `rules/`, `skills/` e `agents/` do plugin desejado pro `.cursor/` do projeto (mesma ideia da migração Cursor↔Claude Code do `PADRONIZACAO-FRONTEND.md` §12.3, só que copiando os arquivos ao invés de instalar).

As rules deste repositório já vêm com frontmatter dupla (`paths` pro Claude Code, `globs`+`alwaysApply` pro Cursor) no mesmo arquivo — não precisa de cópia separada por ferramenta.

## Como atualizar

Uma vez instalado, atualizações no plugin (nova versão publicada aqui) chegam automaticamente pras equipes que já instalaram — sem precisar copiar arquivo de novo. Ao alterar um plugin, suba o campo `version` no `plugin.json` dele (**os dois**, `.claude-plugin/plugin.json` e `.cursor-plugin/plugin.json`) e na entrada correspondente de cada `marketplace.json` (idem, os dois).

## Estrutura deste repositório

```
claude-plugins-uniube/
├── README.md
├── .claude-plugin/
│   └── marketplace.json              # catálogo dos plugins (Claude Code)
├── .cursor-plugin/
│   └── marketplace.json              # catálogo dos plugins (Cursor)
├── uniube-frontend-standards/
│   ├── .claude-plugin/plugin.json    # manifesto Claude Code
│   ├── .cursor-plugin/plugin.json    # manifesto Cursor
│   ├── rules/                        # frontmatter dupla (paths + globs/alwaysApply)
│   ├── skills/
│   └── agents/
└── uniube-backend-standards/
    ├── .claude-plugin/plugin.json
    ├── .cursor-plugin/plugin.json
    ├── rules/
    ├── skills/
    └── agents/
```

## Contribuindo com um padrão novo

Antes de adicionar algo aqui, pergunte: isso já foi validado em produção em pelo menos um projeto? Regra nova sem uso real tende a virar ruído. Ao propor uma mudança, atualize também o guia de padronização correspondente (`PADRONIZACAO-FRONTEND.md`/`PADRONIZACAO-BACKEND.md`) — os dois devem contar a mesma história, um em prosa (pra humano ler) e outro em `.claude/` (pra IA carregar).
