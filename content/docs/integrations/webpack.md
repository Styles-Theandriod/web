---
section: integrations

order: 1

navigationTitle: "Webpack"
navigationIconPath: '/images/brands/webpack-icon.svg'
image: '/integrations/webpack/header.jpg'
ogImage: '/integrations/webpack/og-image.jpg'

title: Using Stylify CSS with Webpack
description: "Learn how to use the Stylify CSS with the Webpack. Code your website faster with Stylify CSS and Webpack."
howToSchemaTitle: 'How to use Stylify CSS in Webpack.'
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

Webpack is a module bundler. Its main purpose is to bundle JavaScript files for usage in a browser, yet it is also capable of transforming, bundling and etc.

<note><template>
Integration example for the Webpack can be found in <a href="https://github.com/stylify/integrations-examples/tree/master/webpack" target="_blank" rel="noopener">integrations examples repository</a>.
</template></note>

## How to integrate the Stylify CSS into the Webpack

First install the [@stylify/unplugin](/docs/unplugin) package using NPM or Yarn:

```
npm i -D @stylify/unplugin
yarn add -D @stylify/unplugin
```

Next, add the following configuration into the `webpack.config.js` file:

```js
const path = require('path');
const { stylifyWebpack } = require('@stylify/unplugin');

const mode = 'development';
const stylifyPlugin = stylifyWebpack({
	bundles: [{
		outputFile: './index.css',
		files: ['./**/*.html'],
		rewriteSelectorsInFiles: mode === 'production'
	}],
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
});

module.exports = {
	entry: './input.js',
	mode: mode,
	plugins: [ stylifyPlugin ],
	module: {
		rules: [{
			test: /\.css$/i,
			use: ["style-loader", "css-loader", "postcss-loader"]
		}],
	},
	output: {
		path: path.resolve(__dirname),
		filename: 'index.js',
		libraryTarget: 'umd'
	}
};
```

Now add the generated `index.css` file into the `index.js` entry file.

<where-to-next />
