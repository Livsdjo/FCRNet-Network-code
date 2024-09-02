## FCRNet implementation

Pytorch implementation of FCRNet for paper "FCRNet:learning non-linear correspondences variation via a graph-based feature embedding for false correspondence removal", by Ruiyuan Li and ZhaoLin Xiao.

## Requirements & Compilation

1. Requirements

Required packages are listed in [requirements.txt](requirements.txt). 

The code is tested using Python-3.8.5 with PyTorch 1.7.1.

2. Compile extra modules

```shell script
cd network/knn_search
python setup.py build_ext --inplace
cd ../pointnet2_ext
python setup.py build_ext --inplace
cd ../../utils/extend_utils
python build_extend_utils_cffi.py
```
According to your installation path of CUDA, you may need to revise the variables cuda_version in [build_extend_utils_cffi.py](utils/extend_utils/build_extend_utils_cffi.py).

## Generate training and testing data

First download YFCC100M dataset.
```bash
bash download_data.sh raw_data raw_data_yfcc.tar.gz 0 8
tar -xvf raw_data_yfcc.tar.gz
```

Download SUN3D testing (1.1G) and training (31G) dataset if you need.
```bash
bash download_data.sh raw_sun3d_test raw_sun3d_test.tar.gz 0 2
tar -xvf raw_sun3d_test.tar.gz
bash download_data.sh raw_sun3d_train raw_sun3d_train.tar.gz 0 63
tar -xvf raw_sun3d_train.tar.gz
```

Then generate matches for YFCC100M and SUN3D (only testing). Here we provide scripts for SIFT, this will take a while.
```bash
cd dump_match
python extract_feature.py
python yfcc.py
python extract_feature.py --input_path=../raw_data/sun3d_test
python sun3d.py
```
Generate SUN3D training data if you need by following the same procedure and uncommenting corresponding lines in `sun3d.py`.

## Training
```shell script
cd LNCNet-Network-code/core
python main.py
```
