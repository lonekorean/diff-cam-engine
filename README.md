# diff-cam-engine

Core engine for building motion detection web apps.

### Usage

`diff-cam-engine.js` provides a `DiffCamEngine` object that accesses the webcam, captures images from it, and evaluates motion.

If you have browser compatibility issues, you can try the `adapter.js` shim, which is available here: https://github.com/webrtc/adapter. Add it before `diff-cam-engine.js`.

With that in place, call `DiffCamEngine.init()` to initialize. This will set things up and ask the user for permission to access the webcam.

``` javascript
DiffCamEngine.init({
	// config options go here
});
```

Then call `start()` to begin the actual motion sensing. Do not call `start()` before `init()` has finished. It's a good idea to call `start()` from `initSuccessCallback()` (more on this later).

Call `stop()` to turn things off.

### Configuration

You can customize how `DiffCamEngine` behaves by passing an object of name/value pairs into `init()`. For example:

``` javascript
DiffCamEngine.init({
	video: myVideo,
	captureIntervalTime: 50,
	captureCallback: myCaptureHandler
	// etc.
});
```

##### Variables

The following variables can be passed into `init()`:

| Name | Default | Description |
| --- | --- | --- |
| video | internal &lt;video&gt; (not visible) | The &lt;video&gt; element for showing the live webcam stream |
| motionCanvas | internal &lt;canvas&gt; (not visible) | The &lt;canvas&gt; element for showing the visual motion heatmap |
| captureIntervalTime | 100 | Number of ms between capturing images from the stream |
| captureWidth | 640 | Width of captured images from stream |
| captureHeight | 480 | Height of capture images from stream |
| diffWidth | 64 | Width of (usually downsized) images used for diffing and showing motion |
| diffHeight | 48 | Height of (usually downsized) images used for diffing and showing motion |
| pixelDiffThreshold | 32 | Minimum difference in a pixel to be considered changed |
| scoreThreshold | 16 | Minimum number of changed pixels for an image to be considered as having motion |
| imageMimeType | "image/jpeg" | mime type string e.g. "image/jpeg", "image/png" |
| jpegQuality | 0.7 | "image/jpeg" quality value between 0 and 1 |
| includeMotionBox | false | Flag to calculate and display (on motionCanvas) the bounding box of motion |
| includeMotionPixels | false | Flag to include data indicating all the changed pixels |

##### Callbacks

There are also a couple callback functions you can specify. This is the primary way of interacting with `DiffCamEngine` during execution. Pass these into `init()` just like the variables above.

| Name | Description |
| --- | --- |
| initSuccessCallback | Called when init has successfully completed |
| initErrorCallback | Called when init fails |
| startCompleteCallback | Called once the webcam has begun streaming |
| captureCallback | Called after a captured image from the stream has been evaluated |

`captureCallback` is the only one with a parameter. This parameter is an object with multiple properties on it. These properties are:

| Property | Description |
| --- | --- |
| imageData | The imageData object for the captured image |
| score | The evaluated score for the captured image |
| hasMotion | Whether or not the score meets the score threshold for motion |
| motionBox | An object containg x min/max and y min/max for a box wrapping the region in which motion occurred, only returned if includeMotionBox is `true` |
| motionPixels | An object containg each pixel in the image that changed (indicating motion), only returned if includeMotionPixels is `true` |
| getURL | Convenience function, returns imageData converted to a URL suitable for setting as the src of an image |
| checkMotionPixel | Convenience function, takes in an x and y cooridnate, returns a boolean indicating if the pixel at that location has changed |

### Functions

`DiffCamEngine` exposes the following public functions:

| Function | Description |
| --- | --- |
| init | Initializes everything and requests permission to access the webcam |
| start | Begin streaming from the webcam |
| stop | Stop streaming from the webcam |
| getPixelDiffThreshold | Get pixelDiffThreshold during execution |
| setPixelDiffThreshold | Set pixelDiffThreshold during execution |
| getScoreThreshold | Get scoreThreshold during execution |
| setScoreThreshold | Set scoreThreshold during execution |

### Examples

Check out https://github.com/lonekorean/diff-cam-scratchpad (specifically /diff-cam-example and /turret-security) for examples that use diff-cam-engine. Check out https://github.com/lonekorean/diff-cam-feed for a more complex web app powered by diff-cam-engine.
