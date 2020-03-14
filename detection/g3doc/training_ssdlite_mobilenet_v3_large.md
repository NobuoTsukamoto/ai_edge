# SSDLite MobileNet v3 large Model
## Training
Fine-tuning checkpoint: [ssd_mobilenet_v3_large_coco](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v3_large_coco_2019_08_14.tar.gz)
```
$ CHECKPOINT_PATH=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge/model.ckptPIPELINE_CONFIG_PATH^C
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
$ TRAINED_CKPT_PREFIX=PATH_TO/ai_edge/detection/train_ssdlite_mobilenet_v3_large_320x320_ai_edge/model.ckpt-41189
$ EXPORT_DIR=PATH_TO/ai_edge/detection/ssdlite_mobilenet_v3_large_320x320_ai_edge
$ python object_detection/export_inference_graph.py \
    --input_type=${INPUT_TYPE} \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --trained_checkpoint_prefix=${TRAINED_CKPT_PREFIX} \
    --output_directory=${EXPORT_DIR}
```
## TF-Lite convert
### TF-Lite Model
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
### TF-Lite Float 16 quant model
```
$ tflite_convert \
   --graph_def_file=$OUTPUT_DIR/tflite_graph.pb \
   --output_file=$OUTPUT_DIR/ssdlite_mobilenet_v3_large_320x320_ai_edge_fp16.tflite \
   --input_shapes=1,320,320,3 \
   --input_arrays=normalized_input_image_tensor \
   --output_arrays='TFLite_Detection_PostProcess','TFLite_Detection_PostProcess:1','TFLite_Detection_PostProcess:2','TFLite_Detection_PostProcess:3' \
   --inference_type=FLOAT \
   --allow_custom_ops \
   --quantize_to_float16 \
   --post_training_quantize \
   --change_concat_input_ranges=false
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