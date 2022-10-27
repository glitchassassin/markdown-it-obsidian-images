# Markdown-It Obsidian Images

[![Build Status](https://travis-ci.org/glitchassassin/markdown-it-obsidian-images.svg?branch=master)](https://travis-ci.org/glitchassassin/markdown-it-obsidian-images) [![Coverage Status](https://coveralls.io/repos/github/glitchassassin/markdown-it-obsidian-images/badge.svg?branch=master)](https://coveralls.io/github/glitchassassin/markdown-it-obsidian-images?branch=master)

Renders [Obsidian-style images](https://help.obsidian.md/How+to/Format+your+notes#Images) in [markdown-it](https://github.com/markdown-it/markdown-it). This is useful for making Obsidian-based blogs or digital gardens..

## Usage

Install this into your project:

```bash
npm --save install markdown-it-obsidian-images
```

...and *use* it:

```js
const obsidianImages = require('markdown-it-obsidian-images')(options)
const md = require('markdown-it')()
    .use(obsidianImages)
    .render('![[Image1|A beautiful image]] [[/Image2]]')
```

**Output:**

```html
<p><img src="./Image1.png" alt="A beautiful image" /> <img src="/Image2.png" /></p>
```

## Options

### `baseURL`

**Default:** `/`

The base URL for absolute image URLs.

```js
const html = require('markdown-it')()
  .use(require('markdown-it-obsidian-images')({ baseURL: '/content/' }))
  .render('![[/Hero Image]]')
  // <p><img src="/content/Hero_Image.png" /></p>
```

### `relativeBaseURL`

**Default:** `./`

The base URL for relative wiki links.

```js
const html = require('markdown-it')()
  .use(require('markdown-it-obsidian-images')({ relativeBaseURL: '/content/', suffix: '' }))
  .render('![[Hero Image]]')
  // <p><img src="/content/Hero_Image" /></p>
```

### `makeAllLinksAbsolute`

**Default:** `false`

Render all image URLs as absolute paths.

### `uriSuffix`

**Default:** `.png`

Append this suffix to every URL.

```js
const html = require('markdown-it')()
  .use(require('markdown-it-obsidian-images')({ uriSuffix: '.jpg' }))
  .render('![[Hero Image]]')
  // <p><img src="./Hero_Image.jpg" /></p>
```

### `htmlAttributes`

**Default:** `{}`

An object containing HTML attributes to be applied to every link.

```js
const attrs = {
  'class': 'full-width'
}
const html = require('markdown-it')()
  .use(require('markdown-it-obsidian-images')({ htmlAttributes: attrs }))
  .render('![[Hero Image]]')
  // <p><img src="./Hero_Image.png" class="full-width" /></p>
```

### `postProcessImageName`

A transform applied to every page name.

The default transform does the following things:

* trim surrounding whitespace
* [sanitize](https://github.com/parshap/node-sanitize-filename) the string
* replace spaces with underscores

#### Example

```js
const _ = require('slugify')

function myCustomPostProcessImageName(label) {
  return label.split('/').map(function(pathSegment) {
    return slugify(pathSegment.toLowerCase())
  })
}

const html = require('markdown-it')()
  .use(require('markdown-it-obsidian-images')({ postProcessImageName: myCustomPostProcessImageName }))
  .render('![[Hello World]]')
  // <p><img src="./hello-world.png" /></p>
```

### `postProcessLabel`

A transform applied to every image alt label. You can override it just like `postProcessImageName` (see above).

The default transform trims surrounding whitespace and replaces the characters `<&"` with html-encoded equivalents

## Credits

Based on [markdown-it-wikilinks](https://github.com/jsepia/markdown-it-wikilinks/) by Julio Sepia
