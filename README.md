# nextjs-blog-theme

A personal copy of the [Netlify Next.js blog template](https://github.com/netlify-templates/nextjs-blog-theme), designed by [Bejamas](https://bejamas.io). It is a small blog: posts are MDX files in `posts/`, pages are built with Next.js and styled with Tailwind CSS, and there is a light and dark mode.

![Preview of the blog theme: author Jay Doe, blog name "Next.js Blog Theme", one post](nextjs-blog-theme-preview.png)

Upstream demo: <https://bejamas-nextjs-blog.netlify.app>

## What it contains

- Next.js 16 (Pages Router), React 19, Tailwind CSS 4 with `@tailwindcss/typography`.
- MDX posts rendered with `next-mdx-remote`, with GitHub-flavored Markdown (`remark-gfm`) and Prism code highlighting.
- A home page that lists posts newest first (`pages/index.js`) and a post page with previous and next links (`pages/posts/[slug].js`).
- Light and dark mode. It follows the OS setting, and the footer has a toggle that is saved in `localStorage`.
- Five color themes and three font stacks (`themes.js`), picked with environment variables.
- Five example posts in `posts/`.
- `netlify.toml` for a Netlify build (`npm run build`, publish `.next`).

## Status

This repo is the upstream template as of January 2026 plus dependency security overrides in `package.json`. It has no tests.

Known problems on `main`:

- Pages that read posts fail. The `js-yaml` override in `package.json` puts js-yaml 4 under `gray-matter`, which calls `yaml.safeLoad`, a function js-yaml 4 removed. `npm run dev` returns a 500 on the home page and post pages, and `npm run build` stops while prerendering the first post.
- `npm ci` fails because `package-lock.json` is out of sync with `package.json`. `npm install` works.
- `npm run lint` and `npm run export` call `next lint` and `next export`, which Next.js 16 no longer has.

## Running it locally

The lockfile is for npm.

```shell
npm install
npm run dev
```

Then open <http://localhost:3000>. `npm run dev:watch` also reloads the page when a file in `posts/` changes. `npm run build` and `npm start` build and serve the production site.

## Configuring the blog

Settings come from environment variables, read when the pages are built:

| Variable | What it sets | Default |
| --- | --- | --- |
| `BLOG_NAME` | Name in the header and in page titles | `Jay Doe` |
| `BLOG_TITLE` | Main heading (`h1`) on the home page | `Next.js Blog Theme` |
| `BLOG_FOOTER_TEXT` | Footer text | `All rights reserved.` |
| `BLOG_THEME` | Color theme: `default`, `bejamas`, `netlify`, `reddie`, `greenie` | `default` |
| `BLOG_FONT_HEADINGS` | Font for headings: `sans-serif`, `serif`, `monospace` | `sans-serif` |
| `BLOG_FONT_BODY` | Font for everything else, same options | `sans-serif` |

The upstream README calls the last one `BLOG_FONT_PARAGRAPHS`; the code reads `BLOG_FONT_BODY`.

To change the defaults in code instead, edit `utils/global-data.js` (name, title, footer) and `utils/theme-utils.js` (theme and fonts). The color and font values live in `themes.js`.

## Adding posts

Add a `.mdx` file to `posts/`. The file name becomes the URL (`posts/my-post.mdx` is served at `/posts/my-post`). Front matter fields:

```yaml
---
title: My post
description: One line shown on the home page and under the post title.
date: '2026-01-31'
---
```

MDX files cannot use `import`. To use a React component in a post, add it to the `components` object in `pages/posts/[slug].js`. Links and images are already mapped to `components/CustomLink.js` and `components/CustomImage.js`.

## Dependency updates

`renovate.json` comes from the upstream template and extends the `netlify-templates/renovate-config` preset.

## License

MIT, see [LICENSE](LICENSE). The template is by Bejamas and Netlify (see `package.json` for contributors).
