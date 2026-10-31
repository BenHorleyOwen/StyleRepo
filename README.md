# Style guide

Design tokens live in `_data/themes/<name>.yml`. A Liquid template (`assets/css/tokens.css`)
turns them into CSS custom properties at build time. Sites ingest a theme by name and style
themselves with `var(--...)`. This repo also builds into a page that shows every colour and role.

## Layout

```
_data/themes/matchbox.yml   palette, roles, type, spacing, shape  <- edit this
assets/css/tokens.css       Liquid template -> :root { --color-...: ...; }
assets/css/main.css         reference stylesheet (scrollbar, titles, nav, cards)
index.html                  living style guide page
_config.yml                 style_theme: matchbox
examples/consumer-site/     what to add to a site that uses this
```

## Use it from a site

1. Add the two steps in `examples/consumer-site/workflow-steps.yml` to the site's workflow.
2. Add `style_theme: matchbox` to the site's `_config.yml`.
3. In the page `<head>`, link tokens before main:
   ```html
   <link rel="stylesheet" href="{{ '/assets/css/tokens.css' | relative_url }}">
   <link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}">
   ```
4. In the site's `main.css`, replace raw colours with variables, for example:

   | Was | Becomes |
   |---|---|
   | `background: #d9cfb6` | `background: var(--color-page-bg)` |
   | `color: #341b1e` | `color: var(--color-text)` |
   | nav background | `var(--color-nav-bg)` |
   | title box | `var(--color-title-bg)` with `var(--color-title-text)` |
   | scrollbar | `var(--color-scrollbar-thumb)` / `var(--color-scrollbar-track)` |

   `assets/css/main.css` in this repo shows all of these in use.

## Add another theme

Copy `matchbox.yml` to `_data/themes/<new-name>.yml`, change the values, and set
`style_theme: <new-name>` in the site that should use it. The same roles must exist in every theme
so site CSS keeps working.

## Colour contrast notes (matchbox)

| Pairing | Ratio | Note |
|---|---|---|
| mahogany on papyrus | about 10:1 | Body text, fine |
| mahogany on sage | about 8.6:1 | Fine |
| mahogany on slate-blue | about 3.8:1 | Nav text, OK for bold text |
| silver on slate-blue | about 2.2:1 | Title text, below the 3:1 minimum for large text |

If titles feel faint, either make `title-bg` a darker blue (add one to the palette) or swap
`title-text` to `papyrus`/`mahogany`. Only the YAML changes.
