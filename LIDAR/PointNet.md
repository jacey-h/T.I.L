# 3) PointNet

Point cloud -> irregular format

딥러닝 모델에 넣기 위해서 regular format으로 변환 필요
(3D voxel grid로 바꾸거나 image들의 집합)

point cloud를 regular format 으로 만드는데 생긴 문제점

데이터를 불필요하게 크게 만듬
데이터의 natural invariance를 가릴 수 있는 quantization artifact가 있을 수 있음

**1. Permutation invariant**

Unordered

n개의 point가 있을때 이들이 얻어지는 순서의 경우의 수는 n!개 만큼 존재
어떠한 순서로 오더라도 output 결과는 동일하도록 network가 동작해야 함

**2. Rigid motion invariant**

transformation을 해도 point들 간의 distance와 방향은 그대로 유지됨

점들을 변환시킨다고 해서 class 분류 값이나 segmentation값들이 변하면 안 됨

## PointNet architecture

pointclouds_pl = tf.placeholder(tf.float32, shape=(batch_size, num_point, 3))

**1. Permutation invariant**

input data들을 어떤 순서로 받든지 output에는 영향을 끼치지 않아야 함

이를 만족하기 위해  symmetric function을 사용함

symmetric function이란?
변수들의 위치(순서)가 바뀌어도 결과가 같은 함수
어떤 함수 f가 f(x, y, z)=f(y, z, x) 를 만족하면 symmetric function

ex) 덧셈, 곱셈연산은 symmetric binary function

- max pooling

f(x, y, z) = max(x, y, z) 을 의미하므로
 permutation invariant를 만족하는 symmetric function

**2. Rigid motion invariant**

input에 어떤 transformation이 가해져도 output 결과에는 영향을 끼치지 않아야함

이를 만족하기 위해 spatial transformer network(STN)를 사용함

피쳐 추출하기 전에 모든 input set을 canoncial space(표준공간)에 맞춤

미니 네트워크 T-net 사용 
큰 네트워크와 유사 (mlp -> max pooling -> mlp)

### classification

최종 결과로 k개의 class에 대한 output score

### segmentation

global feature뿐만 아니라 local feature도 함께 이용
point별로 class들에 대한 score m개가 나와 nxm의 shape이 output

코드실습 결과