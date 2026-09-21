---
name: novo-middleware
description: Cria um middleware Express novo (tipicamente uma conexão de banco alternativa). Use quando pedir middleware novo no backend.
---

# Novo middleware

## Antes de criar

Verifique se um middleware existente já resolve (ex.: o middleware de conexão de banco padrão, ou um alternativo já existente pra outro schema/servidor). Middleware novo só se nenhum dos existentes atender.

## Passos

1. Adicionar a função nova no arquivo central de middlewares do projeto, ao lado dos existentes — não criar um arquivo/pipeline paralelo sem necessidade.
2. Reaproveitar a função central de conexão (`openConnection` ou equivalente) — nunca duplicar usuário/senha de banco no middleware novo.
3. Se for específico de uma rota (não de todo um módulo): aplicar como segundo argumento do handler, não globalmente.
4. Se for de todo um módulo: confirmar que o loader já aplica no nível certo, sem precisar duplicar em cada rota.

## Errado

```javascript
// ❌ Credencial de banco duplicada no middleware novo
const dbConfig = { user: 'outro_usuario', password: 'outra_senha', connectString: '...' };

// ❌ Middleware global novo pra resolver um caso de uma rota só
app.use(meuMiddlewareEspecifico);
```
