# Project: Deploy a People Counter App at the Edge

Use a useful person detection model (https://docs.openvinotoolkit.org/latest/omz_models_public_mobilenet_ssd_mobilenet_ssd.html) and convert it to an Intermediate Representation for use with the Model Optimizer. Utilizing the Inference Engine of OpenVINO, it will perform inference on an input video, and extract useful data concerning the count of people in frame and how long they stay in frame. Then send this information over MQTT, as well as sending the output frame, in order to view it from a separate UI server over a network.

## Explaining People Counter App

1. Download model 
/opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name \*ssd\* -o .

2. To use mo.py to convert mobilenet_ssd model to IR xml/bin format types (in folder /home/workspace/public/mobilenet-ssd) as following,

root@5efb74f6c3a1:/home/workspace/public/mobilenet-ssd# ls -lt
total 45376
-rw-r--r-- 1 root root    15884 Dec 24 04:06 mobilenet-ssd.mapping
-rw-r--r-- 1 root root    66654 Dec 24 04:06 mobilenet-ssd.xml
-rw-r--r-- 1 root root 23133680 Dec 24 04:06 mobilenet-ssd.bin
-rw-r--r-- 1 root root 23147564 Dec 24 03:59 mobilenet-ssd.caffemodel
-rw-r--r-- 1 root root    29353 Dec 24 03:59 mobilenet-ssd.prototxt
root@5efb74f6c3a1:/home/workspace/public/mobilenet-ssd# 

3. Launch MQTT/UI/FFmpeg services

4. Issue following command for test app,
python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m /home/workspace/public/mobilenet-ssd/./mobilenet-ssd.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3004/fac.ffm

5. Please refer the test result below,
https://raw.githubusercontent.com/zcwang/nd131-openvino-fundamentals-project-starter/master/Project_Result-Gary1224.jpg (Project_Result-Gary1224.jpg)

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were...

The difference between model accuracy pre- and post-conversion was...

The size of the model pre- and post-conversion was...

The inference time of the model pre- and post-conversion was...

## Assess Model Use Cases

A use case for this type of applications can be counting people in a supermarket or company, and putting several cameras distributed around those area could inform us the areas with the highest population density there for broadcasting warning for avoiding to visit there or work from home due to COVID19.

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a
deployed edge model. The potential effects of each of these are as follows...

## Model Research

[This heading is only required if a suitable model was not found after trying out at least three
different models. However, you may also use this heading to detail how you converted 
a successful model.]

In investigating potential people counter models, I tried each of the following three models:

- Model 1: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...
  
- Model 2: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...

- Model 3: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...
