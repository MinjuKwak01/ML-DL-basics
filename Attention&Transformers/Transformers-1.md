# Transformers

![](https://i.imgur.com/3uBIU1L.png)

가정 : input x 는 여러개의 organically related(유기적으로 연관)된 element로 표현될 수 있다.
- **society 안에 있는 사람들**
- **문장 안에 단어들**
- **비디오 안에 프레임들**

셀프 어텐션(Self Attention) : **각 요소는 자신의 표현을 개선하기 위해 맥락을 참조하여 학습함**
(문장 안에서 각 단어들은 의미가 다르기 때문에 문장 속에서 자기에 대한 표현방식을 조금씩 바꿔가는 방식)
- 더 구체적으로는, **시퀀스 내 다른 요소들의 가중치 합**으로 표현을 개선함

### Attention 리뷰
![](https://i.imgur.com/lzPEv1H.png)


- - -

Transformer에서는 **Query, Key, Value를 만들어서 씀**
![](https://i.imgur.com/yMMHCpz.png)


- 자기 자신이 Query, Key, Value가 될 때 weight을 곱해서 만들어서 씀
- input 토큰이 n개가 들어왔을 때,
- 각각의 토큰이 Query, Key, Value로 쓰여야 함 -> linear transformation을 해줌
- linear transformation 한 결과 : **W(Q), W(K), W(V)로 이름붙임 (이건 파라미터)
- 이 W들은 Query, Key, Value로써 표현하기 위한 방법들을 배움

- **attention value** (value들의 weighted sum)은 위 x(i)라는 단어를 표현하는 새로운 방법이 됨
![](https://i.imgur.com/uQ7MZqo.png)
- W(O)는 원래 차원으로 다시 되돌려놓기 위한 용도

- - -

**i = 1일때** (첫 번째 단어)
- x(1)이 Query가 됨
- **이번 텀에는 너가 주인공 !!**
- 나머지 단어들 + x(1)-> 자기자신은 mirror that reflects you
![500](https://i.imgur.com/qLORpKM.png)

- 본인(x(1))포함 나머지 것들은 **Key** 역할을 함
![400](https://i.imgur.com/FyyydXp.png)
- 내적을 한 후 softmax를 씌워서 합이 0~1이 되도록 만듬
- x에서 구한 value들을 만들어내고 가중치를 곱해서 더함 ->
	- 지금 첫 번째 단어에 대해 얼마나 연관이 있느냐에 관련해서 보고있기 때문에 현재, 첫번째 단어에 대한 가중치가 제일 높음 (0.93은 attention score)
![500](https://i.imgur.com/yg5W8LP.png)
- z(1)을 구함으로써 x(1)을 새롭게 표현해줌
	- x(1)을 z(1)으로 transform 시킴
	- x(1) 본인이 많이 유지되긴하지만 다른 단어들도 조금씩 섞여 들어가서 전체 context에 맞게 벡터들이 업데이트 됨 (contextualize 시킴)

**예시**
![](https://i.imgur.com/4znPoAI.png)
![](https://i.imgur.com/y5vUMMJ.png)
![](https://i.imgur.com/AILZMap.png)

