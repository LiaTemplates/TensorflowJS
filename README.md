<!--
author:   Your Name

email:    your@mail.org

version:  0.0.1

language: en

narrator: US English Female

comment:  Try to write a short comment about
          your course, multiline is also okay.

script:   https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@0.13.3/dist/tf.min.js


@eval
<script>
function reportError(error) {
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
  send.lia("eval", msg + "\n" + error.message, details, false);
};

async function eee(code) {
  let oldLog = console.log;

  console.log = function(e){ send.lia("output", e + "\n") };

  try {
    const evalString = '(async function runner() { try { ' + code + '} catch (e) { reportError(e) } })()';

    await eval(evalString).catch(function(e) {
      reportError(e);
      console.log = oldLog;
    });
    send.lia("eval", "LIA: stop");
  }
  catch(e) {
    console.log = oldLog;
    reportError(e);
  }
};
setTimeout(function(e){ eee(`@input`+"\n") }, 100);
"LIA: wait";
</script>
@end


@eval2
<script>
function reportError(error) {
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
  send.lia("eval", msg + "\n" + error.message, details, false);
};

async function eee() {
  let file1 = `@input(0)` + "\n";
  let file2 = `@input(1)` + "\n";
  let oldLog = console.log;

  console.log = function(e){ send.lia("output", e + "\n") };

  try {
    const evalString = '(async function runner() { try { ' + file1 + file2 + '} catch (e) { reportError(e) } })()';

    await eval(evalString).catch(function(e) {
      reportError(e);
      console.log = oldLog;
    });
    send.lia("eval", "LIA: stop");
  }
  catch(e) {
    console.log = oldLog;
    reportError(e);
  }
};
setTimeout(function(e){ eee(`@input`+"\n") }, 100);
"LIA: wait";
</script>
@end


-->

# TensorJS Template

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
@eval
