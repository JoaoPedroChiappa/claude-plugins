---
name: novo-dashboard
description: Cria dashboards de módulo com cards de indicadores. Use quando pedir dashboard, cards de contagem, home do módulo ou indicadores clicáveis.
---

# Novo dashboard de módulo

## Antes de escrever qualquer código

1. Encontre o dashboard de outro módulo já existente e use como template visual e estrutural.
2. Confirme com o usuário: quais indicadores/cards, o endpoint de dashboard da API (ou peça pra criar no backend), e pra onde cada card clicável navega.

## Passos

1. `page.tsx` do módulo com `'use client'`.
2. Busca inicial via `useEffectEvent` chamando a API de dashboard (ou `useCallback` + `useEffect`, sempre `.then()/.catch()`).
3. State com um contador nomeado por indicador.
4. Cards com `Card`/`CardHeader`/`CardTitle`/`CardContent`/`Badge`, ícone `lucide-react` por card, borda lateral colorida coerente com o status que representa.
5. Navegação de cards clicáveis via `window.location.href` — nunca `useRouter`/`router.push`.
6. Tratamento de erro via `tratamentoMensagens` + `window.scrollTo({ top: 0 })`.

## Checklist de conformidade (ver rule `dashboards` para o detalhe de cada item)

- [ ] Fetch inicial com `useEffectEvent` ou `.then()/.catch()`, nunca `try/await` + `setState`
- [ ] Nenhum `eslint-disable-next-line`
- [ ] Navegação entre módulos via `window.location.href`
- [ ] Cores de status em classes utilitárias (`bg-blue-50`, `bg-yellow-50`, ...)
- [ ] Nenhum Server Component com fetch — o padrão é client-side
