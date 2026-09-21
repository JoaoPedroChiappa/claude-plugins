---
paths:
  - "menu/**/*.json"
globs:
  - "menu/**/*.json"
alwaysApply: false
---

# Menu / navegação lateral

Cada módulo tem um arquivo `menu/<modulo>.json` com suas entradas:

```json
{
  "programa": "LOG041",
  "descricao": "Grupos de Despesas",
  "endereco": "/logistica/cadastros/operacionais/grupos-despesas",
  "icone": "Package"
}
```

- `programa`: código do programa — **deve bater exatamente** com o `codigo` passado ao `ProtectedRoute` da tela.
- `descricao`: nome exibido no menu, em português.
- `endereco`: rota exata correspondente à pasta em `app/(routes)/`.
- `icone`: nome do ícone `lucide-react`, em PascalCase.
- Itens com `children`: o item pai leva um `programa` descritivo e um array `children`.

Checklist ao criar uma tela nova: criar a pasta/rota → adicionar o item no JSON do módulo → garantir que `programa` bate com o `codigo` do `ProtectedRoute`.
