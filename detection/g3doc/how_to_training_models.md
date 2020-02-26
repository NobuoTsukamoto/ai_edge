# Training the models

# Installation
HW requirements
- GPU: NVIDIA GTX1070 (or higher)
- MEM: 32GB (or higher)

SW requirements
- OS: Linux (My environment: Fedora 31)
- Python 3.x (My environment: Python 3.7.6)
- CUDA 10.1 + CUDNN 7.6.5
- TensorFlow: GPU 1.15.x (not 2.x)
- Jupyter notebook
- [Edge TPU Compiler 2.0.291256449](https://coral.ai/docs/edgetpu/compiler/)

Clone tensorflow/models repogitory (my fork repogitory).
```
git clone -b ai_edge https://github.com/NobuoTsukamoto/models.git
```
Installation TensorFlow Object Detection API ([Please see this](https://github.com/NobuoTsukamoto/models/blob/ai_edge/research/object_detection/g3doc/installation.md)).

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
Execute [Conversion Script script](https://github.com/NobuoTsukamoto/models/blob/ai_edge/research/object_detection/dataset_tools/create_ai_edge_tf_record.py) and generate the TFRecord file.<br>
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
(models/research directory)
## SSDLite MobileNet EdgeTPU Quant Model
### Training
Fine-tuning checkpoint: [ssd_mobilenet_edgetpu_coco](https://storage.cloud.google.com/mobilenet_edgetpu/checkpoints/ssdlite_mobilenet_edgetpu_coco_quant.tar.gz)
```
$ PIPELINE_CONFIG_PATH=<PATH_TO/ai_edge/detection/config/ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant.config>
$ MODEL_DIR=/PATH_TO/ai_edge/detection/train_ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant/
$ NUM_TRAIN_STEPS=350000
$ SAMPLE_1_OF_N_EVAL_EXAMPLES=1
$ python object_detection/model_main.py \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --model_dir=${MODEL_DIR} \
    --num_train_steps=${NUM_TRAIN_STEPS} \
    --sample_1_of_n_eval_examples=$SAMPLE_1_OF_N_EVAL_EXAMPLES \
    --alsologtostderr
```
### # Frozen graph
```
$ INPUT_TYPE=image_tensor
$ TRAINED_CKPT_PREFIX=PATH_TO/ai_edge/detection/train_ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant/model.ckpt-350000
$ EXPORT_DIR=PATH_TO/ai_edge/detection/ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant
$ python object_detection/export_inference_graph.py \
    --input_type=${INPUT_TYPE} \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --trained_checkpoint_prefix=${TRAINED_CKPT_PREFIX} \
    --output_directory=${EXPORT_DIR}
```
### Frozen graph (TF-Lite)
```
$ CONFIG_FILE=PATH_TO/ai_edge/detection/ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant/pipeline.config
$ CHECKPOINT_PATH=PATH_TO/ai_edge/detection/ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant/model.ckpt
$ OUTPUT_DIR=OUTPUT_DIR=PATH_TO/ai_edge/detection/ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant/tflite_model
$ python object_detection/export_tflite_ssd_graph.py \
    --pipeline_config_path=$CONFIG_FILE \
    --trained_checkpoint_prefix=$CHECKPOINT_PATH \
    --output_directory=$OUTPUT_DIR \
    --add_postprocessing_op=true
```
### TF-Lite convert(UINT8 Model)
```
$ tflite_convert \
  --output_file="${OUTPUT_DIR}/output_tflite_graph.tflite" \
  --graph_def_file="${OUTPUT_DIR}/tflite_graph.pb" \
  --inference_type=QUANTIZED_UINT8 \
  --input_arrays="normalized_input_image_tensor" \
  --output_arrays="TFLite_Detection_PostProcess,TFLite_Detection_PostProcess:1,TFLite_Detection_PostProcess:2,TFLite_Detection_PostProcess:3" \
  --mean_values=128 \
  --std_dev_values=128 \
  --input_shapes=1,320,320,3 \
  --change_concat_input_ranges=false \
  --allow_nudging_weights_to_use_fast_gemm_kernel=true \
  --allow_custom_op

# Rename tflite model.
$ cd ${OUTPUT_DIR}
$ mv ./output_tflite_graph.tflite ./ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant.tflite
  ```
  ### Convert Edge TPU Model
  ```
  $ edgetpu_compiler -s -m 13 ./ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant.tflite
  ```