---
paths:
  - "auth/**/*.js"
  - "app.js"
globs:
  - "auth/**/*.js"
  - "app.js"
alwaysApply: false
---

# Autenticação e `app.js`

## Fluxo

1. `POST /login` — body `{ token }` (token de origem/SSO); retorna `access` e `refresh` (JWT).
2. Demais rotas: header `x-access-token`, validado por um middleware de verificação.
3. Rotas em `/api/*` passam pelo loader depois do middleware de auth.

**Rotas livres (sem token):** manter uma lista explícita no controller de auth. Ao criar uma rota pública, adicionar o path completo nessa lista — nunca desabilitar auth globalmente.

## Headers esperados

| Header | Uso |
|---|---|
| `x-access-token` | JWT de acesso |
| `usuario` | Identificador do usuário — alimenta o contexto de sessão do banco |

## `app.js`

- CORS já configurado — não duplicar headers em rotas individuais.
- Secrets JWT vêm de `.env` — nunca commitar `.env`.
- Desabilitar o header `X-Powered-By` do Express (`app.disable('x-powered-by')`) — evita disclosure de tecnologia em scans de segurança.

```javascript
// ordem em app.js
const app = express();
app.disable('x-powered-by');
app.post('/login', authController.login);
app.use(authController.verifyToken);
app.use('/api', routes);
```
