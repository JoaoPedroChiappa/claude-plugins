---
paths:
  - "**/_modal/**/*.tsx"
globs:
  - "**/_modal/**/*.tsx"
alwaysApply: false
---

# Formulários e modais

## Estrutura

```tsx
'use client'
interface ModalProps {
  isOpen: boolean;
  entidadeId?: string;
  onCloseModal: (refresh: boolean) => void;
}

// formData com campos UPPER_SNAKE
const [formData, setFormData] = useState({ DESCRICAO: '' });

const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setFormData(prev => ({ ...prev, [name]: value.toUpperCase() }));
};
```

## Checklist

1. Base: modal simples (poucos campos) ou modal com etapas/abas para casos complexos — copiar o exemplo mais próximo do que você precisa.
2. Props: `{ isOpen, entidadeId?, onCloseModal: (refresh: boolean) => void }`.
3. `formData` com chaves **UPPER_SNAKE**, sem lib de formulário.
4. `handleInputChange` com `value.toUpperCase()` para campos de cadastro textual.
5. Se for edição: GET no `useEffect` (`.then()`/`useEffectEvent`), nunca `try/await` + `setFormData`.
6. Validação de obrigatórios com `toast.warning('Campo X é obrigatório.')` antes do submit.
7. Salvar: `api.post` (novo) ou `api.put` (edição) + `tratamentoMensagens`.
8. Layout: `Dialog` > `DialogHeader` > `DialogTitle` > grid de campos (`grid grid-cols-12 gap-4`) > botão Salvar.
9. Campo obrigatório: `<span className="text-red-500">*</span>` no `Label`.
10. Combo com busca: componente de busca da lib compartilhada (`componentes-uniube`) — nunca `react-select` nem combo próprio.
11. Máscaras (CPF, CNPJ, CEP, telefone) via utilitário central, nunca reimplementadas.
12. `if (!isOpen) return null;` no topo do componente.

Variações típicas (adaptar caso a caso): modal simples de 1–5 campos · modal em etapas (stepper) · abas dentro do modal · máscaras de documento · rich text · combo com busca assíncrona.

## Errado

```tsx
// ❌ react-hook-form
const { register, handleSubmit } = useForm();

// ❌ camelCase nos campos de API
setFormData({ descricao: value });

// ❌ sincronizar valor calculado com setState no effect/render
useEffect(() => { setFormData(prev => ({ ...prev, VALOR_CALCULADO: calculado })); }, [deps]);

// ❌ eslint-disable exhaustive-deps — usar useEffectEvent para toast/onCloseModal
```
