# 강화학습 실습

# Train PPO Agent for Automatic Parking Valet

주차장   
![image](https://user-images.githubusercontent.com/81483791/171119741-928e8a99-c2be-4979-9025-99fdcc8f8d16.png)

차량의 목표 포즈
x = 47.7500
y = 4.9000
θ = -1.5708

센서

- 카메라    
![image](https://user-images.githubusercontent.com/81483791/171119793-29bba40e-32a6-4860-b912-614d21b3b5e0.png)

d : 주차위치까지의 거리
φ : 주차위치에 대한 각도

- 라이다    
![image](https://user-images.githubusercontent.com/81483791/171119835-8e6d4c5a-583b-40ca-9d56-8cf0011c122c.png)

12개의 라이다 선분 (최대 6m)

### Simulink 모델    
![image](https://user-images.githubusercontent.com/81483791/171119910-2d6442da-be70-4e21-8a59-6478da0fb28f.png)

Enabled Subsystem 불록으로 
차량이 빈 자리를 탐색할  때는 MPC controller
주차 모드일 때는 RL controller

**MPC (Model Predictive Control) :**     
![image](https://user-images.githubusercontent.com/81483791/171119983-797e4e23-43b3-49da-ae09-8f9d5ef232b4.png)

모델예측제어는 미래 출력치를 예측하고 이를 
최적화하여 얻어진 제어 입력을 사용하는 제어 기법

### Actor - Critic

PPO는 Advantage Actor Critic 알고리즘 중 하나로 볼 수 있으므로
이와 같은 방식으로 업데이트 진행

- Actor 네트워크를 업데이트 1. Surrogate Function을 최대화

2. Action Entropy를 최대화

- Critic 네트워크를 업데이트
State-Value의 차이를 최소화

PPO는 하나의 식으로 통합하여 최적화 수행

- Value function critic *V*(*S*) 네트워크 구조

```matlab
criticNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(128,'Name','fc3')
    reluLayer('Name','relu3')
    fullyConnectedLayer(1,'Name','fc4')];
```

- Stochastic policy actor *π*(*S*) 네트워크

```matlab
actorNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(numActions, 'Name', 'out')
    softmaxLayer('Name','actionProb')];
```

- PPO agent 생성

```matlab
agentOpts = rlPPOAgentOptions(...
    'SampleTime',Ts,...
    'ExperienceHorizon',200,...
    'ClipFactor',0.2,... 
    'EntropyLossWeight',0.01,...
    'MiniBatchSize',64,...
    'NumEpoch',3,...
    'AdvantageEstimateMethod',"gae",...
    'GAEFactor',0.95,...
    'DiscountFactor',0.998);
agent = rlPPOAgent(actor,critic,agentOpts);
```

ExperienceHorizon=200 : 200단계의 경험범위에 도달하거나 에피소드가 종료될 때까지 경험을 수집한다

ClipFactor=0.2 : 목적함수 클립 계수가 0.2이면 훈련 안정성이 향상

EntropyLossWeight=0.01 :

MiniBatchSize=64 : 64개의 경험으로 구성된 미니배치에서 훈련

NumEpoch=3 : 3개의 에포크 동안

AdvantageEstimateMethod=gae 

GAEFactor=0.95 : GAE 계수가 0.95인 일반화된 이점 추정 방법에 의해 비평의 편차 출력이 감소합니다.

DiscountFactor=0.998 : 할인계수값이 0.998이면 장기 보상이 권장됨

- AGENT 훈련하기

```matlab
trainOpts = rlTrainingOptions(...
    'MaxEpisodes',10000,...
    'MaxStepsPerEpisode',200,...
    'ScoreAveragingWindowLength',200,...
    'Plots','training-progress',...
    'StopTrainingCriteria','AverageReward',...
    'StopTrainingValue',80);
```

이 예에서는 최대 10000개의 에피소드에 대해 에이전트를 교육하고 각 에피소드는 최대 200개의 시간 단계를 지속합니다. 최대 에피소드 수에 도달하거나 100회 이상의 평균 보상이 100회를 초과하면 교육이 종료됩니다.

③ 예제에서 네트워크의 구조, 훈련 옵션 등을 변경해가며 실험했을 때 결과는 어떻게 변화 하는가?

1. 첫번째 변화

```matlab
criticNetwork = [
featureInputLayer(numObservations,'Normalization','none','Name','observations')
fullyConnectedLayer(128,'Name','fc1')
reluLayer('Name','relu1')
fullyConnectedLayer(128,'Name','fc2')
reluLayer('Name','relu2')
fullyConnectedLayer(128,'Name','fc3')
reluLayer('Name','relu3') 
fullyConnectedLayer(128,'Name','fc4')
reluLayer('Name','relu4')
fullyConnectedLayer(1,'Name','fc5')];
```

```matlab
actorNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(128,'Name','fc3')
    reluLayer('Name','relu3') 
    fullyConnectedLayer(128,'Name','fc4')
    reluLayer('Name','relu4')
    fullyConnectedLayer(numActions, 'Name', 'out')
    softmaxLayer('Name','actionProb')];
```

```matlab
agentOpts = rlPPOAgentOptions(...
    'SampleTime',Ts,...
    'ExperienceHorizon',100,...
    'ClipFactor',0.2,...
    'EntropyLossWeight',0.01,...
    'MiniBatchSize',32,...
    'NumEpoch',5,...
    'AdvantageEstimateMethod',"gae",...
    'GAEFactor',0.95,...
    'DiscountFactor',0.998);
agent = rlPPOAgent(actor,critic,agentOpts)
```

```matlab
doTraining = true;
if doTraining
    trainingStats = train(agent,env,trainOpts);
else
    load('rlAutoParkingValetAgent.mat','agent');
end
```

![변경후1](https://user-images.githubusercontent.com/81483791/171120074-5518f35e-ec39-4aad-aa5e-6c1627cf2e40.png)


직접 훈련시킨 결과 우리가 원하는 7번 자리에 맞게 들어갈때 가장 높은 리워드를 받은 것을 확인할 수 있다.

![변경1_한시간_학습](https://user-images.githubusercontent.com/81483791/171120101-a595f47b-9676-45f1-b288-d7dd6a4341a7.png)


한시간 가량 학습을 진행했지만 평균 보상이 올라가지 않고 그대로 유지 

잘못된 결과 → 다시 변경할 필요있음

https://user-images.githubusercontent.com/81483791/171120116-e604cf95-c70b-4ed6-92ce-d8198c2c9d1f.mp4


1. 변화2

```matlab
criticNetwork = [
featureInputLayer(numObservations,'Normalization','none','Name','observations')
fullyConnectedLayer(128,'Name','fc1')
reluLayer('Name','relu1')
fullyConnectedLayer(128,'Name','fc2')
reluLayer('Name','relu2')
fullyConnectedLayer(128,'Name','fc3')
reluLayer('Name','relu3') 
fullyConnectedLayer(128,'Name','fc4')
reluLayer('Name','relu4')
fullyConnectedLayer(1,'Name','fc5')];
```

```matlab
actorNetwork = [
    featureInputLayer(numObservations,'Normalization','none','Name','observations')
    fullyConnectedLayer(128,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(128,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(128,'Name','fc3')
    reluLayer('Name','relu3') 
    fullyConnectedLayer(128,'Name','fc4')
    reluLayer('Name','relu4')
    fullyConnectedLayer(numActions, 'Name', 'out')
    softmaxLayer('Name','actionProb')];
```

```matlab
agentOpts = rlPPOAgentOptions(...
    'SampleTime',Ts,...
    'ExperienceHorizon',200,...
    'ClipFactor',0.2,... 
    'EntropyLossWeight',0.01,...
    'MiniBatchSize',64,...
    'NumEpoch',3,...
    'AdvantageEstimateMethod',"gae",...
    'GAEFactor',0.95,...
    'DiscountFactor',0.998);
agent = rlPPOAgent(actor,critic,agentOpts);
```

네트워크 층 수만 바꿈

```matlab
doTraining = true;
if doTraining
    trainingStats = train(agent,env,trainOpts);
else
    load('rlAutoParkingValetAgent.mat','agent');
end
```

![변경후2](https://user-images.githubusercontent.com/81483791/171120151-9bca5755-3acb-4802-b2d8-c5b5d3f2fca1.png)


학습결과 평균 보상값이 크게 상승하였고 제대로 학습이 이루어졌다

1. 변경3번째

네트워크 레이어 변경없이 

 

```matlab
agentOpts = rlPPOAgentOptions(...
    'SampleTime',Ts,...
    'ExperienceHorizon',100,...
    'ClipFactor',0.2,... 
    'EntropyLossWeight',0.01,...
    'MiniBatchSize',32,...
    'NumEpoch',5,...
    'AdvantageEstimateMethod',"gae",...
    'GAEFactor',0.95,...
    'DiscountFactor',0.998);
agent = rlPPOAgent(actor,critic,agentOpts);
```

값만 살짝 바꿔줌

![변경3](https://user-images.githubusercontent.com/81483791/171120173-a75de85e-6a5e-4773-af20-9a552d52ad3d.png)
