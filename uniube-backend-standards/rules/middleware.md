---
paths:
  - "middleware/**/*.js"
globs:
  - "middleware/**/*.js"
alwaysApply: false
---

# Middleware

Middlewares típicos: um middleware de conexão de banco padrão, e um alternativo pra uma conexão diferente (ex.: outro schema/servidor). A conexão fica em `req.db`; o header `usuario` alimenta o contexto Oracle (`DBMS_SESSION.set_identifier`).

```javascript
async function databaseMiddleware(req, res, next) {
    try {
        const conn = await openConnection(dbConfig);
        await conn.execute(`BEGIN DBMS_SESSION.set_identifier(:id); END;`, { id: req.headers['usuario'] });
        req.db = conn;
        next();
    } catch (err) {
        console.error('Erro ao conectar com o banco de dados:', err);
        res.status(500).send('Erro ao conectar com o banco de dados.');
    }
}
```

**Antes de criar um middleware novo:** verificar se um middleware existente já atende. Conexão base sempre via uma função central tipo `openConnection` — nunca duplicar usuário/senha em arquivo novo.

## Onde encaixar

| Caso | Onde |
|---|---|
| Todas as rotas de um módulo | Já aplicado pelo loader em `routes/index.js` |
| Conexão alternativa específica | Middleware alternativo já existente, ou um novo exportado do mesmo arquivo central |
| Uma rota só | Segundo argumento do handler: `router.get('/', middlewareAlternativo, async ...)` |

**Evitar:** duplicar usuários/senhas de banco em arquivos novos · criar pipeline de middleware paralelo sem necessidade · hardcodar connection strings em regras, skills ou código novo.
