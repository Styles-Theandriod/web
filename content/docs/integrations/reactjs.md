---
section: integrations

order: 1

navigationTitle: "React.js"
navigationIconPath: '/images/brands/react.png'
image: '/integrations/reactjs/header.jpg'
ogImage: '/integrations/reactjs/og-image.jpg'

title: Using Stylify CSS in React.js
description: "Learn how to use the Stylify CSS with the React.js. Code your React.js website faster with Stylify CSS."
howToSchemaTitle: 'How to use Stylify CSS in React.js.'
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

React.js is a JavaScript library for building user interfaces.

Stylify can be used with React.js in varous ways:
- With Vite.js (this example)
- [Next.js](/docs/integrations/nextjs)

<stack-blitz-link link="stylify-react-vite"></stack-blitz-link>

## How to integrate the Stylify CSS with React.js and Vite.js

The example below works with the Vite - React.js template. You can however use the example below and configure it for Svelte, Vue and any other framework you use.

First install the [@stylify/unplugin](/docs/unplugin) package using NPM or Yarn:

```
npm i -D @stylify/unplugin
yarn add -D @stylify/unplugin
```

Next add the following configuration into the `vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { stylifyVite } from '@stylify/unplugin';

const stylifyPlugin = stylifyVite({
    bundles: [{outputFile: './src/stylify.css', files: ['./src/**'] }],
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

export default defineConfig({
	plugins: [stylifyPlugin, react()]
});
```

Now you can add the path to the generated `src/stylify.css` into `src/main.js` file:

```js
import './stylify.css';
```

Now run `yarn dev`. The `src/stylify.css` file will be generated.

## Configuration
There is a lot of options you can configure:
- [@stylify/unplugin](/docs/unplugin)
- [Compiler config](/docs/stylify/compiler)

<where-to-next />
