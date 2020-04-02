# Traffic Light Detection

<p align="center">
<img src="./result.gif">
</p>

## Objectives 

- Detect traffict lights from traffic iamges (videos)
- Classify the lights into 3 colors 
- This is used for self driving car 

## Approach 

- Use transfer learning of pre-trained model 
- Atchitecture
    - Use pretrained Mobile Net (CNN) for quick and memory saving detection, since speed is reauired. Mobile Net is trained for modification for this task
    - Use SSD (Single Shot Detection) to detect the location and the classification at the same time 
    - SSD_Mobilenet 11.6.17 version from <a href = "https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md"> Tensorflow model zoo  </a>
- Training Data: Used Bosch Small Traffic Light dataset (already annoted the class and positions)  https://github.com/bosch-ros-pkg/bstld

## Result 
- See video above 
- Detected red lights well, but still some missing lights
- Detecting other color lights not tested 


## Setup (for Windows 10)

- Download Bosch dataset and convert that into tfrecords format

- Setup the transfer learning (Following comes from comes from repo of Alex Lechner's <a href="https://github.com/alex-lechner/Traffic-Light-Classification">repo </a> . Appreciate the describing the complex Tensorflow (of an older version of TF that has limited info.) setup!)

-  Install Tensorflor 1.4:  `pip install tensorflow==1.4`  (newer version is not compatible!) 
    - (for Linux machine with GPU, `pip install tensorflow-gpu==1.4`)
-  Install relevant packages: `pip install pillow lxml matplotlib`  
-  Download `protoc-3.4.0-win32.zip` to extract to `C:\Program Files\protoc-3.4.0-win32`
    - (for Linux: `sudo apt-get install protobuf-compiler python-pil python-lxml python-tk`)
-  Create `/TensorFlow` directory 
-  Clone Models repository into `TensorFlow` directory: `git clone https://github.com/tensorflow/models.git`    
-  Navigate to the `/models` directory and change the repo version to: `git checkout f7e99c0`
-  Navigate to  `research` folder, and creating .py files by executing: `"C:\Program Files\protoc-3.4.0-win32\bin\protoc.exe" object_detection/protos/*.proto --python_out=.`
- Add following paths to environment variable `PYTHONPATH` (you may need to restart the terminal to activate the env variables): 
    - `models`
    - `models/research`
    - `models/research/slim`
    - `models/research/object_detection`
- run: `python builders/model_builder_test.py` at `/object_detection` directory
- configure a tensorflow config file and place in `/config` folder. 
- train the model: `python train.py --logtostderr --train_dif=./models/train --pipeline_config_path=./config/<TFMODELCONFIG.config>`
- freeze the model: `python export_inference_graph.py --input_type image_tensor --pipeline_config_path./config/ssd_mobilenet_c1-B.config --trained_checkpoint_prefix ./models/train/model.ckpt-2000 --output_directory models`
   

## Reference

- Tensorflow Object Detection install  https://github.com/tensorflow/models.git
- Bosch Small Traffic Light dataset

## Note 

- This repository is not fully updated, there maybe some parts that are not updated