# Site spec — as built

This document describes the current Jekyll site after the rebuild and refinements. It supersedes the initial empty-state requirements.

## Identity

- `_config.yml` keeps `title: bhanuharya@sec` and `author: harya`; added `url`, `baseurl`, `lang`, and the `/blog/:title/` permalink format.
- No Gemfile required for Pages; remains GitHub Pages-safe (minima theme, plain CSS, no unsupported gems). Local build uses `github-pages` gem if available, otherwise CI builds.

## Layouts

- `default` — HTML shell with skip link, canonical URL, Open Graph / Twitter meta, JSON-LD (WebSite for pages, BlogPosting for posts), RSS link, header nav (Blog, About, Terminal, RSS, Theme toggle), footer with RSS and privacy note. Theme choice (light/dark) is stored in localStorage and can be forced with a `?theme=` query parameter.
- `home` — short intro, latest notes list (eight newest, with reading time and tags), an about-this-site block, and a link to the terminal view. The interactive terminal moved to `/terminal/` in the 19fa9ab rebuild; the homepage is plain HTML/CSS.
- `post` — title, meta (date, reading time, author), tags, progressive TOC from h2/h3 (hidden until JS populates; dedupes slugs, handles empty slugs, hidden via `<noscript>` when JS disabled), content, code copy buttons, post nav (Back to blog + Home + next/previous).
- `page` — title, content, nav (Back to home, Blog).
- `404.html` — styled not-found page with links home/blog.

## Content

- `index.md` — layout home (intro + latest notes list + about-this-site block).
- `terminal.md` — `/terminal/` standalone interactive terminal: visitor CLI (`ls`, `cat`, `posts`, `read`, `tags`, `links`, `whoami`, `neofetch`, `theme green|amber|white`, `crt on|off`, `pulsar`, `banner`, `history`, `clear`), tab completion, command history, and ASCII art. Requires JavaScript; noscript fallback links to `/blog/`.
- `blog.md` — `/blog/` with lead, interactive tag filtering, post cards (title, date, reading time, tags, excerpt, CTA). Empty state kept.
- `about.md` — `/about/` standalone about page; `#about` anchor still exists on home for deep link.
- `_posts/` — five published posts: what happens when I revoke an agent's access, 4B vs 23B on the 3060, can my local 8B do what Jev does, building a SAST setup on top of SonarQube, and Dart security rules for SonarQube Community.
- `_drafts/next-article-template.md` — working template retained.

## Design & Aesthetics

- Plain text first: light/dark themes on the page shell, phosphor-terminal styling (green/amber/white) and a CRT scanline texture (`crt on|off`) inside `/terminal/` only. The old Solaris/CDE, mono, matrix, amber, cyber, and monochrome full-page themes plus the homepage pulsar SVG, mini-games, and audio synth no longer exist after the 19fa9ab rebuild and were removed from the layouts.
- Body uses a system sans-serif stack; the terminal page is monospace.
- Decorative animations respect `prefers-reduced-motion: reduce`.
- Touch targets: nav links 44px, input 44px, visible focus outlines.

## Constraints honored

- `.github/workflows/jekyll-gh-pages.yml` untouched.
- No external fonts/CDN, zero external dependencies, no analytics.
- Sanitized: no secrets/IPs/hostnames in content.

## Verification

- `bundle exec jekyll build` where Ruby available; otherwise inspect front matter and generated HTML for one h1 per page, canonical/OG tags, valid internal links. No local Ruby/Jekyll required — GitHub Actions (`actions/jekyll-build-pages@v1`) is the authoritative build on push to `main`.
- Manual checks: terminal keyboard nav (Enter, Tab completion, history, Ctrl+L, Escape), reduced-motion, no-JS fallback, mobile at 320/375/414/768.
- Ignored: `session-*.md`, `_site/`, `.jekyll-cache/`, `.bundle/`, `vendor/` (see `.gitignore`).
