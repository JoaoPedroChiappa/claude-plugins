---
alwaysApply: true
---

# Verificação de UI e uso do Browser pane

Mudança de UI, layout, rota, modal, formulário ou dado em tela: **pergunte** ao usuário se ele quer que a verificação seja feita no Browser pane (ou lance o agent de testes, se o projeto tiver um). Só dirija o browser se ele pedir ou confirmar. Descrever a mudança sem realmente abrir a tela não conta como verificação — mas também não force o passo se não foi pedido.

Frases que valem como "sim": `testa isso`, `teste isso`, `test-runner`, `sim` (logo após a pergunta).

## Ao dirigir o browser

1. Carregue as ferramentas de automação de browser da plataforma **antes** de tentar usá-las, caso apareçam como "ferramenta adiada"/não carregada.
2. Leia a árvore de acessibilidade da página (ou equivalente) antes de clicar em algo — não adivinhe coordenadas de tela sem antes localizar o elemento.
3. Autentique-se pelo fluxo de desenvolvimento já documentado no projeto (login local + geração de token, `?token=` na URL, etc.) — **nunca** peça ao usuário para logar manualmente, nem invente credencial diferente da documentada.
4. Se a tela pedir permissão/captcha que não é falta de sessão: **pare e reporte**. Não tente contornar.
5. Se as ferramentas de automação de browser não existirem nessa instalação: **não finja que testou**. Reporte a limitação e o que precisaria ser verificado manualmente.

## Nunca

- Instalar Playwright/Cypress/Selenium como substituto da automação nativa da plataforma.
- Expor token/JWT/credencial de sessão no chat ou em `echo`/log — grave em arquivo temporário e use apenas como parâmetro de navegação.
