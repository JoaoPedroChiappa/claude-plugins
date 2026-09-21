---
name: testar-rota
description: Roda um smoke test HTTP contra uma rota da API no ar (o projeto não tem suíte automatizada). Use quando pedir pra testar uma rota, endpoint ou API do backend.
---

# Testar rota (smoke test)

O projeto **não tem** suíte automatizada — teste aqui é um smoke test HTTP real contra o servidor rodando. Nunca proponha Jest/supertest sem pedido explícito.

## Passos

1. Derivar a URL: `http://localhost:{PORT}/api` + caminho da pasta em `routes/` até a rota.
2. Confirmar que o servidor está no ar — **não suba sozinho sem avisar o usuário**.
3. Autenticar: `POST /login` (com o token de origem/SSO) e usar `access` como `x-access-token` nas chamadas seguintes, mais o header `usuario` — exceto se a rota estiver na lista de rotas livres.
4. Usar o método certo: GET/DELETE via query string; POST/PUT via corpo JSON.
5. Cobrir pelo menos: sucesso (200), parâmetro ausente (400/404 texto), erro de banco (500 `text/plain`).
6. Preferir testar contra o ambiente de homologação/desenvolvimento, nunca produção, a menos que o usuário peça e confirme.
7. Reportar status HTTP, corpo resumido e falhas.

## O que verificar na resposta

| Resultado | Esperado |
|---|---|
| Sucesso | `200` + JSON ou texto conforme a rota vizinha |
| Erro de validação | `400`/`404` + mensagem em texto |
| Erro de servidor | `500` + `text/plain` + mensagem Oracle tratada, nunca stack em JSON |

## Exemplos curl (substituir placeholders)

```bash
# login
curl -s -X POST "http://localhost:{PORT}/login" \
  -H "Content-Type: application/json" \
  -d '{"token":"SEU_TOKEN_DE_ORIGEM"}'

# GET autenticado
curl -s "http://localhost:{PORT}/api/<modulo>/<rota>" \
  -H "x-access-token: SEU_ACCESS_TOKEN" \
  -H "usuario: MATRICULA"

# POST
curl -s -X POST "http://localhost:{PORT}/api/<modulo>/<rota>" \
  -H "Content-Type: application/json" \
  -H "x-access-token: SEU_ACCESS_TOKEN" \
  -H "usuario: MATRICULA" \
  -d '{"campo":"valor"}'
```

**Nunca** imprima o `access`/`refresh` token completo no chat além do necessário pra montar o próximo comando — e nunca deixe esses valores em arquivos commitados.

## Problemas de loader (rota devolve 404 inesperado)

- O módulo está registrado em `routes/index.js` (se for módulo de primeiro nível)?
- O arquivo termina em `Funcoes.js`? Esse sufixo é ignorado pelo loader de propósito.
- O arquivo `.js` realmente exporta `router` (`module.exports = router`)?
