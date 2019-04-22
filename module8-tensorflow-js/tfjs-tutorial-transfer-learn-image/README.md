# TensorFlow.js handwritten digit recognition tutorial

This folder has a full working version of TensorFlow.js
[transfer learn for images tutorial](https://codelabs.developers.google.com/codelabs/tensorflowjs-teachablemachine-codelab/index.html#0).

1. Install Node.js
1. Run `npm install` to install the modules we need
1. Open `index.html` on a browser window

Optional: if you plan to modify and play with the code, install `live-server`
to automatically reload the browser window when the code is changed.

1. `npm install -g live-server`
1. Run `live-server` to open `index.html` on a browser (may need to open a new
   terminal window to make `live-server` part of the path)
1. Modify the code - it will be automtically reloaded

**NOTE**: to make this example work, the `img` that loads the image in
`index.html` had to be enhanced with `referrerPolicy="no-referrer"`. Otherwise
the imgur returns a 403 error.
