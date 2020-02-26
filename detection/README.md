# Detection Models

- [Training models](g3doc/how_to_training_models.md)

# Model Zoo

Model and Latency.
|Model Name|RasPi4<br>+EdgeTPU (USB3.0)|Jetson Nano(TF-TRT)|RasPi4(TF-Lite)|
|:--|--:|--:|--:|
|ssdlite_mobilenet_v3_small_320x320|-|0|0|
|ssdlite_mobilenet_v3_large_320x320|-|0|0|
ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant|8 ms [*1]<br>11 ms [*2]|-|-|

[*1] libedgetpu: libedgetpu1-max (maximum operating frequency)<br>
[*2] libedgetpu: libedgetpu1-std (normal operating frequency)<br>

## HW/SW
- Raspberry Pi 4
    - Raspbian Buster (10.5)
    - Coral Edge TPU USB Accelerator 
    - Edge TPU runtime (libedgetpu): 13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - Edge TPU Python library: 2.13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - TensorflowLite: 1.5.1 ([PINTO0309/TensorflowLite-bin](https://github.com/PINTO0309/TensorflowLite-bin))