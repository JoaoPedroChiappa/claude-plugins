---
name: nova-rota
description: Cria uma rota Express nova (CRUD, consulta, processo ou relatório) seguindo o padrão local do projeto. Use quando pedir nova rota, endpoint, API, cadastro, consulta, processo ou relatório no backend.
---

# Nova rota

## Antes de escrever qualquer código

1. Confirme o tipo de rota (cadastro CRUD, consulta, processo/fluxo, relatório) e o módulo.
2. Encontre 1-2 exemplos reais parecidos já existentes no projeto — use o agent `buscador-rota-similar` deste plugin se disponível, ou busque você mesmo em `routes/<modulo>/`.
3. Confirme os campos de entrada, a tabela/consulta Oracle envolvida, e se a rota precisa de autenticação (a maioria precisa — só é pública se o usuário disser explicitamente).

## Passos

1. Criar `routes/<modulo>/<tipo>/<entidade>/index.js`, copiando a estrutura do exemplo encontrado (ver rule `rotas-express`).
2. Query Oracle com binds nomeados, `OUT_FORMAT_OBJECT` em SELECT (ver rule `sql-oracle`).
3. Se houver lógica que não cabe direto no handler: `indexFuncoes.js` ao lado (ver rule `funcoes-helpers`).
4. Tratamento de erro em texto plano com `gerais.extrairErroOracle` (ou equivalente do projeto).
5. `req.db.close()` no `finally`; `commit()`/`rollback()` se for escrita.
6. Se o módulo já existe: não mexer no loader, a subpasta carrega sozinha.
7. Se o módulo é novo em primeiro nível: registrar em `routes/index.js`.
8. Se a rota for pública: adicionar na lista de rotas livres do controller de auth.
9. `node --check` no arquivo antes de considerar pronto.
10. Perguntar se o usuário quer que a rota seja testada (ver skill `testar-rota`) antes de considerar a tarefa concluída.

## Errado

```javascript
// ❌ SQL com valor do usuário direto na string
const query = `SELECT * FROM t WHERE id = ${req.query.id}`;

// ❌ Erro genérico em JSON com stack
catch (err) { res.status(500).json({ error: err.stack }); }

// ❌ Desabilitar auth globalmente pra liberar uma rota
app.use((req, res, next) => next()); // pulando verifyToken
```
