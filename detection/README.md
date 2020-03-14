# Detection Models

- [Training models](g3doc/how_to_training_models.md)

# Model Zoo
|Model Name|TF-TRT FP16|TF-Lite|TF-Lite<br>float16<br>quant|TF-Lite<br>integer<br>quant|TF-Lite<br>EdgeTPU|
|:--|:--|:--|:--|:--|:--|
|ssd_mobilenet_v2_300x300|No|No|No|No|No|
|ssd_mobilenet_v2_300x300_quant|No|No|No|No|No|
|[ssdlite_mobilenet_v3_small_320x320](https://drive.google.com/open?id=1yjjMwJxLglShaurTuHwsYjtaXj2T9dur)|Yes|Yes|Yes|No|No|
|[ssdlite_mobilenet_v3_large_320x320](https://drive.google.com/open?id=152_iyi4tZCieL4RHqwDzBJfZMaMUdPWU)|Yes|Yes|Yes|No|No|
|[ssdlite_mobilenet_edgetpu_320x320](https://drive.google.com/open?id=1_E3sc8JwDtWdKMGvPzqvsDPF4K12S0O9)|Yes|Yes|Yes|No|No|
|[ssdlite_mobilenet_edgetpu_320x320_quant](https://drive.google.com/open?id=1ZoUmySJZBorN7r5-5cd96vPX683bSARN)|No|No|No|Yes|Yes|


# Model and Latency.

## Raspberry Pi 4 
Note: links are youtube url.
|Model Name|TF-Lite|Float16<br>quant|Intger<br>quant|EdgeTPU|
|:--|--:|--:|--:|--:|
|ssd_mobilenet_v2_300x300|ms|ms|-|-|
|ssd_mobilenet_v2_300x300_quant|-|-|ms|ms|
|ssdlite_mobilenet_v3_small_320x320|[70 ms](https://youtu.be/MGd_jIkaZ3E)|ms|-|-|
|ssdlite_mobilenet_v3_large_320x320|[209 ms](https://youtu.be/UweTO9GcCzM)|ms|-|-|
|ssdlite_mobilenet_edgetpu_320x320|[553 ms](https://youtu.be/GnWSc0XF2IA)|ms|-|-|
|ssdlite_mobilenet_edgetpu_320x320_quant|-|-|[125 ms](https://youtu.be/aLzk4xlsyqA)|[8 ms](https://youtu.be/fHVE7uv48wg) [*1]<br>11 ms [*2]|

[*1] libedgetpu: libedgetpu1-max (maximum operating frequency)<br>
[*2] libedgetpu: libedgetpu1-std (normal operating frequency)<br>

## Jetson Nano
|Model Name|TF-TRT 16|
|:--|--:|
|ssd_mobilenet_v2_300x300|- [*3]|
|ssdlite_mobilenet_v3_small_320x320|24 ms|
|ssdlite_mobilenet_v3_large_320x320|41 ms|
|ssdlite_mobilenet_edgetpu_320x320|38 ms|

[*3] Convert tf-trt fp16 model (wit force nms cpu) is failed. please check [my blog post](https://nextremer-nbo.blogspot.com/2020/02/jetson-nanotf-trtjetpack43.html). 

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
