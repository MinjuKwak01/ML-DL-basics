### Many-to-Many RNN
![](https://i.imgur.com/ZQf60P8.png)

만약에 문장 번역 문제를 푼다고 하면,
![400](https://i.imgur.com/mlljRhy.png)
- - -
![](https://i.imgur.com/qExLkjm.png)
이러한 결과를 도출해내기 위해선 일단 모델이 훈련된 상태여야 함
**but... 이 방식의 문제점은?**
![](https://i.imgur.com/hvfT2dn.png)

- RNN은 1:1 관계임 -> 하나가 들어가면 하나만 나와야 함
- 근데 문장은 어순도 다르고 길이도 다름
- 하나하나씩 만들어내는 것은 불가능한 것 같다.. 다른방법??
	- ex) Vivo 는 "I live"에 해당
	- **Encoder, Decoder Structure**을 도입

- - - 
### Encoder, Decoder Structure
![](https://i.imgur.com/ABlsKxi.png)

- 문장이 끝날때까진 output을 내지 않고 문장을 기억만 함
- input은 단어 단위로 임베딩
- hidden state는 문장단위의 내용을 기억할 수 있도록 함
	- 마지막 hidden state는 **문장 전체의 내용을 담고 있는 벡터**가 나옴
![](https://i.imgur.com/UcR8TcS.png)
- 인코딩 하는 단계에서는 loss가 없음
- 디코딩에서 loss를 보는 작업이 필요

- - -
### Decoder : Auto-Regressive Generation
![](https://i.imgur.com/iJp5uUm.png)

- **h(0)**은 번역해야 할 full 문장의 의미 (스페인 문장)
- **SOS token** : 문장의 시작

- h(0)이랑 SOS 가 들어왔을때 h(1)가 갖고 있는 정보
	-  첫번째 자리에 있다는 정보
	- 번역할 문장의 내용을 갖고 첫번째 output : y^(1)을 냄
- output y^(1)을 그 다음 input으로 줌 **(Auto-Regressive 하다고 함)**
- 훈련을 시킬 땐 y(1)에 "I"다 라는 ground truth를 주고 예측한 값과 비교해서 "I" 일 확률이 올라가도록 cross-entropy를 주면 가중치 업데이트가 됨

- - -
### Teacher Forcing
![](https://i.imgur.com/pc4K7Mx.png)

훈련을 시킬때, 처음엔 랜덤한 값으로 초기화를 할 텐데, 그 값을 갖고 훈련시키면 뒤에 내용은 줄줄이 다 틀릴 것
따라서 훈련할때는 예측값이 무슨 단어가 나오던 간에 그 **다음 input은 ground truth를 넣어줌**
모델을 완성시키고 추론을 할때는 그 다음 input은 예측한 값을 넣어줌
- - -
![](https://i.imgur.com/xY17BK3.png)

**인코더(encoder)은 many-to-one 처럼 씀**
**디코더(decoder)은 one-to-many 처럼 씀**
+학습 시, 인코더, 디코더 둘 다 학습시켜야 함
+위 사진은 RNN이 아닌 LSTM으로 생각하면 됨

![](https://i.imgur.com/Cgz9wbI.png)
![](https://i.imgur.com/4fUITsc.png)

