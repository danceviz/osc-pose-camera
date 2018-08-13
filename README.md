# DanceViz OSC Camera

Application (OSC server), that streams skeletal data,
captured in real time by Neural Network from webcam (see [PoseNet whitepaper](https://arxiv.org/pdf/1803.08225.pdf) and [tensorflowjs model](https://github.com/tensorflow/tfjs-models/tree/master/posenet)).    

Can be used in various scenarious, see examples [by RunWayML](https://github.com/runwayml):

* [__Unity__ (C#)](https://github.com/runwayml/examples_unity) Skeletal animation recorder.
* [__Processing__ (Java)](https://github.com/runwayml/examples_processing/blob/master/openpose/openpose.pde)
* [__OpenFrameworks__ (C++)](https://github.com/runwayml/examples_openFrameworks/blob/master/openpose/src/ofApp.cpp)
* __Arduino__
* __PureData__
* __Max/MSP__
* __TouchDesigner__
* __VVVV, VDMX, Resolume, MadMapper__ and other VJ'ing software, that can recieve OSC signals.

Also checkout __[Wekinator](http://www.wekinator.org/)__ — tool for creating gestures by analyzing the OSC data.

## Keypoints

<img src="https://raw.githubusercontent.com/irealva/tfjs-models/master/posenet/demos/camera.gif"/>

The body parts and their ids are:

| Id | Part |
| -- | -- |
| 0 | nose |
| 1 | leftEye |
| 2 | rightEye |
| 3 | leftEar |
| 4 | rightEar |
| 5 | leftShoulder |
| 6 | rightShoulder |
| 7 | leftElbow |
| 8 | rightElbow |
| 9 | leftWrist |
| 10 | rightWrist |
| 11 | leftHip |
| 12 | rightHip |
| 13 | leftKnee |
| 14 | rightKnee |
| 15 | leftAnkle |
| 16 | rightAnkle |

All keypoints are indexed by part id.

## Configuration

PoseNet runs with either a <strong>single-pose</strong> or <strong>multi-pose</strong> detection algorithm. The single person pose detector is faster and more accurate but requires only one subject present in the image.
<br>
<br> The <strong>output stride</strong> and <strong>image scale factor</strong> have the largest effects on accuracy/speed. A <i>higher</i> output stride results in lower accuracy but higher speed. A <i>higher</i> image scale factor results in higher accuracy but lower speed.

* **multiplier** - An optional number with values: `1.01`, `1.0`, `0.75`, or `0.50`. Defaults to `1.01`.   It is the float multiplier for the depth (number of channels) for all convolution operations. The value corresponds to a MobileNet architecture and checkpoint.  The larger the value, the larger the size of the layers, and more accurate the model at the cost of speed.  Set this to a smaller value to increase speed at the cost of accuracy.

**By default,** PoseNet loads a model with a **`1.01`** multiplier.  This is recommended for computers with **powerful GPUs.**  A model with a **`0.75`** muliplier is recommended for computers with **mid-range/lower-end GPUS.** A model with a **`0.50`** architecture is recommended for **mobile.**

### Single-Person Pose Estimation

Single pose estimation is the simpler and faster of the two algorithms. Its ideal use case is for when there is only one person in the image. The disadvantage is that if there are multiple persons in an image, keypoints from both persons will likely be estimated as being part of the same single pose—meaning, for example, that person #1’s left arm and person #2’s right knee might be conflated by the algorithm as belonging to the same pose.

* **imageScaleFactor** - A number between 0.2 and 1.0. Defaults to 0.50.   What to scale the image by before feeding it through the network.  Set this number lower to scale down the image and increase the speed when feeding through the network at the cost of accuracy.
* **flipHorizontal** - Defaults to false.  If the poses should be flipped/mirrored  horizontally.  This should be set to true for videos where the video is by default flipped horizontally (i.e. a webcam), and you want the poses to be returned in the proper orientation.
* **outputStride** - the desired stride for the outputs when feeding the image through the model.  Must be 32, 16, 8.  Defaults to 16.  The higher the number, the faster the performance but slower the accuracy, and visa versa.

## Result

It returns a `pose` with a confidence score and an array of keypoints indexed by part id, each with a score and position.

```js
GET /pose
{
  "score": 0.32371445304906,
  "keypoints": [
    {
      "position": {
        "y": 76.291801452637,
        "x": 253.36747741699
      },
      "part": "nose",
      "score": 0.99539834260941
    },
    {
      "position": {
        "y": 71.10383605957,
        "x": 253.54365539551
      },
      "part": "leftEye",
      "score": 0.98781454563141
    },
    {
      "position": {
        "y": 71.839515686035,
        "x": 246.00454711914
      },
      "part": "rightEye",
      "score": 0.99528175592422
    },
    {
      "position": {
        "y": 72.848854064941,
        "x": 263.08151245117
      },
      "part": "leftEar",
      "score": 0.84029853343964
    },
    {
      "position": {
        "y": 79.956565856934,
        "x": 234.26812744141
      },
      "part": "rightEar",
      "score": 0.92544466257095
    },
    {
      "position": {
        "y": 98.34538269043,
        "x": 399.64068603516
      },
      "part": "leftShoulder",
      "score": 0.99559044837952
    },
    {
      "position": {
        "y": 95.082359313965,
        "x": 458.21868896484
      },
      "part": "rightShoulder",
      "score": 0.99583911895752
    },
    {
      "position": {
        "y": 94.626205444336,
        "x": 163.94561767578
      },
      "part": "leftElbow",
      "score": 0.9518963098526
    },
    {
      "position": {
        "y": 150.2349395752,
        "x": 245.06030273438
      },
      "part": "rightElbow",
      "score": 0.98052614927292
    },
    {
      "position": {
        "y": 113.9603729248,
        "x": 393.19735717773
      },
      "part": "leftWrist",
      "score": 0.94009721279144
    },
    {
      "position": {
        "y": 186.47859191895,
        "x": 257.98034667969
      },
      "part": "rightWrist",
      "score": 0.98029226064682
    },
    {
      "position": {
        "y": 208.5266418457,
        "x": 284.46710205078
      },
      "part": "leftHip",
      "score": 0.97870296239853
    },
    {
      "position": {
        "y": 209.9910736084,
        "x": 243.31219482422
      },
      "part": "rightHip",
      "score": 0.97424703836441
    },
    {
      "position": {
        "y": 281.61965942383,
        "x": 310.93188476562
      },
      "part": "leftKnee",
      "score": 0.98368924856186
    },
    {
      "position": {
        "y": 282.80120849609,
        "x": 203.81164550781
      },
      "part": "rightKnee",
      "score": 0.96947449445724
    },
    {
      "position": {
        "y": 360.62716674805,
        "x": 292.21047973633
      },
      "part": "leftAnkle",
      "score": 0.8883239030838
    },
    {
      "position": {
        "y": 347.41177368164,
        "x": 203.88229370117
      },
      "part": "rightAnkle",
      "score": 0.8255187869072
    }
  ]
}
```
