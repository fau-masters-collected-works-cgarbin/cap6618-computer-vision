# Learning TensorFlow.js

The goal of this work is to understand how to work with machine learning in a
constrained environment. In this context, "constrained environment" is in
comparison with having access to the entire computer's environment.

To do that, we chose to learn more about TensorFlow.js running inside a
browser (it can also run on the server, with Node.js, but this is outside of
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
to run with Node.js and npm. Please see the development environment section
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
othe ways to import a module in Javascript - the result is similar, though):

    <script src="./node_modules/@tensorflow/tfjs/dist/tf.min.js"></script>

> To develop:
>
> -   an editor
> -   a test environment
> -   debuggers: vstudio Chrome debugger, Chrome dev console, tfjs viz

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
