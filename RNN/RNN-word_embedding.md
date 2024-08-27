단어도 어떤 **d차원 공간에 벡터**로서 표현을 하게 된다.
예 : expect라는 단어의 벡터
![200](https://i.imgur.com/nIGqBMI.png)

#### word2vec
- 주변에 있는 단어들을 보고 현재 단어를 맞춤
- 현재 단어를 보고 주변에 있는 단어들을 맞춤
	- 맞춘다는 것 : **확률 분포를 예측한다는 것!!**
![](https://i.imgur.com/mp8sHy6.png)
-> 만약에 **m이 2인 경우** : 앞의 두개의 단어와 뒤에 두개의 단어를 보고 확률분포를 구해서 예측


#### GloVe (Global Vector)
![](https://i.imgur.com/4FMsevR.png)

- 어떤 단어가 나올지 **asymmetric(비대칭)하게** 예측함
- ex) ice라는 단어가 주어졌을 때, solid, gas, water, fashion이라는 단어가 나오는 빈도를 count한 값
- 세 번째를 보면, 비율을 계산했더니 solid는 크게 나오고 gas는 작게 나옴

- - -

#### Sentiment Analysis Problem (감정분석문제)
- 문장 (텍스트의 나열)이 주어지고, 이 문장이 positive한지 negative한지 **이진분류**하는 문제
1. CNN 방식 시도
	- 이미지의 경우엔 input의 크기가 일정해야 함
	- 이미지는 이미지의 의미를 크게 해치지 않는 한 resize하기 상대적으로 쉬움
	- 하지만 문장들은 다양한 길이를 갖고 있음
	- -> 실패
2. RNN 방식 시도
	- variable length를 받을 수 있는 **encoder**가 필요함
	![](https://i.imgur.com/tc7ynNd.png)
	![700](https://i.imgur.com/nMr5k5G.png)
	-  RNN을 이용하여 해당 문장이 positive 한지 negative 한지 판별
	- 학습시킬땐 역전파를 사용하여 학습시킴
	- input이 늘어나도 학습시키는 파라미터는 Whh, Wxh, Why만 학습시키면 되기 때문에 cnn을 쓰지 않고 rnn을 씀

	- 아래 사진은 역전파 과정
	![](https://i.imgur.com/AAHNixM.png)

#### Language Model
![](https://i.imgur.com/jw4ILOg.png)

- - -

**RNN for Language Model**

![](https://i.imgur.com/TVFzKDg.png)

- 각 단어 단위로 확률을 구함
- 매번 input 토큰이 들어갈 때마다 어떤 단어가 나올 지 예측을 함
- 매번 step에서 예측하는 것 : 현재 hidden state에서 주어진 상태에서 모든 단어들의 확률을 계산
	- 만약에 10,000개의 단어가 있다면 그 다음 단어의 10,000개의 단어에 대한 각각의 확률
- 학습을 시킬 땐, ground truth가 매번 주어지기 때문에 그 값과 비교해서 업데이트 시킴
![](https://i.imgur.com/3VBwUT4.png)

**RNN을 사용했을때의 단점**
- 순차적으로 처리해야하기 때문에 느림
- 병렬화가 어려움
- 훈련 단계에서 vanishing gradient가 발생함
- long range dependence를 기억하지 못함