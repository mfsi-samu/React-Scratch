# Create React App from Scratch
### This repository contains the source code of demo react app which is created from scratch without using any CLI command.
All the basic setup for a react app development done by own. So main moto of this artical and repository is to create a react app from scratch that helps to details understanding about **Webpack** and **Babel** and learn how they work.

**1. Initial Setup**
- Create a directory that contains all your project code and structure. So this directory represent your project. My main directory name is "react-scratch"
- Open CMD and goto the created directory and run **npm init**. this command create package.json file for your react app. when you run npm init command then some question will come that helps you to customise your package.json file like package name/version/description/entry point/test command/git repository/ keywords/author/licencse and finally create package.json file in your directory.
- Now you open it in an editor of your choice. I used VS Code.
- Craete two directory **public** and **src** inside main directory. **public** directory will handle any static assets, and most importantly our index.html file, which utilize to render your app. 
- Add index.html file in public directory and copy the below code into the file.
    ```
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
            <title>React Scratch</title>
        </head>
        <body>
            <div id="root"></div>
            <script src="../build/bundle.js"></script>
        </body>
    </html>
    ```
    ***<div id="root"></div>***
    This line helps to hook our built react app using root.
    You can give any name of your built script whatevern you like, I used bundle.js here.

    Now Initial setup of react app is done.

***2. Babel Configuration***
- First you need to install some package for babel.
    ***npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/preset-react***
    - ***babel-core*** helps to do any transformations on our code like converting jsx code to react basic code.
    - ***babel-cli*** allows you to compile files from command line.
    - ***preset-react & preset-env*** both presets that transform specific flavors of code, ***env*** preset allows us to transform ES6+ into more traditinal javascript for old browser and the ***react*** preset does the same with JSX. 
- Create ***.babelrc*** file in the root directory of your project here we declear the use of ***env*** and ***react*** preset. copy the below code into .babelrc file
    ```
    {
  "presets": ["@babel/env", "@babel/preset-react"]
    }
    ```

***3. Webpack***
- Install the required packages for configure Webpack
    ***npm install --save-dev webpack webpack-cli webpack-dev-server style-loader css-loader babel-loader***
    - Webpack uses loaders to process different types of files for bundling. It also works easily alongside the development server that we’re going to use to serve our React project in development and reload browser pages on (saved) changes to our React components.
