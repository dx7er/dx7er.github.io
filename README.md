# dx73r.me

Personal cybersecurity portfolio hacker-themed, minimal, fast.  
Built with vanilla HTML, CSS, and JavaScript. No frameworks. No bloat.

**Live:** [dx73r.me](https://dx73r.me)

---

## About

**Saqlain Naqvi (@dx7er)** — Cybersecurity professional based in London, UK.  
Specialising in Penetration Testing, Digital Forensics, OSINT, and Cloud Security (Azure).  
Currently completing an MSc in Cybersecurity & Forensics at the University of Westminster.

---

## Pages

| Page | Description |
|---|---|
| **Home** | Landing page with navigation |
| **About** | Bio, services, certifications, experience, skills, education |
| **Projects** | Live GitHub API — auto-updates when new repos are published |
| **Blog** | Markdown-powered writeups fetched from the `blogs/` folder |
| **Contact** | EmailJS contact form + social links |

---

## Tech Stack

- HTML5, CSS3, Vanilla JavaScript
- GitHub API — live project and blog fetching
- Markdown — blog content with frontmatter support
- EmailJS — contact form
- Highlight.js + Marked.js — blog post rendering
- GitHub Pages — hosting

---

## Structure

```
dx7er.github.io/
├── index.html
├── style.css
├── CNAME
├── assets/
│   └── images...
└── blogs/
    └── *.md
```

---

## Writing a Blog Post

Create a `.md` file in `blogs/` with frontmatter:

```markdown
---
title: "Your Post Title"
date: 2026-01-24
tags: [pentesting, active-directory]
---

Content here...
```

Push to `main` — it appears on the blog automatically.

---

## Deployment

Hosted on GitHub Pages. Any push to `main` deploys instantly.

```bash
git clone https://github.com/dx7er/dx7er.github.io
cd dx7er.github.io
# make changes
git add . && git commit -m "update" && git push origin main
```

---

## Connect

[dx73r.me](https://dx73r.me) · [GitHub](https://github.com/dx7er) · [LinkedIn](https://www.linkedin.com/in/dx73r/) · [dx73r@protonmail.com](mailto:dx73r@protonmail.com)
