# Detection Models

- [Training models](g3doc/how_to_training_models.md)

# Model Zoo

Model and Latency.
|Model Name|RasPi4<br>+EdgeTPU (USB3.0)|Jetson Nano<br>TF-TRT FP16|RasPi4<br>TF-Lite|
|:--|--:|--:|--:|
|[ssdlite_mobilenet_v3_small_320x320](https://drive.google.com/open?id=1UoYm_sKj2oOk1G-9jrn_5SJrg3CzBCXB)|-|24 ms|0|
|[ssdlite_mobilenet_v3_large_320x320](https://drive.google.com/open?id=1EpGit2RaTdy5dgmEYD_D2jL7RlpdFzFA)|-|41 ms|0|
|[ssdlite_mobilenet_edgetpu_320x320_ai_edge_quant](https://drive.google.com/open?id=1DPjrqAQGJbUZvOFrSCakIJ34H82hMw0c)|8 ms [*1]<br>11 ms [*2]|-|-|

[*1] libedgetpu: libedgetpu1-max (maximum operating frequency)<br>
[*2] libedgetpu: libedgetpu1-std (normal operating frequency)<br>

## HW/SW
- Raspberry Pi 4
    - Raspbian Buster (10.5)
    - Coral Edge TPU USB Accelerator 
    - Edge TPU runtime (libedgetpu): 13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - Edge TPU Python library: 2.13.0 ([January 2020 Updates](https://coral.ai/news/updates-01-2020/))
    - TensorflowLite: 1.5.1 ([PINTO0309/TensorflowLite-bin](https://github.com/PINTO0309/TensorflowLite-bin))