# SSD MobileNet v2 Model
## Training
Fine-tuning checkpoint: [ssd_mobilenet_v2_coco](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz)
```
$ PIPELINE_CONFIG_PATH=PATH_TO/ai_edge/detection/config/ssd_mobilenet_v2_300x300_ai_edge.config
$ MODEL_DIR=PATH_TO/ai_edge/detection/train_ssd_mobilenet_v2_300x300_ai_edge/
$ NUM_TRAIN_STEPS=33000
$ SAMPLE_1_OF_N_EVAL_EXAMPLES=1
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
$ TRAINED_CKPT_PREFIX=PATH_TO/ai_edge/detection/train_ssd_mobilenet_v2_300x300_ai_edge/model.ckpt-70000
$ EXPORT_DIR=PATH_TO/ai_edge/detection/ssd_mobilenet_v2_300x300_ai_edge
$ python object_detection/export_inference_graph.py \
    --input_type=${INPUT_TYPE} \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --trained_checkpoint_prefix=${TRAINED_CKPT_PREFIX} \
    --output_directory=${EXPORT_DIR}
```
## Frozen graph (TF-Lite)
```
$ CONFIG_FILE=PATH_TO/ai_edge/detection/ssd_mobilenet_v2_300x300_ai_edge/pipeline.config
$ CHECKPOINT_PATH=PATH_TO/ai_edge/detection/ssd_mobilenet_v2_300x300_ai_edge/model.ckpt
$ OUTPUT_DIR=PATH_TO/ai_edge/detection/ssd_mobilenet_v2_300x300_ai_edge/tflite_model
$ python object_detection/export_tflite_ssd_graph.py \
    --pipeline_config_path=$CONFIG_FILE \
    --trained_checkpoint_prefix=$CHECKPOINT_PATH \
    --output_directory=$OUTPUT_DIR \
    --add_postprocessing_op=true
```
## TF-Lite convert
### TF-Lite Model
```
$ tflite_convert \
   --graph_def_file=$OUTPUT_DIR/tflite_graph.pb \
   --output_file=$OUTPUT_DIR/ssd_mobilenet_v2_300x300_ai_edge.tflite \
   --input_shapes=1,300,300,3 \
   --input_arrays=normalized_input_image_tensor \
   --output_arrays='TFLite_Detection_PostProcess','TFLite_Detection_PostProcess:1','TFLite_Detection_PostProcess:2','TFLite_Detection_PostProcess:3' \
   --inference_type=FLOAT \
   --inference_input_type=QUANTIZED_UINT8 \
   --allow_custom_ops \
   --mean_values=128 \
   --std_dev_values=128 \
   --change_concat_input_ranges=false
```

### TF-Lite Float 16 quant model
```
$ tflite_convert \
   --graph_def_file=$OUTPUT_DIR/tflite_graph.pb \
   --output_file=$OUTPUT_DIR/ssd_mobilenet_v2_300x300_ai_edge_fp16.tflite \
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
$ python3 convert.py --path=./ssd_mobilenet_v2_300x300_ai_edge --output=./ssd_mobilenet_v2_300x300_ai_edge/tftrt_fp16_model

# Rename TF-TRT FP16 model.
$ cd ssdlite_mobilenet_v3_small_ssd_mobilenet_v2_300x300_ai_edge320x320_ai_edge/tftrt_fp16_model/
$ mv model_frozen_fp16.pb ssd_mobilenet_v2_300x300_ai_edge.pb
```