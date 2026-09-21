---
paths:
  - "routes/**/*.js"
---

# Rotas Express

## Estrutura de arquivo

```javascript
const express = require('express');
const router = express.Router();
const { databaseMiddleware } = require('.../middleware/middleware');
const oracledb = require('oracledb');
const gerais = require('.../utils/gerais');

//#region Rotas de Get
router.get('/', databaseMiddleware, async (req, res) => { ... });
//#endregion

module.exports = router;
```

- Preferir `index.js` na pasta da tela; `*Funcoes.js` ao lado quando houver lógica extraída — não exporta router.
- Arquivo novo: incluir o middleware de banco padrão no handler (padrão dominante do projeto).

## Loader

- Só alterar `routes/index.js` se criar módulo de primeiro nível.
- Subpastas novas dentro de módulo existente são carregadas automaticamente.

## HTTP — entrada e validação por método

| Método | Entrada | Sem validação |
|---|---|---|
| GET | `req.query` | 400 ou 404 + texto |
| POST/PUT | `req.body` | 400 ou 404 + texto |
| DELETE | `req.query` | 400 ou 404 + texto |

## Bom vs evitar

```javascript
// BOM — padrão do projeto
router.get('/', databaseMiddleware, async (req, res) => {
    try {
        const result = await req.db.execute(query, {}, { outFormat: oracledb.OUT_FORMAT_OBJECT });
        return res.status(200).json(result.rows);
    } catch (err) {
        return res.status(500).set('Content-Type', 'text/plain')
            .send('Erro ao obter dados.\nMotivo: ' + gerais.extrairErroOracle(err.message));
    } finally {
        req.db.close();
    }
});

// EVITAR — JSON de erro genérico, sem close, SQL concatenado
catch (err) { res.status(500).json({ error: err.stack }); }
```
