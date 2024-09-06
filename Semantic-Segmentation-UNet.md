# U-Net
**Deconvolution Network와 거의 똑같음**
![](https://i.imgur.com/mETp3jY.png)
#### Encoder부분
- Convolution을 해서 사이즈를 줄여나가는 과정
- 제일 처음 input 채널이 하나임 (흑백 사진)
- convolution을 할 수록 이미지 크기가 줄어들고 있음 (padding을 안해줬기 때문)
- max pooling을 해주고 채널의 개수는 두 배로 늘려감
- 이 작업을 여러 번 반복
=> 이 과정은 extracting spatial patterns **(부분적인 패턴을 추출하는 과정)**

#### Decoder부분
- up-conv (deconvolution)을 해서 이미지 크기가 두 배로 늘어남
- 채널의 개수를 반으로 줄여나감

전에 봤던 deconvolution과는 달리
**맨 처음 input과 output의 크기를 비교해보면 크기가 다름 (572 x 572 / 388 x 388)**
(문제가 있지 않나... ?)
- 사실 실제로 원하던 output의 크기는 388 x 388이었음
- 고려해서 가장자리는 다 padding으로 함

아무래도 크기를 줄였다가 늘리면 정보 손실이 있을 수 밖에 없음
그래서 **encoder에서의 정보의 일부를 decoder에서 가져오도록**
(taking advantage of multi-resolution encodings at decoding stage)
![500](https://i.imgur.com/KdRo6Vb.png)

가장자리는 다 padding으로 한다고 했는데, padding을 그냥 검은 색으로 하면 정확도가 떨어진다고 생각하여 다른 방법을 생각해냄
==(아이패드로 그려서 나중에 수정)==
![](https://i.imgur.com/VKjqFr6.png)
- **Mirror extrapolation** : 
	- 손실되는 입력 데이터('파란색 구간에서 노란색 구간을 제외한 부분' 중 '실제 데이터가 없이 빈 부분')은 mirroring 한 데이터로 extrapolate(외삽)한다.
	- padding 하는 공간을 원본 부분에 반사시켜서 적용
- **Overlap-tile strategy** : 
	- 노란색 부분을 segmentation 하기 위해서 파란색 부분이 필요함 (작은 영역을 예측하기 위해서 큰 영역을 학습해야 함)
	- 각 패치의 경계 부분에서 성능이 저하될 수 있는데 이 문제를 해결하기 위해 겹치도록 설정하여 주변 정보를 충분히 포함하도록 하는 전략


### U-Net의 Loss Function
- pixel wise softmax over final feature map + cross entropy loss
- **Weighted cross-entropy**
	- 기존 cross-entropy에 weighted map을 결합함
	  ![](https://i.imgur.com/XgVb3R8.png)
	- border pixel들은 상대적으로 드물다.
	- 가중치를 가장자리에 있는 pixel에게 높게 주기 위함 (가장자리를 정확하게 예측하는 것이 segmentation에서 가장 중요하기 때문)
	![400](https://i.imgur.com/4TUnR6Y.png)

