---
name: nova-funcao-helper
description: Cria uma função helper nova (indexFuncoes.js de uma rota, ou uma função transversal em gerais.js). Use quando pedir helper, função utilitária, ou lógica extraída de uma rota no backend.
---

# Nova função/helper (backend)

## Onde colocar

| Escopo | Arquivo |
|---|---|
| Lógica de uma tela/processo específico | `indexFuncoes.js` na pasta da rota |
| Transversal ao projeto inteiro (CPF, e-mail, log, erro Oracle) | `routes/utils/gerais.js` |

Verifique primeiro se já existe uma função equivalente em `gerais.js` antes de criar uma nova.

## Passos

1. Se a função usa banco: primeiro argumento é `db` (a conexão) — **não abra nem feche conexão dentro da helper**, quem chama é quem controla o ciclo de vida da conexão.
2. Retorno típico pra helper de tela: `{ success: true|false, mensagem: '...', dados: [] }`.
3. Função pura (sem banco): sem efeitos colaterais, export nomeado junto das demais.
4. `module.exports` incluindo a nova função ao lado das existentes (não substituir o objeto de exports).

## Padrão

```javascript
const minhaFuncao = async (db, parametro) => {
    try {
        if (!parametro) return { success: false, mensagem: 'Parâmetro obrigatório.', dados: [] };
        const result = await db.execute(query, { PARAMETRO: parametro }, { outFormat: oracledb.OUT_FORMAT_OBJECT });
        return { success: true, mensagem: '', dados: result.rows };
    } catch (error) {
        return { success: false, mensagem: error.message, dados: [] };
    }
};
module.exports = { minhaFuncao, /* ...outras já existentes */ };
```
