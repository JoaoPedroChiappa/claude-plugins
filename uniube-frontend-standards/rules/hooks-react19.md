---
paths:
  - "app/**/*.tsx"
  - "app/**/*.ts"
---

# React 19 / ESLint — padrões de hooks

Estas regras existem para satisfazer o compiler/lint de React sem recorrer a `eslint-disable`, que é **proibido** neste padrão.

## Fetch em `useEffect`

A regra `react-hooks/set-state-in-effect` acusa `setState` executado depois de um `await` dentro do effect. Usar `.then()/.catch()`, nunca `try { const x = await ...; setState(x) }`:

```tsx
const fetchDados = useCallback(async () => {
  await api.get(`${API_URL}<modulo>/<endpoint>`)
    .then((response) => { setDados(response.data || []); })
    .catch((error) => { tratamentoMensagens(error, 'Erro ao buscar dados.'); });
}, [tratamentoMensagens]);

useEffect(() => { fetchDados(); }, [fetchDados]);
```

## Nunca `eslint-disable-next-line react-hooks/exhaustive-deps`

Callbacks que o componente pai recria a cada render (ex.: `onCloseModal`, funções de toast) não entram nas deps do effect — entram em `useEffectEvent`:

```tsx
const avisarErro = useEffectEvent((error: unknown, mensagem: string) => {
  tratamentoMensagens(error, mensagem);
});
const fecharSemRefresh = useEffectEvent(() => { onCloseModal(false); });

useEffect(() => {
  if (!isOpen) return;
  let cancelado = false;
  api.get(`${API_URL}...`)
    .then(({ data }) => { if (!cancelado) setFormData(data); })
    .catch((error) => { if (!cancelado) avisarErro(error, 'Erro ao buscar.'); });
  return () => { cancelado = true; };
}, [isOpen, entidadeId]);
```

As deps do effect ficam só com o que **deve** disparar o refetch (`isOpen`, `id`).

## Estado derivado, não sincronizado

Não usar `setState` síncrono dentro do effect nem no corpo do render para manter um valor calculado ou limpar uma lista filha. Derivar no render:

```tsx
// valor calculado
const valorDiaria = diariaDoDestino(...) || formData.VALOR_DIARIA;

// lista filha que depende de outra chave
const centrosAtivos = contaAtual ? centrosCusto : [];
```

Errado: `useEffect(() => { setFormData(prev => ({...prev, VALOR_DIARIA: calculado })) }, [deps])` ou `setLista([])` no effect para "limpar" — o React 19 compiler acusa isso.

## `useMemo` com dependência de objeto de `.find()`

Evitar `useMemo` cuja dependência é um objeto derivado de `.find()` (ex.: `categoria?.CONTA`) — dispara `preserve-manual-memoization`. Calcular a string/lista direto no render.
