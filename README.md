# Setup React with webpack 3, babel and NPM
### by Jakob Lind
[Setup React with webpack 3, babel and NPM](http://blog.jakoblind.no/react-with-webpack-babel-npm/)

A few years ago jQuery was the best practice for Javascript/frontend development. Getting started with jQuery was as simple as creating an index.html document, include jQuery and start writing code.

Today, the frontend ecosystem has evolved. To get started with React you have to learn and configure NPM, webpack, babel AND React.

It is overwhelming to have to learn that many new technologies at once. And when talking to people and reading about webpack/babel, you get the impression it’s difficult.

I want to show you that it is not as complicated as it seems at first. Try out this tutorial step by step to see for yourself!
What we are going to do

We are going to set up a simple React project from scratch using the following tools:

* webpack for building
* babel for transpiling React and ES6
* npm for running the app and dependency management

Even tough our setup is simple it is going to be a production-ready setup.

## What about CRA?

You have probably heard of [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) which is an official tool to start a new React project. The recommended way to get started for newbies is to use CRA, but there are many reason to set up your own build setup with webpack/babel/NPM:

1. CRA supports ejecting, which gives you a generated webpack config that you can edit. It is well documented but can be a pretty tough start to learn webpack anyway. For me personally, I learn best to code things from scratch!
2. The underlying technologies of CRA are webpack/babel/NPM. To master React you want to know how it works under the hood.
3. CRA does not support building an app with more advanced features such as server side rendering. You need to either “eject” from CRA, or set up the project from scratch.

## Set up a new NPM project

Let’s start by creating a new NPM project:
```
mkdir react-starter
cd react-starter
npm init
```
npm init  will ask you a bunch of questions. Just press enter for the defaults. If you want, you can later change the config it generates by editing package.json that is created by this command.

Create a JavaScript source code file

Next, we are going to create our minimal React app and HTML file.
```
import React from "react";
import ReactDOM from "react-dom";

class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

var mountNode = document.getElementById("app");
ReactDOM.render(<HelloMessage name="Jane" />, mountNode);
```
*src/app.js*


&nbsp;
&nbsp;


```
<!DOCTYPE html>
<html>
    <head>
        <title>React starter app</title>
    </head>
    <body>
        <div id="app"></div>
        <script src="bundle.js"></script>
    </body>
</html>
```
*dist/index.html*

&nbsp;

We have created two directories: src  and dist.

* src is the directory will contain our source files
* dist is the directory we will configure webpack to put the compiled JavaScript files. To keep things simple, we put the index.html file straight in the dist folder because it’s static and don’t need to go through webpack.

## Create the webpack config file

Our webpack config file looks like this:
```
const webpack = require('webpack');
const path = require('path');

const config = {
    entry: './src/app.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                use: 'babel-loader'
            }
        ]
    }
};
module.exports = config;
```
*webpack.config.js*

It contains the following:

1. The file src/app.js that we created previously is the entry point
2. The file dist/bundle.js is the output file.
3. The module section describes how webpack should transpile the code. It uses the babel-loader for all files that has a .js or a .jsx extension. In the next step we will configure babel-loader.

## Add babel config

Babel is a highly configurable tool to transpile JavaScript. We are going to use a subset of babels features: transpiling ES6 and React JSX.

Our webpack config uses babel-loader to run babel which uses the regular babel config file.

We configure babel like this in the file .babelrc:
```
{
  "presets": ["es2015", "react"]
}
```
*.babelrc*


Now we have all the files we need

This is an overview of the files and folders we have created:
```
.
├── package.json
├── dist
│   └── index.html
├── src
│   └── app.js
└── webpack.config.js
```
*2 directories, 4 files*

## Add dependencies

Before we run the application we must add NPM dependencies:
```
npm install --save-dev webpack
npm install --save-dev babel-loader
npm install --save-dev babel-core
npm install --save-dev babel-preset-es2015
npm install --save-dev babel-preset-react
npm install --save react
npm install --save react-dom
```
Our dependencies are now installed and our package.json file is updated with dependency information.

## Run it

We are going to use NPM to start the build with webpack. Add this to the script area in package.json:
```
  "scripts": {
    "dist": "webpack -p"
  },
```
Let’s run it and see what happens:
```
npm run dist
```
It works!

Now open up the file *index.html* in your browser. If you are on a mac, you can do it with this command: `open dist/index.html`

## What’s next

Run the code on your machine, and use that as your starting point for continuing your React journey.
