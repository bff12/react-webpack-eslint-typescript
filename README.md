
# Table of Contents

1.  [React and TypeScript Essential packages](#org50fcb1e)
2.  [Config Typescript tsconfig.json(root path)](#org338cf2b)
3.  [Root React Component(src folder)](#org4787dd9)
4.  [Root index HTML(src folder)](#org5a87c48)
5.  [Adding Babel Packages](#org54ea4e0)
6.  [Babel Config (.babelrc at root path)](#orgfedf5ce)
7.  [Adding linting](#orgb6a9268)
8.  [ESLint configuration file (.eslintrc.json at root path)](#org0954fd5)
9.  [Webpack Package](#orgbb7ee0d)
10. [Package to transpile react and typescript code into JavaScript](#org2ba3703)
11. [Package to generate the HTML](#orgbae4916)
12. [Webpack Configuration](#org8ba7fe2)
    1.  [Support images and fonts](#org2d42d91)
        1.  [Configuration to support images and fonts with url loader](#orgdceb122)
        2.  [Asset Modules](#orgf2aa077)
    2.  [Webpack Development configuration](#orgb0e3fcb)
        1.  [Config package.json to run in dev mode](#org3ec5959)
        2.  [Add type checking package to webpack](#org2b0baf0)
        3.  [Add type checking to webpack config `webpack.dev.config.ts`](#org1dae882)
        4.  [Add linting into the webpack process](#org53d76f2)
        5.  [Add ESLint config to webpack.dev.config.ts](#org7b60c23)
    3.  [Webpack Production configuration](#orgdf787c8)
        1.  [Configuring production (webpack.prod.config.ts)](#org8f1c701)
        2.  [NPM script to build the app for production](#org8fd06f5)
        3.  [Build Process](#org2ab7843)
    4.  [Setting up CSS with webpack](#org6fa0724)
        1.  [Add the configuration to webpack](#org727b278)
    5.  [Setting up CSS with webpack (Production)](#org7f9d699)
        1.  [Configuration to production mode](#orge9aad8a)
    6.  [Setting up Sass with webpack](#orgd0b1103)
        1.  [Sass configuration](#org32d2f02)
        2.  [Sass configuration for production environment](#org9e7d5ec)


<a id="org50fcb1e"></a>

# React and TypeScript Essential packages

    npm install react react-dom
    npm install --save-dev typescript
    npm install --save-dev @types/react @types/react-dom


<a id="org338cf2b"></a>

# Config Typescript tsconfig.json(root path)

    {
      "compilerOptions": {
        "lib": ["dom", "dom.iterable", "esnext"],
        "allowJs": true,:
        "allowSyntheticDefaultImports": true,
        "skipLibCheck": true,
        "esModuleInterop": true,
        "strict": true,
        "forceConsistentCasingInFileNames": true,
        "moduleResolution": "node",
        "resolveJsonModule": true,
        "isolatedModules": true,
        "noEmit": true,
        "jsx": "react"
      },
      "include": ["src"]
    }

TypeScript is only used for type checking. Babel transpile the code.

-   **lib**: The standard typing to be included in the type checking process. In our case, we have chosen to use the types for the browser’s DOM and the latest version of ECMAScript.
-   **allowJs**: Whether to allow JavaScript files to be compiled.
-   **allowSyntheticDefaultImports**: This allows default imports from modules with no default export in the type checking process.
-   **skipLibCheck**: Whether to skip type checking of all the type declaration files (\*.d.ts).
-   **esModuleInterop**: This enables compatibility with Babel.
-   **strict**: This sets the level of type checking to very high. When this is true, the project is said to be running in strict mode.
-   **forceConsistentCasingInFileNames**: Ensures that the casing of referenced file names is consistent during the type checking process.
-   **moduleResolution**: How module dependencies get resolved, which is node for our project.
-   **resolveJsonModule**: This allows modules to be in .json files which are useful for configuration files.
-   **noEmit**: Whether to suppress TypeScript generating code during the compilation process. This is true in our project because Babel will be generating the JavaScript code.
-   **jsx**: Whether to support JSX in .tsx files.
-   **include**: These are the files and folders for TypeScript to check. In our project, we have specified all the files in the src folder.


<a id="org4787dd9"></a>

# Root React Component(src folder)

    import React from "react";
    import ReactDOM from "react-dom";
    
    const App = () => (
      <h1>My React and TypeScript App!</h1>
    );
    
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );

`index.tsx`


<a id="org5a87c48"></a>

# Root index HTML(src folder)

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8" />
        <title>My app</title>
      </head>
      <body>
        <div id="root"></div>
      </body>
    </html>

`index.html`

Component at index.tsx eventually will be displayed in index.html.


<a id="org54ea4e0"></a>

# Adding Babel Packages

    npm install --save-dev @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime @babel/runtime

-   **@babel/core**: As the name suggests, this is the core Babel library.
-   **@babel/preset-env**: This is a collection of plugins that allow us to use the latest JavaScript features but still target browsers that don’t support them.
-   **@babel/preset-react**: This is a collection of plugins that enable Babel to transform React code into JavaScript.
-   **@babel/preset-typescript**: This is a plugin that enables Babel to transform TypeScript code into JavaScript.
-   **@babel/plugin-transform-runtime** and **@babel/runtime**: These are plugins that allow us to use the async and await JavaScript features.


<a id="orgfedf5ce"></a>

# Babel Config (.babelrc at root path)

    {
      "presets": [
        "@babel/preset-env",
        "@babel/preset-react",
        "@babel/preset-typescript"
      ],
      "plugins": [
        [
          "@babel/plugin-transform-runtime",
          {
            "regenerator": true
          }
        ]
      ]
    }

This configuration tells Babel to use the plugins we have installed.


<a id="orgb6a9268"></a>

# Adding linting

    npm install --save-dev eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/parser @typescript-eslint/eslint-plugin

-   **eslint**: This is the core ESLint library.
-   **eslint-plugin-react**: This contains some standard linting rules for React code.
-   **eslint-plugin-react-hooks**: This includes some linting rules for React hooks code.
-   **@typescript-eslint/parser**: This allows TypeScript code to be linted.
-   **@typescript-eslint/eslint-plugin**: This contains some standard linting rules for TypeScript code.

ESLint can be configured in a .eslintrc.json file in the project root


<a id="org0954fd5"></a>

# ESLint configuration file (.eslintrc.json at root path)

    {
      "parser": "@typescript-eslint/parser",
      "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
      },
      "plugins": [
        "@typescript-eslint",
        "react-hooks"
      ],
      "extends": [
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended"
      ],
      "rules": {
        "react-hooks/rules-of-hooks": "error",
        "react-hooks/exhaustive-deps": "warn",
        "react/prop-types": "off"
      },
      "settings": {
        "react": {
          "pragma": "React",
          "version": "detect"
        }
      }
    }

We have configured ESLint to use the TypeScript parser, and the standard React and TypeScript rules as a base set of rules. We’ve explicitly added the two React hooks rules and suppressed the react/prop-types rule because prop types aren’t relevant in React with TypeScript projects. We have also told ESLint to detect the version of React we are using.


<a id="orgbb7ee0d"></a>

# Webpack Package

    npm install --save-dev webpack webpack-cli
    npm install --save-dev webpack-dev-server @types/webpack-dev-server

TypeScript types are included in the webpack package. the second package is the webpack-dev-server that we will use during development.


<a id="org2ba3703"></a>

# Package to transpile react and typescript code into JavaScript

    npm install --save-dev babel-loader


<a id="orgbae4916"></a>

# Package to generate the HTML

    npm install --save-dev html-webpack-plugin


<a id="org8ba7fe2"></a>

# Webpack Configuration

The webpack configuration file is JavaScript based as standard. However, we can use TypeScript if we install a package called ts-node.

    npm install --save-dev ts-node


<a id="org2d42d91"></a>

## Support images and fonts

    npm i --save-dev url-loader

-   **url-loader**: loader for webpack which transforms files into base64 URIs.


<a id="orgdceb122"></a>

### Configuration to support images and fonts with url loader

    rules: [
              ....
              {
                    test: /\.(woff|woff2|eot|ttf|svg|jpg|png)$/,
                    use: {
                      loader: 'url-loader',
                    },
              },
              ....
              ]

Working Example: 

      @font-face {
        font-family: 'Open Sans Italic';
        src: url('./Assets/Fonts/OpenSansItalic.eot');
        src: url('./Assets/Fonts/OpenSansItalic.eot?#iefix') format('embedded-opentype'),
             url('./Assets/Fonts/OpenSansItalic.woff') format('woff'),
             url('./Assets/Fonts/OpenSansItalic.ttf') format('truetype'),
             url('./Assets/Fonts/OpenSansItalic.svg#OpenSansItalic') format('svg');
        font-weight: normal;
        font-style: italic;
    }
    
    @import url(./font.scss);
    import img from './image.png';


<a id="orgf2aa077"></a>

### Asset Modules

Prior to webpack 5 it was common to use:

-   **raw-loader**: to import a file as a string
-   **url-loader**: to inline a file into the bundle as a data URI
-   **file-loader**: to emit a file into the output directory

Asset Modules type replaces all of these loaders by adding 4 new module types:

-   **asset/resource**: emits a separate file and exports the URL. Previously achievable by using file-loader.
-   **asset/inline**: exports a data URI of the asset. Previously achievable by using url-loader.
-   **asset/source**: exports the source code of the asset. Previously achievable by using raw-loader.
-   **asset**: automatically chooses between exporting a data URI and emitting a separate file. Previously achievable by using url-loader with asset size limit.

    rules: [
        {
          test: /\.png/,
          type: 'asset/resource'
        }
      ]


<a id="orgb0e3fcb"></a>

## Webpack Development configuration

    import path from "path";
    import { Configuration, HotModuleReplacementPlugin } from "webpack";
    import HtmlWebpackPlugin from "html-webpack-plugin";
    
    const config: Configuration = {
      mode: "development",
      output: {
        publicPath: "/",
      },
      entry: "./src/index.tsx",
      module: {
        rules: [
          {
            test: /\.(ts|js)x?$/i,
            exclude: /node_modules/,
            use: {
              loader: "babel-loader",
              options: {
                presets: [
                  "@babel/preset-env",
                  "@babel/preset-react",
                  "@babel/preset-typescript",
                ],
              },
            },
          },
        ],
      },
      resolve: {
        extensions: [".tsx", ".ts", ".js"],
      },
      plugins: [
        new HtmlWebpackPlugin({
          template: "src/index.html",
        }),
        new HotModuleReplacementPlugin(),
      ],
      devtool: "inline-source-map",
      devServer: {
        static: path.join(__dirname, "build"),
        historyApiFallback: true,
        port: 4000,
        open: true,
        hot: true
      },
    };
    
    export default config;

-   **mode**: tells Webpack whether the app needs to be bundled for production or development. We are configuring Webpack for development, so we have set this to "development". Webpack will automatically set process.env.NODE<sub>ENV</sub> to "development" which means we get the React development tools included in the bundle.
-   **output.public**: tells Webpack what the root path is in the app. This is important for deep linking in the dev server to work properly.
-   **field**: tells Webpack where to start looking for modules to bundle. In our project, this is index.tsx.
-   **module**: tells Webpack how different modules will be treated. Our project is telling Webpack to use the babel-loader plugin to process files with .js, .ts, and .tsx extensions.
-   **resolve.extensions**: tells Webpack what file types to look for in which order during module resolution. We need to tell it to look for TypeScript files as well as JavaScript files.
-   **HtmlWebpackPlugin**: creates the HTML file. We have told this to use our index.html in the src folder as the template.
-   **HotModuleReplacementPlugin** and **devServer.hot**: allow modules to be updated while an application is running, without a full reload.
-   **devtool**: tells Webpack to use full inline source maps. This allows us to debug the original code before transpilation.
-   **devServer**: configures the Webpack development server. We tell Webpack that the root of the webserver is the build folder, and to serve files on port 4000. historyApiFallback is necessary for deep links to work in multi-page apps. We are also telling Webpack to open the browser after the server has been started.


<a id="org3ec5959"></a>

### Config package.json to run in dev mode

    "scripts": {
        "start": "webpack serve --config webpack.dev.config.ts",
    }

The script starts the Webpack development server. We have used the config option to reference the development configuration file we have just created. Notice that Webpack hasn’t bundled any files in the build folder. This is because the files are in memory in the Webpack dev server.


<a id="org2b0baf0"></a>

### Add type checking package to webpack

    npm install --save-dev fork-ts-checker-webpack-plugin @types/fork-ts-checker-webpack-plugin


<a id="org1dae882"></a>

### Add type checking to webpack config `webpack.dev.config.ts`

    ...
    import ForkTsCheckerWebpackPlugin from 'fork-ts-checker-webpack-plugin';
    
    const config: Configuration = {
      ...,
      plugins: [
        ...,
        new ForkTsCheckerWebpackPlugin({
          async: false
        }),
      ],
    };

We have used the async flag to tell Webpack to wait for the type checking process to finish before it emits any code.


<a id="org53d76f2"></a>

### Add linting into the webpack process

    npm install --save-dev eslint-webpack-plugin


<a id="org7b60c23"></a>

### Add ESLint config to webpack.dev.config.ts

    ...
    import ESLintPlugin from "eslint-webpack-plugin";
    
    const config: Configuration = {
      ...,
      plugins: [
        ...,
        new ESLintPlugin({
          extensions: ["js", "jsx", "ts", "tsx"],
        }),
      ],
    };

We have used the extensions setting to tell the plugin to lint TypeScript files as well as JavaScript files.


<a id="orgdf787c8"></a>

## Webpack Production configuration


<a id="org8f1c701"></a>

### Configuring production (webpack.prod.config.ts)

    import path from "path";
    import { Configuration } from "webpack";
    import HtmlWebpackPlugin from "html-webpack-plugin";
    import ForkTsCheckerWebpackPlugin from "fork-ts-checker-webpack-plugin";
    import ESLintPlugin from "eslint-webpack-plugin";
    import { CleanWebpackPlugin } from "clean-webpack-plugin";
    
    const config: Configuration = {
      mode: "production",
      entry: "./src/index.tsx",
      output: {
        path: path.resolve(__dirname, "build"),
        filename: "[name].[contenthash].js",
        publicPath: "",
      },
      module: {
        rules: [
          {
            test: /\.(ts|js)x?$/i,
            exclude: /node_modules/,
            use: {
              loader: "babel-loader",
              options: {
                presets: [
                  "@babel/preset-env",
                  "@babel/preset-react",
                  "@babel/preset-typescript",
                ],
              },
            },
          },
        ],
      },
      resolve: {
        extensions: [".tsx", ".ts", ".js"],
      },
      plugins: [
        new HtmlWebpackPlugin({
          template: "src/index.html",
        }),
        new ForkTsCheckerWebpackPlugin({
          async: false,
        }),
        new ESLintPlugin({
          extensions: ["js", "jsx", "ts", "tsx"],
        }),
        new CleanWebpackPlugin(),
      ],
    };
    
    export default config

The Webpack configuration for production is a little different - we want files to be bundled in the file system optimized for production without any dev stuff.

This is similar to the development configuration with the following differences:

-   **mode**: production -  Webpack will automatically set process.env.NODE<sub>ENV</sub> to "production" which means we won’t get the React development tools included in the bundle.
-   **output**: tells Webpack where to bundle our code. In our project, this is the build folder. We have used the [name] token to allow Webpack to name the files if our app is code split. We have used the [contenthash] token so that the bundle file name changes when its content changes, which will bust the browser cache.
-   **CleanWebpackPlugin**: plugin will clear out the build folder at the start of the bundling process.

    npm install --save-dev clean-webpack-plugin


<a id="org8fd06f5"></a>

### NPM script to build the app for production

    ...,
      "scripts": {
        ...,
        "build": "webpack --config webpack.prod.config.ts",
      },
      ...


<a id="org2ab7843"></a>

### Build Process

    npm run build

After a few seconds, the Webpack will place the bundled files in the build folder.

If we look at the JavaScript file, we will see it is minified. Webpack uses its `TerserWebpackPlugin` out of the box in production mode to minify code. The JavaScript bundle contains all the code from our app as well as the code from the react and react-dom packages.

If we look at the html file, we will see all the spaces have been removed. If we look closely, we will see a script element referencing our JavaScript file which the `HtmlWebpackPlugin` did for us.


<a id="org6fa0724"></a>

## Setting up CSS with webpack

Webpack by default only understands javascript and in order to make webpack understand other resources like .css, .scss, etc we need the help of loaders to compile it. Let see how to do it.

    npm i --save-dev css-loader style-loader

-   **css-loader**: interprets @import and url() like import/require(). css-loader will take all the CSS from the CSS file and generate it to a single string and will pass this to style-loader.
-   **style-loader**: will take this string and will embed it in the style tag in index.html


<a id="org727b278"></a>

### Add the configuration to webpack

    ...
       module: {
            rules: [
                {
                    test: /\.(css)$/,
                    use: ['style-loader','css-loader']
                }
            ]
        }
    ...

-   **test** will tell the webpack to **use** style-loader and css-loader for all the .css files


<a id="org7f9d699"></a>

## Setting up CSS with webpack (Production)

    npm i --save-dev mini-css-extract-plugin


<a id="orge9aad8a"></a>

### Configuration to production mode

    module: {
            rules: [
              {
                 test: /\.(css)$/,
                 use: [MiniCssExtractPlugin.loader,'css-loader']
              }
            ]
    	},
    plugins: [new MiniCssExtractPlugin()],

-   **MiniCssExtractPlugin** extracts CSS and create a CSS file per JS file


<a id="orgd0b1103"></a>

## Setting up Sass with webpack

    npm i --save-dev sass-loader node-sass

-   **node-sass**: provides binding Node.js to LibSass (The C version of Sass)
-   **sass-loader**: loads a Sass/SCSS file and compiles it to CSS. (This Loader has internal dependencies which require node-sass)


<a id="org32d2f02"></a>

### Sass configuration

    {
        rules: [
          {
    	test: /\.(s(a|c)ss)$/,
    	use: ['style-loader','css-loader', 'sass-loader'] 
          {	
        ]
    }

We just need to add the sass-loader ahead of css-loader, so now first, the .scss file compiles back to CSS and after that process remains the same as explained above.


<a id="org9e7d5ec"></a>

### Sass configuration for production environment

    {
        rules: [
              {
                 test: /\.(s(a|c)ss)$/,
                 use: [MiniCssExtractPlugin.loader,'css-loader', 'sass-loader']
              }
        ]
    }

[React, Webpack, ESLint, TypeScript](https://www.carlrippon.com/creating-react-app-with-typescript-eslint-with-webpack5/)

[CSS, Sass](https://dev.to/deepanjangh/setting-up-css-and-sass-with-webpack-3cg)

[Asset Modules](https://webpack.js.org/guides/asset-modules/)

