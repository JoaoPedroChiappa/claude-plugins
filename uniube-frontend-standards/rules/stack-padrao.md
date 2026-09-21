---
alwaysApply: true
---

# Padrão geral de frontend (Uniube)

Baseado no Guia de Padronização — Front-end da empresa. Vale para todo projeto Next.js que usa a lib de UI compartilhada `componentes-uniube`.

## Regra de ouro

Se o código novo divergir do template canônico do projeto, **o template ganha**. Copie a tela/modal/hook mais parecido já existente; não reinvente. A maior fonte histórica de divergência entre projetos não foi falta de padrão — foi modernização pontual (nova lib, novo padrão de estado) aplicada num projeto e não nos demais.

## Stack de referência

| Camada | Escolha padrão |
|---|---|
| Framework | Next.js, App Router (sem Pages Router em projeto novo) |
| UI runtime | React estável mais recente (ex. React 19) |
| Linguagem | TypeScript |
| Estilo | Tailwind CSS — cores de marca via classes utilitárias do design system, nunca hex direto |
| Componentes de UI | `componentes-uniube` — **não** criar componente novo enquanto a lib já oferecer o equivalente |
| Ícones | `lucide-react` |
| HTTP | `axios` com interceptors, exportado de um contexto único (`LoadingContext` ou equivalente). Chamadas **inline** no componente — nunca uma camada `services/`/`api/` |
| Autenticação/permissões | Contexto próprio (`AuthContext`) + guarda de rota (`ProtectedRoute`). Permissões por tela: `permissoesTela.incluir/.alterar/.excluir` |

## Proibido por padrão (mudar exige decisão explícita, não é escolha de quem está codando)

- `react-hook-form`, `Formik`, `Zod`, `React Query`, `Redux`
- Camada `services/` ou `api/` separada das telas
- Server Components para telas com dados (o padrão é client-side)
- Criar componente de UI novo (arquivo ou função) no lugar do que `componentes-uniube` já exporta — só com autorização explícita do usuário
- Instalar qualquer dependência nova sem aprovação

## Estrutura de pastas

```
app/
  (routes)/
    <modulo>/
      page.tsx                        # dashboard do módulo (cards, indicadores)
      cadastros/
        <entidade>/
          page.tsx                    # listagem/CRUD
          _modal/
            Modal<Entidade>.tsx       # form de criação/edição
      processos/
        <fluxo>/
          page.tsx
          indexFuncoes.ts             # lógica extraída quando a tela cresce
  _hooks/
    use*.ts                          # hooks reativos (export nomeado)
  _utils/
    funcoesGerais.ts                  # máscaras, Excel, formatação
    pdfUtils.ts                       # geração de PDF
    utils.ts                          # cn() — merge de classes Tailwind
  _context/
    AuthContext.tsx                   # token, permissoesTela, userIdVirtual
    LoadingContext.tsx                 # api (axios), GlobalLoader
    ProtectedRoute.tsx                 # guarda de rota + permissões
    TelaContext.tsx                    # tela/programa/aba ativa
menu/
  <modulo>.json                       # entradas do menu lateral desse módulo
```

Convenções: pastas de rota em **kebab-case**, em **português**. `*Funcoes.ts`/`_utils`/`_hooks` nunca são rotas — só o loader do App Router enxerga `page.tsx`. Contextos ficam em `_context/`; só criar contexto novo se o estado for **de fato compartilhado** entre telas.

## Regras gerais obrigatórias

- `'use client'` no topo de toda página.
- Campos que entram/saem da API em **UPPER_SNAKE_CASE** (`GRUPO_DESPESA`, `DESCRICAO`) — não normalizar para camelCase no front.
- Textos de UI em português.
- Alias de import padronizado (ex. `@/app/...`).
