---
section: integrations

order: 1

navigationTitle: "Esbuild"
navigationIconPath: '/images/brands/esbuild.svg'
image: '/integrations/esbuild/header.jpg'
ogImage: '/integrations/esbuild/og-image.jpg'

title: Using Stylify CSS with Esbuild
description: "Learn how to use the Stylify CSS along with the Esbuild. Code your website faster with Stylify CSS and Esbuild."
howToSchemaTitle: 'How to use Stylify CSS in Esbuild.'
howToSchemaSteps: [
	{
		"name": "Installation",
		"text": "Install @stylify/unplugin package using CLI like YARN or NPM.",
		"url": "#installation",
	},
	{
		"name": "Usage",
		"text": "Add Stylify CSS build module into vite.config.js.",
		"url": "#usage",
	},
	{
		"name": "Configuration",
		"text": "Extend Stylify CSS configuration however you need. Configure variables, components, custom selectors and a lot more.",
		"url": "#configuration",
	},
]
---

Esbuild is a module bundler for JavaScript which compiles small pieces of code into something larger and more complex, such as a library or application.
The main goal of the Esbuild bundler project is to bring about a new era of build tool performance, and create an easy-to-use modern bundler along the way.

## How to integrate the Stylify CSS into the Esbuild

First install the [@stylify/unplugin](/docs/unplugin) package using NPM or Yarn:

```
npm i -D @stylify/unplugin
yarn add -D @stylify/unplugin
```

Next, add create a configuration file `esbuild.config.js`:

```js
const esbuild = require('esbuild');
const { stylifyEsbuild } = require('@stylify/unplugin');

esbuild.build({
	entryPoints: ['./index.js'],
	bundle: true,
	outfile: './output.js',
	plugins: [
		stylifyEsbuild({
			bundles: [{ files: ['./index.html'], outputFile: './index.css' }],
			// Optional
			compiler: {
				// https://stylifycss.com/docs/stylify/compiler#variables
				variables: {},
				// https://stylifycss.com/docs/stylify/compiler#macros
				macros: {},
				// https://stylifycss.com/docs/stylify/compiler#components
				components: {},
				// ...
			}
		})
	]
});
```

Now you can run `node esbuild.config.js` command. This will generate the `output.js` and `index.css` and mangle selectors in files, if configured.

<where-to-next />
