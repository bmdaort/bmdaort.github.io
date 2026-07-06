# Site guide — non-developer edition

This is your cheat sheet. The site runs on **Jekyll**, a static site
generator: you write `.md` (Markdown) files, Jekyll compiles them into HTML,
and GitHub Pages serves them for free at `https://bmdaort.github.io`.

---

## 1. Mental model

```
Edit .md files   →   git push to GitHub   →   GitHub builds   →   Site updated
```

No database, no backend. Just a folder of text files.

---

## 2. What each folder is for

| Path | What it is | Do you touch it often? |
|------|------------|------------------------|
| `_config.yml` | Global settings: site title, sections, plugins. | Rarely. |
| `Gemfile` | Ruby dependency list (Jekyll + plugins). | Almost never. |
| `_layouts/` | HTML templates that wrap your content. | Only when redesigning. |
| `_includes/` | Reusable HTML snippets (`head.html`, `sidebar.html`). | Only when redesigning. |
| `assets/css/main.scss` | **All** styles for the site. | When changing colors, fonts, spacing. |
| `assets/<section>/<slug>/` | Images for a specific post. | Every time you publish a post with figures. |
| `_ciencia/` | Posts in the **Ciencia** section. One `.md` = one post. | Yes — this is where you write. |
| `_libros/` | Posts in **Libros**. | Yes. |
| `_notas/` | Posts in **Notas personales**. | Yes. |
| `ciencia/index.md`, `libros/index.md`, `notas/index.md` | Section index pages (list of posts). | Almost never. |
| `index.md` | Home page. | Rarely. |
| `about.md` | "About" page. | Whenever. |
| `AGENTS.md` | Notes for AI coding agents. **Not pushed to GitHub** (it's in `.gitignore`). | When the repo layout changes. |
| `DOCUMENTATION.md` | This file. | When you change the structure. |

---

## 3. How to write a new post

**Example: a post in "Libros"**

1. Create a file inside `_libros/` named `YYYY-MM-DD-slug.md`, e.g.
   `_libros/2026-08-01-ficciones.md`.

2. Start with this block (called *front matter*):

   ```markdown
   ---
   title: Ficciones — Borges
   date: 2026-08-01
   tags: [ficción, cuentos]
   ---
   ```

3. Below that, write in plain Markdown.

4. If you want to add images:
   - Create the folder `assets/libros/ficciones/`.
   - Drop `portada.jpg` inside.
   - Link to it with `![Portada](/assets/libros/ficciones/portada.jpg)`.

5. Publish:

   ```bash
   git add .
   git commit -m "Add post: Ficciones"
   git push
   ```

6. Wait ~30 seconds and refresh `https://bmdaort.github.io`.

---

## 4. Preview locally before pushing (optional)

First time only:

```bash
bundle install
```

Then every time you want to preview:

```bash
bundle exec jekyll serve --livereload
```

Open `http://localhost:4000`. Save a file and it reloads automatically.

> **Note:** the site is pinned to the `github-pages` gem, which currently
> requires Ruby ≥ 3.0. If `bundle install` complains about your Ruby
> version, install a newer one with `brew install ruby` and follow the
> instructions Homebrew prints. This only affects local preview — GitHub
> Pages builds the site remotely with its own Ruby.

---

## 5. Adding a new section

Say you want to add "Recetas" (recipes). Steps:

1. **`_config.yml`** — under `collections:` add:

   ```yaml
   recetas:
     output: true
     permalink: /recetas/:path/
     label: Recetas
     icon: "🍳"
   ```

   Append `- recetas` to the `sections:` list (this controls sidebar order).

   Add a matching `defaults:` block, mirroring the existing sections.

2. Create the folder `_recetas/` and add one sample post.

3. Create `recetas/index.md`:

   ```markdown
   ---
   layout: section
   section: recetas
   title: Recetas
   permalink: /recetas/
   ---
   ```

4. In `_includes/sidebar.html`, `_layouts/home.html`, and
   `_layouts/section.html` there is a `{% case key %}` block — add a branch
   for `recetas`.

If you want, hand these steps to an AI agent (see `AGENTS.md` for the repo
context it should follow).

---

## 6. Changing the look

- **Colors, fonts, spacing**: `assets/css/main.scss`. At the top of the file
  there are CSS variables (`--bg`, `--text`, `--accent`, ...). Change them
  there.
- **Sidebar / header structure**: `_includes/sidebar.html` and
  `_includes/head.html`.
- **Page templates**: `_layouts/`. `default.html` wraps everything else.
  `home.html`, `section.html`, `post.html`, `page.html` are the four
  variants.

---

## 7. Moving off GitHub Pages

Nothing in the site depends on GitHub-specific features. To migrate:

1. **Netlify / Cloudflare Pages / Vercel**: point them at the repo, set the
   build command to `bundle exec jekyll build`, and set the publish
   directory to `_site`.
2. **Your own server**: run `bundle exec jekyll build` locally and upload
   `_site/` via FTP or `rsync`.

You can also swap the `github-pages` gem for plain `gem "jekyll"` if you
want newer Jekyll versions than GitHub Pages supports.

---

## 8. Things that will break the site if you touch them

- Renaming `_ciencia`, `_libros`, or `_notas` without updating
  `_config.yml`.
- Deleting `_layouts/default.html`.
- Changing `permalink:` values in `_config.yml` (breaks old URLs).

---

## 9. Markdown cheat sheet

```markdown
# Big heading
## Subheading
### Sub-sub

**bold**  *italic*  ~~strikethrough~~

[link text](https://example.com)

![image alt](/path/to/image.png)

- bulleted
- list
  - nested

1. numbered
2. list

> Block quote

`inline code`

​```python
code block
​```

| Col A | Col B |
|-------|-------|
| a     | b     |
```

---

## 10. Where to look for help

- Jekyll docs: <https://jekyllrb.com/docs/>
- GitHub Pages docs: <https://docs.github.com/en/pages>
