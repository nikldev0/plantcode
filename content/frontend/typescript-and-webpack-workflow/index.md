---
title: "Typescript and Webpack Workflow"
date: 2021-06-01T11:20:43+01:00
draft: false
subtitle: "A simple, framework-free dev environment"
banner: "https://hugo-og-73w37x2fd-nikldev0.vercel.app/TypeScript%20and%20Webpack%20Workflow%20%F0%9F%8C%BF%20plantcode%20.png?theme=light&md=1&fontSize=100px&images=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fhyper-color-logo.svg&images=https%3A%2F%2Fcdn.iconscout.com%2Ficon%2Ffree%2Fpng-512%2Ftypescript-1174965.png&images=data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAAOEAAADhCAMAAAAJbSJIAAAAk1BMVEX%2F%2F%2F%2BO1vsceMCF0%2FvK6%2F0Abbyg3PuX2fvu%2Bf70%2B%2F8Xdr%2Fd5%2FIDdL%2Fh6%2FU2g8UAcL0Aa7vl9f7d8v6v4fyS1%2Fu45PzB6P3T7v3q9%2F6l3vzG6f3Y8P3G2ezt9PqjwuHU4vG60eiErti0zeZpntBPkMubvN5fmM6NtNp6qNVDisgmfcLO3%2B%2FB1utgmc6ev%2BBSkcsAY7hTOIC%2FAAAJ4UlEQVR4nO2d6VbjOBCFyWJCjMGhm7XZaWj2Zt7%2F6cZOQuIqLS6VypbcR%2FfPnJkJsT5HVbqSrdLOTlJSUlJSUlJSUlJSUlJSv7qcrf55PtsL25COdL47WRPuTSYnYdvSheY%2FJ6PRhnA0mvwK2x5xnVZQgHA0Ob4M2yRRnY2WgICwYtw9D9ssMR3NVkSYsGL8OQ%2FbNBHNL755VMKK8TRs6wR0sqXREVbd9yxsAz01HTUBdYQV4%2BwobCM99OMYoBgIK8aLYYbj3gUCMRJWjAN0APMTBcNCWDFOw7bXWb9UBjthFY4%2FwjbZSZc4AAmEdTgOxZBXFltL0EZYh%2BMQUs7SYjMJK8VvyE8tracQxm7Iz0Y2QAph3IZ8a7F9COM15HN1hGcSRmrIdSM8mzBCQz61B6AzYR2OMRlyxWILEMYUjhqLLUIYSzhqLbYQYdX1wxvyX3Q%2BBmF4Q26y2HKEYQ252WJLEoYz5DaLLUtYKUQ4Wi22OGH%2FhrzFYosT9m3IWy12B4R9OgCCxTbo2IuwNwfgMMKj9m3ctMBXdCeaxdaqsUDByMPfjB2vkP9gBqA6pjmPpdtv6jAcz9kBqFu397hbXYUjP3oMPYs35Iy6Ckcniw2ac2z2zhzbsPpScUPuaLEbTUGG6%2Br3dfNfXeZe6ItFDfkRPy3AkNl%2FzIrsHsQkff6sfLeYIfdI7TDtXf%2FO8vF4XBRP8P6xU46QIefHCuxH8%2BesGC%2BVl%2BMHcIkzdgxYYpwqfr5DCfSmWPMtVR5egf%2BrfxpHuYynIWd3IJzP797KfNxUnv3Zb37AI%2BX4OIA5O0LgI6T31wzyLRkXKK1yw93vEfklp5OiSx780fDVKspn8EFWyvYfNZxHepxAXxZ6vvpnLIobcDHq0vL2YhIjv1vvwZH%2FBBKMyli%2BfYDPu81dpN5voOcbfEsf8tL4A25Szust%2BBv66DT5KcNXizZm4AR6ddjKt2L85KQc%2F4Ub2FrCnVUdGoWvFnZyhGnaZAQW3%2BaccNyF39HmH5FLXDs0qrCTa5s8ort5ynrleBf3A1uiww7tPrMmGE1XVZycJTDQhLoa0lgpZ1f9YaaGi%2BKnmTelI99SVCeH%2FOByPYRNOEL2XZsEUEioDo38O2ZfB%2BBqWic30bXIhxDZd2UFCU9x3w8zFt6KETk5NfiRX%2Fr2Iz6Eul5vvqLRoVGlODmQcia7INy3mcGPUMHYjhwODo38M2Int11AQd1pr9GdfAkrgdFxva6Px9xnVoJRGQ1ODo0IIEgFCFECq8YrhkMjM2qcHOouUxSe%2FoRKOE7hd35wE6iJ8ROlVXyDOyC0rTXf0h0aVdjJNXFVeyVEaFprdnRoZMbySXc17dq7FKHW0Ls7NKoUJ1dL7%2BbkCNWFn7%2BLjviWysYw5ZimqpKEOGtLjIFGobHRPHOUJcQvRux%2Fefg0m%2FLsHrTIMkcVJty8cvAt4aHim%2B8PGDB25rbXwzsm3Nl5krEzDT48lwpNuDOXHTGwOY2AsBr1X8XCMV%2B8aEb7EIS3jyBS7salCCCaCF%2B9hSPcX2RwNvfsP%2FhXwzyYWVQzziIgYVYFDLAd3gYOLbrN7xd5HpZQmenc0paBDT9gBlcxbvKinhOHJVQXVx5yblfNXsEzxffVzQpPqC6u8MKxHN81v%2BT6c93hYyA0t44u812Kg7AOR10PowoH4EO%2BHXgiIVSH6Rv7k0PIdwizFXgqHg1h3dH%2BNr%2BgmhrTfsYit444EREq64AHXwTGYgEDUDHxMRHW%2FQ0audaHpPjx6MdY%2BYO4COsmo3C0zauqOdJ788Pap6qxEapLDy%2FGrqp8UrsgEh%2BhMnvd14djjlZETck3QkJ1BUKzzIE%2FcmVcCYmScLlkDb4NZcgq64Kf2ZZ1IyVUR7lmkBUlXKSwjpzREtbhCJzKJlEq6bblvaloCVW3ebd8AJc9ujnYmAlVw%2FJcIttz3f5YPG7Cal71dgeIYABSHhvHTljP3G8Nl6CtBsRPqGSWtW51bw4Pk1CZV9Wir8oNglB9v8LhQccwCNErXU4vhg2FsF7mWBu5udsTjuEQjvPD1Z8dLNz%2BbICEjn%2BWCBNhIkyEiTARJsJEmAgTYSJMhIkwESbCRJgIE%2BG%2FT%2Bi4LMgkLMMROr6gzyPcvLQY5l39%2FUeHPUEcwsbruaF2IxCLX%2FAI8%2Bxr%2B0S8T0K48%2FmhIDI6E6I3jyyVJIT3rilVfVqK0PAINbsPzXXxRPcf6gqIzV8ouxCdCPU7SE0VjwQJTUVEDj7bU44DYaF%2FMG7cgihGaKsc1r7viUyId3IDabeRSu3lhpuAj2ZtRdl4hJrd%2BPDGarYCy%2BzHR1V96u5iL6zHIszLQ01FBRQcym5SCUKlqs%2F6PzuU3iEQGqtioJ25eEu%2BPyHqJ836JvoClzxCW406JUYk62LoqvqYf15jCaw2wnzxYi36hUqsnDX%2Br2d9GlTYRM3XxDJmdkL80rCuwhAC2X7CixD9QvoxF%2FsAbaETKyF6a592nU0NIg9CVNHEXO%2BLUKzGTJiXb9RKXyi1rauc8Gt9zYxVfTTXxgWHcFo1EhYFfO3NXq0NheOyABmb0NTtadfGRaMMhIyKe7DcV9UuHiG%2BV22XXTIqpVnbCPGb0qSqiWpy5xCewIJ7xPqX%2BDCRu8Y%2BJg1hnj3yKl%2Bi%2FHDmW6Tdpeg2dnJ%2FN05OIdQ7NOp1JKvrO1YxVpzcevKICPMS7nR3LfwuV13%2FzAlvdXFtIUxIKFBFWOawC2bBa9SHlqWymoTVFFeiErT%2FYRf8M2VwH%2FqonNyGUC1Wxr2Mfzjyz9lQigrfFOsSHgf%2FvSKHxr2EblnMXR4nyyiFodeE9FqsbRcQOnWGfXYAJU58OoncUSX8aFScHBL%2F6BrhAdHnDAqLZ%2BSfnCGRRbF8osVwLJxP7%2B%2Fk2CfhBk0n4rfMW16dCmV19tE8I%2FO6u4T4J21BJ8c%2FmkdxhOISufc%2BCbSHYxDZR1v5nw4ocnIOQXwT6XfC46jH40i547TnGZZe58m4iue1vM4h7f1YYNMpAt0Q9hSASO7hyCXs43BOrZwPheOe6RzwiHVHQ847l7u3Q3L1cjLkrLPVuz1ZlSIHQ%2B5MGCwAoeiG3JUwZABCUY20G2HoAISiGXIXwn5PGqeIYsjphPh0njjU7gDohB3OcX3UOj8mEnY9x%2FVRiyEnEU4uYgtAKOuTYgJhGIvtJks4Egh7m%2BP6yGzI2wj7neP6yGTI7YT9z3F9pDfkNsIYLLabdIbcTBiJxXaTxpAbCYcTgFCKITcQxmWx3YQMuZYQH4A5NAFDriEcZABCNVfIVcJ45rg%2B2r6ziQmHHIBQ3%2B%2FdQsL45rg%2BmiqEnT3HDaY6HBuEXT7HDaX5xWRDONQRvk1Hu6t%2Fns%2BGZLGTkpKSkpKSkpKSkpKS%2Fg39D3U87OUOi%2F8UAAAAAElFTkSuQmCC"
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

