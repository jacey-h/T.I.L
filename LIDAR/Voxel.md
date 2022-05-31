
# 2) Voxel     
![image](https://user-images.githubusercontent.com/81483791/171217746-66cdef48-b9b9-476c-a454-7ad297ce6b99.png)

복셀화는 2차원적인 픽셀(Pixcel)을 3차원의 형태로 구현한 것을 정의하며 
Volume+ Pixcel의 합성어로, 부피를 가진 픽셀

## VoxelNet

Point cloud로 3D object detection을 할 때 갖는 문제점

2D image와 비교하였을 때 매우 sparse하고 센서의 상대적 위치에 따라 point의 수가 달라짐
->  highly variable point density를 갖는 문제

해결방법
point cloud를 3D object detection에 적합하게 변형하기 위해 
직접 feature를 찾아서 변환하는 hand crafted feature representation 사용

hand crafted feature를 찾고 RPN(region proposal network)을 사용하는 복잡한 과정 없이
feature extraction과 bounding box prediction을 single stage로 합쳐서 
end-to-end로 학습 할 수 있음

### VoxelNet 논문 리뷰

VoxelNet architecture     
![image](https://user-images.githubusercontent.com/81483791/171217823-8c45126e-696d-4684-bf5c-bc7c63937a2a.png)

1. Feature Learning Network
2. Convolutional middle layers
3. Region Proposal Network(RPN)

**1. Feature Learning Network**     
![image](https://user-images.githubusercontent.com/81483791/171217901-cf1bdf0d-956f-495c-b236-99ed356f340d.png)

주어진 point cloud를 위의 그림처럼 
D x H x W(z, y, x축에 해당)의 개수와
vD, vH, vW 크기의 equally space voxel로 나눔    
![image](https://user-images.githubusercontent.com/81483791/171217943-333274fc-9b6b-45d9-b588-2060a8b5bd04.png)

하나의 voxel grid 내에 있는 point를 같은 그룹으로 묶음

(Point cloud는 density의 variance가 심하고 sparse하기 때문에 
voxel은 다양한 숫자의 point를 포함)    
![image](https://user-images.githubusercontent.com/81483791/171217983-033da291-882a-4930-809e-b6b4ab60530b.png)

T개 이상의 point를 가진 voxel에서 T개의 point를 랜덤하게 샘플링해서 
계산량을 줄이고 sampling bias를 줄임    
![image](https://user-images.githubusercontent.com/81483791/171218048-b40538db-bd45-4079-bb23-2385908704b0.png)

point는 그 자체로 CNN을 적용하기 어렵기 때문에 3차원 공간을 직육면체 형태의 voxel들로 쪼개 3D CNN을 적용하기 적절한 구조를 만들고, 각 voxel의 feature를 계산한 것 = voxel-wise feature     
![image](https://user-images.githubusercontent.com/81483791/171218080-86c353ee-2ec5-426b-914c-ae494b0476e4.png)

non-empty voxel들을 처리함으로써 voxel feature들의 list를 얻음

이 피쳐들의 list는 sparse한 C x D’ x H’ x W’ 크기의 4D tensor로 변환

**2. Convolutional middle layers**

3D CNN + BatchNormalize + ReLU

**3. Region Proposal Network(RPN)**    
![image](https://user-images.githubusercontent.com/81483791/171218152-8cd0461a-2faf-496b-aaea-45fbc287d7e3.png)

최종적인 class score와 bounding box regression 결과를 얻음

각 block의 첫 번째 layer가 stride 2를 가져 feature map을 절반으로 downsampling

각 block을 거쳐 나온 feature들을 Deconvolution을 통해 upsampling해서 같은 size로 만들어 준 후 합쳐줌
