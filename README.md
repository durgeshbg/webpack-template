# Webpack Template

This is template repository created to ease the process of configuring multiple complex tools like webpack, eslint, babel, etc.
Basically I use this as a base without configuring everything from scratch 😅.

## Config Info:

### [Webpack](./webpack/):

Why?
I used the webpack module bundler due its popularity and community support.
The webpack config is divided into three parts:

#### [webpack.common.js](./webpack/webpack.common.js)

Consists of the common configuration for both the environments like:

- Cleaning the `output` directory upon each build.
  ```js
  output: {
    clean: true,
  }
  ```
- `html-webpack-plugin` module to create a html template.
  ```js
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ];
  ```
- Bundling the CSS files using `style-loader` and `css-loader` module.

  ```js
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  ```

- Bundling fonts and image assets
  ```js
  module: {
    rules: [
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: 'asset/resource',
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/i,
        type: 'asset/resource',
      },
    ],
  },
  ```

#### [webpack.prod.js](./webpack/webpack.prod.js)

The poduction config sets the environment variable and `devtool` to `source-map`.

```js
{
  mode: 'production',
  devtool: 'source-map',
}
```

#### [webpack.dev.js](./webpack/webpack.dev.js)

The development config sets the environment varible and `devtool` to `inline-source-map`.

```js
{
  mode: 'development',
  devtool: 'inline-source-map',
}
```

The common config is combined with relevant config file of that particular environment (production and development) using the `webpack-merge` module.

### [Babel](./babel.config.js)

Bable is configured with a basic setup to transpile to `current` node version

```js
{
  presets: [['@babel/preset-env', { targets: { node: 'current' } }]],
}
```

### [Prettier](.prettierrc)

Prettier is also configured to a basic form to use `singleQuote` and `jsxSingleQuote`

```js
{
  "singleQuote": true,
  "jsxSingleQuote": true
}
```

### [ES Lint](.eslintrc)

ES Lint is configured to use the `airbnb-base` style guide and work smoothly with `prettier`.

```js
{
  "extends": ["airbnb-base", "prettier"],
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
}
```

And `no-unused-vars` set to `warn`

```js
{
  "rules": {
    "no-unused-vars": ["warn"]
  }
}
```

### NPM Scripts:

#### Developmet Script

Spins up webpack in watch mode with the webpack development config

```json
"scripts": {
  "dev": "webpack --watch --config ./webpack/webpack.dev.js",
  // other scripts
},
```

#### Build Script

Builds the dist folder with the `index.html` and `main.js` files and assets bundled

```json
"scripts": {
  "build": "webpack --config ./webpack/webpack.prod.js",
  // other scripts
},
```

#### Tests Script

Starts all the tests in project in `watch` mode

```json
"scripts": {
  "tests": "jest --watch *.js"
  // other scripts
},
```

#### Deploy Script

Uses the gh-pages module to deploy the dist files on a seperate branch on GitHub call `gh-pages` make sure that your GitHub pages point to this branch for publishing your page.

```json
"scripts": {
  "deploy": "gh-pages -d dist",
  // other scripts
},
```

### Jest

Jest is installed and configured with a small unit test to make sure its working fine, a test for a test (pun intended!)

## Conclusion

Please feel free to contribute your valuble expierence to this repository.
