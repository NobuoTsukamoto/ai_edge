# Training the model

- [Installation](install_tensorflow_object_detection_api.md)

# Prepare dataset
First clone this repogitory.
```
$ git clone https://github.com/NobuoTsukamoto/ai_edge.git
```
Download detection datasets from [AI EDGE CONTEST OPEN DATA PORTAL](https://signate.jp/dlp/ai-edge-contest-data) and put the file in the following directory:
```
ai_edge
  +- data
    +- *.zip (Download files)
```
Launch Jupyter notebook
```
$ jupyter notebook
```
Select *detection / preprocessing.ipynb* and *Run All* it. Unzip files and place the image and json files in the following directory:
```
ai_edge
  +- data
    +- annotations
      +- *.json
    +- images
      +- *.jpg
```
Run [Conversion Script script](https://github.com/NobuoTsukamoto/models/blob/ai_edge/research/object_detection/dataset_tools/create_ai_edge_tf_record.py) and generate the TFRecord file.<br>
Label map file is [here](https://github.com/NobuoTsukamoto/models/blob/ai_edge/research/object_detection/data/ai_edge_label_map.pbtxt).
```
# models/research 

$ python ./object_detection/dataset_tools/create_ai_edge_tf_record.py \
    --data_dir=<PATH_TO/ai_edge/data/annotations>
    --image_dir=<PATH_TO/ai_edge/detection/data/images>
    --output_dir=<PATH_TO/ai_edge/detection/tf_recode>
    --label_map_path=./object_detection/data/ai_edge_label_map.pbtxt
```
This will produce the following output files:
```
ai_edge
  +- data
    +- tf_recode
      +- ai_edge_object_train.record-000??-of-00010 (train data)
      +- ai_edge_object_val.record-000??-of-000?? (val data)

```
Config files are *ai_edge/detection/config*. Change the path according to your environment.

# Training models
- [SSDLite MobileNet EdgeTPU Model](training_ssdlite_mobilenet_edgetpu.md)
- [SSDLite MobileNet EdgeTPU quant Model](training_ssdlite_mobilenet_edgetpu_quant.md)
- [SSDLite MobileNet v3 small Model](training_ssdlite_mobilenet_v3_small.md)
- [SSDLite MobileNet v3 large Model](training_ssdlite_mobilenet_v3_large.md)

