초기에 데이터들을 zero-centering, normalize해주는 것이 좋다.
처음 input을 normalization 해주면, sigmoid function에서 gradient kill(기울기 소실), gradient saturation(기울기 포화 : 제대로 전달되지 않음)문제가 안 일어나는 거 아닌가?
→ 맞다. 하지만 두 번째 레이어도 normalize 되어있을지 보장할 수 없다. 현대 100개 이상의 layer을 쌓기 때문에.. 

#### Data Augmentation (데이터 증강)
우리가 갖고 있는 데이터들은 그저 현실의 tiny subsamples이다.
horizontal flip, random crop, scaling.. 등등

### Weight Initialization
**Small Gaussian Random**
![](https://i.imgur.com/3oayvZe.png)

- 0.01으로 작은 값을 곱했다. → 그러면 안됨!!
- Activation function으로 tanh을 쓴다고 가정했을때,
- 첫 번째 Layer에서 output 값이 0~1 사이로 고르게 분포해 있다.
- 하지만, Layer을 더 많이 쌓을수록 점점 더 홀쭉해진다. → 다 0만 나옴
    - 왜 그럴까?
    ![350](https://i.imgur.com/OUomlG7.png)
	- tanh를 미분했을때, x는 0에 가까운 값이 들어가면 점점 0으로 확정되는 값들의 비율이 점점 늘어난다.

**Large Gaussian Random**
![](https://i.imgur.com/pwDXpAd.png)
- 0.5를 곱했다.
- 1 아니면 -1이 나온다.
- 적당한 중간 값을 곱해야 한다.

**Xavier Initialization**
- input 크기에 루트를 씌운 것 만큼 나눠주면 된다.
- Layer가 깊어질수록 그래도 -1~1 분포가 고르게 나타난다.
- d_in은 Layer로 들어가는 input 파라미터의 개수
    - conv layer의 경우, F x F x C(필터 크기 x 필터 크기 x 채널 수)
- 어떻게 이게 성립하는지?

**Weight Initialization for ReLU**
![](https://i.imgur.com/N6UbQim.png)
- tanh를 쓸때와 마찬가지로 0으로 몰려드는 현상이 비슷하게 나타난다.![](https://i.imgur.com/st4dtVh.png)
- Kaiming/MSRA Initialization for ReLU 를 사용한다.
    - 음수였던 값들이 0으로 나타나고, 양수 값 들도 비슷한 분포를 갖는 것을 확인할 수 있다.