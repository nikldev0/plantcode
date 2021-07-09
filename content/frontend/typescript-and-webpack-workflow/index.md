---
title: "Typescript and Webpack Workflow"
date: 2021-06-01T11:20:43+01:00
draft: false
subtitle: "A simple, framework-free dev environment"
banner: https://www.plantcode.blog/content/me/banner.png
categories: frontend
tags:
  - typescript
  - webpack
---

Web application development relies on a chain of tools that compile the code and prepare it for the delivery and execution of the application by the JavaScript runtime. The TypeScript compiler is the only development tool in the project at present for a node.js project.

Frameworks like Vue, Angular and React hide these development tools.

Webpack is a bundling tool that helps us with our development workflow. It bundles all of our source files and code into a web optimised output folder ready for distribution.

{{< figure src="bundler.png" caption="webpack bundler" >}}

We add a bundler because web applications don't have direct access to our file system. Instead, files have to be requested over HTTP. A bundler resolves the dependencies during compilation and packages all the files that the application uses into a single file.

One HTTP request delivers all the JavaScript required to run the application.

The first step is to initialise typescript in your project using

`tsc --init`

this creates a tsconfig.json file where you can specify different options about how TypeScript is going to work in your local directory.

Running `npm init` will create a package.json file, allowing you to keep track of all your dependencies.

```npm
npm install --save-dev webpack@5.17.0
npm install --save-dev webpack-cli@4.5.0
npm install --save-dev ts-loader@8.0.14
```
Note we are saving the above packages as dev dependencies. The webpack package contains the main bundler features, and the webpack-cli package adds command-line support. Webpack uses packages known as loaders to deal with different content types, and the ts-loader package adds support for compiling TypeScript files and feeding the compiled code into the bundle created by webpack.

Next task is to create a webpack.config.js file.
