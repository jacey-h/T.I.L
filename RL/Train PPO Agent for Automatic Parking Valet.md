# 강화학습 실습

복습: No
작성일시: 2022년 5월 31일 오전 7:33

# Train PPO Agent for Automatic Parking Valet

주차장

차량의 목표 포즈
x = 47.7500
y = 4.9000
θ = -1.5708

센서

- 카메라

d : 주차위치까지의 거리
φ : 주차위치에 대한 각도

- 라이다

12개의 라이다 선분 (최대 6m)

### Simulink 모델

Enabled Subsystem 불록으로 
차량이 빈 자리를 탐색할  때는 MPC controller
주차 모드일 때는 RL controller

**MPC (Model Predictive Control) :**

모델예측제어는 미래 출력치를 예측하고 이를 
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

![변경후1.PNG](%E1%84%80%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20d6a36a36a0244e27aba51c2a24536dfd/%EB%B3%80%EA%B2%BD%ED%9B%841.png)

직접 훈련시킨 결과 우리가 원하는 7번 자리에 맞게 들어갈때 가장 높은 리워드를 받은 것을 확인할 수 있다.

![변경1 한시간 학습.PNG](%E1%84%80%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20d6a36a36a0244e27aba51c2a24536dfd/%EB%B3%80%EA%B2%BD1_%ED%95%9C%EC%8B%9C%EA%B0%84_%ED%95%99%EC%8A%B5.png)

한시간 가량 학습을 진행했지만 평균 보상이 올라가지 않고 그대로 유지 

잘못된 결과 → 다시 변경할 필요있음

[변경1일때 학습.mp4](%E1%84%80%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20d6a36a36a0244e27aba51c2a24536dfd/%EB%B3%80%EA%B2%BD1%EC%9D%BC%EB%95%8C_%ED%95%99%EC%8A%B5.mp4)

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

![변경후2.PNG](%E1%84%80%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20d6a36a36a0244e27aba51c2a24536dfd/%EB%B3%80%EA%B2%BD%ED%9B%842.png)

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

![변경3.PNG](%E1%84%80%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20d6a36a36a0244e27aba51c2a24536dfd/%EB%B3%80%EA%B2%BD3.png)