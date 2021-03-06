# webpack-tutorial
### A simple guide to using Webpack with different frameworks

On this page I'll explain the general concepts and configuration options used by webpack:
* Basic configuration: enrty file and output bundle file
* Loaders: Loading more than just plain JavaScript files
* Plugins: Extending webpack capabilities more
* Local server: Using webpack-dev-server package to create a local server
* Next part: Webpack and Angular4 configuration

**What is webpack?**

As we all know Webpack is a JavaScript bundler, which means that it allows us to use multiple files on development environment to be later bundled to one or more javascript files (On production or dev-server). As I’ve said before in it’s mosts basic form, Webpack is nothing more than a JavaScript bundler. 

##### from webpack docs:
>At its core, webpack is a static module bundler for modern JavaScript applications. When webpack processes your application, it recursively builds a dependency graph that includes every module your application needs, then packages all of those modules into one or more bundles."

I'll be using ES6 import and export syntax for importing modules. Import and Export statements are what we are using to connect different parts of the codes together. Each part of the code being exported or imported by webpack refered to as a module. So we are importing different modules that in the end will be bundled together as one or more files.

**If you need help on setting a basic webpack environment on your local machine, here is a [basic guide](basic-setting.md) for doing just that**

[![N|webpack basic file bundling](basic1.png)]


###### Lets see a simple example:
File: greetings.js
```javascript
export function getGreetings(name){
    return "Hello " + name + ", have a nice day"
}

export const unusedConstant = 'unused';
```

File: entry.js
```javascript
import {getGreetings} from 'greetings.js'
var greetIdan = getGreetings('Idan'); /* returns: Hello Idan, have a nice day */
```

Using webpack basic configuration:

```javascript
module.exports = {
  entry: './entry.js', /* Our entry file */
  output: {
    filename: 'bundle.js' /* The final results */
  }
};
```
The generated 'bundle.js' file will contain the code we've imported from both files:
```javascript
function getGreetings(name){
    return "Hello " + name + ", have a nice day"
}
/* Notice that in the basic version unused code can get imported as well.
   Tree-shaking (Removing unused code) can be set using UglifyJS plugin (next section) */
var unusedConstant = 'unused'; 

var greetIdan = getGreetings('Idan'); /* returns: Hello Idan, have a nice day */
```
### Loaders
#### Loading more than just plain JavaScript files
Usually we'll want to use and import more than just plain JavaScript files in our projects, that is where **loaders** come in to play. Like I've said before, Weback in it's core is just a plain JavaScript bundler, it can't import or understand other types of files. Using **loaders** we are able to tell webpack how to interpret and import other type of files.

##### from webpack docs:
#
>Loaders enable webpack to process more than just JavaScript files (webpack itself only understands JavaScript). They give you the ability to leverage webpack's bundling capabilities for all kinds of files by converting them to valid modules that webpack can process.
#
**In the following example, taken from webpack site, we are using a 'raw-loader' Loader to tell Webpack how to handle 'txt' files:**
```javascript
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};

module.exports = config;
```

**We are using 'raw-loader' here to tell webpack to import 'txt' files as strings in our code.**

```javascript
  /* Loaders are added in the module section unser 'rules' */
  module: {
    rules: [
      { 
        /* The first property ('test') is where we usually 
           set the file extension ('txt') using regex. */ 
        test: /\.txt$/, 
        
        /* The second property ('use') is where we set the loader or loaders
          'raw-loader' in this case. */
        use: 'raw-loader' 
      }
    ]
  }
module.exports = config;
```
**Here is an example of 2 loaders used for 'css' files**

```javascript
{
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" }
        ]
      }
    ]
  }
}
```
The first loader is used to create an inline **style** element in the DOM for every 'css' file we import. The second loader 'css-loader' tells webpack how to interpret and load css files.

There are different types of loaders that can do different type of things, check webpack loaders page:
https://webpack.js.org/loaders/

### Plugins
#### Extending webpack capabilities a little more

Plugins allow us to do a lot of things unnecessarily related to the modules we are using, like setting the output HTML file, limiting the chunk file size and much more.   

###### From webpack docs:
>While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks. Plugins range from bundle optimization and minification all the way to defining environment-like variables. The plugin interface is extremely powerful and can be used to tackle a wide variety of tasks.

One of the most common plugins we'll often see in different webpack configuration files is 'HtmlWebpackPlugin'. We can use it in our app to automatically inject the bundle file into our HTML file.

```javascript
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    /**
     * Correctly inject complied budle JS files to HTML
     */
    new HtmlWebpackPlugin({
        template: "index.html",
        inject: "body",
    })
  ]
};
```