- Create a new file at the root directory of the project ***webpack.config.js***
    ```
    const path = require("path");
    const webpack = require("webpack");
    module.exports = {
    entry: "./src/index.js",
    mode: "development",
    module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: path.join(__dirname, "public/"),
    port: 3000,
    publicPath: "http://localhost:3000/dist/",
    hotOnly: true
  },
  plugins: [new webpack.HotModuleReplacementPlugin()]
    };
    ```
    ***Some Details info about the above code***
    1. ***entry***  tells Webpack where our application starts and where to start bundling our files.
    2. ***module*** object helps define how your exported javascript modules are transformed and which ones are included according to the given array of rules.
        - First rule is all about transforming our ES6 and JSX syntax.The test and exclude properties are conditions to match file against. In this case, it’ll match anything outside of the node_modules and bower_components directories. Since we’ll be transforming our .js and .jsx files as well, we’ll need to direct Webpack to use Babel. Finally, we specify that we want to use the env preset in options.
        - The next rule is for processing CSS. Since we’re not pre-or-post-processing our CSS, we just need to make sure to add style-loader and css-loader to the use property. css-loader requires style-loader in order to work. loader is a shorthand for the use property, when only one loader is being utilized.
    3. ***resolve*** property allows us to specify which extensions Webpack will resolve-this allows us to import modules without needing to add their extensions.
    4. ***output*** property tells Webpack where to put our bundled code. The publicPath property specifies what directory the bundle should go in, and also tells webpack-dev-server where to serve files from.
    5. ***publicPath***  property is a special property that helps us with our dev-server. It specifies the public URL of the the directory-at least as far as webpack-dev-server will know or care. If this is set incorrectly, you’ll get 404’s as the server won’t be serving your files from the correct location!
    6. ***devServer*** We set up webpack-dev-server in the devServer property. This doesn’t require much for our needs-just the location we’re serving static files from (such as our index.html) and the port we want to run the server on. Note that devServer also has a publicPath property. This publicPath tells the server where our bundled code actually is.
    7. ***plugins*** we want to use Hot Module Replacement so we don’t have to constantly refresh to see our changes. All we do for that in terms of this file is instantiate a new instance of the plugin in the plugins property and make sure that we set hotOnly to true in devServer. We still need to set up one more thing in React before HMR works, though.
    
 Here We completed the webpack setup.
 
 ***4. React setup***
 - Install packages for react implementation.
 ***npm install react react-dom***
 - Create ***index.js*** file in the src directory.
    ```
    import React from "react";
    import ReactDOM from "react-dom";
    import App from "./App.js";
    ReactDOM.render(<App />, document.getElementById("root"));
    ```
    Here we tell our React app where to hook into the DOM(index.html).
    ***ReactDom.render*** is the function that tells React what to render and where to render it. In the above code we pass ***App*** component to rendered at the DOM element with the ID root 
 - Create ***App.js*** file in the src directory
    ```
    import React, { Component} from "react";
    import "./App.css";
    class App extends Component{
        render(){
                return(
                        <div className="App">
                            <h1> Hello, World! </h1>
                        </div>
                        );
                }
    }
    export default App;
    ```
    
 - Create ***App.css*** file in the src directory
    ```
    .App {
        margin: 1rem;
        font-family: Arial, Helvetica, sans-serif;
        }
    ```
 
 ***5. Configure Hot Module Replacement***
 - This helps to reflect latest changes on the browser when user save there changes.
 - Install ***npm react-hot-loader***
 - Update ***app.js*** file. import react-hot-loader in App.js and mark the exported object as hot-reloaded by modifying to code.
    ```
    import React, { Component} from "react";
    import {hot} from "react-hot-loader";
    import "./App.css";

    class App extends Component{
        render(){
            return(
                <div className="App">
                    <h1> Hello, World! </h1>
                </div>
                );
            }
        }
    export default hot(module)(App);
    ```
    
 ***6. Update package.json file***
 - You need to update your package.json file for ***build*** and ***start*** of your project.
    ```
    {
    "name": "react-scratch",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --open",
    "build": "webpack --mode development",
    "buildProduction": "webpack --mode production"
    },
    "author": "Samu Horo",
    "license": "ISC",
    "devDependencies": {
    "@babel/cli": "^7.1.2",
    "@babel/core": "^7.1.2",
    "@babel/preset-env": "^7.1.0",
    "@babel/preset-react": "^7.0.0",
    "babel-loader": "^8.0.4",
    "css-loader": "^1.0.0",
    "style-loader": "^0.23.1",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2",
    "webpack-dev-server": "^3.1.9"
    },
    "dependencies": {
    "react": "^16.5.2",
    "react-dom": "^16.5.2",
    "react-hot-loader": "^4.3.11"
     }
    }
    ```
 - Added 
    ***"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --open",
    "build": "webpack --mode development",
    "buildProduction": "webpack --mode production"
  }***
    ***webpack --mode development*** is used for create build for devlopment server and build files are not minified.
    ***webpack --mode production*** is used for production build and build files contains minified version of code that helps to improve in execution time of app in production site.
  ***webpack-dev-server --open*** is used to run application on the local browser.

You can follow the above steps and create your own react app basic setup from scratch.
if you want to run my code than you need to clone or download it.
***Run***
- ***npm i***
- ***npm run build***
- ***npm start*** 
You can see your React app is running on the browser. Now if you change anything and save it then changes are reflect on the browser.
