# Segmantation models

- [Training models](g3doc/how_to_training_models.md)

# Model Zoo
|Model Name|FP32<br>(Original)|TF-TRT FP16|TF-Lite|TF-Lite<br>float16<br>quant|TF-Lite<br>integer<br>quant|TF-Lite<br>EdgeTPU|
|:--|:--|:--|:--|:--|:--|:--|
|[deeplabv3_mv2]()|-|-|-|-|-|-|
|[deeplabv3_mv2_dm05]()|-|-|-|-|-|-|
|[deeplab_mv3_large]()|Yes<br>(54.79%)|-|-|-|Yes<br>(49.73%)|No|
|[deeplab_mv3_small]()|Yes<br>(44.83%)|-|--||Yes<br>(39.05%)|No|
|[edgetpu_deeplab]()|-|-|-|-|-|-|
|[edgetpu_deeplab_slim]()|-|-|-|-|-|-|

[*1] (xx%): eval/miou_1.0_overall(val)  
[*2] Input size: 769x769

# Model and Latency.

## Raspberry Pi 4 
Note: links are youtube url.
|Model Name|TF-Lite|Float16<br>quant|Intger<br>quant|EdgeTPU|
|:--|--:|--:|--:|--:|
|deeplabv3_mv2|-|-|-|-|
|deeplabv3_mv2_dm05|-|-|-|-|
|deeplab_mv3_large|-|-|[538 ms](https://youtu.be/3VymTQIeTJE)|-|
|deeplab_mv3_small|-|-|[324 ms](https://youtu.be/cuWKttGEVyc)|-|
|edgetpu_deeplab|-|-|-|-|
|edgetpu_deeplab_slim|-|-|-|-|

[*1] libedgetpu: libedgetpu1-max (maximum operating frequency)<br>
[*2] libedgetpu: libedgetpu1-std (normal operating frequency)<br>

## HW/SW
- Raspberry Pi 4
    - Raspbian Buster (10.5)
    - Coral Edge TPU USB Accelerator 
    - Edge TPU runtime (libedgetpu): 13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - Edge TPU Python library: 2.13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - TensorflowLite: 1.5.1 ([PINTO0309/TensorflowLite-bin](https://github.com/PINTO0309/TensorflowLite-bin))

- Jetson Nano
    - JetPack 4.3
    - TensorRT: 6.0.1
    - TensorFlow: 1.15.0+nv20.1