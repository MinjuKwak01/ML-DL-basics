# Learning Rate Scheduling
Learning Rate
- 모델의 가중치를 업데이트할 때, 한번의 경사 하강법 단계에서 이동하는 크기를 결정함

지금 있는 곳에서 어디 방향으로 가야할지 → 미분으로 결정
얼마큼 갈 것인지 → Learning rate로 결정
**training data를 처음부터 끝까지 한 번 보는 것 → 1 epoch**
똑같은 데이터를 다른 순서로 다시 봄 → 다른 epoch
![400](https://i.imgur.com/PLmWFlt.png)
어떻게 하는지?
- 큼직큼직하게 여러 개 해본다. ex) 10^8, 10^4, 1, 10^-4 10^-8….
- 레이어 하나 바뀌어도 learning rate가 계속 바뀐다.

**Learning Rate Decay**
![400](https://i.imgur.com/AyT7NmX.png)
- 처음에 큰 learning rate를 쓰고 점점 learning rate를 줄여나가는 방식으로 하는 것

![](https://i.imgur.com/9gRDu5f.png)


- Learning rate을 10^-4를 쓰고, 학습이 절반 진행되었을때 1/10으로 줄이고, 75% 진행되었을때 또 1/10으로 줄인다.
- 1/10으로 줄였을때의 기점으로 Train loss가 많이 줄어든 걸 볼 수 있다.