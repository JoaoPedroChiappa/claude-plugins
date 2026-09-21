---
paths:
  - "routes/**/*.js"
globs:
  - "routes/**/*.js"
alwaysApply: false
---

# SQL Oracle

- Sempre binds nomeados: `:PARAMETRO` no SQL + a mesma chave no objeto `params` — nunca concatenar valor do usuário direto na string.
- SELECT: `{ outFormat: oracledb.OUT_FORMAT_OBJECT }`.
- Escrita: `req.db.commit()` no sucesso; `req.db.rollback()` no `catch`.
- Sempre `req.db.close()` no `finally` (ou `if (req.db) req.db.close()`).
- Auditoria em INSERT/UPDATE/DELETE: usar a função central de contexto de usuário Oracle quando a rota vizinha já usa.

## SQL condicional — interpolar só com bind correspondente

```javascript
// BOM
const query = `SELECT ... FROM mtd.tabela t
    ${tipoDespesa ? 'WHERE t.tipo_despesa = :tipoDespesa' : ''}`;
const params = {};
if (tipoDespesa) params.tipoDespesa = tipoDespesa;
```

```javascript
// EVITAR — valor do usuário direto na string (SQL injection)
const query = `SELECT * FROM t WHERE id = ${req.query.id}`;
```

## Erros Oracle

```javascript
// BOM
.send('Erro ao buscar: ' + gerais.extrairErroOracle(err.message));

// FK ao excluir — padrão do projeto
if (err.message.includes('ORA-02292')) return res.status(400).send('Registro possui vínculos.');
```
