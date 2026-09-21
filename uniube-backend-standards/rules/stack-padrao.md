# Padrão geral de backend (Uniube)

Baseado no Guia de Padronização — Back-end da empresa. Vale para APIs Node.js + Express + Oracle internas.

## Stack de referência

| Camada | Escolha padrão |
|---|---|
| Runtime | Node.js + Express, CommonJS (`require`/`module.exports`) — não migrar para ES modules em arquivo existente |
| Banco | Oracle via `oracledb` — binds nomeados sempre, `OUT_FORMAT_OBJECT` em SELECT |
| Autenticação | JWT (`x-access-token`), emitido por `POST /login` a partir de um token de origem (SSO) |
| Roteamento | Loader recursivo em `routes/index.js`, montando `/api/<modulo>/...` a partir da árvore de pastas |
| Erros | Texto plano com mensagem Oracle tratada — nunca JSON de erro genérico com stack |

## Proibido por padrão

- TypeScript, ORM, ou camadas `controllers/`/`services/` que não existem no projeto
- Reformatar um arquivo inteiro ao tocar nele, trocar `module.exports` por ES modules, ou unificar variantes legadas em massa
- Duplicar credenciais/connection strings de banco em arquivos novos
- Instalar dependência nova (Jest, supertest, etc.) sem o usuário pedir
- SQL com valor do usuário concatenado diretamente na string

## Estrutura de pastas

```
app.js                          # bootstrap: CORS, /login, verifyToken, /api -> routes
auth/
  authController.js             # login, verifyToken, rotasLivres (paths sem auth)
config/
  database.js                   # openConnection(dbConfig)
  uploadConfig.js                # multer, quando a rota tem upload
middleware/
  middleware.js                 # databaseMiddleware, openMvConnection
routes/
  index.js                      # loader recursivo, diretoriosPrincipais
  <modulo>/                     # um por módulo de negócio
    cadastro/<entidade>/
      index.js                  # rota (router) — exporta module.exports = router
      indexFuncoes.js           # helpers da tela, NÃO exporta router (loader ignora)
    consulta/<entidade>/
      index.js
    processo/<fluxo>/
      index.js
      indexFuncoes.js
    relatorios/<tipo>/<nome>/
      index.js
  utils/
    gerais.js                   # helpers transversais: extrairErroOracle, validaCpf, setOracleUserContext...
```

Convenções:

- `index.js` é a rota (`module.exports = router`); `*Funcoes.js` nunca é rota — o loader ignora esse sufixo.
- URL pública = `/api` + caminho da pasta a partir de `routes/`.
- Só editar `routes/index.js` se o módulo for **novo em primeiro nível**; subpastas dentro de um módulo existente carregam sozinhas.
- Import de `gerais` sempre aponta para `routes/utils/gerais.js` — ajustar a profundidade de `../` conforme a pasta.
