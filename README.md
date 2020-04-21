# Knowledge distillation kit for PyTorch

## Requirements
- Python 3.6
- pipenv
- [myutils](https://github.com/yoshitomo-matsubara/myutils)


## How to setup
```
git clone https://github.com/yoshitomo-matsubara/kdkit.git
cd kdkit/
git submodule init
git submodule update --recursive --remote
pipenv install
```

## Examples
### 1. ImageNet
#### 1.1 Download and unzip ImageNet datasets (ILSVRC2012 in this example)
#### 1.2 Execute the following commands
```
cd kdkit/
mkdir ./resource/dataset/ilsvrc2012/{train,val} -p

# Download the training and validation datasets from ImageNet website
# Untar them under train and val dirs respectively

wget https://raw.githubusercontent.com/soumith/imagenetloader.torch/master/valprep.sh
mv valpre.sh ./resource/dataset/ilsvrc2012/val/
cd ./resource/dataset/ilsvrc2012/val/
sh valpre.sh
```
#### 1.3 Distill knowledge of ResNet-152
e.g., Teacher: ResNet-152, Student: AlexNet  
a) Use GPU(s) for single training process
```
pipenv run python src/image_classification.py --config config/image_classification/kd/alexnet_from_resnet152.yaml
```  
b) Use GPUs for multiple distributed training processes
```
pipenv run python -m torch.distributed.launch --nproc_per_node=${NUM_GPUS} --use_env src/image_classification.py --world_size ${NUM_GPUS} --config config/image_classification/kd/alexnet_from_resnet152.yaml
```
c) Use CPU
```
pipenv run python src/image_classification.py --device cpu --config config/image_classification/kd/alexnet_from_resnet152.yaml
```  
#### 1.4 Top 1 accuracy of student models
| Teacher \\ Student    | AlexNet   | ResNet-18 |  
| :---                  | ---:      | ---:      |  
| Pretrained (no *KD*)  | 56.52     | 69.76     |  
| ResNet-152            | 57.22     | 70.34     |


## References
- [:mag:](src/image_classification.py) [pytorch/vision/references/classification/](https://github.com/pytorch/vision/blob/master/references/classification/)
- Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean. ["Distilling the Knowledge in a Neural Network"](https://fb56552f-a-62cb3a1a-s-sites.googlegroups.com/site/deeplearningworkshopnips2014/65.pdf?attachauth=ANoY7co8sQACDsEYLkP11zqEAxPgYHLwkdkDP9NHfEB6pzQOUPmfWf3cVrL3WE7PNyed-lrRsF7CY6Tcme5OEQ92CTSN4f8nDfJcgt71fPtAvcTvH5BpzF-2xPvLkPAvU9Ub8XvbySAPOsMKMWmGsXG2FS1_X1LJsUfuwKdQKYVVTtRfG5LHovLHIwv6kXd3mOkDKEH7YdoyYQqjSv6ku2KDjOpVQBt0lKGVPXeRdwUcD0mxDqCe4u8%3D&attredirects=1)
- Adriana Romero, Nicolas Ballas, Samira Ebrahimi Kahou, Antoine Chassang, Carlo Gatta, and Yoshua Bengio. ["FitNets: Hints for Thin Deep Nets"](https://arxiv.org/abs/1412.6550)
- Junho Yim, Donggyu Joo, Jihoon Bae, and Junmo Kim. ["A Gift From Knowledge Distillation: Fast Optimization, Network Minimization and Transfer Learning"](http://openaccess.thecvf.com/content_cvpr_2017/html/Yim_A_Gift_From_CVPR_2017_paper.html)
- Jangho Kim, Seonguk Park, and Nojun Kwak. ["Paraphrasing Complex Network: Network Compression via Factor Transfer"](http://papers.neurips.cc/paper/7541-paraphrasing-complex-network-network-compression-via-factor-transfer)
