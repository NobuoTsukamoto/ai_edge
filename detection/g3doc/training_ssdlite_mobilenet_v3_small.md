# SSDLite MobileNet v3 small Model
## Training
Fine-tuning checkpoint: [ssd_mobilenet_v3_large_coco](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v3_large_coco_2019_08_14.tar.gz)
```
$ PIPELINE_CONFIG_PATH=PATH_TO/ai_edge/detection/config/ssdlite_mobilenet_v3_large_320x320_ai_edge.config
$ NUM_TRAIN_STEPS=70000
$ SAMPLE_1_OF_N_EVAL_EXAMPLES=1
$ MODEL_DIR=PATH_TO/ai_edge/detection/train_ssdlite_mobilenet_v3_large_320x320_ai_edge/
$ python object_detection/model_main.py \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --model_dir=${MODEL_DIR} \
    --num_train_steps=${NUM_TRAIN_STEPS} \
    --sample_1_of_n_eval_examples=$SAMPLE_1_OF_N_EVAL_EXAMPLES \
    --alsologtostderr
```
## Frozen graph
```
$ INPUT_TYPE=image_tensor
$ PIPELINE_CONFIG_PATH=PATH_TO/ai_edge/detection/config/ssdlite_mobilenet_v3_large_320x320_ai_edge.config
$ TRAINED_CKPT_PREFIX=PATH_TO/ai_edge/detection/train_ssdlite_mobilenet_v3_large_320x320_ai_edge/model.ckpt-73649
$ EXPORT_DIR=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge
$ python object_detection/export_inference_graph.py \
    --input_type=${INPUT_TYPE} \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --trained_checkpoint_prefix=${TRAINED_CKPT_PREFIX} \
    --output_directory=${EXPORT_DIR}
```
## Frozen graph (TF-Lite)
```
$ CONFIG_FILE=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge/pipeline.config
$ CHECKPOINT_PATH=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge/model.ckpt
$ OUTPUT_DIR=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge/tflite_model
$ python object_detection/export_tflite_ssd_graph.py \
    --pipeline_config_path=$CONFIG_FILE \
    --trained_checkpoint_prefix=$CHECKPOINT_PATH \
    --output_directory=$OUTPUT_DIR \
    --add_postprocessing_op=true
```
## TF-TRT FP16 Model
copy ${EXPORT_DIR} dir to Jetson nano tf_trt_models/examples/detection
```
# tf_trt_models/examples/detection
$ python3 convert.py --path=./ssdlite_mobilenet_v3_large_320x320_ai_edge/ --output=ssdlite_mobilenet_v3_large_320x320_ai_edge/tftrt_fp16_model/

# Rename TF-TRT FP16 model.
$ cd ssdlite_mobilenet_v3_large_320x320_ai_edge/tftrt_fp16_model/
$ mv model_frozen_fp16.pb ssdlite_mobilenet_v3_large_320x320_ai_edge_fp16.pb
```