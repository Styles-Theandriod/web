---
slug: syntax-cheat-sheet
section: get-started

order: 2

navigationTitle: Syntax Cheat Sheet

title: "Syntax Cheat Sheet"
description: "Complete Stylify CSS selectors Syntax Cheat Sheet."
---

This page is a guide for you to easily learn Stylify CSS Syntax at one place.

It contains often use selectors patterns, so you can quickly find the one you don't know how to write.

<note>
If you haven't found the selector you were looking for, let us know, and we will add it.
</note>

## Special characters
- When creating a selector, use `_` (underscore) instead of space and `^` for a quote. This applies to any form of a selector.
- To preserve an underscore or a hat character, use `\` (backslash) => `\_`.

- If you defined variables, you can use them within selectors. Just add `$` before the variable.

## Selectors

```html
color:blue
👉 .color\:blue {}

<!-- Using variable $blue -->
color:$blue
👉 .color\:\$blue {}

<!-- Using _ -->
margin:0_24px
👉 .margin\:0_24px {}

<!-- Using ^ -->
content:^✅^
👉 .content\:\^\✅\^

hover:color:blue
👉 .color\:blue:hover {}

<!--
Wrap classes into pseudo-classes group.
The example below is a shortcut for
hover:color:red + hover:font-weight:bold.
-->
hover:{color:red;font-weight:bold}
👉 hover\:\{color\:red;font-weight:bold\}:hover {}

<!-- The same as above but also with screen -->
md:hover:{color:red;font-weight:bold}
👇
@media (min-width: 1024px) {
	md\:hover\:\{color\:red;font-weight:bold\}:hover {}
}

lg:hover:color:blue
👇
@media (min-width: 1024px) {
	.color\:blue:hover {}
}

minw640px:color:blue
👇
@media (min-width: 640px) {
	.color\:blue {}
}

minw640px&&maxw1023px:color:blue
👇
@media (min-width: 640px) and (max-width: 1023px) {
	.minw640px\&\&maxw1023px\:color\:blue {}
}
```

## Custom Selectors

- Custom selectors have the following pattern:
```
[css selectors]{stylify selectors split by ;}
```

- When the `&` character is used within class attribute, it refers to current element.

Below are examples for custom selectors within `class` attribute:
```html
[a]{color:blue}
👉 .\[a\]\{color\:blue\} a {}

[a]{hover:color:blue}
👉 .\[a\]\{hover\:color\:blue\} a:hover {}

[a]{lg:hover:color:blue}
👇
@media (min-width: 1024px) {
	.\[a\]\{lg\:hover\:color\:blue\} a:hover {}
}

<!-- Multiple selectors split by , -->
[h1,h2]{font-weight:bold}
👇
.\[h1\,h2\]\{font\-weight\:bold\} h1,
.\[h1\,h2\]\{font\-weight\:bold\} h2 {}

<!-- Using & -->
[&+div]{margin-left:12px}
👉 .\[\&\+div\]\{margin\-left\:12px\}+div {}

<!-- Preserving _ (underscore) using \ before _ -->
[.custom\_\_section]{color:blue}
👉 .\[\.custom\_\_section\]\{color\:blue\} .custom__section {}

[li:nth-of-type(even)]{color:blue}
👉 .\[li\:nth\-of\-type\(even\)\]\{color\:blue\} li:nth-of-type(even) {}

[&[open]_summary_~_*]{visibility:visible}
👉 .\[\&\[open\]\_summary\_\~\_\*\]\{visibility\:visible\}[open] summary ~ * {}

[summary::-webkit-details-marker,summary::marker]{display:none}
👇
.\[summary\:\:\-webkit\-details\-marker\,summary\:\:marker\]\{display\:none\} summary::-webkit-details-marker,
.\[summary\:\:\-webkit\-details\-marker\,summary\:\:marker\]\{display\:none\} summary::marker {}

[:not(summary)]{opacity:O}
👉 .\[\:not\(summary\)\]\{opacity\:O\} :not(summary) {}
```

- The nested syntax is similar to SCSS and the actual submitted CSS-nesting module (in proposal).

- The `&` character always refers to the upper level.

Here are equialents written within Stylify CSS nesting syntax:
```js
const compilerConfig = {
	customSelectors: {
		a: 'color:blue',
		`h1,h2`: `
			margin-top:0

			& + p {
				margin-top:0
			}
		`
	}
}
```

CSS output:
```css
a {}
h1, h1 + p,
h2, h2 + p {}
```


## Components

```js
const compilerConfig = {
	components: {
		'title': `
			font-size:24px

			&--larger {
				font-size:32px
			}
		`,
	}
}
```

CSS output:
```css
.title {}
.title--larger {}
```
