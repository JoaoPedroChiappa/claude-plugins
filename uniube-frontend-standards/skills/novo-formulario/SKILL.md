---
name: novo-formulario
description: Cria modais e formulários de cadastro/edição com useState e componentes-uniube. Use quando o usuário pedir modal, formulário, cadastro, edição, abas de modal ou stepper.
---

# Novo modal/formulário

## Antes de escrever qualquer código

1. Encontre um modal existente parecido (mesma complexidade: simples, com abas, ou stepper) e copie a estrutura dele.
2. Confirme os campos, quais são obrigatórios, e se algum campo precisa de máscara (CPF/CNPJ/CEP/telefone) ou combo com busca.

## Passos

1. Criar `_modal/Modal<Entidade>.tsx` com as props `{ isOpen, entidadeId?, onCloseModal: (refresh: boolean) => void }`.
2. `formData` com chaves UPPER_SNAKE via `useState`, sem lib de formulário.
3. Se o modal serve tanto criação quanto edição: `useEffect` que busca os dados quando `entidadeId` existe, usando `.then()/.catch()` (nunca `try/await` + `setFormData`).
4. Validação de campos obrigatórios com toast antes de chamar a API.
5. `salvar()`: `api.post` quando novo, `api.put` quando `entidadeId` existe, com `tratamentoMensagens` no sucesso/erro.
6. Layout: `Dialog > DialogHeader > DialogTitle` + grid de campos (`grid grid-cols-12 gap-4`) + botão de salvar.
7. Combo com busca: usar o componente de busca da lib de UI compartilhada — nunca criar um novo nem usar `react-select` direto sem autorização explícita.
8. `if (!isOpen) return null;` logo no início do componente.

## Checklist de conformidade (ver rule `formularios-modais` para o detalhe de cada item)

- [ ] Props com a assinatura padrão (`isOpen`, `entidadeId?`, `onCloseModal`)
- [ ] `formData` em UPPER_SNAKE
- [ ] Texto de cadastro em `toUpperCase()` no `onChange`, quando fizer sentido pro campo
- [ ] Toast e `onCloseModal` chamados via `useEffectEvent`, não direto no corpo do effect
- [ ] Nenhum `eslint-disable-next-line`
- [ ] Campo obrigatório com `<span className="text-red-500">*</span>` no `Label`
- [ ] Nenhuma lib de formulário (`react-hook-form`, `Formik`, `Zod`) introduzida
