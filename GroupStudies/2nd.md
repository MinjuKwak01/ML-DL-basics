### RNN이란?

- 순차적 데이터를 처리하며, 이전 단계의 정보를 ==**기억**==하여 현재 단계의 출력에 반영하는 신경망
  - 주식 가격 예측
  - 기상 예보 예측
  - 텍스트에 담긴 감정 분류
    ![](https://i.imgur.com/GaXurmj.png)
- **Many to one**

  - ex) 단어가 하나하나 들어가고, 이게 긍정적인 내용인지 부정적인 내용인지 맞추는 것
    ![300](https://i.imgur.com/XVQJk3w.png)

- **Many to many**
  - ex) 영상의 한 프레임 (input)이 들어올 때마다 하나의 단어 (output)가 나가는 경우!
- **One to many**
  - ex) 하나의 이미지를 여러개의 단어들로 변환하는 경우!

### 단어 임베딩이란?

- 단어를 다차원 공간에서 벡터로 표현하는 방법으로, 벡터 간의 거리와 방향은 해당 단어 간의 유사성과 관계를 반영한다.
- 쉽게 말하자면 단어의 의미와 관계를 수치로 표현하여 모델이 이해할 수 있도록 만드는 것!
  - 대표적인 임베딩 방법 : word2vec, GloVe, ..등등
- 모델에 입력하기 전에 단어를 먼저 임베딩 벡터로 변환한 후 RNN에 입력한다.
  ![](https://i.imgur.com/QkqWu1h.png)
- 만약에 여기 x1에 expect 라는 단어가 들어간다면 이러한 d차원 벡터들이 들어가게 된다.
- 자세한 원리를 알고 싶다면 위 방법들 검색해보기!
  ![300](https://i.imgur.com/aGhlPKT.png)

### RNN에서의 Language model (언어 모델) 이란?

- Language modeling : 주어진 단어들의 시퀀스에 대해 다음에 어떤 단어가 나올지 예측하는 작업
- Language model : 위의 작업을 수행하는 시스템
  ![500](https://i.imgur.com/TjVLeLB.png)
  - 단어의 순서 (word ordering)
    - p(I study machine learning) **>** p(Machine I learning study)
    - (왼쪽 문장이 더 자연스럽기 때문에 이 문장의 확률이 더 높다고 계산된다.)
  - 단어 선택 (word choice)
    - p(My computer is very slow) **>** p(My computer is very shirt)
    - (요것도 왼쪽 문장이 더 자연스럽기 때문에 이 문장의 확률이 더 높다고 계산된다.)

![](https://i.imgur.com/IwJ5R8v.png)

- 이것들이 모두 Language model을 활용한 예!!

- 위 사진처럼 단어들의 시퀀스가 주어졌을 때, 다음 단어의 확률 분포를 계산하는 것
- 각 단어 단위로 매번 다음에 올 단어에 대한 확률을 구한다. - 매번 계산하는 것 : 현재 주어진 상태에서 모든 단어들의 확률을 계산 - 만약에 10,000개의 단어가 있다면, 그 다음 단어의 10,000개의 단어에 대한 각각의 확률을 구하는 것 - 그리고 그 중 제일 높은 확률을 가진 것을 선택한다. - 뭔가 단어를 단순히 고정된 규칙으로 예측하는 것이 아니라 "확률 기반" 으로 예측한다는 것이 신기하다.
  ![550](https://i.imgur.com/Q9FypVa.png)
  **RNN을 사용했을때의 단점**
- 순차적으로 처리해야하기 때문에 느리다.
- 병렬화가 어렵다.
- 엄청 긴 문장은 기억하기 힘들다.

-> 해결책으로 LSTM -> GRU -> Transformer 이렇게 발전해왔다!
현재 핫 한 Transformer은 Self-Attention 메커니즘을 활용한다.

- 각 단어와 문장의 다른 모든 단어들과의 관계를 한 번에 계산하여, 빠른 학습과 추론이 가능하다.

#### Attention

- 문맥에 따라 집중할 단어를 결정하는 방식
- 문맥을 파악하는 핵심 : 문장을 읽을 때 가장 중요하다고 생각하는 단어에 집중한다.
  ![](https://i.imgur.com/TudOqAb.png)
- 위 사진을 보면 "카페"와 "cafe"랑 강한 Attention 상관관계에 있다.

#### Attention 모델의 구조

- Encoder (인코더)
  - 입력 데이터를 -> 압축 데이터로 변환 및 출력해주는 역할
    - 정보를 압축함으로써 연산량을 최소화할 수 있다!
- Decoder (디코더) - 압축 데이터를 -> output(출력) 데이터로 출력해주는 역할
  ![500](https://i.imgur.com/94SNWBp.png)
- 위 사진은 영어 -> 스페인어 번역 한다고 가정한 경우
  - 이래저래 머시기해서 나온 Attention coefficient(계수) (그냥 값이라고 생각)가 젤 높은게 you이다.
  - 따라서 그 다음 올 단어는 "estan" 이 될 수 있는 것
