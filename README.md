# Object Detection - Tensorflow Object Detection API

# Steps to start on custom dataset <br>

1.Clone this repository <br>
<br>
2.Clone the models repository at https://github.com/anand-sonawane/models <br>
<br>
3.Add the following in .bashrc file <br>
path_to_models:path_to_models/models/research:path_to_models/models/research/slim <br>
<br>
4.Execute the following command in path_to_models/models/research <br>
``` bash
protoc object_detection/protos/*.proto --python_out=.
```
<br>
5.Convert the data into a format like Pascal VOC <br>
<br>
6.Create Tf-records <br>
<br>
7.Move the Tf-records inside the data folder <br>
<br>
7.Fix which base classifier to use <br>
<br>
8.Modify the config file for the model you selected (place it where my config file is present in folder structure) <br>
<br>
9.Create models folder <br>
<br>
9.Download the pretrained checkpoints and graph of the pretrained classifier you selected and move those 3 files to the models folder  <br>
Download from https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md <br>
<br>
10.Train the object_detection_model

# Create TF Records <br>
<br>
You will need to change the following file paths in TF Records file <br>
<br>
1.Change the path to Annotations folder<br>
<br>
2.Change the path to Images folder<br>
<br>
3.Create a train.txt file with files names of the Images which you will use during training process<br>
<br>
4.Create a val.txt file with files names of the Images which you will use during training process<br>
<br>
5.Run the create_tfrecords file <br>
<br>

# Changes to be done in the config file  <br>
1. The *fine_tune_checkpoint* path to point to the model.ckpt file. If you followed the above steps it would look like
``` python
fine_tune_checkpoint: "models/model.ckpt"
```
<br>

2. Edit the parameter *num_steps* depending upon the size of your dataset and how long you are ready to train the model
<br>
<br>

3. Change the *input_path* and *label_map_path*, the input path is the path to your records file and label_map_path is path to your labels file which we have created(You can look for the sample of it in the repo) so it would look like

``` python
train_input_reader: {
  tf_record_input_reader {
    input_path: "data/train.record"
  }
  label_map_path: "pascal_label_map.pbtxt"
}
```
Also similarly for eval,
``` python
eval_input_reader: {
  tf_record_input_reader {
    input_path: "val.record"
  }
  label_map_path: "pascal_label_map.pbtxt"
  shuffle: false
  num_readers: 1
}
```
<br>
<br>

# Training  <br>

``` bash
python train.py --logtostderr --train_dir=./models/train --pipeline_config_path='path_to_config_file'
```
<br>
<br>


# Eval  <br>

``` bash
python eval.py \
        --logtostderr \
        --checkpoint_dir=path/to/checkpoint_dir \
        --eval_dir=path/to/eval_dir \
        --pipeline_config_path=pipeline_config.pbtxt
```
<br>
<br>

# Other Optimisations
<br>
If you are running out of memory and this is causing training to fail, there are a number of solutions you can try. <br>
First try adding the arguments

``` python
batch_queue_capacity: 2
prefetch_queue_capacity: 2
```

to your config file in the train_config section. <br>
For example, placing the two lines between *gradient_clipping_by_norm* and *fine_tune_checkpoint* will work. The number 2 above should only be starting values to get training to begin. The default for those values are 8 and 10 respectively and increasing those values should help speed up training.
<br>

# Export frozen model
<br>

For testing the model, you can create a .pb file which will give you a single model to be loaded. This can be done using the below script

``` python
python export_inference_graph.py --input_type image_tensor --pipeline_config_path ./rfcn_resnet101_coco.config --trained_checkpoint_prefix ./models/train/model.ckpt-5000 --output_directory ./fine_tuned_model
```


