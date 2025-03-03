<div align="center">
<img src="./asserts/Logo.png" alt="Image Alt Text" width="150" height="150">
<h3> EditGuard: Versatile Image Watermarking for Tamper Localization and Copyright Protection </h3>
</div>


## Installation
```
pip install -r requirements.txt
```

## Checkpoint
| Condition | Link |
|------------|------------|
| Clean with better fidelity     | [clean.pth](https://drive.google.com/file/d/1w4e1gpdInAv7Lj_NQ7EGgmMuInyfUYgi/view?usp=sharing)|  
| Support Localization under Gaussian Noise ($\sigma$=0-5), JPEG (Q=70-95), Poission Noise |[degrade.pth](https://drive.google.com/file/d/1fAC2EIrMfPKuQa_DdYdxmUwLBbbsTJXC/view?usp=sharing)| 

Note that EditGuard is mainly used for tamper localization, and its copyright embedding and extraction are only trained on a few degradations such as Gaussian noise, Jpeg, and Poisson noise. If you want to get better robustness, please add more degradations and retrain it.

## Testing
Download the [testing dataset](https://drive.google.com/file/d/1s3HKFOzLokVplXV65Z6xcsBJ9qI91Qfv/view?usp=sharing) and place it in the "./dataset/valAGE-Set" and "./dataset/valAGE-Set-Mask". Download the pre-trained checkpoint "clean.pth" and put it in the "./checkpoints".
```
cd code
python test.py -opt options/test_editguard.yml --ckpt ../checkpoints/clean.pth
```
To extract the tampered masks:
```
python maskextract.py --threshold 0.2
```
To test the localization results under degradation conditions, set the ''degrade_shuffle=True'' in Line 25 of the "options/test_editguard.yml" and download the "degrade.pth".
```
cd code
python test.py -opt options/test_editguard.yml --ckpt ../checkpoints/degrade.pth
python maskextract.py --threshold 0.4
```

## Training
Download the [COCO2017](http://images.cocodataset.org/zips/train2017.zip) dataset and modify the path of the training dataset in the config file.

**Stage 1:** Train the BEM and BRM. 
```
python train_bit.py -opt options/train_editguard_bit.yml
```
**Stage 2:** First modify the checkpoint path of pretrained BEM and BRM in Line 87 "pretrain_model_G: " of "train_editguard_image.yml". Then, please run:
```
python train.py -opt options/train_editguard_image.yml
```


