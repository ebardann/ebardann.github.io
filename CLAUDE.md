# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Emilio Bardán's personal academic site: Hugo with a hand-written theme in `themes/plain`, deployed to GitHub Pages. See `README.md` for the front matter fields used when adding content.

## Commands

```sh
hugo server                                            # preview at http://localhost:1313
hugo --gc --minify --panicOnWarning -d /tmp/site-out   # same strictness as CI, into a clean directory
hugo --panicOnWarning --logLevel info                  # also shows deprecation notices, which are info-level
hugo new research/my-paper.md                          # new paper or project from archetypes/research.md
```

There are no tests or linters. A change is checked by building with no warnings and looking at the rendered pages. `public/` can hold stale files, and removed pages have still been served from it by `hugo server`, so check removals against a build into a clean directory.

## Constraints

- No client-side JavaScript. The `<script>` seen under `hugo server` is Hugo's dev livereload and isn't in production builds.
- No third-party theme and no CDNs. Fonts and KaTeX are vendored in `themes/plain/static`.
- Typography: one serif body font (Source Serif 4), a measure of about 70ch, light and dark through `prefers-color-scheme` only. Colours are CSS variables at the top of `themes/plain/assets/css/main.css`.

## Architecture

**Identity and navigation.** The name shown across the site comes from `title` in `hugo.toml`, not from `content/_index.md`, whose front matter supplies the home page's links and whose body is the research statement. The menu is `[[menus.main]]` in `hugo.toml`.

**Research (`content/research/`).** Each file is one entry, but no paper has its own page: `_index.md` cascades `build.render: never` to its children. `layouts/research/section.html` splits them into Papers and Projects on `project: true` and renders each with `_partials/entry.html`. The filename becomes the anchor, `/research/#file-name`, and the body becomes the collapsible abstract.

**Writing (`content/writing/`).** The list page has two sources, both in `layouts/section.html`. Essays published elsewhere, such as Substack, are `external` entries in the front matter of `_index.md` and render as image cards. Local posts are ordinary files and render as a plain list. Only this section outputs RSS, set in its `_index.md`; `external` entries aren't in the feed.

**CV.** There is no CV page. The home page's `links` in `content/_index.md` point straight at `static/cv.pdf`; it isn't in the menu.

**Math.** Three pieces work together:
- `[markup.goldmark.extensions.passthrough]` in `hugo.toml` hands `$…$` and `$$…$$` to the render hook untouched.
- `layouts/_markup/render-passthrough.html` calls `transform.ToMath` at build time. Invalid LaTeX fails the build.
- `baseof.html` loads the KaTeX CSS on every page on purpose. Hugo's per-page "has math" flag doesn't see math rendered from child pages, so conditional loading would leave math unstyled on the Research page.

Math also works in titles, because templates use `.Title | .RenderString`.

## Pitfalls

- **Paths in front matter:** `pdf` and `image` are relative to `static/` with no leading slash (`papers/x.pdf`), because they go through `relURL`.
- **KaTeX version:** `themes/plain/static/katex` must match the version embedded in Hugo (0.18.4 for Hugo 0.169.0), listed in `internal/warpc/js/package.json` in Hugo's source. Check it whenever `HUGO_VERSION` in `.github/workflows/deploy.yml` changes.
- **Vendored KaTeX CSS:** it is `dist/katex.min.css` with the `woff` and `ttf` sources stripped, so only `woff2` fonts ship.
- **Literal dollars:** write `\$` in content. An unescaped pair in one paragraph silently becomes math.
- **Punctuation after inline math:** a following comma or period can wrap onto the next line alone, so write `$x,$` rather than `$x$,`. A word joiner (U+2060) in the render hook made no difference when tested. A `nowrap` wrapper does work, but it means post-processing `.Content` at every call site.
- **Future dates:** `buildFuture = true` is deliberate, so forthcoming papers aren't dropped.

## Deployment

`.github/workflows/deploy.yml` runs on every push to `main`. It installs a pinned, checksum-verified Hugo, builds with `--panicOnWarning`, so any warning fails the deploy, and takes `baseURL` from `actions/configure-pages`. The repository's Pages source must be set to GitHub Actions.

## Remaining placeholder content

- `content/research/example-paper.md` is the real EEG paper despite its filename, but its abstract, `arxiv` and `code` values are placeholders.
- `content/research/example-project.md`, `content/writing/example-post.md` and `static/papers/example-paper.pdf` are placeholders.
- `description` in `hugo.toml` still says "Your Name".
