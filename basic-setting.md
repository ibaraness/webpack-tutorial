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
Press the 'enter' key for the rest of the options npm asks you about. When you're done you should have a **package.json** file on the folder.

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


Lets Start from writnig our index.html file. Create index.html in the 'src' folder we've created before.  


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
  <script src="./../bundle.js"></script>
</head>

<body>
  <h1 class="app-title"></h1>
</body>

</html>
```

Next we'll create our greeting.js and entry.js files. Let create the JavaScript files in the following path 'src/app/'. 

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
The last file we going to create on this step is webpack.configure.js. We are going to start with a very basic and simple configuration and extend it as we go. Lets create it in the root folder of our project where package.json file and node_modules folder sits.

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
Now we can run the npm script which will create the bunlde.js file in our root folder. Webpack will automatically use our webpack.config.js file to configure itself.
```
$ npm run build
```
If everything went right, we are going to see somthing like so in the terminal:
```
webpack-project@1.0.0 build ~/webpack-project
> webpack

Hash: a7992d9729ded8872030
Version: webpack 3.10.0
Time: 84ms
    Asset     Size  Chunks             Chunk Names
bundle.js  3.15 kB       0  [emitted]  main
   [0] ./src/app/entry.js 209 bytes {0} [built]
   [1] ./src/app/greeting.js 61 bytes {0} [built]
```
Now we can run index.html on the browser and check that our code works. If everything worked as it should, we should see a 'Hello, Idan' message on the page. 
Creating a bundle file from all of our multiple javascript files is great, but for every little change we want to do, we have to run the 'build' script again in order to generate a new bundle file. Webpack have a solution for that problem.
Add another script in package.json
```javascript
{
      ...
       "build": "webpack",
       "watch": "webpack --watch"
      ...
}
```
Now if we'll run 'npm run watch', we will see that the script run just like before, but this time, the script doesn't end. Webpack is watching for any changes on our javascript files. If any of the files get changed, the bundle.js will be updated with the new changes.
_to stop the script hold 'ctrl' and 'c' letter keys together._ 

```
$ npm run watch

webpack-project@1.0.0 build ~/webpack-project
> webpack --watch

Webpack is watching the files…

Hash: a7992d9729ded8872030
Version: webpack 3.10.0
Time: 84ms
    Asset     Size  Chunks             Chunk Names
bundle.js  3.15 kB       0  [emitted]  main
   [0] ./src/app/entry.js 209 bytes {0} [built]
   [1] ./src/app/greeting.js 61 bytes {0} [built]
```

Lets update greeting.js file and save it:

```javascript
export function greeting(name){
  return "Hello, " + name + ", How are you?";
}
```

If our watch script is still running, we can refresh the browser and see that the message was changed from 'Hello, Idan' to 'Hello, Idan, How are you?'. Watching changes is great but what if we could refresh the page on each change on our app? We can do just that using 'webpack-dev-server'.

#### Installing webpack development server


