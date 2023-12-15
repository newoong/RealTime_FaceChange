# RealTime_FaceChange
Real Time Face Toonify with SE-pix2pix

## Flow
  ![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/da0d4691-614a-4e34-a7d4-89b2968009b7)
  ##### FFHQ StyleGAN & Toonify Model을 이용하여 pair dataset 생성 필요 (3000~5000장)
  ##### -> VToonify 이용(VToonify/vtoonify_generate.ipynb 실행)
  ##### VToonify/checkpoint 내 [face parser & encoder model](https://drive.google.com/drive/folders/117azv6pQ89KzX9p4i6lalAHEKCdtkrfo?usp=sharing) 저장 필요
  ##### [ffhq model 및 VToonify 예시 모델들](https://drive.google.com/drive/folders/196tG3ai-rzC-T7RZz_ZBCWMnFLUyJ82U?usp=sharing)

## Problem
#### Toonify, StyleGAN2 모델 등을 실시간에 바로 차용 -> face mapping & face align등에 많은 시간 소요 -> 실시간 구현 x


## Solution

### - 대안으로 img2img 모델인 pix2pix 모델을 사용(U-Net 기반의 모델)
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/19943424-831b-44d8-9adb-7992886b3901)

### - 모델 성능 향상을 위해 pix2pix 모델 개조 -> SE(squeeze & excitation)-Block 사용
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/3943a914-5602-46ff-b252-12081e936723)
##### 적은 모델 복잡성 및 순전파 시간 증가로 큰 퀄리티 향상에 도움이 되는 Attention block인 SE-Block
##### U-Net 구조에서 Encoder에서의 input을 Decoder에 concatenate되기 직전에 SE-Block에 통과시킴
    1. Global Avg Pooling
    2. Squeeze the number of channel to divided number with squeeze ratio(parameter) by Linear Function
    3. ReLU
    4. Restore the number of channel by Linear Function

### - Generation 부분의 Activation Function 변경 (ReLU -> Hard Swish)
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/f8f7cde0-f75d-4fe2-a62f-3a10c28612a4)

### - Data Augmentation
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/e54fea22-3522-4180-bee4-f6c4de5edd50)
##### Use Random Affine & Random Perspective & Gaussian Noise...

## Results
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/4f810bf4-82ad-42ae-b963-4e6defd5529d)

## Quick Start
1. prepare pair dataset (3000~5000) into train_imgs folder
2.     sh run.sh

          python train.py \
          --batch_size 4 \
          --crop_size 512 \
          --dataroot /train_imgs/disney/ffhq_disney_512 \  # directory path where pair datasets are
          --name ffhq_disney_512_pix2pix \  # directory path to save (in checkopints folder)
          --dataset_mode aligned \
          --direction AtoB \  # pair dataset left to right(AtoB) right to B(BtoA)
          --epoch_count 1 \
          --gan_mode lsgan \  # lsgan is best
          --init_type normal \
          --load_size 512 \
          --lr 0.0002 \
          --model pix2pix \
          --n_epochs 400 \  # no lr decay until
          --n_epochs_decay 100 \  
          --n_layers_D 3 \
          --ndf 64 \
          --ngf 64 \
          --netD basic \
          --netG unet_256 \ # model select (unet_128 | unet_64 | resnet_9blocks | resnet_6blocks)
          --norm batch \
          --preprocess resize_and_crop \
          --rotate_perspective \ # use data augmentation 
          --squeeze 4 \ # squeeze ratio (divide num of channel with this parameter)
          --activation swish \ # (relu | swith)
          --print_freq 1000 \
          --save_epoch_freq 25 \ # model save frequency
          --lr_decay_iters 1000








