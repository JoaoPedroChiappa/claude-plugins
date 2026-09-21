---
paths:
  - "routes/utils/**/*.js"
  - "**/*Funcoes.js"
---

# Funções helpers

| Escopo | Arquivo |
|---|---|
| Lógica de uma tela/processo | `indexFuncoes.js` na pasta da rota |
| Transversal ao projeto (CPF, e-mail, log, erro Oracle) | `routes/utils/gerais.js` |

**Assinatura:** primeiro argumento é `db` (conexão Oracle) quando a helper usa banco. **Não fechar a conexão dentro da helper** — quem abriu, fecha. Retorno típico em helpers de tela: `{ success: true|false, mensagem: '...', dados: [] }`.

```javascript
// BOM — indexFuncoes.js
const minhaFuncao = async (db, parametro) => {
    try {
        if (!parametro) return { success: false, mensagem: 'Parâmetro obrigatório.', dados: [] };
        const result = await db.execute(query, { PARAMETRO: parametro }, { outFormat: oracledb.OUT_FORMAT_OBJECT });
        return { success: true, mensagem: '', dados: result.rows };
    } catch (error) {
        return { success: false, mensagem: error.message, dados: [] };
    }
};
module.exports = { minhaFuncao };
```

```javascript
// EVITAR — helper fecha conexão que a rota ainda usa
finally { db.close(); }
```

## Uso na rota

```javascript
const { minhaFuncao } = require('./indexFuncoes');
const resultado = await minhaFuncao(req.db, valor);
if (!resultado.success) return res.status(400).send(resultado.mensagem);
```

`gerais.js`: funções puras (validação de CPF, senha) não recebem `db`; funções com Oracle recebem `connection`/`db` explicitamente e são exportadas junto às existentes.
