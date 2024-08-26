### RNN이란?
RNN : 순차적 데이터를 처리하며, 이전 단계의 정보를 기억하여 현재 단계의 출력에 반영하는 신경망
만약에 input이 sequence로 된 여러 개의 데이터라면...?
ex)
- 영상 데이터
- 주식
- 텍스트
- 기상예보

**label y** 는? -> 문제에 따라 다름. (하나일수도, 여러 개 일수도)

**순서를 가진 데이터 (sequential data)**
- 이미지, 텍스트 등등
- input 자체가 sequence든 아니든 label y는 풀고자 하는 task에 따라 sequence일 수도 있고 아닐 수도 있음
![](https://i.imgur.com/PhtUOno.png)
- - -
- Many-to-one
	- 예를 들면 프레임 3개가 들어가고, 풀고자 하는 테스크는 하나 인 경우
- Many-to-many
	- 예를 들면 프레임이 들어올때마다 추가적인 작업을 하고싶은 경우
	- Video -> words
- One-to-Many
	- 예를 들면 image 하나 -> words (단어들)

- - -
### RNN
**특징**
- input 순서를 하나하나 읽는다.
- internal state 를 유지한다.
	- 지금까지 읽어들인 전체 sequence의 내용을 기억한다는 뜻
- 각각의 step에서 internal state가 바뀌게 된다.
  ![200](https://i.imgur.com/l8MvuHh.png)

	- old state가 input을 받아서 새로운 internal state를 결정한다.
- 처음에 h0으로 시작 (initial state)
- x(i)번째를 부를때마다 (= input을 하나씩 읽어올때마다) h(i-1) 이 h(i)로 바뀌어간다.
  ![](https://i.imgur.com/BYpQxNR.png)

- 새로운 input을 받으면 변경된 hidden state를 전달함
  ![300](https://i.imgur.com/8X1tej3.png)

- 맨 위 사진과 그 밑 사진이 같다고 할 수 있을 때는 같은 function (f) 를 사용할때 (같은 파라미터를 사용할때)


- - -
#### Many to One

- fully connected을 상기시켜보면...
	- input x가 들어오면 어떤 weight을 곱해서 non-linear function을 통과시키는 것
- 비슷한 접근 방식으로 만약에 2개의 input이 있다면..
  ![400](https://i.imgur.com/cSMO00V.png)

	- 각각 W를 곱해준다. h(t-1)에 곱해진 W는 W(hh), x(t)에 곱해진 건 W(xh)로 하고 non-linear function 써줌 (tanh를 씀)
	  ![250](https://i.imgur.com/0nK28Xg.png)
	  ![400](https://i.imgur.com/xmzx1PQ.png)

- weight (W)은 모든 step에 대해서 공유한다. (W(xh), W(hh) 는 같다)
  ![](https://i.imgur.com/nMWPPt2.png)

  ![](https://i.imgur.com/tEGB3lI.png)
- 마지막 h3가 나오고 나면 h3는 전체 sequence에 대한 정보를 담음
	- **이진 분류를 하고싶다면**
		- W(hy) 라는 것을 붙임 -> sigmoid function를 씀
	- **회귀 분석을 하고 싶다면**
		- W (hy) 라는 것을 붙임


input : 파란색
모델 내부 파라미터 : 빨간색
output : 초록색

- - - 
#### Many to Many
![](https://i.imgur.com/Hwi09gj.png)
- 하나의 input이 들어올때마다 하나의 output이 나가는 형태