# Semantic Segmentation 1
**Computer vision tasks**
- Classification
- Semantics Segmentation
	- 픽셀 단위로 분할 된 부분이 어떤 것인지 아는 것
- Object Detection 
- Instance Segmentation
	- 각각의 강아지가 다른 객체로 인식되게끔 하는 것


## Semantic Segmentation
- instance를 구별하지 않음
- pixel만 신경씀
![](https://i.imgur.com/anrAqw5.png)
- 사진에서 어떤 하나의 픽셀을 주고 그것을 classification하는 것은 불가능하다.
	- 픽셀 하나가 검은 색 -> 고양이? (불가능)
		- 주변의 context를 봐야 함 (주변 pixel들)
		- 사진에서 일정 크기의 패치를 모두 잘라내서 그 주변의 픽셀들을 보고 classification하는 방법이 있음
		  ![](https://i.imgur.com/4AaE6bn.png)
			- 매우 비효율적이다.
			- 개선을 시킨다면, 학습을 시킬때 모든 픽셀을 사용하는 대신, 몇 개의 샘플을 쓸 수 있을 것임
			- 하지만 testing에서는 inference cost를 줄일 방법이 없음 (모든 픽셀을 학습시키지 않았기 때문에)
	- **해결법?**
		- 일단 전체 이미지를 conv net을 통과시켜서 feature들을 뽑아낸 다음에 semantic segmation을 그 위에 적용시키는 방법
		  ![](https://i.imgur.com/3x1rGM4.png)
		- but **문제점이 있음**
		- 대부분의 conv net들은 쌓을 수록 feature map의 크기를 줄인다. 하지만 이 segmentation 문제에서는 output 크기와 input 크기가 같아야 하는데...
		- 그렇다면? padding을 줘서 다운샘플링(크기 줄이기)을 하지 않게끔 하면 되지 않을까?
		  ![](https://i.imgur.com/KKkXghg.png)
		- but, backpropagation 하는데에 시간이 엄청 오래 걸림
	- 최종적인 solution : **(Deconvolution Networks)**
	  ![](https://i.imgur.com/CAgelVL.png)
		- **downsampling과 upsampling**을 하는 구조로 디자인을 함
		- downsampling은 우리가 이미 알고있는 **pooling과 stride** 방법을 이용함
		- upsampling은 어떻게 할 수 있을까?

- - -
### Deconvolution Networks : Upsampling
![](https://i.imgur.com/loN39iD.png)
- 정보량을 늘려야한다!!
- **Nearest Neighbor** 방식으로 확대 or **Bed of Nails** 방식으로 확대 하는 아이디어가 나왔음
- 아래 사진은 한 단계 더 나아가서 Max Pooling 방식과 대응된 아이디어인 Max Unpooling (하지만 많이 쓰이진 않음)
![](https://i.imgur.com/MJ01QMV.png)
- downsampling 레이어와 upsampling 레이어는 대응이 될 수 있도록

- - -

- **제일 많이 쓰이는 것**
### Learnable Upsampling
Recall : convolution에서 stride가 1보다 큰 것은 downsampling 처럼 작동함
1. 이미지에서 지역을 정한다.
2. filter과 지역의 score을 계산한다.
![](https://i.imgur.com/n0erGEA.png)
![](https://i.imgur.com/7bO5WPV.png)
- input 이미지에 적혀있는 값을 filter과 맞춰보고 값을 모아서 하나의 값으로 써주는 방식이 downsampling operator였음 
	- -> Picking up things in the corresponding region (주워담기)

(이제 그 방식의 반대로 해야 함!!)

- Deconvolution을 하는데, stride가 1보다 크면 upsampling을 함
- 역으로 input값이 있고, filter의 모양대로 마치 도장을 찍는 것 처럼 찍는다.
![](https://i.imgur.com/tw160Uc.png)
- Spreading out the unique shape of a filter to wider range (Stamping) 
	- -> 도장찍기

**예제)**
3x3 deconvolution with stride 2
1. 1과 filter과 곱해서 도장을 찍고
2. 2와 filter을 곱해서 도장을 찍고
3. 겹치는 부분은 값들을 더함
![](https://i.imgur.com/3tZZ6gD.png)

### 1차원 Deconvolution
- stride 가 1일때
![500](https://i.imgur.com/0EGfcH3.png)

- stride가 2일때
![500](https://i.imgur.com/9bnlC7N.png)

#### Deconvolution은 Transpose Convolution이라고도 한다.
convolution을 행렬 연산으로 표현하자면
![400](https://i.imgur.com/JqIuWap.png)

![400](https://i.imgur.com/6AwLk8r.png)
- 왜 transpose convolution이라고 불리는지?
	- input은 똑같이 column vector으로 있는데, convolution은 row wise로 써지고 deconvolution은 column wise로 써지는데 이게 서로 transpose 관계에 있음
![](https://i.imgur.com/zmpZjNF.png)
