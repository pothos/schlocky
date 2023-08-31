# A schlocky client-side static web site generator

Got carried away on a Sunday afternoon and the result is a static site generator that…

- requires no build step or deployment pipeline because the Markdown rendering is done in the browser through [showdown](https://github.com/showdownjs/showdown)
- fetches internal links without a full page reload
- ships as a single `index.html` file, to use as-is or as base for customizations
- loads a default style from `style.css` and default Javascript from `scripts.js`, both optional
- has no template language (for now) but allows to prepend shared Markdown/HTML/CSS/JS parts, e.g., for a menu
- lacks any advanced features!

As improvement one could do the link rewriting after fetching to show the final links in the tooltip and retain the base path context for links in includes that are relative.

Live demo: [https://pothos.github.io/schlocky/](https://pothos.github.io/schlocky/)

Or use it to render the [Rust contribution guide](https://pothos.github.io/schlocky/?i=https://raw.githubusercontent.com/rust-lang/rust/master/CONTRIBUTING.md).

## Usage

The file to render gets specified in the URL as query parameter `?i=FILE`.
The default file is `index.md`. The `.md` suffix can be omitted, e.g., `/?i=about` (or `/index.html?i=about` if `/` does not directly serve `index.html`). You can even point it to a GitHub branch with `?i=https://raw.githubusercontent.com/USER/REPO/BRANCH/FILE.md`. Note that the paths are resolved with the `index.html` folder path prepended to absolute and relative paths. For external HTTP paths relative movement works but absolute paths will be resolved according to the current page.

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

The `menu.md` could contain this (here absolute paths are used to make the links also work when opened from subfolders where the menu is included with `includes: ../menu.md`, as the main page URL path still gets prepended to absolute links):

```
* [Index](?=/)
* [About](?i=/about)
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
