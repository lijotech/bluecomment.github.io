---
layout: post
title: "Purpose of Webpack in Angular Application"
date: 2020-07-04 15:37:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["webpack", "angular webpack", "purpose of webpack", "hot module reloading", "hot module replacement", "HMR"]
permalink: /post/purpose-of-webpack-in-angular-application
---

If you are new to angular please refer to this article to know how to [set up your first project in angular](/post/how-to-create-first-angular-project-using-command-line-interface "set up your first project in angular").

Webpack is a build automation tool used by angular to bundle the script files and style sheets and minifies them at runtime.  
When we run the _`ng serve`r_ command to run the angular application we can see a set of files compiled and minified. This bundling process is done using Webpack.

![](/assets/img/posts/2020/07/webpack-compiling.jpg)

when we make any change in Html, style sheet or script files web pack will automatically rebuild the full application and reloads the web page.

Webpack also injects the compiled files reference to the index.html dynamically.  
So a refresh is not required on the page to see new changes and this feature is called **Hot Module reloading or Hot Module Replacement (HMR)**.

**Here are some key points**

1. **Bundling**: Webpack bundles your application source code into convenient chunks, which can be loaded from a server into a browser. This helps in managing dependencies and improving load times.

2. **Loaders**: These are transformations applied to the source code of your modules. For example, you can use loaders to preprocess TypeScript, SASS, or LESS files before bundling.

3. **Plugins**: Webpack plugins extend its capabilities. Common plugins include `HtmlWebpackPlugin` for generating HTML files and `CommonsChunkPlugin` for splitting bundles.

4. **Configuration**: Webpack is configured using a JavaScript file (`webpack.config.js`). This file defines entry points, output paths, loaders, and plugins.

5. **Development and Production Modes**: Webpack can be configured differently for development (with features like hot module replacement) and production (with optimizations like minification and tree shaking).
