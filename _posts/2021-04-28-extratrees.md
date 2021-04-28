---
title: "[머신러닝 톺아보기] Extra Trees vs Random Forest"
date: 2021-04-28 12:00:00 -0400
categories: MachineLearning
---

끙정입니다.

 

일전에 데이콘에서 주최하는 '시스템 품질 변화로 인한 사용자 불편 예지' 경진대회를 참가했을 때, 팀원이 pycaret을 사용해 모델의 성능들을 비교한 적이 있습니다. 생전 처음 보는 모델들도 종종 있었는데, 그중에서도 도대체 이건 뭘까 하는 모델이 있었죠. 바로 Extra Trees입니다.

 

Extra Trees를 제대로 이해하고 싶어서 인터넷도 뒤지고, 책도 뒤졌지만 제대로 설명해주는 곳이 많지 않습니다. 특히나 한글로 된 블로그는 그 차이를 제대로 설명하고 있는 곳이 없습니다. 핸즈온 머신러닝에서도 많은 지면을 할애하지 않아 이해가 잘 가지 않았습니다. 다행히도 해외 유튜버와 해외 블로그를 뒤져서 조금은 이해를 했습니다.

 

엑스트라 트리는 랜덤 포레스트의 친구라고 보면 될 정도로 닮아 있습니다.

다만 일부 다른 점이 존재하는데요.

그 특징을 이름이 설명하고 있습니다. Extra. 조금 더 극단적으로 Random. 하게 만든 모델입니다.

 

Extra Trees의 장점은 Random Forest 보다 연산이 더 빠르고(약 3배),

bias와 variance를 낮출 수 있다고 주장합니다. (최곤데?!)

그 이유는 아래와 같은 차이점에서 기인한다고 합니다.

 

1) 부트스트랩핑의 유무
 Random Forest는 Bagging의 기법 중 하나로, 그 어원에서도 볼 수 있듯이 Bootstraping을 기반으로 Weak Tree를 생성합니다. 그렇기 때문에 각 Weak Tree가 다른 분포의 데이터를 학습하고, 그것이 Aggregating 되었을 때 Ensemble 효과를 내는 것입니다.

 

 반면에 Extra Trees는 Bootstraping을 하지 않고, Whole Origin Data를 그대로 가져다 씁니다. 그렇기 때문에 Extra Trees는 Random Forest의 친구이기는 하지만 Bagging이라고는 할 수 없습니다. (일부 영상에서는 비복원추출로 설명하기도 하는데, 같은 말입니다. 비복원추출을 하면 결국 Whole Origin Data를 쓰는 것입니다.)

 

2) Split 선택 기준
 두 친구의 또 다른 차이점은 바로 Split에 쓸 Feature에 대한 선택 기준이 다르다는 것입니다. 우리는 Random Forest의 각각의 Weak Tree가 Node를 분할해가는 과정을 알고 있습니다. (wyatt37.tistory.com/7) Random Forest는 주어진 샘플에 대해서 모든 Feature에 대한 Loss를 계산하고 가장 낮은 Loss를 가지는, 즉 설명력이 제일 높은 Feature의 Partition을 Split Node로 선택합니다. 그러려면 주어진 모든 Feature에 대한 Partition을 나누기 위해 Loss를 계산하고, 그것들을 전부 비교해서 가장 최선의 Split Node(feature)를 찾아야 합니다. 성능이 좋은 결정 트리를 만들 수 있지만, 연산량이 많이 들겠죠?

 

 반면에 Extra Trees는 Split을 할 때 무작위로 Feature를 선정합니다. Random Forest가 모든 Feature에 대한 Partition을 계산하여 비교해서 최선의 Split Node(Feature)를 선택한 반면, Extra Trees는 모든 Feature 중에서 일단 아무거나 고른 다음에, 그 Feature에 대해서만 최적의 Partition을 찾아 Node를 분할합니다. 언뜻 보면 이게 성능이 제대로 나올까 싶기도 하고 학습이 제대로 될지 모르겠지만 Weak Tree들이 다 모였을 때 꽤나 높은 성능을 내게 됩니다. 속도가 훨씬 빠른 건 덤이죠.

 


 

 3) 그럼 무엇이 더 나은 알고리즘이죠?

 일반적으로 Bootstraping을 쓰지 않고 Whole Origin Data를 그대로 쓰는 Extra Trees가 Random Forest에 비해 Bias를 낮출 수 있다고 말합니다. 또한 Split Point를 Randomly 하게 선택하면서 Variance를 줄일 수 있다고 이야기합니다. 나아가 위에서 말했듯이 연산 속도를 약 1/3로 줄일 수 있다고 합니다.

 

 

 그러나 언제나 최신 모델이 최고의 모델은 아닌 것도 같습니다. 부스팅에서도 여전히 LGBM이 최강자인 것처럼...

 

아직도 이해가 잘 안 가시는 분은 아래 글을 읽어보시기 바랍니다. 찾아본 글 중 가장 잘 설명되어 있는 글이며, 아래 글을 참고했습니다.

quantdare.com/what-is-the-difference-between-extra-trees-and-random-forest/



출처: https://wyatt37.tistory.com/6 [끙정의 쪽집게 데이터 사이언스 블로그]
