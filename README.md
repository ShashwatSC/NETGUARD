# facedetection

This is an open source library for CNN-based face detection in images. The CNN model has been converted to static variables in C source files. The source code does not depend on any other libraries. What you need is just a C++ compiler. You can compile the source code under Windows, Linux, ARM and any platform with a C++ compiler.

SIMD instructions are used to speed up the detection. You can enable AVX2 if you use Intel CPU or NEON for ARM.

The model files are provided in `src/facedetectcnn-data.cpp` (C++ arrays) & [the model (ONNX) from OpenCV Zoo](https://github.com/opencv/opee/master/models/face_detection_yunet). You can try our scripts (C++ & Python) in `opencv_dnn/` with the ONNX model. View the network architecture [here](https://netron.app/?url=https://raw.githubusercontent.com/detection.train/master/onnx/yunet*.onnx).

Please note that OpenCV DNN does not support the latest version of YuNet with dynamic input shape. Please ensure you have the exact same input shape as the one in the ONNX model to run latest YuNet with OpenCV DNN.

examples/detect-image.cpp and examples/detect-camera.cpp show how to use the library.

The library was trained by [libfacedetection.train](https://giiYu/libfacedetection.train).

![Examples](/images/cnnr.png "Detection example")

## How to use the code

You can copy the files in directory src/ into your project,
and compile them as the other files in your project.
The source code is written in standard C/C++.
It should be compiled at any platform which supports C/C++.

Some tips:

  * Please add facedetection_export.h file in the position where you copy your facedetectcnn.h files, add #define FACEDETECTION_EXPORT to  facedetection_export.h file. See: [issues #222](https://github.com/Shiqdetection/issues/222)
  * Please add -O3 to turn on optimizations when you compile the source code using g++.
  * Please choose 'Maximize Speed/-O2' when you compile the source code using Microsoft Visual Studio.
  * You can enable OpenMP to speedup. But the best solution is to call the detection function in different threads.

You can also compile the source code to a static or dynamic library, and then use it in your project.

[How to compile](COME.md)


## CNN-based Face Detection on Intel CPU

Using **AVX2** instructions
| Method             |Time          | FPS         |Time          | FPS         |
|--------------------|--------------|-------------|--------------|-------------|
|                    |  X64         |X64          |  X64         |X64          |
|                    |Single-thread |Single-thread|Multi-thread  |Multi-thread |
|cnn (CPU, 640x480)  |  50.02ms     |  19.99      |   6.55ms     |  152.65     |
|cnn (CPU, 320x240)  |  13.09ms     |  76.39      |   1.82ms     |  550.54     |
|cnn (CPU, 160x120)  |   3.61ms     | 277.37      |   0.57ms     | 1745.13     |
|cnn (CPU, 128x96)   |   2.11ms     | 474.60      |   0.33ms     | 2994.23     | 

Using **AVX512** instructions
| Method             |Time          | FPS         |Time          | FPS         |
|--------------------|--------------|-------------|--------------|-------------|
|                    |  X64         |X64          |  X64         |X64          |
|                    |Single-thread |Single-thread|Multi-thread  |Multi-thread |
|cnn (CPU, 640x480)  |  46.47ms     |  21.52      |   6.39ms     |  156.47     |
|cnn (CPU, 320x240)  |  12.10ms     |  82.67      |   1.67ms     |  599.31     |
|cnn (CPU, 160x120)  |   3.37ms     | 296.47      |   0.46ms     | 2155.80     |
|cnn (CPU, 128x96)   |   1.98ms     | 504.72      |   0.31ms     | 3198.63     | 

* Minimal face size ~10x10
* Intel(R) Core(TM) i7-7820X CPU @ 3.60GHz
* Multi-thread in 16 threads and 16 processors.


## CNN-based Face Detection on ARM Linux (Raspberry Pi 4 B)

| Method             |Time          | FPS         |Time          | FPS         |
|--------------------|--------------|-------------|--------------|-------------|
|                    |Single-thread |Single-thread|Multi-thread  |Multi-thread |
|cnn (CPU, 640x480)  |  404.63ms    |  2.47       |  125.47ms    |   7.97      |
|cnn (CPU, 320x240)  |  105.73ms    |  9.46       |   32.98ms    |  30.32      |
|cnn (CPU, 160x120)  |   26.05ms    | 38.38       |    7.91ms    | 126.49      |
|cnn (CPU, 128x96)   |   15.06ms    | 66.38       |    4.50ms    | 222.28      |

* Minimal face size ~10x10
* Raspberry Pi 4 B, Broadcom BCM2835, Cortex-A72 (ARMv8) 64-bit SoC @ 1.5GHz
* Multi-thread in 4 threads and 4 processors.
