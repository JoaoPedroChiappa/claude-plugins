---
name: escrever-testes
description: Orienta criação e execução de testes de frontend. Use quando pedir teste, cobertura, spec, Vitest, RTL ou validação automatizada de componentes.
---

# Escrever testes (frontend)

## Se o projeto ainda não tem test runner configurado

Não instale Jest/Vitest/Playwright silenciosamente. Em vez disso:

1. Rode `npm run lint` e garanta que está limpo (sem `eslint-disable` pra contornar `set-state-in-effect`/`exhaustive-deps`/`preserve-manual-memoization`).
2. Liste o que **seria** testado — comportamentos observáveis, não detalhes de implementação.
3. Se o usuário pedir explicitamente pra criar testes pela primeira vez: proponha (com aprovação) **Vitest** + **@testing-library/react** + **jsdom**, script `"test": "vitest"`, testes colocalizados (`Componente.test.tsx` ao lado do componente).

## Ao escrever testes (runner já existe ou acabou de ser aprovado)

- Colocalizar `*.test.tsx`/`*.test.ts` ao lado do arquivo fonte.
- Mockar: o cliente HTTP central do projeto, o hook/contexto de autenticação e permissões, `ProtectedRoute` (pass-through), navegação.
- Testar comportamento — fluxos de CRUD, permissões, validação, tratamento de erro de API.
- **Não** testar estado interno, ordem de hooks, nem fazer snapshot de markup inteiro.
- **Não** criar componente de UI só pra facilitar o teste, em vez de usar a lib de UI compartilhada como o app usa de verdade.
- **Não** depender de uma API real rodando.

## Casos mínimos pra uma tela CRUD

| Caso | O que verificar |
|---|---|
| Render | Lista exibe dados mockados |
| Filtro | Filtro client-side reduz resultados |
| Modal | Botão "Novo" abre modal (quando há permissão) |
| Permissão | Botão desabilitado sem a permissão correspondente |
| Validação | Campo obrigatório vazio → aviso antes do submit |
| Erro de API | Resposta 4xx/5xx → mensagem via tratamento central |
