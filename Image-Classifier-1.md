### Image Classification
컴퓨터는 이미지를 볼 때 단지 RGB 픽셀들로 이루어진 값들이다.
![](https://i.imgur.com/Xyg27Ou.png)
### Nearest Neighbor Classifier

train 단계 : 모든 데이터와 레이블을 암기
predict 단계 : 가장 비슷한 training example의 레이블을 output으로
![](https://i.imgur.com/PXRexsi.png)
사진과 사진과의 similarity를 계산할 수 있어야 한다.

- - -
#### L1 distance
![](https://i.imgur.com/GedevkY.png)
#### L2 distance
![](https://i.imgur.com/cyKU9Mv.png)
![](https://i.imgur.com/MYp4qVL.png)
**각 단계의 시간복잡도**
- training : O(1)
- prediction : O(N) -> 굉장히 안 좋은 시간복잡도
	- 모든 훈련 데이터의 샘플을 확인해야 하기 때문

![](https://i.imgur.com/cU87azN.png)
- k는 가장 가까운 이웃의 수를 의미한다.
- k=1일때 decision boundary가 울퉁불퉁함 -> 오버피팅 될 가능성이 높다.

**KNN의 단점**
(이미지 분류를 할때 knn은 아예 안쓴다.)
- 차원의 저주 : 차원이 커지면 커질수록 데이터 포인트 개수가 많아져야만 비슷한 수준의 거리를 유지할 수 있게 된다. -> 정확하지 않다.
	- 고차원에서는 대부분의 데이터가 매우 희소하게 분포하며 데이터 포인트 간의 평균 거리가 크게 증가한다.

**KNN을 사용하는 경우**
![](https://i.imgur.com/94CWujv.png)
#### Linear Classifier 방식 이용해서 이미지 분류
![](https://i.imgur.com/sahXy1A.png)
![](https://i.imgur.com/D0I9Umj.png)

- W값(가중치)은 픽셀 값 하나 하나가 레이블에 얼마나 기여하는지를 나타내는 값
- b(bias)는 데이터 x를 보지 않고 label 중에 뭐일 것 같냐를 묻는다면 training에서 많이 봤던 거로 답할 것임
	- x 데이터를 보면 그 label을 답할 것임
	- b는 데이터 자체의 분포를 모델링 해주기 위해서 존재하는 것
	- 항상 bias(b)는 존재해야 한다.
![](https://i.imgur.com/Ql3a6uo.png)

**장점**
- w만 갖고 있으면 x와 곱해서 바로 답을 얻어낼 수 있다 -> space efficient함
- 계산이 빠르다.
![](https://i.imgur.com/D16Nzy8.png)

- - -
#### Softmax Classifier
만약에 Linear Classifier처럼 score로 판단한다면 해석하기 어렵다. 얼마가 되어야 좋은지 알 수 없음.
=> **0과 1 사이로 변환하여 확률로 해석하게끔 하면 좋겠다.**
- sigmoid function을 사용한다.
- 만약에 클래스가 2개 이상이라면 softmax function 사용