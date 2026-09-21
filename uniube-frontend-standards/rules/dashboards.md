---
paths:
  - "app/(routes)/*/page.tsx"
globs:
  - "app/(routes)/*/page.tsx"
alwaysApply: false
---

# Dashboards de módulo

Um dashboard de módulo é a `page.tsx` da raiz do módulo (ex.: a home de um dos módulos do sistema), com cards de indicadores.

## Padrão

- `'use client'` + busca inicial via `useEffectEvent` chamando a API de dashboard do módulo (ou `useCallback` + `useEffect`, fetch em `.then()/.catch()`, nunca `try/await` + `setState`). **Não** `eslint-disable exhaustive-deps`.
- State com contadores nomeados por indicador.
- Cards com `Card`/`CardHeader`/`CardTitle`/`CardContent`/`Badge`, ícone `lucide-react` por card, borda lateral colorida por status.
- Contadores clicáveis: `onClick={() => abreTela("/rota")}` usando `window.location.href` — **não** `router.push`/`useRouter` (o padrão do projeto não usa `useRouter` para navegação entre módulos).
- Cores por status como classes utilitárias (`bg-blue-50`, `bg-yellow-50`, `bg-green-50`, ...).
- Erros via `tratamentoMensagens` + `window.scrollTo({ top: 0 })`.
- Condicionais de perfil de usuário quando o módulo exigir (ex.: `usuarioTemPerfilX !== null`).

## Errado

```tsx
// ❌ useRouter para navegação entre módulos (padrão usa window.location.href)
router.push('/modulo/processo');

// ❌ Server Component com fetch
export default async function Dashboard() { ... }
```
