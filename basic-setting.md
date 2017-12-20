# Basic setting of Webpack using NPM

In this guide we'll be creating a working Webpack environment from scratch. Our final app will have a basic working webpack configuration, and a local development server we can run in our local machine.

**We will do the following steps:**
* **First step:** Creating project folder and installing webpack.
* **Second step:** Creating the files for our application.
* **Third step:** Installing webpack development server
* **Forth step:** Configuring our app a little more with loaders and plugins.

_Before we begin, it's important that you'll have [NodeJS](https://nodejs.org/) and [NPM](https://www.npmjs.com/
) installed on your machine:_

#### First step

Lets create a new folder we want our project to be in, like 'webpack-project' and navigate to it like so:

```
$ mkdir webpack-project
$ cd webpack-project
```

Next, inside that folder lets create a new NPM package for our project using 'npm init':
```
$ npm init
```
When npm will ask you for a name, you can enter any url friendly name like 'webpack-project' or so. 
press enter for the rest of options npm gives you. When you're done you should have a **package.json** file on the folder.

Now, it's time to install Webpack and add it to our development dependencies

```
npm install --save-dev webpack
```

Now lets prepare the file structure for our app:
*You can follow my recommendation or set your own structure*

```
webpack-project
      |
      node_modules (crated automatically on modules install using npm)
      package.json (Created automatically using npm init)
      webpack.config.js (will be created next)
      src--
          |
          index.html (will be created next)
          style.css (will be created next)
          app--
              |
              entry.js (will be created next)
              greeting.js (will be created next)
        
```

#### Second step

After webpack is installed, we can start creating the files we will be needing for our web project to work:

* index.html
* entry.js
* greeting.js
* webpack.config.js


Lets Start from writnig our index.html file.


###### Index.html 
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta content="IE=edge" http-equiv="X-UA-Compatible" />
  <title>My App</title>
  <!--[ We will load webpack bundle file here ]-->
  <script src="bundle.js"></script>
</head>

<body>
  <h1 class="app-title"></h1>
</body>

</html>
```

Next we'll create our greeting.js and entry.js files.

###### entry.js
```javascript
import {greeting} from './greeting';

var greetMessage = greeting("Idan");

document.addEventListener("DOMContentLoaded", function(event) {
  document.querySelector('.app-title').innerHTML = greetMessage;
});

```
###### greeting.js
```javascript
export function greeting(name){
  return "Hello, " + name;
}

```
The last file we going to create on this step is webpack.configure.js. Wearegoing to start with a very basic and simple configuration and extend it as we go.

###### webpack.configure.js
```javascript
module.exports = {

      /* This is where we add our entry file, from which the bundle is going to be created */
      entry: './src/app/entry.js',
      
      /* This is where we et the name and location of the bundle file */
      output: {
            filename: 'bundle.js'
      }
};
```

After all files are created it's time to add a script to our package. Open package.json file, in "scripts", remove 'test' and add 'build' object instead of it like so:

```javascript
{
      ...
       "test": "echo \"Error: no test specified\" && exit 1", --> remove this line
       "build": "webpack"
      ...
}
```
