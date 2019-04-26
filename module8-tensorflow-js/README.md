# Machine learning in a constrained environment - TensorFlow.js

The goal of this work is to understand how to work with machine learning in a
constrained environment. In this context, "constrained environment" is in
comparison with having access to the entire computer's environment.

To do that, we chose to learn more about TensorFlow.js running inside a
browser (it can also run on a server, with Node.js, but this is outside of
the scope of this work).

This report is written from the point of view of someone who has some machine
learning experience with Ptyhon, TensorFlow and Keras. It highlights
differences in the development environment and the runtime environment.

With that in mind, the items investigated for this report are listed below.

1. The differences between the TensorFlow.js environment and the typical
   machine learning environment.
1. How to setup a productive development environment for TensorFlow.js.
1. What are the differences in the runtime environment, compared to having
   access to the entire computer's environment.
1. What are the learning resources to get up to speed quickly, asssuming
   previous experience with TensorFlow and Keras, but not with Javascript and
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

This is the usual beginning of Javascript TensorFlow.js program (there are
other ways to import a module in Javascript - the result is similar, though):

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
Javascript code requires reloading the web page every time any change is made.
[live-server](https://www.npmjs.com/package/live-server) is a web server that
automatically reloads the page when a file changes. Sounds like a small detail,
but manually reloading pages can quickly become time consuming in the
development workflow.

Once a web server is in place, we can start the typical development workflow:
write code, test code, and (inevitably) debug code.

In the follow in sections we will put in place a set of tools for these steps.

### Write code

### Test and debug the code

### Putting it all together

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
