---
slug: 'astro'
section: snippets

order: 1

navigationTitle: "Astro.build"

title: "Stylify CSS snippets for Astro.build"
description: "Stylify CSS configuration snippets for Astro.build integration for rapid web development."
---

## Automated CSS bundles

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/OYJn23w8fqI" loading="lazy" fetchpriority="low" class="width:100%" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The snippet belows allows you to automatically split bundles for each `page` and `layout`.

It works in the following steps:
1. It finds all pages in `src/pages` and all layouts in `src/layouts` and calls the `createBundles` to create bundles configuration for us with the correct layer name and output file.
2. The Stylify integration is initialized and CSS layers order is configured so it will generate the order only into a file, that has the `layout` CSS layer name.
3. <nuxt-link to="/docs/bundler#hooks">bundler:fileToProcessOpened</nuxt-link> hook is added. This hook has two parts. One part is done, when this file is a layout or a page and the another for every opened file.
 - When a layout or a page file is opened, it checks, whether it contains a path to CSS file and if not, it adds it to the head of the file.
 - For all other files, it tries to check for `imports`. If any component import is found, it maps it as a dependency. This way it can map a whole components dependency tree automatically so you just keep adding/removing them and the CSS is generated correctly
4. When Bundler is initialized we can start watching for newly added files within the `layout` and `pages` directory. Every time a new file is added, we create a new Bundle.
5. The Stylify Integration is added to the Astro config.

You can also try this [example on Stack Blitz](https://stackblitz.com/edit/stylify-astro-automated-bundles-example?file=src%2Fpages%2Findex.astro).

```js
import stylify from '@stylify/astro';
import { hooks } from '@stylify/bundler';
import fastGlob from 'fast-glob';
import path from 'path';
import fs from 'fs';
import { fileURLToPath } from 'url';

const pagesDir = 'src/pages';
const layoutsDir = 'src/layouts';
const stylesDir = 'src/styles';

/** @type { import('@stylify/bundler').BundlerConfigInterface[]} */
const stylifyBundles = [];
const layoutCssLayerName = 'layout';
const pageCssLayerName = 'page';

const getFileCssLayerName = (filePath) =>
  filePath.includes('/pages/') ? pageCssLayerName : layoutCssLayerName;

const getOutputFileName = (file) => {
  const parsedFile = path.parse(file);
  const fileName = parsedFile.name.toLowerCase();
  const dirNameCleanupRegExp = new RegExp(`${pagesDir}|${layoutsDir}|\\W`, 'g');
  console.log(parsedFile.dir);
  const dir = parsedFile.dir.replace(dirNameCleanupRegExp, '');
  console.log(dir);
  return `${dir.length ? `${dir}-` : ''}${fileName}.css`;
};

const createBundle = (file) => {
  const fileCssLayerName = getFileCssLayerName(file);

  return {
    outputFile: `${stylesDir}/${fileCssLayerName}/${getOutputFileName(file)}`,
    files: [file],
    cssLayer: fileCssLayerName,
  };
};

const createBundles = (files) => {
  for (const file of files) {
    stylifyBundles.push(createBundle(file));
  }
};

// 1. Map files in layouts/pages and create bundles
createBundles(fastGlob.sync(`${pagesDir}/**/*.astro`));
createBundles(fastGlob.sync(`${layoutsDir}/**/*.astro`));

// 2. Init Stylify Astro Integraton
const stylifyIntegration = stylify({
  bundler: {
    id: 'astro',
    // Set CSS @layers order
    cssLayersOrder: {
      // Order will be @layer layout,page;
      order: [layoutCssLayerName, pageCssLayerName].join(','),
      // Layers order will be exported into file with layout @layer
      exportLayer: [layoutCssLayerName],
    },
  },
  bundles: stylifyBundles,
});

// 3. Add hook that processes opened files
/** @param { import('@stylify/bundler').BundleFileDataInterface } data */
hooks.addListener('bundler:fileToProcessOpened', (data) => {
  let { content, filePath } = data;

  // 3.1 Only for layout and page files
  if (filePath.includes('/pages/') || filePath.includes('/layouts/')) {
    const cssFileName = path.parse(filePath).name;
    const cssFilePathImport = `import '/${stylesDir}/${getFileCssLayerName(
      filePath
    )}/${getOutputFileName(filePath)}';`;

    if (!content.includes(cssFilePathImport)) {
      if (/import \S+ from (?:'|")\S+(\/layouts\/\S+)(?:'|");/.test(content)) {
        content = content.replace(
          /import \S+ from (?:'|")\S+\/layouts\/\S+(?:'|");/,
          `$&\n${cssFilePathImport}`
        );
      } else if (/^\s*---\n/.test(content)) {
        content = content.replace(/^(\s*)---\n/, `$&${cssFilePathImport}\n`);
      } else {
        content = `---\n${cssFilePathImport}\n---\n${content}`;
      }

      fs.writeFileSync(filePath, content);
    }
  }

  // 3.2 For all files
  const regExp = new RegExp(
    `import \\S+ from (?:'|")\\S+(\\/components\\/\\S+)(?:'|");`,
    'g'
  );
  let importedComponent;
  const importedComponentFiles = [];
  const rootDir = path.dirname(fileURLToPath(import.meta.url));

  while ((importedComponent = regExp.exec(content))) {
    importedComponentFiles.push(
      path.join(rootDir, 'src', importedComponent[1])
    );
  }

  data.contentOptions.files = importedComponentFiles;
});

// 4. Wait for bundler to initialize and watch for directories
// to create new bundles when a file is added
hooks.addListener('bundler:initialized', ({ bundler }) => {
  // Watch layouts and pages directories
  // If you plan to use nested directories like blog/_slug.astro
  // for which you want to automatize bundles configuration
  // You will need to add the path to them here
  const dirsToWatchForNewBundles = [layoutsDir, pagesDir];
  for (const dir of dirsToWatchForNewBundles) {
    fs.watch(dir, (eventType, fileName) => {
      const fileFullPath = path.join(dir, fileName);

      if (eventType !== 'rename' || !fs.existsSync(fileFullPath)) {
        return;
      }

      bundler.bundle([createBundle(fileFullPath)]);
    });
  }
});

export default {
  // 5. Add Stylify Astro Integration
  integrations: [stylifyIntegration]
};
```
