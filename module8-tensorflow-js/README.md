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

1. How to setup a productive development environment for TensorFlow.js.
1. What are the differences in the runtime environment, compared to having
   access to the entire computer's environment.
1. What are the learning resources to get up to speed quickly, asssuming
   previous experience with TensorFlow and Keras, but not with Javascript and
   TensorFlow.js.

Most of the code in this report is based on the
[official tutorials](https://www.tensorflow.org/js/tutorials). The tutorials
have been converted in complete, running examples. They are saved in the
folders named `tfjs-tutorial-...` (in [this GitHub repository](https://github.com/cgarbin/cap6618-computer-vision/tree/master/module8-tensorflow-js),
if you are reading this report outside of GitHub).

This report is structured as follows:

1. The first section explains how to get a development environment in place.
1. The second section reviews what makes TensorFlow.js running on a browser
   a different environment.
1. The last section has links to learning resources.

## Getting a development environment in place

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
