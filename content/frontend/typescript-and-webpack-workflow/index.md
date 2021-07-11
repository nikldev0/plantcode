---
title: "TypeScript and Webpack Workflow"
date: 2021-07-06T11:20:43+01:00
draft: false
subtitle: "A simple, framework-free dev environment"
description: "Configure a stand-alone TypeScript application using Webpack for a simple dev learning environment"
banner: https://plantcode.blog/frontend/typescript-and-webpack-workflow\typescript-and-webpack-workflow.jpg
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

Go ahead and create an `src` folder within your project directory, and then an index.ts file:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
 console.log("Hello, World")
{{< / highlight >}}

You can compile and execute this result like so:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
tsc
node dist/index.js
{{< / highlight >}}

This will compile your TypeScript file into JavaScript that is readable by your terminal. Here you can [learn more about how the TypeScript compiler works](../understanding-the-typescript-compiler) and how to compile and execute your TypeScript faster using nodemon.

Next we'll look at the specific webpack dependencies we would need to install.

{{< highlight html "linenos=tables,linenostart=1" >}}
npm install --save-dev webpack@5.17.0
npm install --save-dev webpack-cli@4.5.0
npm install --save-dev ts-loader@8.0.14
{{< / highlight >}}

Note we are saving the above packages as [dev dependencies](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file). You may need to spend some time looking at package versions and cross-compatability if you're following along. The webpack package contains the main bundler features, and the [webpack-cli](https://webpack.js.org/api/cli/) package adds command-line support which make working with webpack easier. Webpack uses packages known as loaders to deal with different content types and preprocess files, and the [ts-loader package](https://github.com/TypeStrong/ts-loader) adds support for compiling TypeScript files.

Next task is to add a webpack.config.js file to your root folder. Once created, implement the following configuration:

{{< highlight json "linenos=tables,linenostart=1" >}}
 module.exports = {
    mode: "development",
    devtool: "inline-source-map",
    entry: "./src/index.ts",
    output: { filename: "bundle.js" },
    resolve: { extensions: [".ts", ".js"] },
    module: {
        rules: [
            { test: /\.ts/, use: "ts-loader", exclude: /node_modules/ }
        ]
    }
 };
{{< / highlight >}}

To run through what this is doing, [mode](https://webpack.js.org/configuration/mode/) 'tells webpack to use its built-in optimizations accordingly' i.e. lets webpack know what kind of environment this project is concerned with.

[Devtool](https://webpack.js.org/configuration/devtool/) option controls if and how [source maps](http://blog.teamtreehouse.com/introduction-source-maps) are generated. Here we are doing it inline.

[Entry](https://webpack.js.org/configuration/entry-context/) is simply where webpack looks to start the bundler process.

Likewise, [output](https://webpack.js.org/configuration/output/) tells webpack where it should output whatever we are bundling.

[resolve.extensions](https://webpack.js.org/configuration/output/) refers to the order in which we are resolving files with the specified extensions based on the module rule. Here, the rule configures webpack to use the ts-loader package to process files with the `.ts` file extension.

Next you can run

{{< highlight typescript "linenos=tables,linenostart=1" >}}
 npx webpack
{{< / highlight >}}

to allow webpack to work its way through the dependencies and use ts-loader to compile TypeScript files.

You'll get an executable bundle file upon running:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
 node dist/bundle.js
{{< / highlight >}}

And should now see our `"Hello, World"` flash in the terminal! 🥳

Just a bundle file itself isn't enough though, we need a web server to deliver the bundle file to the browser so it can be executed. Luckily, there's a package for this called the [Webpack Dev Server(WDS)](https://github.com/webpack/webpack-dev-server).

{{< highlight html "linenos=tables,linenostart=1" >}}
 npm install --save-dev webpack-dev-server@3.11.2
{{< / highlight >}}

We then need to change our initial webpack configuration to set up a port for WDS to work on.

{{< highlight json "linenos=tables, hl_lines=12-15,linenostart=1" >}}
 module.exports = {
    mode: "development",
    devtool: "inline-source-map",
    entry: "./src/index.ts",
    output: { filename: "bundle.js" },
    resolve: { extensions: [".ts", ".js"] },
    module: {
        rules: [
            { test: /\.ts/, use: "ts-loader", exclude: /node_modules/ }
        ]
    },
    devServer: {
        contentBase: "./assets",
        port: 4500
    }
 };
{{< / highlight >}}

This is telling WDS to listen for HTTP requests on port 4500. Now we just need an HTML file that can be used to respond to browsers. I've created a HTML boilerplate file called index.html in a folder I created within the project directory called assets:

{{< highlight html "linenos=tables, linenostart=1" >}}
<!-- assets/index.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Webapp</title>
    <div id="app"></div>
</head>

<body>
    <h1>This is my TypeScript + Webpack app.</h1>
</body>

</html>

{{< / highlight >}}

We can now run...

{{< highlight typescript "linenos=tables,linenostart=1" >}}
 npx webpack serve
{{< / highlight >}}

and get our webapp live! The contents of our index.html file are displayed on the browser. You can make changes to the HTML file and view them upon refresh. However, the browser is reloaded automatically when we make changes to the index.ts file as we've specified that as our entry point.

{{< figure src="webapp.png" caption="Our running webapp on localhost:4500!" >}}
