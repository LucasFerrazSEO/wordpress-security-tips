[English](README.md) · **Português (Brasil)**

# wordpress-security-tips

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Conjunto de trechos de código para reforçar a segurança de um site
WordPress: regras de `.htaccess`, funções para `functions.php` e regras
de WAF para quem usa Cloudflare. Tudo é copiar e colar, sem plugin.

## Sumário

- [Arquivos](#arquivos)
- [Uso](#uso)
- [Perguntas frequentes](#perguntas-frequentes)
- [Limitações](#limitações)
- [Como contribuir](#como-contribuir)
- [Autor](#autor)
- [Licença](#licença)

## Arquivos

- **`htaccess.txt`**: bloqueia `xmlrpc.php`, força HTTPS e barra
  hotlinking de imagens (uso das suas imagens em outro domínio sem
  autorização), entre outras regras.
- **`functions.php`**: remove informações de versão do WordPress no
  `<head>`, desativa o XML-RPC, remove o header `X-Pingback` e faz
  outras reduções de superfície de ataque.
- **`cloudflare rules.txt`**: duas regras de WAF (Web Application
  Firewall) prontas para colar no Cloudflare. Uma bloqueia user-agents
  e assinaturas de scanner conhecidas (incluindo as assinaturas do
  scanner de RDP BlueKeep e do masscan, e o user-agent do AhrefsBot
  7.0). A outra bloqueia tentativas comuns de SQL injection e XSS na
  query string.

## Uso

### htaccess.txt

1. Abra o `.htaccess` do seu site (raiz do WordPress).
2. Cole o conteúdo de `htaccess.txt`, de preferência logo após as
   regras padrão do WordPress (`# BEGIN WordPress` / `# END WordPress`).
   Deixe de fora a primeira linha do arquivo (`htaccess`), que é só um
   rótulo e não é uma diretiva válida.
3. Na seção de proteção contra hotlink, troque `YOUR-WEBSITE.com` pelo
   seu domínio real.
4. Teste o site depois de salvar. Uma regra mal colada pode gerar
   erro 500.

### functions.php

Copie os blocos de função para o `functions.php` do seu tema (ou tema
filho). Cada bloco é independente; não precisa usar todos.

### cloudflare rules.txt

No painel da Cloudflare, vá em **Security → WAF → Create Rule**. Cole a
expressão de cada regra (o arquivo já vem no formato de expressão do
Cloudflare) e defina a ação como **Block**.

## Perguntas frequentes

**Isso substitui um plugin de segurança (Wordfence, Sucuri etc.)?**
Não. É uma camada adicional, de baixo custo e sem plugin. É útil em
conjunto com um plugin de segurança, não no lugar dele.

**Preciso saber programar para usar?**
Para o `.htaccess` e o Cloudflare, não: é copiar e colar. Para o
`functions.php`, ajuda entender minimamente PHP para adaptar ao seu
tema.

**Essas regras de Cloudflare bloqueiam gente de verdade por engano?**
Podem gerar falso positivo em casos raros (por exemplo, uma query
string legítima que contenha `%40`, um "@" codificado). Monitore o log
de eventos do WAF depois de ativar.

## Limitações

Trechos de código para adaptar ao seu ambiente. Não é um plugin com
atualização automática nem funciona em todo tipo de hospedagem
(algumas diretivas do `.htaccess` dependem de o servidor rodar Apache
com `mod_rewrite` e `mod_headers` habilitados).

## Como contribuir

Relatos de erro e sugestões são bem-vindos pelas [Issues do GitHub](https://github.com/LucasFerrazSEO/wordpress-security-tips/issues).

## Autor

[Lucas Ferraz](https://lucasferraz.com) é especialista em SEO, criação de sites e Generative Engine Optimization e fundador da [Lucas Ferraz SEO](https://lucasferrazseo.com).

## Licença

MIT. Veja o arquivo [LICENSE](LICENSE).
