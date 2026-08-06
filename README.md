# dx73r.me

Source for my personal site at [dx73r.me](https://dx73r.me).

Single-page site, written in plain HTML/CSS/JS. The blog and projects list are dynamic: blog posts get pulled from `blogs/*.md` and rendered client-side, and the projects grid queries the GitHub API so I don't have to touch the site every time I push a new repo. Everything lives in one `index.html`, styled by `style.css`, hosted on GitHub Pages, pointed at the domain via CNAME.

## Repo layout

```
index.html      the whole site
style.css
CNAME           dx73r.me
.nojekyll       skip Jekyll on Pages
assets/         images
blogs/          markdown posts, rendered on the client
```

## Adding a blog post

Drop a markdown file in `blogs/` with front matter:

```markdown
---
title: "Post title"
date: 2026-01-24
tags: [pentesting, cloud]
---

Body starts here.
```

Push to `main`. The blog index picks it up automatically.

Keep the front matter minimal: `title`, `date`, `tags`. Skip `layout:` or `categories:` unless you want to fight Jekyll (I don't; that's what `.nojekyll` is for).

## Running locally

Any static file server will do:

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000`.

## Under the hood

- [marked.js](https://marked.js.org/) for markdown parsing
- [highlight.js](https://highlightjs.org/) (github-dark theme) for code blocks
- [EmailJS](https://www.emailjs.com/) for the contact form
- [Font Awesome](https://fontawesome.com/) for icons

No build step, no bundler, no framework.

## Contact

- Site: [dx73r.me](https://dx73r.me)
- GitHub: [@dx7er](https://github.com/dx7er)
- LinkedIn: [dx73r](https://www.linkedin.com/in/dx73r/)
- Email: dx73r@protonmail.com

## License

Code is MIT (see `LICENSE`). Blog post text and images are mine. Ask before republishing.
