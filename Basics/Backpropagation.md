# BackPropagation

![](https://i.imgur.com/dgYGYdj.png)

- 사람이 인위적으로 사진의 feature을 뽑는 것이 아닌, feature을 뽑아내는 것도 머신러닝 모델을 쓰자!
- end-to-end training 은 input 부터 output까지 하나의 loss를 갖고 쭉 업데이트를 해주는 것
	- end-to-end training의 단점?
		- 사실 이미 백그라운드 지식이 있으면 모델에 적용을 해줘서 간단하게 처리할 수 있게 하고 모르는 지식에 대해서만 end-to-end training을 적용하는 것이 좋다.

- - -
#### Neural Network
![](https://i.imgur.com/sNHbvMf.png)

- 한 직선으로 빨간색 두 점과 파란색 두 점을 구분할 수 없었음
- = XOR을 표현 할 방법이 없었음
	- 퍼셉트론을 여러개 사용해서 해결!
![600](https://i.imgur.com/gXd5Q1w.png)
- f 는 activation function
![600](https://i.imgur.com/pK7irqz.png)
- activation function이 없으면 linear regression
![450](https://i.imgur.com/xTmdD2T.png)
-  위 그림은 input을 3072차원의 이미지가 들어오고 어떤 가중치를 곱해서 10개의 score을 만들어내는 한 층짜리 neural network임
- 각각에 있는 값들이 모든 output으로 연결되는 가중치가 하나씩 있음.

- - -

![450](https://i.imgur.com/n1rUZT5.png)
![450](https://i.imgur.com/pkIW13y.png)
- **single layer perceptron**과 **multilayer perceptron**은 이렇게 식으로 표현할 수 있다.
	- 그런데, 생각해보면 행렬의 곱셈은 결합법칙이 성립하기 때문에 MLP(multilayer perceptron)는 W1과 W2를 곱해서 W로 표현할 수 있다.
	- 그러면 수학적으로 single layer perceptron과 똑같은게 아닌가..?
	- single layer perceptron으로 해결하지 못했던 XOR 문제를 17년 걸려서 해결했다고 했는데.... 뭔가 이상함
	- 놓친 부분이 있는데 그게 바로 activation function! -> 비선형성을 주는 것 (이게 없으면 안됨)
![600](https://i.imgur.com/g7KsZUY.png)
![](https://i.imgur.com/9wUF36n.png)
#### Gradient 계산
결국 우리가 해야 하는 것은 loss를 계산해서 올바른 방향으로 업데이트 해줘야 함
W1, W2가 업데이트해서 배워나가야 하는 파라미터들임
loss를 W1, W2에 있는 각각의 값들로 편미분해서 업데이트해야 함
![](https://i.imgur.com/PwN9dkF.png)
![](https://i.imgur.com/nqyxYF6.png)

### Backpropagation
제일 뒤에서 loss가 발생하는데 이것을 앞쪽으로 전달하는 역할
각각에 있는 값들이 최종적인 값을 내는데에 얼마만큼 영향을 주고 있는가를 계산하는 과정
![](https://i.imgur.com/J6Q5Fg3.png)
- - -
![200](https://i.imgur.com/OM1LPb8.png)
![300](https://i.imgur.com/4p1lbyc.png)
![300](https://i.imgur.com/GUzAjU9.png)
- - -
![400](https://i.imgur.com/1IZ8iWN.png)
- - -
![](https://i.imgur.com/naYvLbu.png)
노드를 만날때마다 **Upstream Gradient**가 뒤에서부터 오고 **Local Gradient**를 곱해서 **Downstream Gradient**로 나간다.

#### Backpropagation Example
![](https://i.imgur.com/H9yYqdp.png)
- - - 
![400](https://i.imgur.com/2n9RegF.png)
- - -
![350](https://i.imgur.com/FcTplpx.png)
- - -
![400](https://i.imgur.com/FkuOxFx.png)
- - -
![400](https://i.imgur.com/63jH1mB.png)
- - -
**구하고자 하는 것**
- gradient를 구해서 파라미터들이 얼마나 틀렸는지를 알고 업데이트하고 싶음
- 파라미터 : w0, w1, b
- -0.2, -0.4, 0.2가 젤 필요한 것!
![](https://i.imgur.com/qTKApSo.png)
