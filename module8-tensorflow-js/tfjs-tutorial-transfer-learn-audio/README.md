# TensorFlow.js handwritten digit recognition tutorial

This folder has a full working version of TensorFlow.js
[transfer learn for audio tutorial](https://codelabs.developers.google.com/codelabs/tensorflowjs-audio-codelab/index.html#0).

Instructions to train the audio recognizer are
[here](https://codelabs.developers.google.com/codelabs/tensorflowjs-audio-codelab/index.html#9).

**NOTE**: Loading modules locally didn't work for this tutorial. The
speech-command module cannot find an `EPSILON` variable (constant). The modules
are being loaded from `unpkg.com`, as in the original tutorial code.
