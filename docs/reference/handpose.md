# HandPose

<center>
  <img class="header-img" src="assets/header-handpose.png" alt="HandPose Header Image" >
  <p class="img-credit"> Image Credit: <a href="https://thenounproject.com/creator/dinosoftlab/" target="_blank" title="DinosoftLabs">DinosoftLabs</a> | <a href='mailto:info@ml5js.org'>Contribute ♥️</a> </p>
</center>

## Description

HandPose is a machine-learning model that allows for palm detection and hand-skeleton finger tracking in the browser. It can detect multiple hands at a time and for each hand, and provides 21 2D and 3D hand keypoints that describe important locations on the palm and fingers.

The ml5.js HandPose model is based on the [HandPose implementation](https://github.com/google/mediapipe/blob/master/docs/solutions/hands.md) by TensorFlow.js.

The following functionality is provided:

- **Hand Keypoint Detection**: HandPose can detect the 2D and 3D coordinates of 21 keypoints on a hand.
- **Handedness**: HandPose can determine the handedness (left or right) of the detected hand.
- **Multiple Hands**: HandPose can detect multiple hands at the same time.

## Quick Start

Run and explore a pre-built example! [This HandPose example](https://editor.p5js.org/ml5/sketches/QGH3dwJ1A) displays 21 hand keypoints that describe the geometry of each hand in real-time from the webcam.

</br>

[DEMO](iframes/handpose ":include :type=iframe width=100% height=550px")

## Examples

- [HandPose Keypoints](https://editor.p5js.org/ml5/sketches/QGH3dwJ1A): Draw the keypoints of the detected hand from the webcam.
- [HandPose Single Image](https://editor.p5js.org/ml5/sketches/8VK_l3XwE): Detect the keypoints of the hand from a single image.
- [HandPose Parts](https://editor.p5js.org/ml5/sketches/DNbSiIYKB): Draw specific hand parts of the detected hand.
- [HandPose Start-stop](https://editor.p5js.org/ml5/sketches/W9vFFT5RM): Start and stop the detection of the hand.
- [HandPose Skeletal Connections](https://editor.p5js.org/ml5/sketches/fnboooD-K): Draw skeletal connections of the hand.

## Step-by-Step Guide

Now, let's together build the [HandPose Keypoints example](https://editor.p5js.org/ml5/sketches/QGH3dwJ1A) from scratch, and in the process, learn how to use the HandPose model.

### Create a new project

To follow along, start by creating an empty project in the [p5.js web editor](https://editor.p5js.org/).

### Set up ml5.js

Import the ml5.js library in your `index.html` file.

```html
<script src="https://unpkg.com/ml5@1/dist/ml5.js"></script>
```

?> If you are not familiar with how to import the ml5.js library and need more detailed guidance, please check out our [Getting Started](/?id=set-up-ml5js) page.

### Load model

Let's open the `sketch.js` file and define a variable to store the HandPose model.

```javascript
let handPose;
```

Now, we can load the HandPose model in the `preload` function. Using the `preload` function ensures that the model is loaded before the `setup` and `draw` functions are called.

```javascript
function preload() {
  handPose = ml5.handPose();
}
```

### Fetch webcam video

Let's define a variable `video` to store the webcam video.

```javascript
let video;
```

Resize the canvas dimensions to 640x480, a common resolution for webcams.

```javascript
function setup() {
  createCanvas(640, 480);
```

Fetch the webcam video, resize it to fit the canvas, and hide it from the display.

```javascript
  // Create the video and hide it
  video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
}
```

### Detect hand keypoints with the model

Before we use the HandPose model to detect hand keypoints, we need to define a variable `hands` to store the detected hands. Note that the `hands` variable will store an array of detected hands, and each hand has a property `keypoints` that will contain an array of keypoints.

```javascript
let hands = [];
```

To start detecting the keypoints of the hands, in the `setup` function, we need to call the `detectStart` method of the `handPose` object. This method takes the webcam video as input and a callback function to handle the output.

```javascript
function setup() {
  // ...
  video.hide();

  // Start detecting hands from the webcam video
  handPose.detectStart(video, gotHands);
}
```

The `gotHands()` function is a callback function that will be called when the `handPose.detectStart()` method detects hands. Once the hands are detected, the output `results` will be passed to `gotHands()`, and then saved to the `hands` variable.

```javascript
// Callback function for when handPose outputs data
function gotHands(results) {
  // Save the output to the hands variable
  hands = results;
}
```

### Draw the keypoints on the canvas

Before we draw the keypoints, we need to draw the webcam video on the canvas.

```javascript
function draw() {
  image(video, 0, 0, width, height);
```

Now, we can loop through the `hands` array, fetch the `i`th dectected hand, and store it in the `hand` variable.

```javascript
  // Draw all the tracked hand points
  for (let i = 0; i < hands.length; i++) {
    let hand = hands[i];
```

Iterate though all the keypoints of the `i`th detected hand, fetch the `j`th keypoint, and store it in the `keypoint` variable.

```javascript
    for (let j = 0; j < hand.keypoints.length; j++) {
      let keypoint = hand.keypoints[j];
```

Finally, draw a green circle at the location of the `j`th keypoint.

```javascript
      fill(0, 255, 0);
      noStroke();
      circle(keypoint.x, keypoint.y, 10);
    }
  }
}
```

Note we are iterating through all the keypoints (`j` is ranging from 0 to the length of the keypoints array) of the detected hand (`i` is ranging from 0 to the length of the hands array). This will result in green landmarks on all detected hand(s) in the webcam video.

### Run your sketch

Voila! You have successfully built the HandPose Keypoints example. Press the <img class="inline-img" src="assets/facemesh-arrow-forward.png" alt="run button icon" aria-hidden="true"> `run` button to see the code in action. You can also find the [complete code](https://editor.p5js.org/ml5/sketches/QGH3dwJ1A) in the p5.js web editor.

?> If you have any questions or spot something unclear in this step-by-step code guide, we'd love to hear from you! Join us on [Discord](https://discord.com/invite/3CVauZMSt7) and let us know how we can make it better.

## Methods

### ml5.handPose()

This method is used to initialize the handPose object.

```javascript
const handPose = ml5.handPose(?options, ?callback);
```

**Parameters:**

- **options**: Optional. An object to change the default configuration of the model. The default and available options are:

  ```javascript
  {
    maxHands: 2,
    flipped: false,
    runtime: "tfjs",
    modelType: "full",
    detectorModelUrl: undefined, //default to use the tf.hub model
    landmarkModelUrl: undefined, //default to use the tf.hub model
  }
  ```

  Options for hand detection:

  - _maxHands_ - Optional
    - Number: The maximum number of hands to detect. Default: 2.
  - _modelType_ - Optional
    - String: The type of model to use: "lite" or "full". Default: "full".
  - _flipped_ - Optional
    - Boolean: Flip the result data horizontally. Default: false.
  - _runtime_ - Optional
    - String: The runtime of the model: "mediapipe" or "tfjs". Default: "tfjs".

  For using custom or offline models:

  - _solutionPath_ - Optional
    - String: The file path or URL to the model. Only used when using "mediapipe" runtime.
  - _detectorModelUrl_ - Optional
    - String: The file path or URL to the hand detector model. Only used when using "tfjs" runtime.
  - _landmarkModelUrl_ - Optional
    - String: The file path or URL to the hand landmark model. Only used when using "tfjs" runtime.

  More info on options [here](https://github.com/tensorflow/tfjs-models/tree/master/hand-pose-detection/src/mediapipe#create-a-detector) for "mediapipe" runtime.

  More info on options [here](https://github.com/tensorflow/tfjs-models/tree/master/hand-pose-detection/src/tfjs#create-a-detector) for "tfjs" runtime.

- **callback(handPose, error)**: Optional. A function to run once the model has been loaded. Alternatively, call `ml5.handPose()` within the p5 `preload` function.

**Returns:**

- **Object**: The handPose object. This object contains the methods to start and stop the hand pose detection process.

---

### handPose.detectStart()

This method repeatedly outputs hand estimations on an image media through a callback function.

```javascript
handPose.detectStart(media, callback);
```

**Parameters:**

- **media**: An HTML or p5.js image, video, or canvas element to run the estimation on.
- **callback(results, error)**: A callback function to handle the output of the estimation. See below for an example output passed into the callback function:

  ```javascript
  [
    {
      confidence,
      handedness,
      keypoints: [{ x, y, confidence, name }, ...],
      keypoints3D: [{ x, y, z, confidence, name }, ...],
      index_finger_dip: { x, y, x3D, y3D, z3D },
      index_finger_mcp: { x, y, x3D, y3D, z3D },
      ...
    }
    ...
  ]
  ```

  See the diagram below for the position of each keypoint.

  <center>
      <img alt="handPose keypoints diagram" width="600" src="assets/handpose-keypoints-map.png">
  </center>

---

### handPose.detectStop()

This method can be called to stop the continuous pose estimation process.

```javascript
handPose.detectStop();
```

For example, you can toggle the hand pose estimation with click event in p5.js by using this function as follows:

```javascript
// Toggle detection when mouse is pressed
function mousePressed() {
  toggleDetection();
}

// Call this function to start and stop detection
function toggleDetection() {
  if (isDetecting) {
    handPose.detectStop();
    isDetecting = false;
  } else {
    handPose.detectStart(video, gotHands);
    isDetecting = true;
  }
}
```

---

### handPose.detect()

This method asynchronously outputs a single hand estimation on an image media when called.

```javascript
handPose.detect(media, ?callback);
```

**Parameters:**

- **media**: An HTML or p5.js image, video, or canvas element to run the estimation on.

- **callback(results, error)**: Optional. A callback function to handle the output of the estimation, see output example above.

---

### handPose.getConnections()

This method returns the skeletal connection information between the hand keypoints in array format.

```javascript
const connections;
function setup() {
  ...
  connections = handPose.getConnections();
  ...
}
```

**Returns:**

- **Array**: An array of arrays representing the connections between keypoints.

  ```js
  [[0, 1], [1, 2], [2, 3], ...[17, 18], [18, 19], [19, 20]];
  ```

Please refer to this image to understand the connections:

  <center>
      <img alt="handPose keypoints diagram" width="600" src="assets/handpose-connections.png">
  </center>
