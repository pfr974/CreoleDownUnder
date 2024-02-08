+++
title = 'Setting up a React + Typescript project with webpack'
date = 2024-02-08T16:56:27+11:00
draft = false
summary = "Intro to React"
tags = ['JavaScript', 'Typescript', 'React', 'Frontend']
+++


This is simply to keep track of the steps I took to set up a React + Typescript project with webpack following [Learn React with TypeScript](https://www.packtpub.com/product/learn-react-with-typescript-second-edition/9781804614204) by Carl Rippon.

Webpack is a tool that "bundles JavaScript files for usage in a browser". As far as I understand this means that it takes all the files necessary for a project to run and group them together so that they can be used in a browser. I guess this is useful knowing that we are going to use React and Typescript and will probably deal with various files of different format.

The git repository for this project can be found [here](https://github.com/pfr974/basic_react_typescript_webpack).

## Initial set up

We are going to start by setting up a `src` folder to hold our source code and write the various configuration files that we place at the root of the project folder. 

We start first with a `package.json` file at the root level and a simple `index.html` HTML file in the `scr` folder:

```json
{
  "name": "my-app",
  "description": "My first React and TypeScript app",
  "version": "0.0.1",
}
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>My app</title>
  </head>
  <body>
    <div id="root"></div>
    <p> Hello World!</p>
  </body>
</html>
```
Nothing dramatic for now. The next thing to is to install Typescript and React in the project via `npm`:
  
```bash
npm install --save-dev typescript
```

With Typescript after running the `npm` command, we also have to create a configuration file, `tsconfig.json` at, you guess it, the root of the project. To be completely honest, I still struggle to understand the content of this file:

```json
{
  "compilerOptions": {
    "noEmit": true,
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "moduleResolution": "node",    
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "jsx": "react",
    "forceConsistentCasingInFileNames": true,
    "strict": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```
From my memory of my previous job, I would say that since we are going to have files with the `.tsx` extension, we need to configure how the Typescript compiler will handle them. We also need to tell it that we are going to use React in this project so that's where the `"jsx": "react"` is coming from.

Now for React, while we do need to use `npm`, it requires slightly more command lines:
  
  ```bash
  npm install react react-dom
  npm install --save-dev @types/react @types/react-dom
  ```

  The first line installs React and React-DOM. `react` is the main library and `react-dom` is needed for web appplications. With the former, I have just realised that it's actually in its name after reading about the DOM concept in my Javascript course...

  At this point, it's also interesting to point out that while we run these `npm` commands, our `package.json` file is being updated:

  ```json
  {
  "name": "my-app",
  "description": "My first React and TypeScript app",
  "version": "0.0.1",
  "devDependencies": {
    "@types/react": "^18.2.55",
    "@types/react-dom": "^18.2.19",
    "typescript": "^5.3.3"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

To finish adding React, we also need an `index.tsx` file in the `src` folder:

```tsx
import React, { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(
  document.getElementById('root') as HTMLElement
  );

function App() {
  return <h1>My FIRST React and TypeScript App!</h1>;
}

root.render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

As I understand it, this file is complementary to the `index.html` file. In the static html file, we have a way to reach out to React via `<div id="root"></div>`. In the `tsx` file, we define a constant `root`. To define this constant, we start by looking for the `root` id in our html file via `document.getElementById`. React is then "used" via the `createRoot` function that we have imported a few line above. `createRoot` load the React component, defined by the `App` function into the `root`constant. Here, our React component is simply a `h1` tag with a `"My FIRST React and TypeScript App!"` message. 

We could have just put that tag into our static HTML file, but why do simple when we can do complicated? In more seriousness, this is a bit overdone here, but I am starting to get it now.

Now that Typescript and React are installed, what do we need? Our goal here is to build a simple web app that says "My FIRST React and TypeScript App!" in the browser. We are going to need turning the Typescript and React code into Javascript and then bundle it all together. This is where Babel and Webpack come in.

We defined earlier a `tsconfig.json` file to configure the Typescript compiler. In it, there was a `"noEmit": true` option. This prevent the Typescript compiler from doing any transpilation. That part is handled by Babel, which we set up via a series of `npm` commands:

```bash
npm i -D @babel/core
npm i -D @babel/preset-env
npm i -D @babel/preset-react
npm i -D @babel/preset-typescript
npm i -D @babel/plugin-transform-runtime @babel/runtime
``` 

The two important lines for me here are the `preset-react` and `preset-typescript`, allowing React and Typescript code to be turned into Javascript. Following these command line, we create a `.babelrc.json` file at the root of the project:

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react",
    "@babel/preset-typescript"
  ],
  "plugins": [
    "@babel/plugin-transform-runtime"
  ]
}
```

## Gluing everything together with Webpack

Now that we have done a lot of set up, we need to glue all of the above together with Webpack. We start by running a series of `npm` commands to install Webpack and its various plugins:

```bash
npm i -D webpack webpack-cli
npm i -D webpack-dev-server
npm i -D babel-loader
npm i -D html-webpack-plugin
```

Note the `babel-loader`  that allows to transpile the Typescript and React code into Javascript.

Now that webpack is installed, it needs to be configured. Again, being compeltely honest here, this is still beyond my understanding. For now, I will just copy and paste the Typescript configuration file that the book suggested and run with it:

 ```typescript
 import path from 'path';
import HtmlWebpackPlugin from 'html-webpack-plugin';
import {
  Configuration as WebpackConfig,
  HotModuleReplacementPlugin,
} from 'webpack';
import { 
  Configuration as WebpackDevServerConfig 
} from 'webpack-dev-server';

type Configuration = WebpackConfig & {
  devServer?: WebpackDevServerConfig;
}

const config: Configuration = {
  mode: 'development',
  output: {
    publicPath: '/',
  },
  entry: './src/index.tsx',
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/i,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react', '@babel/preset-typescript'],
          },
        },
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.html',
    }),
    new HotModuleReplacementPlugin(),
  ],
  devtool: 'inline-source-map',
  devServer: {
    static: path.join(__dirname, 'dist'),
    historyApiFallback: true,
    port: 4000,
    open: true,
    hot: true,
  }
};

export default config;
```

After creating this file at the root of the project, we then need to add the following to our `package.json` file:

```json
  "scripts": {
    "start": "webpack serve --config webpack.dev.config.ts"
  }
```

Finally, to run the app in development mode, we need to run `npm start`. This will trigger the browser to open and display our app!

![My first React and Typescript app](/images/react/first_react_typescript_app_browser.png)

## Conclusion

That was a lot of of work for what's basically a "Hello World" app. But I found it interesting to see how:
- the React component is loaded into the html file
- The Typescript and React code is turned into Javascript via Webpack and Babel