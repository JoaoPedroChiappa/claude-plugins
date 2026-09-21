# Claude Code Plugins — Uniube

Marketplace interno de plugins do Claude Code com os padrões de desenvolvimento da empresa, derivados dos guias de padronização (`PADRONIZACAO-FRONTEND.md` e `PADRONIZACAO-BACKEND.md`). Serve as 3 equipes de desenvolvimento — o conteúdo aqui é **genérico de propósito**: nenhuma porta, credencial de dev, código de tela específico ou nome de projeto entra neste repositório.

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

## Como usar num projeto (qualquer uma das 3 equipes)

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

## Como atualizar

Uma vez instalado, atualizações no plugin (nova versão publicada aqui) chegam automaticamente pras equipes que já instalaram — sem precisar copiar arquivo de novo. Ao alterar um plugin, suba o campo `version` no `plugin.json` dele **e** na entrada correspondente do `marketplace.json`.

## Estrutura deste repositório

```
claude-plugins-uniube/
├── README.md
├── .claude-plugin/
│   └── marketplace.json              # catálogo dos plugins
├── uniube-frontend-standards/
│   ├── .claude-plugin/plugin.json
│   ├── rules/
│   ├── skills/
│   └── agents/
└── uniube-backend-standards/
    ├── .claude-plugin/plugin.json
    ├── rules/
    ├── skills/
    └── agents/
```

## Contribuindo com um padrão novo

Antes de adicionar algo aqui, pergunte: isso já foi validado em produção em pelo menos um projeto? Regra nova sem uso real tende a virar ruído. Ao propor uma mudança, atualize também o guia de padronização correspondente (`PADRONIZACAO-FRONTEND.md`/`PADRONIZACAO-BACKEND.md`) — os dois devem contar a mesma história, um em prosa (pra humano ler) e outro em `.claude/` (pra IA carregar).
