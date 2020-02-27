# Installation 

## Training host PC
### HW requirements
- GPU: NVIDIA GTX1070 (or higher)
- MEM: 32GB (or higher)

### SW requirements
- OS: Linux (My environment: Fedora 31)
- Python 3.x (My environment: Python 3.7.6)
- CUDA 10.1 + CUDNN 7.6.5
- TensorFlow: GPU 1.15.x (not 2.x)
- Jupyter notebook
- [Edge TPU Compiler 2.0.291256449](https://coral.ai/docs/edgetpu/compiler/)

### Install TensorFlow Object detection API

Clone tensorflow/models repogitory (my fork repogitory).
```
$ git clone -b ai_edge https://github.com/NobuoTsukamoto/models.git
```
Installation TensorFlow Object Detection API ([Please see this](https://github.com/NobuoTsukamoto/models/blob/ai_edge/research/object_detection/g3doc/installation.md)).

## NVIDIA JETSON NANO
Convert TF-TRT FP16 model.
### SW requirements
- JetPack 4.3 (r32.3.1)
- TensorRT: 6.0.1 
- [TensorFlow:tensorflow_gpu-1.15.0+nv20.1](https://devtalk.nvidia.com/default/topic/1048776/jetson-nano/official-tensorflow-for-jetson-nano-/post/5322533/#5322533)
- Python: 3.6.9
### Install TF-TRT models
Clone NVIDIA-AI-IOT/tf_trt_models repogitory with Jetson Nano (my fork repogitory) and setup.
```
$ git clone https://github.com/NobuoTsukamoto/tf_trt_models
$ cd tf_trt_models
$ ./install.sh python3
```
