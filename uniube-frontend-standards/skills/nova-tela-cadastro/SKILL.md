---
name: nova-tela-cadastro
description: Cria telas de cadastro/listagem CRUD copiando o padrão existente do projeto. Use quando o usuário pedir nova página, cadastro, listagem, CRUD, rota ou tela de consulta com tabela.
---

# Nova tela de cadastro/listagem

## Antes de escrever qualquer código

1. Encontre o template canônico do projeto (a tela de cadastro simples mais parecida já existente — veja a rule `stack-padrao` deste plugin ou a documentação local do projeto para saber qual é). **Leia esse arquivo inteiro** antes de começar.
2. Confirme com o usuário: nome do módulo, nome da entidade, campos do formulário, e o código de programa (`ProtectedRoute`/menu) que a tela vai usar.
3. Descubra o endpoint da API já existente (ou peça pro usuário indicar/criar no backend) — não invente shape de resposta.

## Passos

1. Criar a pasta `app/(routes)/<modulo>/cadastros/<entidade>/page.tsx` (kebab-case, português), copiando a estrutura do template.
2. Adaptar: nome da entidade, campos da tabela, campos do filtro, código de `ProtectedRoute`.
3. Criar `_modal/Modal<Entidade>.tsx` — ver skill `novo-formulario` deste mesmo plugin para o padrão de modal.
4. Adicionar a entrada correspondente em `menu/<modulo>.json` — `programa` tem que bater com o `codigo` do `ProtectedRoute`.
5. Rodar lint e type-check (`npx tsc --noEmit`, escopando pra só os arquivos tocados se o build completo do projeto estiver bloqueado por outra coisa).
6. Perguntar ao usuário se quer verificação no Browser pane antes de considerar a tela pronta (ver rule `verificacao-ui`).

## Checklist de conformidade (ver rule `telas-crud` para o detalhe de cada item)

- [ ] `'use client'` no topo
- [ ] `ProtectedRoute` com `tela` e `codigo`
- [ ] Paginação client-side (`itemsPerPage = 10`)
- [ ] Filtro client-side com `normalize('NFD')`
- [ ] Modal via `dynamic(..., { ssr: false, loading: () => <GlobalLoader/> })`
- [ ] Botões de ação (Novo/Excel/Editar/Excluir) gated por `permissoesTela`
- [ ] Exclusão com confirmação por toast, nunca `window.confirm`
- [ ] Fetch em `useEffect` com `.then()/.catch()`, nunca `try/await` + `setState`
- [ ] Nenhum `eslint-disable-next-line`
- [ ] Campos de API em UPPER_SNAKE
