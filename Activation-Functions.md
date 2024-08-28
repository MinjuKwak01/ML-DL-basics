# Activation Function
역할 : 비선형성 도입, 출력 범위 제어
- #### sigmoid function
  ![300](https://i.imgur.com/vKFQNAk.png)

	- score을 0~1로 변환할 때 자주 사용된다. (Logistic Regression)
	- 확률을 표현할 때 많이 사용된다.
	- **사용하지 말아야할 이유 (사용을 한다면, 맨 마지막에 확률적으로 해석하기 위해 씀)**
    - **saturated neurons는 gradient 를 죽인다.**
        - 미분 : 접선에서의 기울기
        - x = 10일때 gradient는 거의 0에 가까워진다.
        - x = -10일때 gradient도 거의 0에 가까워진다.
        - 미분했을때의 그래프를 보면 의미있는 기울기는 0에 가까운 작은 숫자들일때만 가능하다.
        - 만약에 x값이 너무 크거나, 너무 작아서 미분값이 0에 가깝다면, back propagation을 할때 upstream gradient는 계~속 0을 받게된다. (학습이 안된다는 뜻)
            - back propagation 하는 이유? w를 다시 업데이트 시켜주기 위함
    - **sigmoid outputs은 not zero centered (center이 0.5이다.)**
	    - 출력 값이 항상 양수라는 뜻
        - 들어오는 input 자체가 다 양수값이라고 가정해보자 (실제로 거의 모든 데이터들이 양수인 경우가 많음)
        - backpropagation을 하게 되면, 뒤쪽에서 받은 gradient의 부호가 +이면 +로 나가고, -면 -로 나간다. (가중치 Layer들의 업데이트 값들이 모두 동일한 부호, 즉 모두 + 이거나 모두 -가 되게됨)
        - 따라서 비효율적으로 gradient update가 이루어진다. (지그재그 현상)
        ![200](https://i.imgur.com/8pOHwt4.png)
		- https://nittaku.tistory.com/267 참고
    - **exp() 는 계산하는 데에 시간이 오래 걸린다.**
- #### tanh
	- output range가 -1~1 이므로 지그재그 현상이 덜 하다.
	- vanishing gradient 문제가 아직도 있음
- #### ReLU
  ![200](https://i.imgur.com/g78Vvem.png)
	- ReLU(x)=max(0,x)
	- 음수가 아닌 값에 대해 기울기가 1로 일정하게 유지된다. -> 역전파 과정에서 경사 소실 문제를 완화해준다.
	- 양수가 들어오면 그 값 그대로 나가고 음수가 들어오면 0이 나간다.
	- +영역에서는 gradient saturate되지 않는다.
	- 계산이 빠르고 효율적이다.
	- loss 수렴 속도가 빠르다.
	- 기울기가 0으로 수렴하는 것이 없다.
	- 사용하지 말아야 할 이유가 있다면 그 이유는?
	    - output이 zero-centered가 아니다.
	    - x=0에서 미분이 되지 않는다.
	    - 처음에 우연히 input이 -면 기울기가 0이고 그건 아예 업데이트가 되지 않는다. (가장 큰 문제)

- - -
#### 결론
- 기본적으로 activation function은 ReLU를 쓴다.
- ReLU, ELU를 적용해서 더 좋아지는지 확인해보는정도로 사용해도 좋다.
- **중간에 있는 노드에선 절대 sigmoid, tanh를 쓰지 않는다.**

