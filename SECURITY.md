# Security

This is my personal portfolio site: a static page hosted on GitHub Pages, no backend, no user accounts, no database. The attack surface is small, but if you find something worth flagging, I'd rather hear about it privately than see it in a public issue.

## What counts

- XSS or content injection in how the blog renderer handles markdown
- Anything that leaks the EmailJS keys or lets a stranger send from my form
- Broken auth or exposed secrets in the repo history
- Supply-chain issues with the CDN scripts I pull (marked, highlight.js, EmailJS, Font Awesome)
- Anything that would let someone deface the site or hijack the domain

## What doesn't

- "You use CDN scripts". Yes, it's a static portfolio, not a bank
- Missing security headers on GitHub Pages I can't configure
- Automated scanner output with no working proof-of-concept
- Rate limiting the contact form (EmailJS handles that on their end)

## How to reach me

Email: **dx73r@protonmail.com**

Please don't file a public GitHub issue for a real vulnerability. Send it to the address above with enough detail to reproduce. I'll reply within a few days.

If it's serious and you'd like credit for the find, I'm happy to acknowledge you in the fix commit or add you here. Just say so in your report.
