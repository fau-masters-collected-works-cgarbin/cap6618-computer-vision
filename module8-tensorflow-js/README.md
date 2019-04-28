# Machine learning in a constrained environment - TensorFlow.js

The goal of this work is to understand how to work with machine learning in a
constrained environment. In this context, "constrained environment" is in
comparison with having access to the entire computer's environment.

To do that, we chose to learn more about [TensorFlow.js](https://www.tensorflow.org/js)
running inside a browser.

TensorFlow.js can also run on a server (with [Node.js](https://www.tensorflow.org/js/guide/nodejs)),
but this is outside of the scope of this document. Here we will concentrate on
a constrained environment, the browser, and how it is different from developing
code directly on a computer.

This report is written from the point of view of someone who has some machine
learning experience with Ptyhon or R, TensorFlow and Keras, but little or no
experience with JavaScript and web development.

The report highlights differences in the development environment, the
development workflow, the runtime environment and the production environment.

With that in mind, the items investigated for this report are listed below.

1. The differences between the TensorFlow.js environment in a browser and the
   typical machine learning environment.
1. How to set up a productive development environment for TensorFlow.js.
1. How the development workflow differs from the typical machine learning
   environment.
1. What the differences in the runtime environment are, compared to having
   access to the entire computer's environment.
1. What the tools and best practices to deploy the models in production are.

The following sections in the report cover each item of the list.

As part of this report, the [official tutorials](https://www.tensorflow.org/js/tutorials)
have been adapted to run locally. They are saved in the folders named
`tfjs-tutorial-...` (in [this GitHub repository](https://github.com/cgarbin/cap6618-computer-vision/tree/master/module8-tensorflow-js),
if you are reading this report outside of GitHub). Please see the
[development environment section](#putting-it-all-together) to get the
environment up and running. Once the environment is in place, you should be
able to run the tutorials from your computer.

## Environment differences

In the typical machine learning environment we have access to all resources in
the computer.

TensorFlow.js runs inside a browser (for our context in this report). The
browser is a [sandboxed environment](<https://en.wikipedia.org/wiki/Sandbox_(computer_security)>),
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
file system. That restriction plays a role from the start of the development
process.

The usual Python TensorFlow program starts by importing the TensorFlow library:

    import tensorflow

This line results in loading the TensorFlow library files from the local disk
into the runtime environment.

A program running inside the browser cannot directly access the library files.
Instead, it has to ask a web server to serve the file: (there are other ways to
import files in JavaScript - the results are similar):

    <script src="./node_modules/@tensorflow/tfjs/dist/tf.min.js"></script>

![Web server to access file system](./images/web-server-file-system.png)

<!-- markdownlint-disable -->

[//]: # 'mermaid text for picture above'
[//]: # 'comment syntax from https://stackoverflow.com/a/20885980'

[//]: # '
graph LR
%% Need to comment empty lines for this comment style to work
application(Web Application)
browser(Web Browser)
server(Web Server)
fs(File System)
%%
style server fill:lightblue
%%
application --> browser
browser --> application
browser --> server
server --> browser
server --> fs
fs --> server
'

<!-- markdownlint-enable -->

Therefore, the development environment needs a web server. There are several
alternatives for that.

For simple tests, Python's [http.server](https://docs.python.org/3/library/http.server.html)
is enough ([SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)
for Python 2).

For more complex development work, [Node.js](https://nodejs.org/en/) is a
better alternative. It has several useful packages that speed up the workflow.

One of these useful packages is [live-server](https://www.npmjs.com/package/live-server),
a package that automates another difference in the environment: running the
JavaScript code requires reloading the web page every time any change is made.
[live-server](https://www.npmjs.com/package/live-server) is a web server that
automatically reloads the page when a file changes. Sounds like a small detail,
but manually reloading pages can quickly become-time consuming in the
development workflow.

Once a web server is in place, we can start the typical development workflow:
write code, test code, and (inevitably) debug code.

In the following in sections we will put in place a set of tools for these
steps.

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
    editor with a large collection of plugins. Some of them are listed in
    the items below.
-   Code analysis ([linting](<https://en.wikipedia.org/wiki/Lint_(software)>)):
    it takes time to learn all the subtleties of a new language, and JavaScript
    is not short of them. Some of these subtleties will result in frustrating
    bugs. As a good practice, always have a code analysis tool automatically
    running. [eslint](<https://en.wikipedia.org/wiki/Lint_(software)>) is the
    best linting tool for JavaScript and is available as an extension for
    Visual Studio Code.
-   Code formatting: keep the code consistent and easier to understand. Also
    prepares it for publication, e.g. in GitHub. [prettier](https://prettier.io/)
    is currently the most used formatter. Also available as an extension for
    Visual Studio Code.

### Testing and debugging the code

This section focuses on Chrome. Other browsers have similar tools in place.

There are two ways to debug JavaScript code running in the browser:

-   Directly in the browser with [Chrome DevTools' JavaScript debugger](https://developers.google.com/web/tools/chrome-devtools/javascript/).
-   From Visual Studio Code with the [Chrome Debugger](https://github.com/Microsoft/vscode-chrome-debug).

The first option is the easiest to start with. The second option has more
features and is more useful in the long run, despite requiring a (minor)
configuration step to get started.

TensorFlow.js also comes with a visualization tool, [tfjs-vis](https://github.com/tensorflow/tfjs-vis).
With tfjs-vis we can follow the code behavior in real time. See [this example](https://storage.googleapis.com/tfjs-vis/mnist/dist/index.html).

### Putting it all together

Putting all pieces together to get the development environment in place:

-   Install [Node.js](https://nodejs.org/en/) and the [live-server](https://www.npmjs.com/package/live-server)
    package to serve the files to the code running inside the browser (and to
    automatically refresh the page when code changes).
-   Install [Visual Studio Code](https://code.visualstudio.com/).
-   Install Visual Studio Code extensions for a linter ([eslint](https://eslint.org/))
    and for a code formatter ([prettier](https://prettier.io/)).
-   Learn how to use [Chrome DevTools' JavaScript debugger](https://developers.google.com/web/tools/chrome-devtools/javascript/).
-   Learn how to use [Visual Studio Code's Chrome debugger](https://github.com/Microsoft/vscode-chrome-debug).
-   Learn how to use real-time visualization with [tfjs-vis](https://github.com/tensorflow/tfjs-vis).

## Development workflow

The typical machine learning workflow looks like this:

![Typical training workflow](./images/typical-training-workflow.png)

<!-- markdownlint-disable -->

[//]: # 'mermaid text for picture above'
[//]: # 'comment syntax from https://stackoverflow.com/a/20885980'

[//]: # '
graph LR
%%
acquire(Acquire Data)
preprocess(Preprocess Data)
augment(Augment Data)
train(Train the Model)
fine-tune(Fine tune the Model)
test(Test the Model)
%%
acquire --> preprocess
preprocess --> augment
augment --> train
train --> fine-tune
fine-tune --> train
fine-tune --> test
'

<!-- markdownlint-enable -->

This workflow presents a problem when the code is running inside the browser:
the code does not have easy, fast access to the training data.

When using the same workflow inside a browser, it looks like this:

![Typical training workflow](./images/web-server-training-workflow.png)

<!-- markdownlint-disable -->

[//]: # 'mermaid text for picture above'
[//]: # 'comment syntax from https://stackoverflow.com/a/20885980'

[//]: # '
graph LR
%%
acquire(Acquire Data)
preprocess(Preprocess Data)
augment(Augment Data)
server(Web Server)
train(Train the Model)
fine-tune(Fine tune the Model)
test(Test the Model)
%%
style server fill:lightblue
%%
acquire --> preprocess
preprocess --> augment
augment --> server
server --> train
train --> fine-tune
fine-tune --> train
fine-tune --> test
'

<!-- markdownlint-enable -->

The browser is able to cache images (and other resources) once they are
fetched. However, this is still not as fast as accessing them directly from the
file system, it is subject to the user's control (the user can clear the
browser's cache at any time) and
[getting browser caching to work correctly is not trivial](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching).

The typical machine learning workflow is therefore usable only for a small
amount of training data. For example, when using transfer learning with a
small set of new images captured directly in the browser, with the computer's
camera.

For larger amounts of training data, we need a new workflow: train on a regular
environment (full computer access), then convert the trained model to use it
with TensorFlow.js. With this workflow we can employ all the tools and
techniques from traditional machine learning (when we have access to the full
machine).

> > pic with training, then conversion

Model conversion is done with [`tensorflowjs_converter`](https://www.tensorflow.org/js/guide/conversion).

However, there are some catches:

1. Not all models can be converted. The converter is being improved, but it is
   not able to convert all models yet.
1. Some of the optimizations for size and performance done by the converter do
   not affect the model, but to reduce the model size we need to use
   [quantization](https://medium.com/tensorflow/introducing-the-model-optimization-toolkit-for-tensorflow-254aca1ba0a3).
   Quantization may affect accuracy. It requires at least another verification
   of the model before using it.

With those catches in mind, the workflow requires a few tweaks to find
problems as early as possible.

1.  Train the model for a few epochs only (at this point it doesn't matter if
    the model is accurate).
1.  Save the model.
1.  Attempt to convert it using [tensorflowjs_converter](https://github.com/tensorflow/tfjs-converter).

If conversion works, resume the usual training process to achieve high
accuracy (or F1 score, or other applicable metrics). The goal up to this point
is just to check if there are no errors in the conversion, before committing
to long training times, just to realize that the model cannot be converted.

Once the model is trained, we can quantize it. The goal in this step is to
reduce the model size. However, there are two levels of quantization and they
may affect accuracy in different ways. Another verification step is
therefore needed.

1. Convert the model again with [tensorflowjs_converter](https://github.com/tensorflow/tfjs-converter),
   this time specifying `quantization_bytes`.
1. Check the converted model's accuracy (e.g. with the test dataset).
1. Repeat for different values of `quantization_bytes` (two values are
   currently supported).

### Workflow overview

Putting all steps together, this is how the workflow looks like.

> > pic here

### Training on a server with Node.js

The previous section was written with the assumption that you are familiar and
comfortable with Python or R. That results in the model conversion step in the
workflow.

It is also possible to train on a server using [Node.js](https://www.tensorflow.org/js/guide/nodejs).
This method removes the conversion step and the limitations from the browser
environment (we are now able to access the file system, for example) and gives
access to GPUs. The training performance is
[equivalent to using Python and R](https://groups.google.com/a/tensorflow.org/forum/#!topic/tfjs/4qIbyhrDZC0).

However, the model training step (model fitting) may be a small part of the
complete training workflow. Other steps, like preparing a dataset (for example,
crop images) and augmenting data, may have more code than the actual model
training part. These steps are significantly different in JavaScript because
they use other libraries (Node.js packages) to perform those functions.

The decision point to use Node.js is if you expect to write mostly JavaScript
code. If so, learning how to code these ancillary steps in JavaScript is
worthwhile. If you plan to stick to Python or R most of the time, training in
those environments then converting the trained model to TensorFlow.js is more
effective in the long run.

### Using pretrained models as a starting point

TensorFlow.js has a [repository of pretrained models](https://github.com/tensorflow/tfjs-models).

They are a good starting pointing because not only they are pretrained, but
they are also already converted, so the risk of failing at the conversion step
is gone if you use the model as is, or greatly reduced if you use the model
for transfer learning.

Using one of the pretrained models with transfer learning requires a few
adjustments to the workflow. Transfer learning also implies a training step.
In this case the model is already converted to TensorFlow.js, so the training
process needs to be done with [Node.js](https://www.tensorflow.org/js/guide/nodejs).

Similarly to [training a model from the start with Node.js](#training-with-nodejs),
using this approach requires learning Node.js packages to read and manipulate
the dataset used for the transfer learning training. As an alternative
approach, we can start with a [TensorFlow pretained model](https://github.com/tensorflow/models),
complete the transfer learning process with Python or R, then converted the
final model to TensorFlow.js.

### Training in the browser

It is also possible to train a model in the browser. This method may be
useful for transfer learning with small datasets.

This method has the same caveat as [using Node.js for training](#training-with-nodejs),
learning Node.js packages to read and manipulate the dataset. On the positive
side, it opens the doors for more engaging training processes. For example, we
can use the device's camera and microphones to let the user capture training
data that is applicable to their task.

Running in the browser may also be useful to speed up training in systems that
do not have a GPU. When running in a browser, TensorFlow.js uses WebGL as the
backend. This backend is [up to 100x faster than a CPU (non GPU) backend](https://www.tensorflow.org/js/guide/platform_environment#backends).

## Runtime environment

One characteristic of web applications is that they can run on any type of
device that has a web browser. This can be a blessing or a curse.

For applications that are CPU intensive, like machine learning applications,
not knowing how powerful (or weak) the device is has serious implications for
the model we choose for the task.

The main issue we need to address is the behavior of the model in low-powered
devices, typically a smartphone. Large models may cause three problems in those
devices:

1.  Battery consumption: more calculations imply more CPU usage, which
    implies more battery consumption. One way to make users of smartphones
    unhappy to the point where they give up on your application is to consume
    a lot of battery.
1.  Latency: more calculations also imply it takes longer to make predictions.
    Users have expectations on how long they will wait for a task to complete.
    A more responsive application is obviously better.
1.  Time to download the model: large models take longer to download, of
    course. This is especially important for smartphone connected via a
    cellular network. Although the browser will cache the model after
    downloading it, the initial download affects the first use of the
    application. If it takes too long, users may lose patience and give up on
    the first try.

If we know the web application is running on a well-configured laptop, with a
charged battery, connected to a Wi-Fi network, we can afford to use a more
accurate, more CPU intensive model. On the other, hand if we know the
application is running on a smartphone connected via a cellular network, we may
need to fall back to a less powerful model, such as one based on [MobileNet](https://arxiv.org/abs/1704.04861),
that uses less CPU and has smaller latency.

There are two ways to approach this problem:

1.  Assume the worst: use a small model from the start and serve that model
    to all devices. This works if the small model has enough accuracy.
1.  Serve different models, based on the device. This approach is
    [already used in web applications in general](https://developer.mozilla.org/en-US/docs/Web/Guide/Mobile/Separate_sites)
    and Node.js even has a [library to detect the device type](https://www.npmjs.com/package/express-device).

Whenever possible, pick the first option. The second option results in
maintaining two code bases, or at least two models. This will get expensive
quickly. It is a cost-benefit analysis: if the extra accuracy of the large
model is important for the application, then separate models may be justified.

## Production environment

Once the model is trained and the application is ready to be used, we need
to put it in production. This step is also different from putting a model in
production on a server, for internal use (as opposed to being used through web
browsers).

These are some of the best practices for web development in general, not only
for deploying machine learning models in that environment.

-   [Minify code and other files](https://developers.google.com/speed/docs/insights/MinifyResources):
    minification generates [files that are smaller to download](https://codeengineered.com/blog/why-minify-javascript/).
    This is especially important for smartphone users.
-   [Create bundles](https://developers.google.com/web/fundamentals/performance/webpack/):
    how the code and its resources are split has a significant effect on how
    long it will take to load the application and, more importantly, how long
    it will take for the application to be ready to interact with the user. At
    the time of this writing, the most commonly-used tool for bundling is
    [webpack](https://webpack.js.org/).
-   Version models to update old cached models: when we need to update an
    application on a server (e.g. we have a new model), we can simply replace
    the files and reload the application. Web applications, on the other hand,
    are cached by the browser on the user's device. To make sure the browser
    fetches the updated application, we need to [add a unique fingerprint to the file name](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#invalidating_and_updating_cached_responses)
    to force the browser to load the new version of our model.

There are tools to automate those steps. A good overview is available [here](https://stackoverflow.com/questions/35062852/npm-vs-bower-vs-browserify-vs-gulp-vs-grunt-vs-webpack).

## Appendix

### Loading TensorFlow.js from a content delivery network (CDN)

In this report we assumed that the TensorFlow.js libraries and models are
loaded from the local computer.

Google maintains the libraries and models in unpkg.com, a [content delivery network (CDN)](https://en.wikipedia.org/wiki/Content_delivery_network). This is how it is loaded in the tutorials:

    <script src="https://unpkg.com/@tensorflow/tfjs"></script>

For quick tests and experiments, loading from the CDN is a good way to start.

However, sooner or later we will develop our own models. Once we reach this
point, being able to load these models from the local computer (with Node.js)
will become mandatory.

### Differences from the standard Keras API and behavior

The TensorFlow.js model API is based on the Keras API. Building models will
look familiar from the start. For example, these are the first layers of the
[CNN digit recognizer tutorial](https://www.tensorflow.org/js/tutorials/training/handwritten_digit_cnn):

    const model = tf.sequential();
    model.add(
        tf.layers.conv2d({
            inputShape: [IMAGE_WIDTH, IMAGE_HEIGHT, IMAGE_CHANNELS],
            kernelSize: 5,
            filters: 8,
            strides: 1,
            activation: 'relu',
            kernelInitializer: 'varianceScaling',
        })
    );
    model.add(tf.layers.maxPooling2d({ poolSize: [2, 2], strides: [2, 2] }));

The differences start to appear in the behavior of the APIs. There is a good
summary [in this TensorFlow.js page](https://www.tensorflow.org/js/guide/layers_for_keras_users).
