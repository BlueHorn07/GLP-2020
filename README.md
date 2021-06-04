# GLP-2020

[2020-2021] GLP 프로젝트: Omni-Detection

## Dataset

Work with 『360-Indoor』 Dataset

<div align="center">
  <img src="https://aliensunmin.github.io/project/360-dataset/WACV2020/WACV2020.jpg" width=500>
</div>

👉 [360-Indoor: Towards Learning Real-World Objects in 360° Indoor Equirectangular Images](https://aliensunmin.github.io/project/360-dataset/)

And to test my new type of kernels, I invented new dataset from the 『360-Indoor』 Dataset: "Mollweide360", "Adaptive360".

### Mollweide360 & Adaptive360 

## Model

### Kernel Design

#### SphereNet

Work with paper [『SphereNet: Learning Spherical Representations for Detection and Classification in Omnidirectional Images(ECCV 2018)』](https://openaccess.thecvf.com/content_ECCV_2018/papers/Benjamin_Coors_SphereNet_Learning_Spherical_ECCV_2018_paper.pdf)

👉 My Implementation: [BlueHorn07/sphereConv-pytorch](https://github.com/BlueHorn07/sphereConv-pytorch)

#### Omni-Kernel

Also, I suggest new type of kernel for equirectangular.

👉 My Implmentation: [BlueHorn07/Equirect-OmniKernel](https://github.com/BlueHorn07/Equirect-OmniKernel)

### Model Design

There are two ways to apply theh new type of kernels.

1. Replace **all** kernels in model
2. Replace **some** of kernels in model

#### Replace All Kernels

Due to the implementation of dynamic sampling of Omni-type kernsl, the first approach needs very huge GPU memoery. For the lack of GPU memory, only few of models can replace the entire kernels. In this project, I only did this on the [DETR](https://github.com/facebookresearch/detr) model.

#### Replace Some Kernels

This is more soft approach to apply the omni-type kernels. I applied this approach only on the [CenterNet](https://github.com/xingyizhou/CenterNet) model.

<div align="center">
  <img src="/iamges/centernet-omniConv.png" width=500>
</div>

However, debugging the model and determining where to replace are the problems.

## Experiment

### Classification

Experiment with Omni-MNIST dataset from SphereNet

| Method        | Test Acc (%) |
| ------------- |:--------------:|
| SphereNet ( ChiWeiHsiao ) | 94% |
| SphereNet ( our ) | 94% |
| MollweideNet ( our; 5x5 ) | 94% |
| MollweideNet ( our; 3x3 ) | 91% |
| EquirectCNN | 91% |

<div align="center">
  <img src="https://github.com/BlueHorn07/Equirect-OmniKernel/raw/no-round/images/mollweideCNN_plot.png" width=500>
</div>

### Detection

Experiment with "Indoor360" dataset from [『360-Indoor』](https://aliensunmin.github.io/project/360-dataset/), and my custom datasets: "Mollweide360", "Adaptive360".

I tested five models of two-stage and one-stage detector. The code of models that I've used are here:

- [Faster R-CNN (Detectron)](https://github.com/facebookresearch/Detectron)
- [YOLOv3](https://github.com/ultralytics/yolov3)
- [DETR: Detection with Transformers](https://github.com/facebookresearch/detr)
- [CenterNet](https://github.com/xingyizhou/CenterNet)
- [CenterNet-HarDNet](https://github.com/PingoLH/CenterNet-HarDNet)

#### Indoor360

|model|mAP(normal)|mAP(SphereNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|16.4|-|
|YOLOv3<br/>(darknet53)|**20.1**|-|
|DETR<br/>(resnet50)|14.8|9.3|
|CenterNet<br/>(DLA34-DCN)|14.9|9.4|
|CenterNet<br/>(hardnet68)|14.2|-|


#### Mollweide360

|model|mAP(normal)|mAP(MollweideNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|12.4|-|
|YOLOv3<br/>(darknet53)|16.4|-|
|DETR<br/>(resnet50)|-|1.4 ??|
|CenterNet<br/>(DLA34-DCN)|8.2|🤯|

#### Adaptive360

|model|mAP(normal)|mAP(AdaptiveNet)|
|:------:|:---:|:---:|
|Faster R-CNN<br/>(reset101)|12.1|-|
|DETR<br/>(resnet50)|12.0|11.x|


## Demo

### Indoor360

<div align="center">
  <img src="/iamges/demo/centernet-demo-train.PNG" width=500>
</div>

### SphereNet

<div align="center">
  <img src="/iamges/demo/spherenet-demo-train.PNG" width=500>
</div>

## Discussion

I'd tried to reproduce the result of [『360-Indoor』](https://aliensunmin.github.io/project/360-dataset/), but only YOLO was reproduced, the other models, Faster R-CNN, Mask R-CNN was degenerated.

The best model is "**YOLOv3**". Maybe, its because the YOLOv3 is robust to **generalization**. (Throught all the experiments, I'd used COCO pre-trained model.)

Despite, the omni-type kernels work well on the classification task, but on the detection task, the overall mAP was degenerated when applying omni-type kernels. To figure out the solution, I'd tried to do several things.

- check gradients and parameters
- check overfitting on small size samples
- check hyper-parameters

But, the none of them solve the degeneration 😥.

In overall experiements, the performance was bad on **small objects**: cup, phone, wine glas, etc.
