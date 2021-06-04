# GLP-2020

[2020-2021] GLP 프로젝트: Omni-Detection

## Dataset

Work with 『360-Indoor』 Dataset

<div align="center">

<img src="https://aliensunmin.github.io/project/360-dataset/WACV2020/WACV2020.jpg" width=500>

</div>

👉 [360-Indoor: Towards Learning Real-World Objects in 360° Indoor Equirectangular Images](https://aliensunmin.github.io/project/360-dataset/)

## Model

### SphereNet

Work with paper [『SphereNet: Learning Spherical Representations for Detection and Classification in Omnidirectional Images(ECCV 2018)』](https://openaccess.thecvf.com/content_ECCV_2018/papers/Benjamin_Coors_SphereNet_Learning_Spherical_ECCV_2018_paper.pdf)

👉 My Implementation: [BlueHorn07/sphereConv-pytorch](https://github.com/BlueHorn07/sphereConv-pytorch)

### Omni-Kernel

Also, I suggest new type of kernel for equirectangular.

👉 My Implmentation: [BlueHorn07/Equirect-OmniKernel](https://github.com/BlueHorn07/Equirect-OmniKernel)

## Experiment

### Classification

### Detection

#### Indoor360

|model|mAP(normal)|mAP(SphereNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|14.2|-|
|YOLOv3<br/>(darknet53)|20.1|-|
|DETR<br/>(resnet50)|14.8|9.3|
|CenterNet<br/>(DLA34)|11.1|14.9|
|CenterNet<br/>(hardnet68)|14.2|-|


#### Mollweide360

|model|mAP(normal)|mAP(MollweideNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|12.4|-|
|YOLOv3<br/>(darknet53)|16.4|-|
|DETR<br/>(resnet50)|-|1.4 ??|
|CenterNet<br/>(DLA34)|-|3.2<br/>(ep5)|

#### Adaptive360

|model|mAP(normal)|mAP(AdaptiveNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|12.1|-|
|DETR<br/>(resnet50)|12.0|11.x|





