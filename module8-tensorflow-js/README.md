# Machine learning in a constrained environment - TensorFlow.js

The goal of this work is to understand how to work with machine learning in a
constrained environment. In this context, "constrained environment" is in
comparison with having access to the entire computer's environment.

To do that, we chose to learn more about TensorFlow.js running inside a
browser. TensorFlow.js can also run on the server (e.g. with [Node.js](https://nodejs.org/en/),
but this is outside of the scope of this document. Here we will concentrate on
a constrained environment, the browser, and how it is different from developing
code directly on a computer.

This report is written from the point of view of someone who has some machine
learning experience with Ptyhon, TensorFlow and Keras. It highlights
differences in the development environment and the runtime environment.

With that in mind, the items investigated for this report are listed below.

> TODO: review these steps once the sections are written.

1. The differences between the TensorFlow.js environment on a browser and the
   typical machine learning environment.
1. How to setup a productive development environment for TensorFlow.js.
1. What are the differences in the runtime environment, compared to having
   access to the entire computer's environment.
1. What are the learning resources to get up to speed quickly, asssuming
   previous experience with TensorFlow and Keras, but not with JavaScript and
   TensorFlow.js.

The following sections in the report cover each item of the list.

Most of the code in this report is based on the
[official tutorials](https://www.tensorflow.org/js/tutorials). The tutorials
have been converted in complete, running examples. They are saved in the
folders named `tfjs-tutorial-...` (in [this GitHub repository](https://github.com/cgarbin/cap6618-computer-vision/tree/master/module8-tensorflow-js),
if you are reading this report outside of GitHub). All tutorials are configured
to run locally with Node.js. Please see the development environment section
below to get the environment up and running.

## Environment differences

In the typical machine learning environment we have access to all resources in
the computer.

TensorFlow.js runs inside a browser. The browser is a [sandboxed environment](<https://en.wikipedia.org/wiki/Sandbox_(computer_security)>),
which means it does not have access to all resources on the computer and, for
the cases where it does have access, it may have to first get explicit
permission from the user. These restrictions are not specific to TensorFlow.js.
Any program running inside the browser is subject to them.

These are some of the restrictions for a program running inside a browser:

-   No access to the file system. It cannot read or write to directories on the
    computer. It has access only to the [web storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API).
    This storage is [significantly smaller](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Browser_storage_limits_and_eviction_criteria#Storage_limits)
    than the computer's file system.
-   Needs permission from the user to access the camera.
-   Needs permission from the user to access the microphone.

## Development environment

The major differences in the development environment are related to the first
item in the environment differences: the program does not have access to the
file system.

That plays a role from the start of the development process. This is the usual
beginning of a Python TensorFlow program:

    import tensorflow

This line results in loading the TensorFlow library files from the local disk
into the runtime environment. However, a program running inside the browser
does not have access to the local disk. It must load the libraries in a
different way.

This is the usual beginning of a JavaScript TensorFlow.js program (there are
other ways to import a module in JavaScript - the result is similar, though):

    <script src="./node_modules/@tensorflow/tfjs/dist/tf.min.js"></script>

The program cannot directly access that file. Instead, it will ask a web
server to serve the file.

> > pic with a web server between the browser and the file system

Therefore, the development environment needs a web server. There are several
alternatives. For simple tests, Python's [http.server](https://docs.python.org/3/library/http.server.html)
is enough ([SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)
for Python 2).

For more complex development work, [Node.js](https://nodejs.org/en/) is a
better alternative. It has several useful modules that speed up the workflow.

One of these useful modules is [live-server](https://www.npmjs.com/package/live-server),
a module that automates another difference in the environment: running the
JavaScript code requires reloading the web page every time any change is made.
[live-server](https://www.npmjs.com/package/live-server) is a web server that
automatically reloads the page when a file changes. Sounds like a small detail,
but manually reloading pages can quickly become time consuming in the
development workflow.

Once a web server is in place, we can start the typical development workflow:
write code, test code, and (inevitably) debug code.

In the follow in sections we will put in place a set of tools for these steps.

**NOTE:** Some of the items in these sections are opinions, clouded by the
biases of the author. Whenever possible, the author explains the reason for
each choice. You are welcome to try alternatives, keeping in mind the reasons
for each choice.

### Writing code

JavaScript, as other languages before it, has its [quirks](https://github.com/denysdovhan/wtfjs).
Some of these quirks will cost time. Code that looks perfectly logical and
functional will not work as expected.

The best defense against that is to have a good code editor, backed by good
code analysis tools.

-   Editor: [Visual Studio Code](https://code.visualstudio.com/) is a free
    editor with has a large collection of plugins. Some of them are listed in
    the items below.
-   Code analysis ([linting](<https://en.wikipedia.org/wiki/Lint_(software)>)):
    it takes time to learn all the subtleties of a new language, and JavaScript
    is not short of them. Some of these subtleties will result in frustrating
    bugs. As a good practice, always have a code analysis tool automatically
    running. [eslint](<https://en.wikipedia.org/wiki/Lint_(software)>) is the
    best linting tool for JavaScript and is available as an extension for
    Visual Studio Code.
-   Code formattting: keep the code consistent and easier to understand. Also
    prepares it for publication, e.g. in GitHub. [prettier](https://prettier.io/)
    is currently the most used formatter. Also available as an extension for
    Visual Studio Code.

### Testing and debugging the code

This section focuses on Chrome. Other browsers have similar tools in place.

There are two ways to debug JavaScript code running on the browser:

-   Directly on the browser with [Chrome DevTools' JavaScript debugger](https://developers.google.com/web/tools/chrome-devtools/javascript/).
-   From Visual Studio Code with the [Chrome Debugger](https://github.com/Microsoft/vscode-chrome-debug).

The first option is the easiest to start with. The second option has more
features and is more useful in the long run, despite requiring a (minor)
configuration step to get started.

TensorFlow.js also comes with a visualization tool, [tfjs-vis](https://github.com/tensorflow/tfjs-vis).
With tfjs-vis we can follow the code behavior in real time. See [this example](https://storage.googleapis.com/tfjs-vis/mnist/dist/index.html).

### Putting it all together

Putting all pieces together to get the development environment in place:

-   Install [Node.js](https://nodejs.org/en/) and the [liver-server](https://www.npmjs.com/package/live-server)
    package to serve the files to the code running inside the browser (and to
    automatically refresh the page when code changes).
-   Install [Visual Studio Code](https://code.visualstudio.com/).
-   Install Visual Studio Code extensions for a linter ([eslint](https://eslint.org/))
    and for a code formatter ([prettier](https://prettier.io/)).
-   Learn how to use [Chrome DevTools' JavaScript debugger](https://developers.google.com/web/tools/chrome-devtools/javascript/).
-   Learn how to use [Visual Studio Code's Chrome debugger](https://github.com/Microsoft/vscode-chrome-debug).
-   Learn how to use real-time visualization with [tfjs-vis](https://github.com/tensorflow/tfjs-vis).

## Development workflow

-   Don't have access to the disk, so training can't local images locally
-   Develop models in a faster, easier to debug environment (e.g. Python)
-   Export model to TensorFlow.js
-   Run final tests in TensorFlow.js

## Runtime environment

> Can't control what brower is used
> Could even be mobile phone browser
> What model to use to cover all cases? Should use MobileNet or try to
> determine the browser and serve a different model?

## Production environment

The sections above cover the development workflow. For a production
environment, where models are released to the end users, we need a few more
steps.

-   Minimize the code
-   Package (expand this...)

There are tools to automated those steps. A good start is [webpack](link here).

## Old - remove later

All tutorials are configured to run with Node.js and npm.

We will run all examples locally, including the TensorFlow.js library itself.
To do that we need to install the modules locally.

-   Install [Node.js](https://nodejs.org/).
-   Install [live-server](https://www.npmjs.com/package/live-server) to act as
    a local web server (with auto-reload).
-   `cd` to a tutorial directory
-   Run `npm install` to install the modules listed in `package.json`, which
    includes the TensorFlow.js modules we need.
-   Run `live-server` to open a browser and load `index.html`.

## Appendix

### Loading TensorFlow.js from a content delivery network (CDN)

> Explain loading TF from a CDN, as used in the tutorials
