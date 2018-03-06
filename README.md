# Object Detection - Tensorflow Object Detection API

Steps :
1.Clone this repository
2.Clone the models repository at https://github.com/anand-sonawane/models
3.Add the following in .bashrc file 
path_to_models:path_to_models/models/research:path_to_models/models/research/slim
4.Execute the following command in path_to_models/models/research
``` bash
protoc object_detection/protos/*.proto --python_out=.
```
5.Convert the data into a format like Pascal VOC
6.Create Tf-records
7.Move the Tf-records inside the data folder
7.Fix which base classifier to use
8.Modify the config file for the model you selected (place it where my config file is present in folder structure)
9.Download the pretrained checkpoints and graph of the pretrained classifier you selected and move those 3 files to the models folder . Download from https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md
