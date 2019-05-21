<!--
author:   André Dietrich
email:    andre.dietrich@ovgu.de
version:  0.0.2
language: en
narrator: US English Female
comment:  Macros for TensorFlowJS

script:   https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@0.13.3/dist/tf.min.js

@onload
window.reportError = function(error) {
  let line = getLineNumber(error);
  let details = [];
  let msg = "An error occured";

  if (line) {
    details = [[{ row : line-1,
               column : 0,
                 text : error.message,
                 type : "error" }]];

    msg += " on line " + line;
  }
  return [ msg + "\n" + error.message, details ];
};
@end

@TF.eval
<script>
async function eee(code) {
  let oldLog = window.console.log;

  window.console.log = console.log;

  try {
    const evalString = '(async function runner() { try { ' + code + '} catch (e) { reportError(e) } })()';

    await eval(evalString).catch(function(e) {
      window.console.log = oldLog;
      let [msg, details] = reportError(e);
      send.lia(msg, details, false);
      send.lia("LIA: stop");
    });
  }
  catch(e) {
    window.console.log = oldLog;
    let [msg, details] = reportError(e);
    send.lia(msg, details, false);
    send.lia("LIA: stop");
  }
  send.lia("LIA: stop");

};
setTimeout(function(e){ eee(`@input`+"\n") }, 10);
"LIA: wait";
</script>
@end


@TF.eval2
<script>
async function eee() {
  let file1 = `@input(0)` + "\n";
  let file2 = `@input(1)` + "\n";
  let code  = file1 + file2;
  let oldLog = window.console.log;

  window.console.log = console.log;

  try {
    const evalString = '(async function runner() { try { ' + code + '} catch (e) { reportError(e) } })()';

    await eval(evalString).catch(function(e) {
      window.console.log = oldLog;
      let [msg, details] = reportError(e);
      send.lia(msg, details, false);
      send.lia("LIA: stop");
    });
  }
  catch(e) {
    window.console.log = oldLog;
    let [msg, details] = reportError(e);
    send.lia(msg, details, false);
    send.lia("LIA: stop");
  }
  send.lia("LIA: stop");
};
setTimeout(function(e){ eee() }, 100);
"LIA: wait";
</script>
@end


-->

# TensorflowJS - Template

This is a template for developing interactive machine-learning courses with
[LiaScript](https://LiaScript.github.io) and
[TensorFlow.js](https://js.tensorflow.org).

__Try it on LiaScript:__

https://liascript.github.io/course/?https://raw.githubusercontent.com/liaTemplates/tensorflowjs/master/README.md

__See the project on Github:__

https://github.com/liaTemplates/tensorflowjs


                         --{{1}}--
There are three ways to use this template. The easiest way is to use the
`import` statement and the url of the raw text-file of the master branch or any
other branch or version. But you can also copy the required functionionality
directly into the header of your Markdown document, see therefor the
[last slide](#3). And of course, you could also clone this project and change
it, as you wish.

                           {{1}}
1. Load the macros via

   `import: https://raw.githubusercontent.com/liaTemplates/tensorflowjs/master/README.md`

2. Copy the definitions into your Project

3. Clone this repository on GitHub


## `@TF.eval`


                         --{{0}}--
Add the macro `@TF.eval` to the end of every JavaScript code-block that runs
some TensorFlow code and that you want to make editable in LiaScript. The given
code gets evaluated asynchroniously and the result is shown in a console below.


```javascript
// Notice there is no 'import' statement. 'tf' is available on the index-page
// because of the script tag above.

// Define a model for linear regression.
const model = tf.sequential();
model.add(tf.layers.dense({units: 1, inputShape: [1]}));

// Prepare the model for training: Specify the loss and the optimizer.
model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

// Generate some synthetic data for training.
const xs = tf.tensor2d([1, 2, 3, 4], [4, 1]);
const ys = tf.tensor2d([1, 3, 5, 7], [4, 1]);

// Train the model using the data.
model.fit(xs, ys, {epochs: 10}).then(() => {
  // Use the model to do inference on a data point the model hasn't seen before:
  // Open the browser devtools to see the output
  model.predict(tf.tensor2d([5], [1, 1])).print();
});
```
@TF.eval

## Implementation

                         --{{0}}--
The code shows how the macro `@TF.eval` is implemented. The script command at
the top loads the TensorFlowJS javascript library and onload is used to define
function reportError at the initialization phase.

``` html
script:   https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@0.13.3/dist/tf.min.js

@onload
window.reportError = function(error) {
  let line = getLineNumber(error);
  let details = [];
  let msg = "An error occured";

  if (line) {
    details = [[{ row : line-1,
               column : 0,
                 text : error.message,
                 type : "error" }]];

    msg += " on line " + line;
  }
  return [ msg + "\n" + error.message, details ];
};
@end

@TF.eval
<script>
async function eee(code) {
  let oldLog = window.console.log;

  window.console.log = console.log;

  try {
    const evalString = '(async function runner() { try { ' + code + '} catch (e) { reportError(e) } })()';

    await eval(evalString).catch(function(e) {
      window.console.log = oldLog;
      let [msg, details] = reportError(e);
      send.lia(msg, details, false);
      send.lia("LIA: stop");
    });
  }
  catch(e) {
    window.console.log = oldLog;
    let [msg, details] = reportError(e);
    send.lia(msg, details, false);
    send.lia("LIA: stop");
  }
  send.lia("LIA: stop");

};
setTimeout(function(e){ eee(`@input`+"\n") }, 10);
"LIA: wait";
</script>
@end
```

                         --{{1}}--
If you want to minimize loading effort in your LiaScript project, you can also
copy this code and paste it into your main comment header, see the code in the
raw file of this document.

                           {{1}}
https://raw.githubusercontent.com/liaTemplates/tensorflowjs/master/README.md
