---
alwaysApply: true
---

# Não refatorar (regra sempre ativa)

Baseado no Guia de Padronização — Back-end da empresa. A maior fonte de risco em uma API legada não é a falta de padrão novo — é uma refatoração não pedida que quebra um comportamento em produção.

## Regra de ouro

**Ao editar um arquivo, copie o estilo daquele arquivo**: imports, se o handler usa o middleware de banco padrão ou não, nomes de variável, formatação. Faça **apenas** o que o pedido exige.

## Sempre

- Copie o vizinho mais parecido ao criar um arquivo novo — não invente um padrão "melhor".
- Se o código legado divergir do padrão documentado, mas o pedido não envolve essa divergência, **deixe como está** — a menos que o usuário peça explicitamente pra corrigir.

## Nunca

- Renomear variáveis/funções fora do escopo do pedido.
- Extrair camadas novas (`controllers/`, `services/`) que o projeto não usa.
- Migrar para `async/await` onde o arquivo já usa outro padrão, ou vice-versa.
- "Limpar" código adjacente ao que foi pedido.
- Reformatar um arquivo inteiro ao tocar nele.
- Trocar `module.exports` por ES modules num projeto que usa CommonJS.
- Unificar variantes legadas em massa numa única mudança.

## Exceções conhecidas

Alguns arquivos legados divergem do padrão de propósito e isso já foi documentado/aceito pelo time (ex.: um handler específico que não usa o middleware de banco padrão). **Não "corrija" esse tipo de exceção sem pedido explícito** — pergunte se não tiver certeza se é exceção conhecida ou desvio real.
