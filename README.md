# gfen-hfci

## Installation
* Clone this repo, and we'll call the directory that you cloned as ${gfen-hfci}
* Install dependencies. We use python 3.7 and pytorch >= 1.2.0
```
conda create -n gfen-hfci
conda activate gfen-hfci
conda install pytorch==1.2.0 torchvision==0.4.0 cudatoolkit=10.0 -c pytorch
cd ${gfen-hfci}
pip install -r requirements.txt
cd src/lib/models/networks/DCNv2_new sh make.sh
```
* We use [DCNv2](https://github.com/CharlesShang/DCNv2) in our backbone network and more details can be found in their repo. 
* In order to run the code for demos, you also need to install [ffmpeg](https://www.ffmpeg.org/).

## Data preparation

```
MOT15
   |——————images
   |        └——————train
   |        └——————test
   └——————labels_with_ids
            └——————train(empty)
MOT20
   |——————images
   |        └——————train
   |        └——————test
   └——————labels_with_ids
            └——————train(empty)
```
Then, you can change the seq_root and label_root in src/gen_labels_15.py and src/gen_labels_20.py and run:
```
cd src
python gen_labels_15.py
python gen_labels_20.py
```
to generate the labels of 2DMOT15 and MOT20. The seqinfo.ini files of 2DMOT15 can be downloaded here [[Google]](https://drive.google.com/open?id=1kJYySZy7wyETH4fKMzgJrYUrTfxKlN1w), [[Baidu],code:8o0w](https://pan.baidu.com/s/1zb5tBW7-YTzWOXpd9IzS0g).

## Training
* Download the training data
* Run:
```
python train.py mot --exp_id mot15_ft_mix_dla34 --load_model '../models/gfen-hfci_dla34.pth' --num_epochs 40 --lr_step '15' --data_cfg '../src/lib/cfg/mot15.json' --batch_size 12
```

## Tracking
* The default settings run tracking on the validation dataset from 2DMOT15. Using the DLA-34 baseline model, you can run:
```
cd src
python track.py mot --load_model ../models/all_dla34.pth --conf_thres 0.6
```
