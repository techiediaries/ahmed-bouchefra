---
layout: post
title: "Using Python with Electron Tutorial"
image: "images/content/python.png"
excerpt: "In this tutorial, you'll learn to build GUIs for your Python applications using Electron and web technologies i.e HTML, CSS and JavaScript" 
tags : [python , nodejs, electron , node, django] 
date:   2019-11-16
canonical: "https://www.techiediaries.com/python-electron-tutorial/"
---

In this tutorial, you'll learn to build GUIs for your Python applications using Electron and web technologies i.e HTML, CSS and JavaScript-this means taking advantage of the latest advancements in front-end web development to build desktop applications but also taking advantages of Python and the powerful libraries it has to easily implement advanced requirements.

You can find the code in this [GitHub repository](https://github.com/techiediaries/python-electron-app).

 
## Creating your First Electron Application

Let's now see how to create our first Electron application. You can develop Electron apps just like you would normally develop Node.js apps.

You first need to start with creating or generating a `package.json` file inside your project's folder using the following command: 

```
npm init -y
```

This will create a basic `package.json` file with default values:

```json
{
  "name": "electronjs-python",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Next, create the two `index.html` and `main.js` files inside the project's folder.

```bash
touch main.js index.html
```

 The `main.js` file is the `main` script so we need to change the main property of our `package.json` file to `main.js` instead of the default `index.js` file (It's only a preference not required):

```json
"main": "main.js",
```

Next, you need to install `electron` from npm:

```bash
npm install --save-dev electron
```

This will install `electron` locally; you can also follow [the official guide](https://electronjs.org/docs/tutorial/installation) for more available options for installing `electron`.

Next, add the `start` script to run the `main.js` file. Open the `package.json` file and add:

```json
  "scripts": {
    "start": "electron .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

Now, let's add the code which runs a GUI window in the main process. Open the `main.js` file and add, the first line to import the `electron` module:

```javascript
const {app, BrowserWindow} = require('electron')
```

Next, add the following function which makes an instance of `BrowserWindow` and load the `index.html` file:

```javascript
  function createWindow () {
    window = new BrowserWindow({width: 800, height: 600})
    window.loadFile('index.html')
  }
```

When the application is ready, run the `createWindow()` method:

```js
  app.on('ready', createWindow)
```

We can also handle different events such as when closing all Windows using:

```js
  app.on('window-all-closed', () => {
    // On macOS it is common for applications and their menu bar
    // to stay active until the user quits explicitly with Cmd + Q
    if (process.platform !== 'darwin') {
      app.quit()
    }
  })
```

Finally, let's add the following content to the `index.html` file:

```html
<!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Hello Python from Electron!</title>
    </head>
    <body>
      <h1>Hello Python!</h1>
    </body>
  </html>
```

Now, you can run the application using:

```bash
npm start
```

This is a screenshot of the application running:

![Electron Python](https://i.imgur.com/bM6cJR3.png)

## Running a Python Script from Electron

Since we want to develop our application using Python and use Electron to build the GUI frontend  with the web; we need to be able to communicate between Python and Electron. 

Let's see how to run a basic Python script from Electron. First create a `hello.py` file and add the following Python code which prints *Hello from Python!* to the standard output:

```python
import sys
print('Hello from Python!')
sys.stdout.flush()
```

In your `main.js` file, run the following code to spawn a Python process and execute the `hello.py` script:

```js
function createWindow () {
    /*...*/
    var python = require('child_process').spawn('python', ['./hello.py']);
    python.stdout.on('data',function(data){
	    console.log("data: ",data.toString('utf8'));
    });
 }
```

![Electron Python](https://i.imgur.com/F4yx1TW.png)

### Using `python-shell` to Communicate between Python and Node.js/Electron

A better way to communicate with Node.js/Electron and Python is through using the `python-shell` package.

[`python-shell`](https://www.npmjs.com/package/python-shell) provides an easy way to run Python scripts from Node.js with basic and efficient inter-process communication and better error handling.

Using `python-shell`, you can:

-   spawn Python scripts in a child process;
-   switch between text, JSON and binary modes;
-   use custom parsers and formatters;
-   perform data transfers through `stdin` and `stdout` streams;
-   get stack traces when an error is thrown.

Head back to your terminal, make sure you are inside the root folder of your project and run the following command to install `python-shell` from npm:

```bash
npm install --save python-shell 
```

You can then simply run a Python shell using:

```js
var pyshell =  require('python-shell');

pyshell.run('hello.py',  function  (err, results)  {
 if  (err)  throw err;
 console.log('hello.py finished.');
 console.log('results', results);
});
```

![Electron python-shell](https://i.imgur.com/ytib7jt.png)


## Conlusion

In this tutorial, we've seen how to use Electron and Python to build a simple desktop application.

We've also seen how to use the `python-shell` module to run a Python shell from a Node.js/Electron application and communicate between Electron and Python.
