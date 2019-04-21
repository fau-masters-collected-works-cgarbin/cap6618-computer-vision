# Learning TensorFlow.js

This folder contains the work to understand TensorFlow.js.

Folders names `tfjs-tutorial-...` are some of the [official tutorials](https://www.tensorflow.org/js/tutorials).

All tutorials are configured to run with Node.js and [yarn](https://yarnpkg.com/en/).

-   npm init
-   Install TensorFlow.js as described [here](https://www.tensorflow.org/js/tutorials/setup#installation_from_npm).
-   Configure entry point to `script.js` to match the tutorial code
-   You may want to use an auto-reload server, such as [live-server](https://www.npmjs.com/package/live-server)
    to speed up the development cycle

We will run all examples locally, inclduing the TensorFlow.js library itself.
Do that we need to make two changes to the exmaples.

First, install the libraries locally:

-   cd to the project directory
-   `npm install @tensorflow/tfjs`
-   `npm install @tensorflow/tfjs-vis`

Then, replace the `script` entries in `index.htlm` to point to the local files:

        <script src="./node_modules/@tensorflow/tfjs/dist/tf.min.js"></script>
        <!-- Import tfjs-vis -->
        <script src="./node_modules/@tensorflow/tfjs-vis/dist/tfjs-vis.umd.min.js"></script>
