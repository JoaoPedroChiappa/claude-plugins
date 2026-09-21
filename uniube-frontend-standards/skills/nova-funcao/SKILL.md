---
name: nova-funcao
description: Cria hooks, utilitários, máscaras e exportação Excel/PDF. Use quando pedir hook, util, máscara, helper, função geral, Excel ou PDF.
---

# Novo hook/util/função geral

## Antes de escrever qualquer código

**Verifique primeiro se já existe algo equivalente** em `app/_hooks/`, `app/_utils/funcoesGerais.ts`, `app/_utils/pdfUtils.ts` e `app/_utils/utils.ts` — duplicar máscara/formatador/gerador já existente é o erro mais comum de divergência entre módulos.

## Onde colocar

| Tipo | Pasta | Assinatura |
|---|---|---|
| Hook reativo (tem estado/efeito) | `app/_hooks/use<Nome>.ts` | `export const use<Nome> = () => { ... }` |
| Função pura (máscara, formatação) | `app/_utils/funcoesGerais.ts` (ou arquivo próprio se for um domínio novo) | `export function <nome>(valor: T): U { ... }`, sem side effects |
| Geração de Excel | usar o utilitário central já existente — não trocar de lib |
| Geração de PDF | `app/_utils/pdfUtils.ts` |
| Contexto global | `app/_context/*.tsx` — só se o estado for de fato compartilhado entre telas, nunca pra estado local de um componente |

## Passos

1. Confirmar que não existe equivalente (ver acima).
2. Implementar como função pura sempre que possível — sem `useState`/`useEffect` quando não precisar reagir a nada.
3. Export nomeado, nunca default, pra hooks e utils.
4. Se for hook com fetch: seguir a rule `hooks-react19` deste plugin (`.then()/.catch()`, `useEffectEvent` pra callbacks instáveis).
5. Rodar type-check nos arquivos que passam a importar a nova função.

## Errado

```typescript
// ❌ Duplicar uma máscara que já existe em outro lugar do projeto
export function formatarCpf(v: string) { ... }

// ❌ Contexto novo pra estado que só um componente usa
export const MeuContexto = createContext(...);
```
