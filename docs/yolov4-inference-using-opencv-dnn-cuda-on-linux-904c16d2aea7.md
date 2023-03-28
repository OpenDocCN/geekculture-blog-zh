# Linux 下基于 OpenCV-DNN-CUDA 的 YOLOv4 推理

> 原文：<https://medium.com/geekculture/yolov4-inference-using-opencv-dnn-cuda-on-linux-904c16d2aea7?source=collection_archive---------13----------------------->

![](img/d881b6825f340ae6d0fe80b4d4b09463.png)

# 在 Linux 上使用 OpenCV-DNN-CUDA 模块运行 YOLOv4 推理。

在之前的博客[“设置支持 CUDA 后端的 OpenCV-DNN 模块(针对 Linux)”](https://techzizou.com/?p=3766)中，我们在 Linux 上构建了支持 CUDA 后端的 OpenCV-DNN 模块。(我有 Ubuntu 18.04)。我们可以使用这个 OpenCV 模块在对象检测模型上运行推理。与没有 CUDA 后端的 OpenCV 相比，使用 CUDA 后端支持构建的 OpenCV-DNN 模块给出了更快的推断。我们需要做的就是将下面两行代码添加到我们的代码中，并使用它来推断模型。

```
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_CUDA)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CUDA)
```

与不支持 CUDA 后端的 OpenCV 相比，支持 CUDA 后端的 OpenCV-DNN 模块可以更快地进行推理。

按照以下步骤，使用我们安装的 OpenCV-DNN 模块运行推理:

*   首先，打开 Anaconda 提示符并激活您用来安装 OpenCV-DNN 模块的虚拟环境。
*   接下来，下载下面的 python *opencv_dnn.py* 脚本并使用 python 运行它。请注意，这使用了 yolov4 模型权重和配置文件。从[这里](https://github.com/AlexeyAB/darknet)下载。

```
import cv2
import timeCONFIDENCE_THRESHOLD = 0.2
NMS_THRESHOLD = 0.4
COLORS = [(0, 255, 255), (255, 255, 0), (0, 255, 0), (255, 0, 0)]class_names = []
with open("classes.txt", "r") as f:
    class_names = [cname.strip() for cname in f.readlines()]cap = cv2.VideoCapture("video.mp4")net = cv2.dnn.readNet("yolov4.weights", "yolov4.cfg")
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_CUDA)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CUDA_FP16)model = cv2.dnn_DetectionModel(net)
model.setInputParams(size=(416, 416), scale=1/255, swapRB=True)while cv2.waitKey(1) < 1:
    (grabbed, frame) = cap.read()
    if not grabbed:
        exit() start = time.time()
    classes, scores, boxes = model.detect(frame, CONFIDENCE_THRESHOLD, NMS_THRESHOLD)
    end = time.time() for (classid, score, box) in zip(classes, scores, boxes):
        color = COLORS[int(classid) % len(COLORS)]
        label = "%s : %f" % (class_names[classid[0]], score)
        cv2.rectangle(frame, box, color, 2)
        cv2.putText(frame, label, (box[0], box[1]-5), cv2.FONT_HERSHEY_SIMPLEX, 0.5, color, 2)

    fps = "FPS: %.2f " % (1 / (end - start))
    cv2.putText(frame, fps, (0, 25), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 255), 2)
    cv2.imshow("output", frame)
```

您也可以下载上面的 python 代码来测试 OpenCV-DNN 推理。

[**下载 opencv_dnn.py**](https://techzizou.com/yolov4-inference-using-opencv-dnn-cuda-on-linux/#opencv_dnn_linux)

**重要提示:**OpenCV-DNN-CUDA 模块只支持推理。因此，虽然你会从中获得更快的推理，但训练将与我们在没有 CUDA 后端支持的情况下设置的 OpenCV 相同。

您还可以使用 OpenCV-DNN-CUDA 模块对来自 TensorFlow、PyTorch 等各种其他框架的模型进行推理。

# 我在 YouTube 上的视频！

# 之前的教程:

## 使用 CUDA 后端支持设置 OpenCV-DNN 模块(适用于 Linux)

[](https://techzizou007.medium.com/setup-opencv-dnn-module-with-cuda-backend-support-for-linux-1677c627f3bd) [## 使用 CUDA 后端支持设置 OpenCV-DNN 模块(适用于 Linux)

### 在本教程中，我们将使用 CUDA 后端支持从源代码构建 OpenCV(OpenCV-DNN-CUDA 模块)。

techzizou007.medium.com](https://techzizou007.medium.com/setup-opencv-dnn-module-with-cuda-backend-support-for-linux-1677c627f3bd) 

## 别忘了留下👏

## 祝您愉快！！！✌

# ♕·特奇佐·♕