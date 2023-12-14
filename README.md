# RealTime_FaceChange
Real Time Face Toonify with SE-pix2pix

- Flow
  ![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/da0d4691-614a-4e34-a7d4-89b2968009b7)

1. Toonify, StyleGAN2 모델 등을 실시간에 바로 차용 -> face mapping & face align등에 많은 시간 소요 -> 실시간 구현 x
2. 대안으로 img2img 모델인 pix2pix 모델을 사용(U-Net 기반의 모델)
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/19943424-831b-44d8-9adb-7992886b3901)
3. 모델 성능 향상을 위해 pix2pix 모델 개조 -> SE(squeeze & excitation)-Block 사용
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/3943a914-5602-46ff-b252-12081e936723)
4. generation 부분의 activation function 변경 (ReLU -> Hard Swish)
![image](https://github.com/newoong/RealTime_FaceChange/assets/94604584/f8f7cde0-f75d-4fe2-a62f-3a10c28612a4)





