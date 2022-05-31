# rnn

복습: No
작성일시: 2022년 5월 31일 오후 11:49

# RNN 이란?

순환신경망

Recurrent Neural Networks (RNN)

시계열 데이터와 같이 시간의 흐름에 따라 변화하는 
데이터를 학습하기 위한 인공신경망

### RNN 활용

1. Vector to Sequence 이미지를 보고 자막을 다는 Image Captioning
2. Sequence to Vector
글을 보고 긍정인지 부정인지를 판별하는
Sentiment Classification
3. Sequence to Sequence
주식가격이나 시계열 데이터 예측
4. Delayed Sequence to Sequence
자동번역

시퀀스 길이에 관계없이 인풋과 아웃풋을 받아들일 수 있는 네트워크 구조이기 때문에
필요에 따라 다양하고 유연하게 구조를 만들 수 있다

### RNN 문제점

Long-term dependency

RNN은 관련 정보와 그 정보를 사용하는 지점 사이 거리가 멀 경우 역전파시 그래디언트가 점차 줄어 학습능력이 크게 저하되는 문제가 발생함

**→  gradient explosion, vanishing 문제**

이를 위한 해결방안으로 나온 것이
LSTM, GRU

# LSTM 이란?

Long Short Term Memory Networks (LSTM)

기존 Vanilla RNN이 hidden state만을 
사용하던 것과 달리
 LSTM에서는 Cell State라는 새로운 Flow를 도입

- ForgetGate : 이전 Cell에서 넘어온 hidden state에 대해서 과거의 정보중 어느 것을 Cell State에 반영할 것인지
- Input Gate : 현재 입력된 정보를 얼마나 Cell State에 반영할 것인지
- Output Gate : 다음 Cell에 전달할 hidden state 값에 Cell State를 얼마나 반영할 것인지

### Forget Gate

Sigmoid 함수를 통해 얻어진 0~1 사이의 가중치를 곱해줘서 
학습을 하면서 상대적으로 중요한 정보에는 높은 가중치를
Gradient 갱신에 별로 좋지 않은 영향을 끼치는 정보에 대해서는 낮은 가중치를 갖게 함

과거 정보 중 중요한 정보만 넘겨주고 불필요한 정보는 가중치를 낮춰 삭제

### Input Gate

현재의 입력값 X_t와 이전 hidden state를 적절히 사용하여 
현재 Cell의 Local State를 얻어내고, 
이를 Global Cell State에 얼마나 반영할지를 결정하는 게이트

시그모이드 함수를 통해 현재의 정보 중요도에 따라 가중치를 달리 줘서 반영함

과거 hidden state에서 적절히 forget이 이루어진 cell state  ft와
현재 cell에서 얻을 수 있는 새로운 데이터를 반영한 cell state  it를 더해서 
최종적인 global cell state를 업데이트 함

### Output Gate

최종적으로 얻어진 Cell State 값에서 어느정도를 취해서 Hidden State로 전달할 것인지 결정

LSTM은 Cell State 개념을 도입하여 과거의 데이터를 유지하면서도 
불필요한 데이터는 Forget Gate를 통해 삭제하여 Gradient Update에 최적의 상태를 유지

# GRU 란?

Gated Recurrent Units (GRU)

RNN의 일종으로  Vanishing gradient problem 을
해결한 LSTM을 더 간략한 구조 문제 해결
cell state 를 사용하지 않음

[실습](rnn%209e5335fd5a4741f2a64fe0bb0075fc23/%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8%20baf847aab14741bcbf7cde5d45e4b756.md)