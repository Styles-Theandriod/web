---
title: 'Using Material Theme from Material Theme Builder in Stylify CSS'
image: '/images/blog/stylify-material-theme/header.jpg'
ogImage: '/images/blog/stylify-material-theme/og-image.jpg'
author: 'Vladimír Macháček'
annotation: 'Learn how to use generated Material Theme from Material Theme Builder in Stylify CSS and increase your productivity in a minute.'
createdAt: 'January 14, 2023'
---

[Material Theme Builder](https://m3.material.io/theme-builder#/custom) is a tool with which you can easily generate Material Theme color palettes and typography sizes for your website.

Features:
- Theme colors can be generated from an uploaded image or by using their color picker.
- Configured theme can be exported into Theme.kt, XML, Dart, CSS and DSP.
- There is a nice preview of each color and a simple explanation how it should be used.

This guide explains how to generate Material Theme, how to export it into CSS file and how to use it in Stylify CSS.

<video autoplay muted loop>
	<source src="/videos/material-theme-builder.mp4" type="video/mp4" />
</video>

## Playground
Stylify has prepared playground with Vite.js. You can check out the configuration and use it within your project.

The vite plugin configuration can be used within all integrations. This means for example Next.js, Nuxt.js, SvelteKit, Vite.js, Webpack and any other.

<stack-blitz-link link="stylify-material-theme-example"></stack-blitz-link>

## How to get a custom Material Theme
Generating the desired theme with the Material Theme Builder is simple:
- Go to the [Material Theme Builder](https://m3.material.io/theme-builder#/custom) website.
- On the left side (on Desktop), click on the primary color.
- The color generator dialog will appear. Select your favorite color.
- Optionally, repeat this process for a secondary, tertiary and neutral color.
- When you finish the configuration, in the top right corner, there is an export button
- Click on it and select Web (CSS).
- Download the file and extract it into your project.

In case you wonder, how to use each color, make sure to check out the section under the device preview. There are color palettes with the name of each color:
- For example `on-primary` color should be used for text on `primary` color. This means for example a button with primary color background (purple) and on primary text color (white)
- Colors are split for light/dark mode
- The complete guide can be found in [material theme docs](https://m3.material.io/styles/color/the-color-system/color-roles)

<img src="/images/blog/stylify-material-theme/colors-setup.png" loading="lazy" fetchpriority="low">

## Integrating Material Theme into Stylify CSS
To finish the configuration process, you have to configure Stylify, so it knows, that variables starting with `md-` are external and that it has to replace variables with CSS variables instead of their value.

### With Unplugin
This part is for `@stylify/unplugin` exports: `stylifyVite`, `stylifyWebpack`, `stylifyEsbuild` and `stylifyRollup`.

If you use the Unplugin in your project (Next.js, Nuxt.js, SvelteKit, Vite, Webpack, Rollup or any other), this guide is what you are looking for:
```js
const stylifyConfig = {
	compiler: {
		replaceVariablesByCssVariables: true,
		externalVariables: [
			// Add external variables identifier
			// This one checks, if variable starts with md-.
			// If so, it is marked as external.
			// Do not add --md. Identifier methods works with Stylify variable name
			// and not CSS variable name. Stylify adds -- automatically
			// if CSS variables are enabled
			(variable) => {
				if (variable.startsWith('md-')) return true;
			}
		]
	}
	// ...
};

stylifyVite(stylifyConfig);
stylifyWebpack(stylifyConfig);
stylifyRollup(stylifyConfig);
stylifyEsbuild(stylifyConfig);
```

### Astro integration
The config is the same like for Unplugin.

```js
stylify(stylifyConfig);
```

### Bundler
In case you use `@stylify/bundler` directly, use the `stylifyConfig` from the above section about Unplugin Integration, but you have to pass it into the Bundler initialization.

```js
const bundler = new Bundler({
	...stylifyConfig
});
```

## Usage
The final step is to import the generated theme into our project:
- Extract the generated theme into your project.
- Import the `theme.css` file into your project (make sure the directory is public). This file will import all other necessary files.

<note>
If you care about optimization a bit, this will also cause at least 4 HTTP requests because of the `@import` rules within the theme file.
You might want to copy the content of those other files into the <code>theme.css</code> file or merge them using some bundler to decrease the number of requests.
</note>

When the file is imported into your project, you can start using Material Theme CSS variables in Stylify utilities.
```html
<button class="
	border:none
	padding:8px_12px
	border-radius:24px

	background:$md-sys-color-primary
	color:$md-sys-color-on-primary
	display-large
">Material Button</button>
```

<img src="/images/blog/stylify-material-theme/buttons.png" loading="lazy" fetchpriority="low">

## Cleanup
In case you plan to use Stylify CSS utilities only, then you can remove the `colors.module.css`. It contains utilities for `color` and `background`.

Apart from the colors module, there is a `typography.module.css`. You might want to remove it as well and rewrite these classes into Stylify components using Stylify dynamic components syntax.

```js
const stylifyConfig = {
	compiler: {
		components: {
			// Example for display class
			// This matches display-small/medium/large
			// and returns utilities dynamically based on the matched size
			'display-(small|medium|large)': ({ matches }) => `
				font-family:$md-sys-typescale-display-${matches[1]}-font-family-name
				font-style:$md-sys-typescale-display-${matches[1]}-font-family-style
				font-weight:$md-sys-typescale-display-${matches[1]}-font-weight
				font-size:$md-sys-typescale-display-${matches[1]}-font-size
				letter-spacing:$md-sys-typescale-display-${matches[1]}-tracking
				line-height:$md-sys-typescale-display-small-height
				text-transform:$md-sys-typescale-display-${matches[1]}-text-transform
				text-decoration:$md-sys-typescale-display-${matches[1]}-text-decoration
			`
		}
	}
}
```

## Credits
A big thanks go to [Oscar Lito M Pablo](https://twitter.com/FlipOneUp) that came up with the idea to make this guide.
