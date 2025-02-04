---
title: 'Simple React like button with Stylify CSS. From Utilities, to Components, mangled selectors and 50% smaller production build.'
image: '/images/blog/stylify-react-like-button/header.jpg'
ogImage: '/images/blog/stylify-react-like-button/og-image.jpg'
author: 'Vladimír Macháček'
annotation: 'Style buttons in React quickly with Stylify CSS. Its like writing CSS directly into the template.'
createdAt: 'October 10, 2022'
---

Check out how to style a button quickly using only utilities and then clean the template using <nuxt-link to="/docs/get-started#defining-a-component">components</nuxt-link>. Learn why the output in production can be 50% and smaller🔥.

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

## The Code
Here is the code behind the button:
```jsx
import { useState } from 'react';
import './stylify.css';

function LikeButton() {
  const [count, setCount] = useState(0);

  return (
    <button
      className="
        color:#222
        font-weight:bold
        border:0
        background:#fff
        font-size:16px
        padding:8px_16px
        border-radius:4px
        cursor:pointer
        box-shadow:0_.3em_.6em_rgba(0,0,0,0.3)
        transition:background_0.3s,scale_0.3
        align-items:center
        display:inline-flex

        [&:hover_span:first-of-type]{transform:scale(1.5)}
        [span]{display:inline-flex}
      "
      onClick={() => setCount(count + 1)}
    >
      <span className="
          margin-right:8px
          font-size:24px
          transition:transform_0.3s
      ">❤️</span>
      <span className="margin-right:6px">Like</span>
      <span
        className="
          background:#eee
          padding:4px
          align-items:center
          justify-content:center
          border-radius:50%
          min-width:24px
          min-height:24px
      ">{count}</span>
    </button>
  );
}

export default LikeButton;
```

The generated CSS for example above:
```css
.color\:\#222{color: #222}
.font-weight\:bold{font-weight: bold}
.border\:0{border: 0}
.background\:\#fff{background: #fff}
.font-size\:16px{font-size: 16px}
.padding\:8px_16px{padding: 8px 16px}
.border-radius\:4px{border-radius: 4px}
.cursor\:pointer{cursor: pointer}
.box-shadow\:0_\.3em_\.6em_rgba\(0\,0\,0\,0\.3\){box-shadow: 0 .3em .6em rgba(0,0,0,0.3)}
.transition\:background_0\.3s\,scale_0\.3{transition: background 0.3s,scale 0.3}
.align-items\:center{align-items: center}
.\[span\]\{display\:inline\-flex\} span,
.display\:inline-flex{display: inline-flex}
.margin-right\:8px{margin-right: 8px}
.font-size\:24px{font-size: 24px}
.transition\:transform_0\.3s{transition: transform 0.3s}
.margin-right\:6px{margin-right: 6px}
.background\:\#eee{background: #eee}
.padding\:4px{padding: 4px}
.justify-content\:center{justify-content: center}
.border-radius\:50\%{border-radius: 50%}
.min-width\:24px{min-width: 24px}
.min-height\:24px{min-height: 24px}
.\[\&\:hover\_span\:first\-of\-type\]\{transform\:scale\(1\.5\)\}:hover span:first-of-type,
.transform\:scale\(1\.5\){transform: scale(1.5)}
```
## Production build - 50% smaller
When you allow <nuxt-link to="/">Stylify</nuxt-link> to mangle selectors, then the output looks like this:
```css
.c{color:#222}
.d{font-weight:bold}
.e{border:0}
.f{background:#fff}
.g{font-size:16px}
.h{padding:8px 16px}
.i{border-radius:4px}
.j{cursor:pointer}
.k{box-shadow:0 .3em .6em rgba(0,0,0,0.3)}
.l{transition:background 0.3s,scale 0.3}
.m{align-items:center}
.b span,.n{display:inline-flex}
.o{margin-right:8px}
.p{font-size:24px}
.q{transition:transform 0.3s}
.r{margin-right:6px}
.s{background:#eee}
.t{padding:4px}
.u{justify-content:center}
.v{border-radius:50%}
.w{min-width:24px}
.x{min-height:24px}
.a:hover span:first-of-type,.y{transform:scale(1.5)}
```

Also, the selectors in JSX are minified
```jsx
<button
  className="c d e f g h i j k l m n a b"
  onClick={() => setCount(count + 1)}
>
  <span className="o p q">❤️</span>
  <span className="r">Like</span>
  <span className="s t m u v w x">{count}</span>
</button>
```

CSS size:
- Dev: **1101 bytes**
- Production: **556 bytes**

The size savings are around **50%** (The size is similar in gzipped mode). If we take the mangled HTML, the difference will be even bigger.

## Template cleanup
What if we have a lot of utilities and want to move them out of the template? With <nuxt-link to="/">Stylify</nuxt-link> you can do that using reusable components. They can be defined within a comment (expects js object without surrounding brackets) in the file where they are used or in a global config.

```jsx
// ...

/*
stylify-components
'like-button': `
  color:#222
  font-weight:bold
  border:0
  background:#fff
  font-size:16px
  padding:8px_16px
  border-radius:4px
  cursor:pointer
  box-shadow:0_.3em_.6em_rgba(0,0,0,0.3)
  transition:background_0.3s,scale_0.3
  align-items:center
  display:inline-flex
  span { display:inline-flex }
  &:hover span:first-of-type {  transform:scale(1.5) }
`,
'like-button__hearth': 'margin-right:8px font-size:24px transition:transform_0.3s',
'like-button__counter': `
  background:#eee
  padding:4px
  align-items:center
  justify-content:center
  border-radius:50%
  min-width:24px
  min-height:24px
`
/stylify-components
*/

function LikeButton() {
  // ...

  return (
    <button className="like-button" onClick={() => setCount(count + 1)}>
      <span className="like-button__hearth">❤️</span>
      <span className="margin-right:6px">Like</span>
      <span className="like-button__counter">{count}</span>
    </button>
  );
}

// ...
```

In production, the components are also mangled.

## Syntax explanation
In the example above, you can see Stylify using CSS-like selectors. With a few differences.
- `_` within a selector is used instead of a space
- `[span]{display:inline-flex}` is an inline custom selector. This allows you to style custom selectors.
- `&` inside `[&:hover_span:first-of-type]` always refers to an upper level like in SCSS
- The indented syntax in components is also like in SCSS. Except, to keep things simple, it supports only nesting and chaining
```html
span {
  display:inline-flex
}
&:hover span:first-of-type {
  transform:scale(1.5)
}
```

## Check out Stackblitz Playground
You can try the playground on [Stackblitz](https://stackblitz.com/edit/stylify-react-like-button-example?file=src%2FLikeButton.jsx).
