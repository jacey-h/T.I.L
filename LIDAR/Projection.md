# 1) Projection

: 투영 
입체적인 물체를 평면 상에 그리는 방식     
![image](https://user-images.githubusercontent.com/81483791/171217281-27ec760a-e7ae-417a-b6f8-81da8410b2ec.png)

3D point cloud 를 카메라 2D 이미지에 투영     
![image](https://user-images.githubusercontent.com/81483791/171217299-69567d7d-e465-41f8-a0f8-fc3f8077f2c9.png)

3D 라이다 데이터를 2D 카메라 이미지에 투영시키기 위해서는
라이다 좌표계와 카메라 좌표계를 통일시키는 것

- 3D 좌표계 변환     
![image](https://user-images.githubusercontent.com/81483791/171217326-ca0a16ff-96ff-4581-81a9-eb0a6cacc00c.png)

월드 좌표계(world coordinate system)에서의 점 (X, Y, Z)로부터 카메라 좌표계에서 봤을 때의 좌표 (Xc, Yc, Zc)를 구하는게 목적     
![image](https://user-images.githubusercontent.com/81483791/171217391-d43af66c-6ea3-4d7b-8bbc-626feae68dcc.png)

[R|t] : 회전/이동변환 행렬
 A : intrinsic camera matrix

world frame의 좌표들을 R과 t를 이용해서 변환하면 Camera frame으로 변환된다.

라이다에서 본 데이터들을 같은 단위계를 사용해서 카메라를 중심점으로 놓고 볼 수 있게 됨

- 3D to 2D    
![image](https://user-images.githubusercontent.com/81483791/171217480-22d1512f-5457-471c-b912-c5c32d6efca3.png)

카메라 이미지에 라이다 데이터를 투영시킨 결과

- 센서 퓨전 

카메라와 라이다 센서 융합기술     
![image](https://user-images.githubusercontent.com/81483791/171217519-a9544c30-58a0-4127-9cc5-bbaf13fe3f08.png)

1. 카메라

-물체에 대한 정확한 인식결과 제공

-물체와의 거리 측정 정확도 떨어짐

2. 라이다

-가려져 있는 물체는 감지할 수 없음 (투과 불가능)

-물체와의 거리 측정 정확도 높음
