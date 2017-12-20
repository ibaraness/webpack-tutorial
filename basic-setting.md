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

#### Next step

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

Next we'll create our greeting and entry JavaScript files

###### entry.js
```javascript
import {greeting} from './greeting';

var greetMessage = greeting("App");

document.addEventListener("DOMContentLoaded", function(event) {
  document.querySelector('.app-title').innerHTML = greetMessage;
});

```
