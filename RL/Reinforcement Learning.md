
# 강화학습 이란?

- 머신러닝의 한 분야

강화학습의 목적: 
 reward를 최대화 하는 policy를 찾는 것

Environment(환경)이 Agent에게 특정 상황(state)을 주면
 Agent는 그에 대해 반응(action)을 하고 Environment은 Agent에게 보상(reward)를 줌

이러한 과정으로 Agent는 Environment와 상호작용을 하며 reward를 많이 취할 수 있는 action들을 학습을 하는 것

### 강화학습 알고리즘의 분류    
![image](https://user-images.githubusercontent.com/81483791/171120444-525a27c0-1bcc-4e62-8ca6-f7d6704feef8.png)

**Policy**
Agent의 행동 패턴 (state에서 어떤 action을 취할지)

- Deterministic(결정적) Policy: 주어진 state에 대해 하나의 action을 줌
- Stochastic(확률적) Policy: 주어진 state에 대해 action들의 확률 분포를 줌

**Value function**
해당 state과 action을 취했을 때 이후에 받을 모든 reward 들의 가중합
뒤에 받을 reward 보다 먼저 받을 reward에 대한 선호를 나타내기 위해 
discounting factor 사용

**Model**
environment(환경)의 다음 state와 reward가 어떨지에 대한 agent의 예상

- State model :
- Reward model :

### 강화학습 알고리즘의 분류    
![image](https://user-images.githubusercontent.com/81483791/171120560-b0c31cd8-0f59-4d7b-be56-9ac0e25d5dbf.png)

- Policy-Based
 Value function이 없이 policy만을 학습하는 agent
- Value-Based
 Value function 만을 학습하고 policy는 암묵적으로만   갖고 있는 알고리즘

### PPO 이전 강화학습 알고리즘

1. **DQN**
Deep Q-Networks

input으로부터 policy를 control하는 것을 배우는 딥러닝 모델

Atari Game의 raw pixel들을 input으로해서 
Q-learning으로 학습된 CNN을 function approximator로 
이용하여 value function을 output으로 냄

- input : raw pixels
- output : value function

1. **A3C**
Asynchronous Advantage Actor-Critic

Actor-Critic 알고리즘은 Actor 네트워크와 Critic 네트워크 사용

- Actor : 상태가 주어졌을 때 행동을 결정 - Critic : 상태의 가치를 평가

여러 actor-learner가 생성한 샘플들을 이용하여 
신경망 모델을 학습시키고, 학습된 신경망 모델을 
다시 actor-learner에 복사하는 방식

1. **TRPO**
Trust Region Policy Optimization

trust region이란 perfomance가 상승하는 방향으로의 update를 보장할수 있는 구간을 의미하며,
TRPO는 이를 이용해 performance가 더 나은 policy로 업데이트 하기위한 optimization 기법에 대한 방법론

Objective Function을 Maximize하기 위한 Policy의 paramater인 세타 값을 찾기 위해, KL divergence의 Hessian을 구하며 
즉, 2차 미분 값들을 구한다

### Proximal Policy Optimization (PPO)

**이전 알고리즘 대비 개선사항**

- Scalability (확장성)
대규모 모델 및 병렬 구현
- Data Efficiency (데이터 효율성)
- Robustness (강건성)
hyperparameter tuning없이 다양한 문제들에 적용되어 해결

**TRPO vs PPO**

TRPO의 data efficiency와 robustness를 유지하면서도
2차 미분을 통해 복잡하고 어려웠던 것을 개선하여

1차 미분으로 근사화하는 방법을 나타냄

1. Clipped Surrogate Objective
2. Adaptive KL Penalty Coefficient

### Proximal Policy Optimization (PPO)    
![image](https://user-images.githubusercontent.com/81483791/171120644-ca374a6f-faf3-42e4-84a9-e8180f283fdc.png)    
![image](https://user-images.githubusercontent.com/81483791/171120700-108b2ba7-ae6b-487c-873b-bea9f797aa11.png)

1. Clipped Surrogate Objective     
![image](https://user-images.githubusercontent.com/81483791/171120749-3cdf540e-9437-4013-b9e6-0472ef313de3.png)

2. Adaptive KL Penalty Coefficient     
![image](https://user-images.githubusercontent.com/81483791/171120787-3a4927ca-a9b6-4e17-a27f-352810c9ae56.png)

성능 비교    
![image](https://user-images.githubusercontent.com/81483791/171120907-46811e87-1b17-480e-97bc-42e27bf7c6fe.png)

Aracde Learning Environment에서 다른 알고리즘과 비교    
![image](https://user-images.githubusercontent.com/81483791/171120967-7b17aad5-44af-46de-b1ca-a18148563be0.png)

49개의 Atari 게임에서 PPO가 가장 많이 빠르게 학습됨 
그러나 마지막 에피소드에 대해서 즉 정확도에서는 ACER가 더 뛰어남
