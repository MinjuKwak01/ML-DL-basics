# CNN Case Studies

#cnn
#AlexNet
#VGGNet
#GoogLeNet
#ResNet

필터 : 입력 데이터에 적용되어 특징을 추출하는 역할
채널 : 이미지 깊이 (색상 또는 특징의 차원)
#### AlexNet
![](https://i.imgur.com/BtUQC3V.png)
- 사진은 224 x 224 인데 논문 상으로는 227으로 되어있음.
- Input **224 x 224 x 3**
	- Conv1 96개 11 x 11 filters, stride 4
	- => output size : **(227 - 11) / 4 + 1 = 55**
		- **55 x 55 x 96**
	- => total number of parameter : **(11 x 11 x 3 +1) x 96**
- input **55 x 55 x 86**
	- Pool1  3 x 3  filters, stride 2
	- => output size : **(55 - 3) / 2 + 1 = 27**
		- **27 x 27 x 96**(채널 개수 그대로)
	- => total number of parameter : 0
	  ![](https://i.imgur.com/ga9RsZQ.png)
- Normalization layer을 사용했는데, 요즘은 사용하지 않는다.

#### VGGNet
- 2014년도에 대회 우승
- 8 layers->19 layers 로 깊어짐
  ![400](https://i.imgur.com/fyEFSJB.png)
- AlexNet에 사용했던 11 x 11 이나, 5 x 5필터를 사용하지 않는다.
	- **3x3 필터들만 사용한다.**
		- 왜 3x3 필터만 사용?
		- => 3x3 두 개를 쌓는 것은 5x5 하나를 쌓는 것과 똑같다.
		  ![400](https://i.imgur.com/qOdVr0w.png)
		- **어떤게 더 좋을까?**
			- 학습해야하는 파라미터 개수가 3x3 여러층을 쌓는게 더 적다. -> 더 가벼운 모델!
			- 성능도 좋다.
				- 여러 층을 쌓으면, 층을 통과할때마다 activation function을 여러번 적용시킬 수 있기 때문
	- VGG 16보다 VGG 19가 메모리를 더 잡아먹지만, 좋은 성능을 보였다.
- Number of Layers : Conv, FC layer만 세면 된다.


#### GoogLeNet
- 22 layers
- 이전에는 고정된 크기에서만 특징을 추출할 수 있었다.
	- 이로 인해 큰 물체는 초기 층에서 제대로 발견되지 않고, 작은 물체는 초기 층에서만 발견되고 이후 층에서는 놓치는 문제가 발생했다.
	- 복잡한 물체도 크기가 작을 수 있고, 단순한 물체도 크기가 클 수 있다.
	- => 필터(커널) **1x1, 3x3, 5x5를 중첩해서 쓰자!**
- 각 필터는 따로따로 크기를 보고 학습하고 channel-wise로 다시 합쳐준다. 
	- 각 채널에 대해 독립적으로 연산을 수행한 후, 그 결과를 합쳐서 하나의 출력 채널로 만든다는 의미
	- (같은 크기가 되어야함) -> 28x28
	![](https://i.imgur.com/CDdg2fc.png)
 - 문제점 : 한 층 계산하는데, 연산량이 어마어마했다. -> 이것을 줄이고 싶었음
 - **1x1 conv 쓰는 이유?**
	 - dimension reduction (차원 줄이기)
	 - 다른 자리를 안보고 모든 자리들이 독립적으로 계산됨
- 1 x1 conv를 써서 일단 dimension을 줄이고 보자
  ![](https://i.imgur.com/6WVCdp6.png)
  ![400](https://i.imgur.com/XBp2I6U.png)
  -  계산량을 크게 줄임
  - fully connected layer 을 없앰
  ![](https://i.imgur.com/WkzLvWg.png)

  - 신경망이 너무 깊어지면 gradient가 앞까지 전달이 안될 수 있음
    - 마지막에 달아놓은 Classifier을 중간에 두 개 더 달아서 거기서 back prop하게끔해서 loss 업데이트가 잘 될 수 있게끔
    - -> (but) 굳이 안해도 된다는 결론이 나왔다.
  - 파라미터 수가 AlexNet, VGGNet보다 훨씬 적다.
    - why? fully connected layer 두 개를 없앴기 때문


#### ResNet
- 152 layers -> 엄청 깊음
- 사람보다 분류를 잘하는 모델임
- 거의 끝판왕
- 더 쌓을수록 유리할까??
	- 이론적으론 그럴 것 같다..!!
	  ![](https://i.imgur.com/83Q3m8v.png)

	- 더 깊게 쌓은 것(56-layer) 보다 더 얕게 쌓은 것(20-layer)이 error rate가 적다. -> 얕은 모델이 더 분류를 잘함?!
		- 오버피팅(과적합)이 일어난건가? (training error는 작아지는데, test error는 높아지는 현상)
			- 아님. test error는 증가하지 않는다.
		- 가설 : 깊은 모델은 최적화하기 어렵다. (ex vanishing gradient problem)
	- 가설을 해결하기 위한 아이디어 : input과 output사이 shortcut을 더해줌.
		- 전 : x -> H(x) 로 바로 모델링을 해야했다.
		- 후 : 디폴트로 x가 가서 h(x)랑 차이가 난다면 모델링을 해달라고 함.
			- => 층을 왕창 쌓아도 안 필요하다면, 안쓰겠지..!!
			- 층을 22 -> 152층까지 올려버렸다.
	  ![](https://i.imgur.com/GqV5DpP.png)

	- 계산량이 너무 많아서 1 x 1 conv layers를 추가해준다.
		- input과 output 크기가 똑같아야하는데, 그래서 마지막에 256으로 차원을 늘려준다.
		- 차원을 줄였다가 늘림
		 ![](https://i.imgur.com/Ior7g1Y.png)

	