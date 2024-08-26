#### RNN의 문제점
- 학습시킬 때, W(가중치)의 Initialization을 조금만 크게 하면 exploding gradient가 발생하고 조금만 작게 해주면 vanishing gradient가 발생하게 됨
	- 뒤로 갈 수록(backpropagation 할때) loss가 업데이트가 잘 되지 않는 문제가 발생
		- 앞에서 무슨 연산을 했던간에 뒤에 마지막 단어 몇개만 갖고 답을 도출해내고 있음
		- 사실상? 단기기억밖에 못하는 것..


**notation!!**
input을 받아서 weight를 곱하는 과정을 **FC (fully connected)**라는 단어로 표현하자.

![](https://i.imgur.com/i06aSfB.png)

- - -
Gradient가 통과하는 방향은 아래 사진과 같음
![](https://i.imgur.com/7121qBq.png)
![](https://i.imgur.com/qG1agPD.png)
- 학습시키면서 gradient를 통과할때 FC를 통과하기 때문에 W가 계속계속 곱해졌었음
	- w가 곱해지는 것을 막고 싶음!!
	- 그래야 gradient 소실을 방지할 수 있음

- - -
### LSTM (Long Short Term Memory)
- **cell state**라는 것을 추가하자
  ![400](https://i.imgur.com/NjrD1JJ.png)
	- FC를 통과하지 않는 state
	- long term memory 역할을 함
- h (hidden state)는 short term memory 역할밖에 못함

- - - 
![](https://i.imgur.com/uoRNEMC.png)

**Forget gate** : 지금까지 기억했던 것들을 얼마나 없애고 새로운 것으로 업데이트 할 지를 결정하는 역할
(이전 시점의 정보 중 어떤 부분을 잊어버릴 지 결정하는 역할)
- h(t-1)와 x(t)를 가지고 장기기억을 업데이트 할 것인지 c(t-1)을 그대로 많이 가져갈 것인지 결정함
- ex) 한 문단을 읽을 때
	- 문장이 끝나는 시점까지의 내용이 연속성을 가지다가 새로운 문장이 시작되면, 아무래도 장기기억에 대한 업데이트가 필요하게됨
	- 그럴 경우에 forget gate 값이 높게 나오길 바라는 것

- - -
![](https://i.imgur.com/pRGgYxg.png)

**Input gate** : 새로운 입력 정보가 셀 상태에 얼마나 반영될지를 제어
(현재 시점의 입력과 이전 은닉 상태를 기반으로 새로운 정보를 생성하고, 이 정보가 얼마나 셀 상태에 추가될 지를 결정)

- - -
![](https://i.imgur.com/JPTfDIU.png)

**Output gate** : 현재 타임스텝의 은닉상태를 결정하는 역할
(다음 타임스텝이나 최종 출력에서 사용할 정보를 선택적으로 출력할 수 있도록 도움)

**간단 요약**
- hidden state 종류를 한 가지 더 추가함 (cell state) -> 장기기억을 전담
- cell state는 직접적으로 FC를 통과하진 않는다. 간접적으로 받는 것은 있음
- 그래도 hidden state의 gradient 소실 문제는 훨씬 완화시킬 수 있음

![](https://i.imgur.com/XOgH5I5.png)
f(t) : forget gate
i(t) : input gate
o(t) : output gate
W(hh) => W
W(xh) => U
b : bias

c(t) 식에 있는 동그라미 의미 : **elementwise multiplication**
- c(t-1) 에서 c(t) 까지의 경로를 최대한 막지 않으려고 설계했는데, 결국 FC를 곱하게 됨
	- 그것을 막기 위해서 (미분하면서 영향을 최소화하기 위해) elementwise multiplication을 사용

- LSTM은 vanilla RNN보다 long-range information을 잘 보존해줌
	- cell sate와 uninterrupted gradient highway(방해받지 않는 경로?) 때문에
- 그렇다고 해서 LSTM은 vanishing / exploding gradient를 완벽하게 해결하진 못함
- 정말 정말 ~ 긴 정보는 기억하지 못함

- - -
### GRU (Gated Recurrent Units)
![](https://i.imgur.com/NP5HWDP.png)

**특징 (LSTM과 유사함)**
- gradient highway를 위해서 **cell state를 추가하지 않았음** (rnn과 가까움)
- hidden state만 갖고 long range 를 보존하고자 하는 목표로 만들어짐
- 파라미터 수가 적음
- gate가 2개 있음 (r(t), z(t)) , convex combination을 사용함 (추후에 더 공부)

- 일반적으로 RNN을 사용해야 할 경우가 있을땐 LSTM을 사용함
- GRU는 파라미터 수가 적기 때문에 계산이 빠르고 오버피팅이 덜 됨
	- 둘 중 (LSTM, GRU 중) 성능이 뭐가 더 좋을 진 모름
- NLP 문제는 대부분 transformers를 이용하여 씀