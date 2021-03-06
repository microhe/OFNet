# OFNet: Occlusion-shared and Feature-separated Network for Occlusion Relationship Reasoning

Created by Lu Rui.

### Introduction

Occlusion relationship reasoning of objects from monocular images are fundamental in computer vision and mobile robot applications. Furthermore, it can be used for scene understanding and perception, such as object detection, segmentation and 3D reconstruction. From the perspective of the observer, occlusion relationship reflects relative depth difference between objects in the scene.

The Data Preparation and Evaluation are following Guoxia Wang with his DOOBNet(https://github.com/GuoxiaWang/DOOBNet). Thanks for his valuable work.

### Citation

If you find OFNet useful in your research, please consider citing:
```
@inproceedings{Lu2019Occlusion,
  Title = {Occlusion-shared and Feature-separated Network for Occlusion Relationship Reasoning},
  Author = {Rui Lu, Feng Xue, Menghan Zhou, Anlong Ming, Yu Zhou},
  Booktitle = {ICCV},
  Year = {2019}
}
```


## Data Preparation


#### PASCAL Instance Occlusion Dataset (PIOD)

You may download the dataset original images from [PASCAL VOC 2010](http://host.robots.ox.ac.uk/pascal/VOC/voc2010/VOCtrainval_03-May-2010.tar) and annotations from [here](https://drive.google.com/file/d/0B7DaWBKShuMBSkZ6Mm5RVmg5ck0/view?usp=sharing). Then you should copy or move `JPEGImages` folder in PASCAL VOC 2010 and `Data` folder and val\_doc_2010.txt in PIOD to `data/PIOD/`. You will have the following directory structure:
```
PIOD
|_ Data
|  |_ <id-1>.mat
|  |_ ...
|  |_ <id-n>.mat
|_ JPEGImages 
|  |_ <id-1>.jpg
|  |_ ...
|  |_ <id-n>.jpg
|_ val_doc_2010.txt
```

Now, you can use data convert tool to augment and generate HDF5 format data for OFNet. 
```
mkdir data/PIOD/Augmentation

python doobscripts/doobnet_mat2hdf5_edge_ori.py \
--dataset PIOD \
--label-dir data/PIOD/Data \
--img-dir data/PIOD/JPEGImages \
--piod-val-list-file data/PIOD/val_doc_2010.txt \
--output-dir data/PIOD/Augmentation
```

#### BSDS ownership

For BSDS ownership dataset, you may download the dataset original images from [BSDS300](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/segbench/BSDS300-images.tgz) and annotations from [here](https://drive.google.com/open?id=0B7DaWBKShuMBd3Z0Vmk3UkZxcUU). Then you should copy or move `BSDS300` folder in BSDS300-images and `trainfg` and `testfg` folder in BSDS\_theta to `data/BSDSownership/`. And you will have the following directory structure:
```
BSDSownership
|_ trainfg
|  |_ <id-1>.mat
|  |_ ...
|  |_ <id-n>.mat
|_ testfg
|  |_ <id-1>.mat
|  |_ ...
|  |_ <id-n>.mat
|_ BSDS300
|  |_ images
|     |_ train
|        |_ <id-1>.jpg
|        |_ ...
|        |_ <id-n>.jpg
|     |_ ...
|  |_ ...
```
Note that BSDS ownership's test set are split from 200 train images (100 for train, 100 for test). More information you can check ids in `trainfg` and `testfg` folder and ids in `BSDS300/images/train` folder, or refer to [here](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/fg/fgdata.tar.gz)

Run the following code for BSDS ownership dataset. 
```
mkdir data/BSDSownership/Augmentation

python doobscripts/doobnet_mat2hdf5_edge_ori.py \
--dataset BSDSownership \
--label-dir data/BSDSownership/trainfg \
--img-dir data/BSDSownership/BSDS300/images/train \
--bsdsownership-testfg data/BSDSownership/testfg \
--output-dir data/BSDSownership/Augmentation 
```

## Training

Firstly, you need to download the Res50 weight file from [Res50](https://drive.google.com/open?id=1nyGjqSj0LGVsY9iBhsEdo-TXSyROGTgZ) and save `resnet50.caffemodel` to the folder `$OFNET_ROOT/models/resnet/`.

#### PASCAL Instance Occlusion Dataset (PIOD)

For training OFNet on PIOD training dataset, you can run:

```
cd $OFNET_ROOT/examples/ofnet/PIOD

./train.sh
```
When training completed, you need to modify `model = '../models/ofnet_piod.caffemodel'` in `deploy_ofnet_piod.py` and then run `python deploy_ofnet_piod.py` to get the results on PIOD testing dataset. For comparation, you can also download our trained model from [here](https://pan.baidu.com/s/1FZJOZCLXysv1krcR3DEvqg). (code: y4b6). The testing results are available at [here](https://pan.baidu.com/s/1X9tJHOfVhG3K_W1HyOsnrQ&shfl=shareset). (code: 85ih).


#### BSDS ownership
For training OFNet on BSDS ownership, you can refer the manner as same as PIOD dataset above. The testing results are available at [here](https://pan.baidu.com/s/1uZDKiu2rgBMGIDgkqaYB0g&shfl=shareset). (code: xj8q).


## Evaluation

Here we provide the PIOD and the BSDS ownership dataset's evaluation and visualization code in `doobscripts` folder.

**Note that** you need to config the necessary paths or variables. More information please refers to `doobscripts/README.md`.

To run the evaluation:
```
run doobscripts/evaluation/EvaluateOcc.m
```

#### Option
For visualization, to run the script:
```
run doobscripts/visulation/PlotAll.m
```

## Acknowledgement
We would like to thank Guoxia Wang for helping with generating DOOBNet experimental results and valuable discussions.
