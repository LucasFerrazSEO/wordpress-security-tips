**English** · [Português (Brasil)](README.pt-BR.md)

# wordpress-security-tips

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A set of code snippets to harden a WordPress site: `.htaccess` rules,
functions for `functions.php` and WAF rules for sites behind
Cloudflare. Everything is copy and paste, with no plugin.

## Contents

- [Files](#files)
- [Usage](#usage)
- [FAQ](#faq)
- [Limitations](#limitations)
- [Contributing](#contributing)
- [Author](#author)
- [License](#license)

## Files

- **`htaccess.txt`**: blocks `xmlrpc.php`, forces HTTPS and blocks image
  hotlinking (other domains using your images without permission),
  among other rules.
- **`functions.php`**: removes WordPress version information from
  `<head>`, disables XML-RPC, removes the `X-Pingback` header and makes
  other attack surface reductions.
- **`cloudflare rules.txt`**: two WAF (Web Application Firewall) rules
  ready to paste into Cloudflare. One blocks known scanner user agents
  and signatures (including the BlueKeep RDP scanner and masscan
  signatures, and the AhrefsBot 7.0 user agent). The other blocks
  common SQL injection and XSS attempts in the query string.

## Usage

### htaccess.txt

1. Open your site's `.htaccess` (WordPress root).
2. Paste the contents of `htaccess.txt`, ideally right after the
   default WordPress rules (`# BEGIN WordPress` / `# END WordPress`).
   Leave out the first line of the file (`htaccess`), which is only a
   label and is not a valid directive.
3. In the hotlink protection section, replace `YOUR-WEBSITE.com` with
   your real domain.
4. Test the site after saving. A badly pasted rule can cause a 500
   error.

### functions.php

Copy the function blocks into your theme's (or child theme's)
`functions.php`. Each block is independent; you do not have to use all
of them.

### cloudflare rules.txt

In the Cloudflare dashboard, go to **Security → WAF → Create Rule**.
Paste the expression of each rule (the file already uses the
Cloudflare expression format) and set the action to **Block**.

## FAQ

**Does this replace a security plugin (Wordfence, Sucuri, etc.)?**
No. It is an extra layer, cheap and plugin-free. It is useful alongside
a security plugin, not instead of one.

**Do I need to know how to code to use it?**
For `.htaccess` and Cloudflare, no: it is copy and paste. For
`functions.php`, a basic understanding of PHP helps you adapt it to
your theme.

**Can these Cloudflare rules block real people by mistake?**
They can cause false positives in rare cases (for example, a
legitimate query string containing `%40`, an encoded "@"). Watch the
WAF event log after turning them on.

## Limitations

Code snippets to adapt to your environment. This is not a plugin with
automatic updates and it does not support every host (some `.htaccess`
directives require Apache with `mod_rewrite` and `mod_headers`
enabled).

## Contributing

Bug reports and suggestions are welcome through [GitHub Issues](https://github.com/LucasFerrazSEO/wordpress-security-tips/issues).

## Author

[Lucas Ferraz](https://lucasferraz.com) is an SEO, website development and Generative Engine Optimization specialist and the founder of [Lucas Ferraz SEO](https://lucasferrazseo.com).

## License

MIT. See [LICENSE](LICENSE).
