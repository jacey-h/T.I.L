# MPC

# LKAS

Lane Keeping Assist System

운전자의 의지와 무관하게 차선을 이탈하였을 경우, 
스티어링 휠을 제어하여 차선을 유지할 수 있도록 보조하는 시스템입니다.

LKA 시스템은 자동차의 전면 스티어링 각도를 조정하여 자동차가 도로의 차선 중심선을 따라 이동하도록 합니다. 
차선 유지 제어의 목표는 lateral deviation(e1)와 relative yaw angle(e2)를 0에 가깝게 구동하는 것입니다.

### LKAS 제어 로직

MPC 제어를 위해 수업시간에 주어진 횡방향 차량 모델식을 통해 상태공간 모델을 만듭니다.

상태공간 모델 출력 Vy, Yaw rate 을 입력으로 받습니다.

Automated Driving Toolbox로 부터 만들어진 시나리오를 통해 lateral deviation(e1)와 relative yaw angle(e2)를 구합니다.

Sample time  : 0.1
Prediction horizon  : 10
Control horizon  :  3

MPC 모델을 튜닝하여 가장 좋은 제어 결과가
나오도록 합니다.

## 결과

- e1, e2 그래프

lateral deviation(e1)와 relative yaw angle(e2) 그래프를 통해
 e1과 e2를 0에 가깝도록 제어하는 목표에 달성한 것을 알 수 있습니다.

- 경로 추종 그래프

차량모델에서 나온 yaw angle과 y position이 
우리가 원하는 desired yaw angle 과 desired y position을 잘 따라가는 것을 확인 할 수 있습니다.

- 드라이브 시나리오

- 조감도

MPC 제어를 통해 횡방향 차량모델이 주어진 시나리오에 대해서 
steering angle 제어로 차선을 유지하면서 주행합니다.

- 궤적 비교 그래프
1. 곡선주행

1. 직선주행

차선을 이탈하며 주행하는 시나리오의 궤적과 비교하여 
LKAS를 통해 차선이 유지되어 도로의 궤적을 잘 따라 가는 것을 확인할 수 있습니다.