# wordpress-security-tips

Conjunto de trechos de código para reforçar a segurança de um site
WordPress: `.htaccess`, funções para `functions.php` e regras de WAF
para quem usa Cloudflare.

## Arquivos

- **`htaccess.txt`** — bloqueia `xmlrpc.php`, força HTTPS e barra
  hotlinking de imagens (uso do seu site em outro domínio sem
  autorização), entre outras regras.
- **`functions.php`** — remove informações de versão do WordPress no
  `<head>`, desativa o XML-RPC, remove o header `X-Pingback` e outras
  reduções de superfície de ataque.
- **`cloudflare rules.txt`** — duas regras de WAF (Web Application
  Firewall) prontas para colar no Cloudflare: uma bloqueia
  user-agents e assinaturas de scanner conhecidas (incluindo a
  assinatura do scanner de RDP Bluekeep e do masscan), a outra
  bloqueia tentativas comuns de SQL injection e XSS na query string.

## Como usar

### htaccess.txt

1. Abra o `.htaccess` do seu site (raiz do WordPress).
2. Cole o conteúdo de `htaccess.txt` — de preferência logo após as
   regras padrão do WordPress (`# BEGIN WordPress` / `# END WordPress`).
3. Na seção de proteção contra hotlink, troque `YOUR-WEBSITE.com` pelo
   seu domínio real.
4. Teste o site depois de salvar — uma regra mal colada pode gerar
   erro 500.

### functions.php

Copie os blocos de função para o `functions.php` do seu tema (ou tema
filho). Cada bloco é independente; não precisa usar todos.

### cloudflare rules.txt

No painel da Cloudflare: **Security → WAF → Create Rule**. Cole a
expressão de cada regra (o arquivo já vem no formato de expressão do
Cloudflare) e defina a ação como **Block**.

## Perguntas frequentes

**Isso substitui um plugin de segurança (Wordfence, Sucuri etc.)?**
Não. É uma camada adicional, de baixo custo e sem plugin — útil mesmo
em conjunto com um plugin de segurança, não no lugar dele.

**Preciso saber programar para usar?**
Para o `.htaccess` e o Cloudflare, não — é copiar e colar. Para o
`functions.php`, ajuda entender minimamente PHP para adaptar ao seu
tema.

**Essas regras de Cloudflare bloqueiam gente de verdade por engano?**
Podem gerar falso positivo em casos raros (uma query string legítima
que contenha, por exemplo, a palavra "select"). Monitore o log de
eventos do WAF depois de ativar.

## Limitações

Trechos de código para adaptar ao seu ambiente — não é um plugin
com atualização automática nem suporte a todo hosting (algumas
diretivas do `.htaccess` dependem de o servidor rodar Apache com
`mod_rewrite` e `mod_headers` habilitados).

## Autor

[Lucas Ferraz](https://lucasferraz.com) — especialista em SEO, criação de
sites e SEO para IA, fundador da [Lucas Ferraz SEO](https://lucasferrazseo.com).

## Licença

MIT — ver [LICENSE](LICENSE).
