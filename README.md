# A schlocky client-side static web site generator

Got carried away on a Sunday afternoon and the result is a static site generator that…

- requires no build step or deployment pipeline because the Markdown rendering is done in the browser through [showdown](https://github.com/showdownjs/showdown)
- fetches internal links without a full page reload
- ships as a single `index.html` file, to use as-is or as base for customizations
- loads a default style from `style.css` and default Javascript from `scripts.js`, both optional
- has no template language (for now) but allows to prepend shared Markdown/HTML/CSS/JS parts, e.g., for a menu
- lacks any advanced features!

As future improvement it should also be possible to do relative movements in subfolders or external URLs.

Live demo: [https://pothos.github.io/schlocky/](https://pothos.github.io/schlocky/)

Or use it to render the [Rust contribution guide](https://pothos.github.io/schlocky/?i=https://raw.githubusercontent.com/rust-lang/rust/master/CONTRIBUTING.md).

## Usage

The file to render gets specified in the URL as query parameter `?i=FILE`.
The default file is `index.md`. The `.md` suffix can be omitted, e.g., `/?i=about` (or `/index.html?i=about` if `/` does not directly serve `index.html`). You can even point it to a GitHub branch with `?i=https://raw.githubusercontent.com/USER/REPO/BRANCH/FILE.md`.

This repository already contains example Markdown files, example `style.css` and `scripts.js` files, and a Linux helper script to run a local Python web server to try it out:

```
./serve.sh
# Now open http://127.0.0.1:8000/
```

When using GitHub pages, make sure you create the `.nojekyll` file in your repo to publish all files.

The Markdown files can start with metadata that sets the page title and may prepend common parts. Here is how an `index.md` could look like:

```
---
title: My Page
includes: menu.md
---
Here is the *index* page!
```

The `menu.md` could contain this (`?` instead of `/` to also work if served from subfolders):

```
* [Index](?)
* [About](?i=about)
```

And `about.md` could be:

```
---
title: My Page - About
includes: menu.md substyle.md
---
This is the *about* page.
```

Where `substyle.md` could inject CSS rules and code that are not part of `style.css` or `scripts.js` and only apply to `?i=about`:

```
<style>
body {
  color: blue;
}
</style>
<script>
console.log("about");
</script>
```

## Similar Projects

[cms.js](https://github.com/chrisdiana/cms.js)
