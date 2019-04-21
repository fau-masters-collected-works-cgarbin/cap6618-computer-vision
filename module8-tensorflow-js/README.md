# Learning TensorFlow.js

This folder contains the work to understand TensorFlow.js.

Folders names `tfjs-tutorial-...` are some of the [official tutorials](https://www.tensorflow.org/js/tutorials).

All tutorials are configured to run with Node.js and npm.

We will run all examples locally, including the TensorFlow.js library itself.
To do that we need to make two changes to the exmaples.

First, install the libraries locally:

-   `cd` to the tutorial directory
-   Run `npm install` to install the modules listed in `package.json`, which
    includes the TensorFlow.js modules we need.

Then, replace the `script` entries in `index.htlm` to point to the local files:

        <script src="./node_modules/@tensorflow/tfjs/dist/tf.min.js"></script>
        ...
        <script src="./node_modules/@tensorflow/tfjs-vis/dist/tfjs-vis.umd.min.js"></script>

If you plan to modify the code, you may want to use an auto-reload server,
such as [live-server](https://www.npmjs.com/package/live-server) to speed up
the development cycle.
