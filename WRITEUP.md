# Project: Deploy a People Counter App at the Edge

Use a useful person detection model (https://docs.openvinotoolkit.org/latest/omz_models_public_mobilenet_ssd_mobilenet_ssd.html) and convert it to an Intermediate Representation for use with the Model Optimizer. Utilizing the Inference Engine of OpenVINO, it will perform inference on an input video, and extract useful data concerning the count of people in frame and how long they stay in frame. Then send this information over MQTT, as well as sending the output frame, in order to view it from a separate UI server over a network.

## Explaining People Counter App

- Download models automatically from GitHub (https://github.com/openvinotoolkit/open_model_zoo/tree/master/models/public/mobilenet-ssd)

`/opt/intel/openvino/deployment_tools/open_model_zoo/tools/downloader/downloader.py --name mobilenet-ssd`

- To use mo.py to convert mobilenet_ssd model to IR xml/bin format types (in folder /home/workspace/public/mobilenet-ssd) as following. It will generate "**mobilenet-ssd.xml**" and "**mobilenet-ssd.bin**" in folder "/home/workspace/public/mobilenet-ssd/"

`/opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model ./public/mobilenet-ssd/mobilenet-ssd.caffemodel --data_type FP16`

- Launch MQTT/UI/FFmpeg services

- Issue following command for test app,

`python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m /home/workspace/public/mobilenet-ssd/./mobilenet-ssd.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3004/fac.ffm`

- Please refer the test result below,
https://raw.githubusercontent.com/zcwang/nd131-openvino-fundamentals-project-starter/master/Project_Result-Gary1224.jpg (Project_Result-Gary1224.jpg)

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations

The accuracy is almost the same among Caffee/IR-with-FP32/IR-with-FP16, but faster in its inference time,
- pre-conversion in Caffee format: 23MB

- post-conversion, FP32 (before optimizer): 23MB size of model, 37~40 FPS

- post-conversion FP16 (best optimizer): 23MB size of model, 40~ FPS

## Assess Model Use Cases

A use case for this type of applications can be counting people in a supermarket or company, and putting several cameras distributed around those area could inform us the areas with the highest population density there for broadcasting warning for avoiding to visit there or work from home due to COVID19.

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a
deployed edge model. The potential effects of each of these are as follows...

