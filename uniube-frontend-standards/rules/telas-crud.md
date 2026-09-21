---
paths:
  - "app/(routes)/**/cadastros/**/page.tsx"
  - "app/(routes)/**/consulta/**/page.tsx"
globs:
  - "app/(routes)/**/cadastros/**/page.tsx"
  - "app/(routes)/**/consulta/**/page.tsx"
alwaysApply: false
---

# Telas de listagem/cadastro

Template canônico de referência: copie a tela de cadastro simples mais parecida já existente no projeto — não recomece do zero.

## Estrutura obrigatória

```tsx
'use client'
// imports: api, GlobalLoader, useMensagens, ProtectedRoute, componentes-uniube, lucide-react, dynamic, useAuth, gerarExcel

export default function MinhaTela() {
  // state: currentPage, itemsPerPage=10, dados, filtros, dadosModal
  // permissoesTela de useAuth()

  return (
    <ProtectedRoute tela="Nome - CODIGO" codigo="CODIGO">
      {dadosModal.show && <Modal ... />}
      <Card>
        <CardHeader>{/* botões Novo + Excel, disabled por permissoesTela */}</CardHeader>
        <CardContent>{/* filtros + Table + Pagination */}</CardContent>
        <SobreATela texto={...} />
      </Card>
    </ProtectedRoute>
  )
}
```

## Checklist

1. Copiar a estrutura da tela mais parecida — **não** usar uma tela de processo como base para um cadastro simples.
2. `page.tsx`: `'use client'`, state (`dados`, `filtros`, `dadosModal`, `currentPage`, `itemsPerPage = 10`).
3. Funções: `fetchDados`, `filterData`, `paginate`, `openModal`, `confirmExcluir`, `onCloseModal`.
4. Modal carregado via `dynamic(() => import('./_modal/Modal...'), { ssr: false, loading: () => <GlobalLoader/> })`.
5. UI: `Card` > `CardHeader` (botões Novo + Excel, `disabled` por `permissoesTela`) > `CardContent` (filtro + Table + Pagination) > rodapé de ajuda.
6. `ProtectedRoute` com `tela` (nome legível) e `codigo` (código do programa) — deve bater com a entrada correspondente no menu.
7. Filtro client-side com `normalize('NFD')` para acentos.
8. Exclusão com confirmação (toast + ação "Confirmar", nunca `window.confirm`).
9. Excel via utilitário central (`gerarExcel(filterData(dados), 'nome-arquivo')`), nunca lib nova.
10. Lint limpo — zerar `set-state-in-effect` e `exhaustive-deps` **sem** disable.

## Errado

```tsx
// ❌ Sem ProtectedRoute
export default function Page() { return <Card>...</Card> }

// ❌ Modal import estático pesado (sempre usar dynamic + ssr:false)
import Modal from './_modal/Modal'

// ❌ try/await + setState no fetch do useEffect
const fetchDados = useCallback(async () => {
  const { data } = await api.get(url);
  setDados(data);
}, []);
```
