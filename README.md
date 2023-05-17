# License-Plate-Detection-and-Blur

# License Plate Detection and Blurring Using Yolov4, Openvino, for Video Surveillance
## Convert yolov4 model (.weights) into Tensorflow (.pb) format

Download yolov4 .weights, .config, and .names files

https://drive.google.com/file/d/1ffeOmcTy7yDPOXSgrr_lR2bNSIBJCZK3/view?usp=sharing

Download video for inference

https://drive.google.com/file/d/1xxcud0qlQ4xcEgrjNntLGmNlxncuTZRU/view?usp=sharing

Clone the repo below & cd into it

https://github.com/theAIGuysCode/tensorflow-yolov4-tflite.git

Create and activate YOLOv4 Virtual environment.

```
python -m venv tf-yolov4
.\tf-yolov4\Scripts\activate
```

Install requirements

```
pip install -r requirements.txt
```

Edit __C.YOLO.CLASSES in '.\core\config.py' to path to appropriate classes
<p align="center"><img src="helpers/custom_config.png" width="640"\></p>

Place downloaded yolov4 model in '.\data'

Convert yolov4 model to TF format

```
python save_model.py --weights .\data\custom.weights --output .\checkpoints\custom-416 --input_size 416 --model yolov4 
```

## Convert Tensorflow (.pb) model into Openvino IR model (.xml) format and run inference

Clone this repo & cd into it

Create and activate Openvino Virtual environment.

```
python -m venv yolov4-openvino
.\yolov4-openvino\Scripts\activate
```

Install requirements

```
pip install -r requirements.txt
```
Extract github.rar into repo

Move all files from'.\checkpoints\custom-416' in to '.\openvino\public\yolo-v4-tf\yolo-v4.savedmodel'

Move downloaded custom.weights file to '\openvino\public\yolo-v4-tf'

Convert yolov4 model to Openvino(IR) format

```
omz_converter --name yolo-v4-tf --download_dir openvino --precisions FP32
```

Move obj.names to .\openvino\public\yolo-v4-tf\FP32

Run inference without blurring

```
python object_detection.py -m .\openvino\public\yolo-v4-tf\FP32\yolo-v4-tf.xml -at yolov4 --labels .\openvino\public\yolo-v4-tf\FP32\obj.names -i lp.mp4 - output.mp4
```

Run inference with blurring

```
python object_detection.py -m .\openvino\public\yolo-v4-tf\FP32\yolo-v4-tf.xml -b -at yolov4 --labels .\openvino\public\yolo-v4-tf\FP32\obj.names -i lp.mp4 - output.mp4
```

## Solution to Errors
No module 'tools.infer'

This problem is caused by conflicting package names, namely the tools package.
To solve this problem just import tools from paddleocr as shown below
<p align="center"><img src="helpers/img1.png" width="640"\></p>
<p align="center"><img src="helpers/img2.png" width="640"\></p>
