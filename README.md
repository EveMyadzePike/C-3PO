# C-3PO

This is the official Pytorch implementation for [How to Reduce Change Detection to Semantic Segmentation](https://arxiv.org/abs/2206.07557). 

Overall, we present a new paradigm that reduces change detection to semantic segmentation which means tailoring an existing and powerful semantic segmentation network to solve change detection.

![reduce](imgs/reduce.jpg "reduce")

Our analysis suggests that there are three possible change types within the change detection task and they should be learned separately. Therefore, we propose a MTF (Merge Temporal Features ) module to learn these changes.

![changes](imgs/changes.jpg "changes")

We propose a simple but effective network, called C-3PO (Combine 3 POssible change types), detects changes in pixel-level, and can be considered as a new baseline network in this field.

![C-3PO](imgs/C3PO.jpg "C-3PO")

![MTF](imgs/MTF.jpg "MTF")

![MSF](imgs/MSF.jpg "MSF")

## Requirements

* Python3
* PyTorch
* Torchvision
* pycocotools
* timm

`Python3`, `Pytorch` and `Torchvision` are necessary. `pycocotools` is required for the `COCO` dataset. `timm` is required for the `Swin Transformer` backbone. 

If you want to use `CSCDNet` in our project, please follow their [instructions](https://github.com/kensakurada/sscdnet) to install the `correlation` module.

## Prepare the dataset

There are three datasets needed in this projects:
* COCO
* PCD
* VL-CMU-CD
* ChangeSim

Please refer to `src/dataset/path_config.py` to understand the folder structure of each dataset. And edit `data_path` according to your system.

Please follow this [site](https://kensakurada.github.io/pcd_dataset.html) to download the PCD dataset. You may need to send e-mails to Takayuki Okatani.

You can download VL-CMU-CD by this [link](https://drive.google.com/file/d/0B-IG2NONFdciOWY5QkQ3OUgwejQ/view?resourcekey=0-rEzCjPFmDFjt4UMWamV4Eg).

Please follow this [page](https://github.com/SAMMiCA/ChangeSim) to prepare the ChangeSim dataset.

## Run

**training**
```
python3 -m torch.distributed.launch --nproc_per_node=4 --use_env src/train.py --train-dataset VL_CMU_CD --test-dataset VL_CMU_CD --input-size 512 --model resnet18_mtf_msf_fcn --mtf id --msf 4 --warmup --loss-weight
```

**testing**
```
python3 src/train.py --test-only --model resnet18_mtf_msf_fcn --mtf id --msf 4 --train-dataset VL_CMU_CD --test-dataset VL_CMU_CD --input-size 512 --resume [checkpoint.pth]
```

We provide all shells to reproduce the results in our paper. Please check files in the `exp` folder. You can use below commands to run experiments.

```
source exp/sota/resnet18_mtf_id_msf4_deeplabv3_cmu.sh
train
```

## Visualization

![CMU](imgs/CMU.png "CMU")

## Citation

If you find the work useful for your research, please cite:

```
@article{wang2022c3po,
  title={How to Reduce Change Detection to Semantic Segmentation},
  author={Wang, Guo-Hua and Gao, Bin-Bin and Wang, Chengjie},
  journal={Pattern Recognition},
  year={2023}
}
```

## reference

* https://github.com/pytorch/vision/tree/main/references/segmentation
* https://github.com/kensakurada/sscdnet
* https://github.com/rcdaudt/fully_convolutional_change_detection
* https://github.com/leonardoaraujosantos/ChangeNet
* https://github.com/SAMMiCA/ChangeSim
