# Attention
Attention : 문장 속 단어들 간의 관계를 파악하기 위한 기술
(신경망이 입력 데이터 중에서 중요한 부분에 더 집중할 수 있도록 도와주는 기술)

RNN은 정해진 크기의 hidden state에 모든 input의 정보를 담아한다는 한계점이 있었음
- 그렇게 되면 앞에 있는 내용들을 다 학습하지 못한채로 output을 만들어내야 함

#### Attention idea
- 디코더가  hidden state의 각 단계에 접근할 수 있도록 허용해주도록함
Attention function : **Query, Key, Value**
Attention value : **value들의 weighted average**
weight는 query를 기준으로 key들과의 relevance를 합산

query : 기준
query : 현재 hidden state
key의 후보 : 각각의 hidden state
value : 각각의 hidden state를 표현하는 벡터
- - -
![](https://i.imgur.com/SJFcGIB.png)
- Query와 Key는 같은 차원이어야 함
- Value와 Attention value는 같은 차원이어야 함
- 대부분의 경우에선, 4개가 다 같은 차원이다.

- - -

seq2seq 모델로 예를 들면
- **Query** : 매 timestep에서의 디코더의 hidden state
- **Key** : encoder hidden state (원래는 디코더에서 참조할 수 없었음)
- **Value** : encoder hidden state

Key와 Value는 별도로 존재해도 되고 같은 것을 써도 된다.

![](https://i.imgur.com/fHwtVJA.png)
- - -
- key와 query 사이의 similarity를 구해줌 (dot product 이용)
![](https://i.imgur.com/jbGntsZ.png)
- dot product 한 결과를 합한 결과가 Attention score
- - -
- 확률로 해석하기 위해 softmax를 씌워줌 -> 결과 : Attention coefficients
![](https://i.imgur.com/R8GwfdJ.png)
- 그 다음에 hidden state를 value로써 사용함
- 이때의 value는 hidden state를 대표하는 벡터로써 사용이 됨
	- 정리하면 **key는 query와의 similarity를 계산하는데에 사용하는 용도**
	- **value는 계산된 similarity를 가지고 합산하는 용도**
- - -
![](https://i.imgur.com/sdR0aAo.png)

a(0)은 우리가 access할 수 있는 hidden state의 가중 합(weighted sum)
관련성이 높은 것들에게 높은 가중치가 가도록 합쳐져 있음
- - -

![](https://i.imgur.com/a7tnqjS.png)
- weighted sum 한 것을 같이 가지고 다음 단계를 진행함
- s(0), a(0)를 같이 가지고 가면 크기가 두 배가 되기 때문에 (한 step 갈때마다 크기가 두배로 변하면 안됨)
	- FC를 하나 통과해서 원래 크기로 돌려놓음
- - -

![](https://i.imgur.com/UDkZfB6.png)
- input 들어가고 output 내놓음
- 이 단계를 계속 반복하여 그 다음엔 s(1)이 query가 됨

- - -

아래 사진은 위 단계들의 수식
![](https://i.imgur.com/oU2bwza.png)

- - -

![](https://i.imgur.com/UylUaBb.png)
Attention 관련 모델이나 논문을 볼때, 이것들을 체크하기 (Query, Key, Value)
![](https://i.imgur.com/n2cxJgG.png)

- - -
### Machine Translation with Attention : Encoding

seq2seq과 비교해서 Attention에서 달라진 것 : hidden state 중간중간 step들을 decoding할때 사용할 수 있다는 것만 달라짐

첫 번째 시작에선 문장의 시작이라는 특수한 토큰이 들어옴
query와 key들(hidden state)와의 **dot product를 구해서 attention score을 구함**
![](https://i.imgur.com/iFQX0yI.png)
- 현재 상태를 봤을 때 어디에 집중해야 하는가에 대한 다섯 개의 score가 나옴
	- 각 입력 요소가 Query에 얼마나 중요한지 나타냄
- - -

- softmax를 통과 시키면 0~1 사이의 확률로 나옴 (사진에선 softmax가 빠져있음)
- **그 다음 attention value를 구함 (value들의 weighted sum)**
![](https://i.imgur.com/YNbkQeZ.png)

- - -

- 그 다음 query에 attention value를 가져다가 붙임
![](https://i.imgur.com/szQgJAJ.png)

- - -

- 첫 번째 스페인어의 output 단어를 구함
![](https://i.imgur.com/oiCU2B1.png)

- - -

- "todavia"가 다음 것이 input으로 들어감
- 디코더가 다음에 받게 되는 것은 기존에 있던 hidden state와 받은 새로운 단어임
- 다음에 key들과 similarity를 다시 계산하고 softmax를 씌워즘
- attention value가 구해지고 또 가져다가 그 다음에 붙임
![](https://i.imgur.com/EJrlNeC.png)
- estan은 you 라는 뜻임 그래서 attention coeffiecient를 보니 you 라는 단어에 젤 많이 집중함 (높은 가중치를 부여함)
![](https://i.imgur.com/2QuyPZ4.png)
- weight는 주로 attention coefficient 또는 attention score을 나타냄