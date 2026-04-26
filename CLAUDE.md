# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Building

This project **requires XeLaTeX** (not `pdflatex`) because fonts are loaded as TTF files via `fontspec`.

```bash
xelatex main.tex
```

Run twice if the table of contents needs to update. With `latexmk`:

```bash
latexmk -xelatex main.tex
```

## Architecture

The class file `recipebook.cls` is the core engine — it loads config, packages, fonts, defines all environments and macros, and controls the entire page layout. Reading it is the authoritative source for how anything renders.

The load order within `recipebook.cls` matters:
1. `recipebook.cfg` is loaded first — it defines `\recipebooklang`, `\title`, `\author`, and PDF metadata commands that the class depends on.
2. `babel` is loaded with the language from `\recipebooklang`.
3. `recipebook-lang.sty` is loaded — it sets label strings (Ingredients, Instructions, etc.) in English by default, then overrides them at `\AtBeginDocument` via `\iflanguage` checks.
4. `hyperref` is loaded last (as required), consuming the metadata commands.

`main.tex` is minimal: it sets the document class, calls `\customtableofcontents`, then `\input`s each recipe file in order.

## Adding a Recipe

1. Create `recipes/your_recipe.tex`.
2. Start with `\setRecipeMeta{Title}{Servings}{Prep Time}{Cook Time}{./images/filename.jpg}`.
3. Wrap content in `\begin{recipe}...\end{recipe}`.
4. Inside, use `\begin{ingredients}` with `\ingredient{...}` items, and `\begin{steps}` with `\step{...}` items.
5. Use `\ingredientGroup{Label}` inside `ingredients` to add a subheading between ingredient items.
6. `\input{recipes/your_recipe}` in `main.tex` to include it.

If no image is available, use `./images/CookbookDefault.jpg` as the image path.

## Fonts

Fonts are loaded from the local `fonts/` directory by `fontspec`. Any new font must be TTF or OTF and placed in `fonts/`. The class defines two font families:
- `\fontseasons` / `\fontseasonslight` — The Seasons, used for titles and decorative headings.
- `\fontpublicsans` — Public Sans, used for body and metadata text.

## Localization

Change `\recipebooklang` in `recipebook.cfg`. Supported values: `english`, `german`, `french`, `spanish`. This affects both `babel` hyphenation and the UI label strings in `recipebook-lang.sty`. To add a new language, add an `\iflanguage{...}` block in `recipebook-lang.sty` for each of the six `\label*` commands.
