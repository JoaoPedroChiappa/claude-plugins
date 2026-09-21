---
paths:
  - "app/_hooks/**/*.ts"
  - "app/_utils/**/*.ts"
  - "app/_context/**/*.tsx"
---

# Hooks, utils e contextos

| Tipo | Pasta | Quando criar |
|---|---|---|
| Hook reativo | `app/_hooks/use*.ts` | Lógica com estado/efeito reaproveitável entre telas |
| Função pura / máscara | `app/_utils/*.ts` | Sem side effects — export nomeado |
| Classes CSS | `app/_utils/utils.ts` (`cn()`) | Merge de classes Tailwind |
| Contexto global | `app/_context/*.tsx` | Só se o estado for compartilhado entre telas, não estado local |

**Antes de criar algo novo:** verificar se já existe função equivalente nos utils/hooks centrais do projeto (máscaras, geração de Excel/PDF, toasts, contexto de tela). Reutilizar é a regra; duplicar máscara/formatador já existente é o erro mais comum de divergência entre módulos.

## Padrão de hook

```typescript
import { useCallback } from "react";

export const useMinhaFuncao = () => {
  const minhaFuncao = useCallback((param: string) => {
    // lógica
  }, []);

  return { minhaFuncao };
};
```

## Padrão de util

```typescript
export function minhaFuncao(valor: string): string {
  // lógica pura, sem side effects
  return valor;
}
```

**Proibido:** criar `services/`, `api/` ou wrappers de axios (usar o `api` inline no componente); duplicar máscaras/formatadores já existentes; `eslint-disable` de `exhaustive-deps` em hooks com fetch.
