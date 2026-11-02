# Personal site

Built with [Hugo](https://gohugo.io) and a small custom theme in `themes/plain`. No client-side JavaScript.

    hugo server                 # preview at http://localhost:1313
    hugo --panicOnWarning       # production build into public/

## Adding content

**A paper or project:** add one Markdown file to `content/research/`, by hand or with `hugo new research/my-paper.md`.

    ---
    title: "My Paper with $\\log n$ in the Title"
    authors: [Your Name, A. Coauthor]
    venue: NeurIPS
    date: 2026-03-01
    pdf: papers/my-paper.pdf    # file in static/papers/
    arxiv: "2401.01234"
    code: https://github.com/you/my-paper
    links:                      # optional extra links
      - {name: Slides, url: https://example.org/slides.pdf}
    # project: true             # list under Projects instead of Papers
    ---

    The abstract goes here.

Every field is optional except `title`. Entries sort newest first, and `weight: 1` pins one to the top. Backslashes in a double-quoted title must be doubled, as above.

**A post:** add a file to `content/writing/`. Essays published elsewhere go under `external` in `content/writing/_index.md`, each with a `title`, `url`, `date`, `source`, and optional `description` and `image` (a file in `static/writing/`). To drop the Writing section, delete that folder and its `[[menus.main]]` entry in `hugo.toml`.

**The CV:** replace `static/cv.pdf`.

## Math

KaTeX renders math at build time through `transform.ToMath`, so readers download CSS and fonts but no JavaScript. Use `$…$` inline and `$$…$$` for display. Write a literal dollar sign as `\$`. Invalid LaTeX fails the build and names the file and line.

Two habits to keep:

- An unescaped pair of dollar signs in one paragraph becomes math without any error, so `$5 and $10` renders wrongly. Write `\$5 and \$10`.
- Browsers can wrap punctuation that follows math onto the next line by itself. Put it inside the delimiters: `$x,$` rather than `$x$,`.

## Deploying

`.github/workflows/deploy.yml` builds and deploys on every push to `main`. In the repository, set **Settings → Pages → Source** to **GitHub Actions** once.

## Upgrading Hugo

The KaTeX CSS in `themes/plain/static/katex` must match the KaTeX version Hugo embeds (0.18.4 in Hugo 0.169.0). That version is listed in `internal/warpc/js/package.json` in the Hugo source. When changing `HUGO_VERSION`, check it and update the CSS and fonts if it moved. The vendored CSS is `dist/katex.min.css` with its `woff` and `ttf` sources removed.

Source Serif 4 ships Latin and Latin Extended only. Greek or Cyrillic in body text falls back to Georgia; math is unaffected.
