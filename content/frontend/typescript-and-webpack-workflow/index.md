---
title: "TypeScript and Webpack Workflow"
date: 2021-07-06T11:20:43+01:00
draft: false
subtitle: "A simple, framework-free dev environment"
banner: https://www.plantcode.blog/me/banner.jpg
categories: frontend
tags:
  - TypeScript
  - Webpack
---

Web application development relies on a chain of tools that compile the code and prepare it for the delivery and execution of the application by the JavaScript runtime.

Frameworks like Vue, Angular and React hide these development tools, but if you're looking to experiment with TypeScript outside of one you may be interested in configuring a stand-alone TypeScript application using [Webpack](https://webpack.js.org/guides/typescript/).

Webpack is a bundling tool that helps us with our development workflow. It bundles all of our source files and code into a web optimised output folder ready for distribution.

{{< figure src="bundler.png" caption="webpack bundler" >}}

We add a bundler because web applications don't have direct access to our file system. Instead, files have to be requested over HTTP. A bundler resolves the dependencies during compilation and packages all the files that the application uses into a single file.

In other words, with Webpack we can have one HTTP request deliver all the JavaScript required to run the application.

The first step is to create a suitable directory for your webpack project. I've just named mine 'webpack'. Once created we can tell the Node Package Manager (npm) to create a package.json file which will help configure our dependencies.

{{< highlight html "linenos=tables,linenostart=1" >}}
cd webapp
npm init --yes
{{< / highlight >}}

Note I've added the `--yes` flag to my [npm init](https://docs.npmjs.com/cli/v7/commands/npm-init) command to skip the questionnaire because I'm happy with defaults.

If you don't already have the TypeScript compiler installed globally, you can fetch it as a local dev dependency.

{{< highlight html "linenos=tables,linenostart=1" >}}
 npm install --save-dev typescript@4.2.2
{{< / highlight >}}

Then run:

{{< highlight html "linenos=tables,linenostart=1" >}}
 tsc --init
{{< / highlight >}}

This creates a tsconfig.json file where you can specify different options about how TypeScript is going to work in our local directory. For example, this my configuration:

{{< highlight json "linenos=tables,linenostart=1" >}}
 {
    "compilerOptions": {
    "target": "es2020",
    "outDir": "./dist",
    "rootDir": "./src"
    }
 };
{{< / highlight >}}

Next we'll look at the specific webpack dependencies we would need to install.

{{< highlight html "linenos=tables,linenostart=1" >}}
npm install --save-dev webpack@5.17.0
npm install --save-dev webpack-cli@4.5.0
npm install --save-dev ts-loader@8.0.14
{{< / highlight >}}

Note we are saving the above packages as [dev dependencies](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file). The webpack package contains the main bundler features, and the [webpack-cli](https://webpack.js.org/api/cli/) package adds command-line support which make working with webpack easier. Webpack uses packages known as loaders to deal with different content types and preprocess files, and the [ts-loader package](https://github.com/TypeStrong/ts-loader) adds support for compiling TypeScript files.

Next task is to create a webpack.config.js file.
