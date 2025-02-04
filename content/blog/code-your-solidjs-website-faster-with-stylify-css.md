---
title: 'Code your SolidJS website faster with Stylify CSS'
image: '/images/blog/stylify-solidjs/header.jpg'
ogImage: '/images/blog/stylify-solidjs/og-image.jpg'
author: 'Vladimír Macháček'
annotation: 'Code your SolidJS website faster with Stylify CSS'
createdAt: 'November 22, 2022'
---

Style your SolidJS app quickly and easily without CSS-in-JS using <nuxt-link to="/">Stylify CSS</nuxt-link> CSS-like utilities.

## Introduction
<nuxt-link to="/">Stylify</nuxt-link> is a library that uses CSS-like selectors to generate optimized utility-first CSS based on what you write.

- ✅ CSS-like selectors
- ✅ No framework to study
- ✅ Less time spent in docs
- ✅ Mangled & Extremely small CSS
- ✅ No CSS purge needed
- ✅ Components, Variables, Custom selectors
- ✅ It can generate multiple CSS bundles

Also we have a page about <nuxt-link to="/docs/get-started/why-stylify-css">what problems Stylify CSS solves and why you should give it a try!</nuxt-link>

## Installation
Install Stylify using CLI:
```
npm i -D @stylify/unplugin
yarn add -D @stylify/unplugin
```

Add the following configuration into `vite.config.js`:
```js
import { defineConfig } from 'vite';
import { stylifyVite } from '@stylify/unplugin';
import solidPlugin from 'vite-plugin-solid';

const stylifyPlugin = stylifyVite({
    bundles: [{ outputFile: './src/stylify.css', files: ['./src/**/*.jsx'] }],
    // Optional
    compiler: {
        // https://stylifycss.com/docs/stylify/compiler#variables
        variables: {},
        // https://stylifycss.com/docs/stylify/compiler#macros
        macros: {},
        // https://stylifycss.com/docs/stylify/compiler#components
        components: {},
        // ...
    },
});

export default defineConfig({
    plugins: [stylifyPlugin, solidPlugin()],
    server: {
        port: 3000,
    },
    build: {
        target: 'esnext',
    },
});
```

Add Stylify CSS in `src/index.js`:
```js
import './stylify.css';
```

## Usage
Stylify syntax is similar to CSS. You just write `_` instead of a space and `^` instead of a quote.

So if we edit the `src/App.jsx`:
```jsx
function App() {
  return (
    <h1 class="font-size:24px margin:12px_24px">
      Hello World!
    </h1>
  );
}

export default App;
```

In production, you will get optimized CSS and mangled HTML:
```html
<h1 class="p u">Hello World!</h1>
```

```css
.p{font-size: 24px}
.u{margin: 12px 24px}
```

## Stackblitz Playground
Go ahead and try [Stylify CSS + SolidJS on Stackblitz](https://stackblitz.com/edit/stylifycss-solidjs-vite?file=src%2FApp.jsx).

## Configuration
The examples above don't include everything Stylify can do:
- Define <nuxt-link to="/docs/stylify/compiler#components">components</nuxt-link>
- Add <nuxt-link to="/docs/stylify/compiler#customselectors">custom selectors</nuxt-link>
- Configure <nuxt-link to="/docs/stylify/compiler#macros">your macros</nuxt-link> like `ml:20px` for margin-left
- Define <nuxt-link to="/docs/stylify/compiler#screens">custom screens</nuxt-link>
- You can map <nuxt-link to="/docs/bundler#files-content-option">nested files</nuxt-link> in the template
- And a lot more

Feel free to <nuxt-link to="/docs/get-started">check out the docs</nuxt-link> to learn more 💎.
