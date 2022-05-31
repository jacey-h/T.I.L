# MPC
시뮬링크를 활용하여 제어 로직 구현하는 프로젝트    

# LKAS

Lane Keeping Assist System    
![image](https://user-images.githubusercontent.com/81483791/171221866-abed7a54-0c20-47c6-866c-ca01e11be4d7.png)

운전자의 의지와 무관하게 차선을 이탈하였을 경우, 
스티어링 휠을 제어하여 차선을 유지할 수 있도록 보조하는 시스템입니다.

LKA 시스템은 자동차의 전면 스티어링 각도를 조정하여 자동차가 도로의 차선 중심선을 따라 이동하도록 합니다. 
차선 유지 제어의 목표는 lateral deviation(e1)와 relative yaw angle(e2)를 0에 가깝게 구동하는 것입니다.

### LKAS 제어 로직     
![image](https://user-images.githubusercontent.com/81483791/171221961-7183bee9-be56-4636-a907-d5a64a4f5e09.png)
  
MPC 제어를 위해 수업시간에 주어진 횡방향 차량 모델식을 통해 상태공간 모델을 만듭니다.    
![image](https://user-images.githubusercontent.com/81483791/171222076-c2c2ca70-f0b3-42bb-8327-60eb1843c093.png)

상태공간 모델 출력 Vy, Yaw rate 을 입력으로 받습니다.    
![image](https://user-images.githubusercontent.com/81483791/171222191-1a522340-2c43-409a-b909-8ded62d5f33b.png)

Automated Driving Toolbox로 부터 만들어진 시나리오를 통해 lateral deviation(e1)와 relative yaw angle(e2)를 구합니다.     
![image](https://user-images.githubusercontent.com/81483791/171222373-80492f57-872d-489a-a93d-86c427ab25f9.png)

Sample time  : 0.1
Prediction horizon  : 10
Control horizon  :  3

MPC 모델을 튜닝하여 가장 좋은 제어 결과가
나오도록 합니다.

## 결과

- e1, e2 그래프    
![image](https://user-images.githubusercontent.com/81483791/171222513-89f799aa-3ff0-499b-9e9b-cb4ad01512fc.png)

lateral deviation(e1)와 relative yaw angle(e2) 그래프를 통해
 e1과 e2를 0에 가깝도록 제어하는 목표에 달성한 것을 알 수 있습니다.

- 경로 추종 그래프     
![image](https://user-images.githubusercontent.com/81483791/171222582-5f6f498d-fa97-48a1-a5cc-c4211d2ccffd.png)

차량모델에서 나온 yaw angle과 y position이 
우리가 원하는 desired yaw angle 과 desired y position을 잘 따라가는 것을 확인 할 수 있습니다.

- 드라이브 시나리오    


https://user-images.githubusercontent.com/81483791/171227559-168959ae-0377-41df-8be1-c25c46868fcb.mp4



- 조감도     

https://user-images.githubusercontent.com/81483791/171227338-e975e891-5da8-492b-b737-389fff3e5bf5.mp4


MPC 제어를 통해 횡방향 차량모델이 주어진 시나리오에 대해서 
steering angle 제어로 차선을 유지하면서 주행합니다.

- 궤적 비교 그래프
1. 곡선주행    
![image](https://user-images.githubusercontent.com/81483791/171227406-0379bf72-51bf-47e2-97ae-0f6e1d34967b.png)

2. 직선주행     
![image](https://user-images.githubusercontent.com/81483791/171227465-8cf45187-6da9-45fd-b1a5-1dee44d664f7.png)

차선을 이탈하며 주행하는 시나리오의 궤적과 비교하여 
LKAS를 통해 차선이 유지되어 도로의 궤적을 잘 따라 가는 것을 확인할 수 있습니다.
