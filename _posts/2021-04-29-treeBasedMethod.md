---
title: "[머신러닝 톺아보기] Extra Trees vs Random Forest"
date: 2021-04-28 12:00:00 -0400
categories: MachineLearning
---

끙정입니다.

 

오늘은 더 심연으로 돌아가서 Tree-Based Method에 대해서 탐구해보고자 합니다.

많은 사람들이 Random Forest를 쓰고, LGBM, XGB 등의 Boosting 알고리즘을 사용합니다.

도대체 어떤 원리로 Tree Based Model들이 작동하는지 기초를 다시 복습해보도록 합시다.

 

오늘 살펴볼 것은 세 가지입니다.

 

ⓐ 어떻게 Tree가 뻗어 나가는가?

ⓑ 어떤 기준으로 가지가 나눠지는가?

ⓒ 왜 그 가지가 선택되었는가?

 

자, 그럼 오늘도 탐험을 시작해보도록 하겠습니다.

 

 

ⓐ 나무는 어떻게 뻗어 나가는가?
 Tree Based Method의 핵심은 데이터를 어떠한 규칙에 의해 연속적으로 나눠가는 것이라고 이해하시면 됩니다. 예를 들어 야구 선수의 메이저리그 활동 기간(year)과 작년 타수(hits)를 통해서 연봉을 예측한다고 보면,

![다운로드](https://user-images.githubusercontent.com/52409420/116540739-85497500-a925-11eb-98ce-79735b685b53.png)


 위 그림과 같이 특정 변수(feature)의 특정 값(split point)을 기준으로 두 영역으로 나뉘는 Partition을 만든 뒤, 해당 split point를 기준으로 각각 두 개의 branch가 뻗어 나가게 됩니다. 각 branch는 하나의 internal node로 향하고, 각 internal node는 다시 특정 변수의 특정 값을 기준으로 partition을 만들어 split 하게 됩니다. 이러한 작업을 반복하여서 최종적으로 결괏값을 가지고 있는 terminal node, 즉 leaf에 다다르게 됩니다. 나무가 얼마나 복잡해지는지는 이 leaf의 수에 의해 결정됩니다.

 

 추가로 위 Tree에 대한 feature space를 아래와 같이 표상할 수 있습니다. 즉 나무는 겹쳐지지 않는 n개의 공간(partition)으로 계층적 분리를 실시한다고 보면 됩니다.
![다운로드 (1)](https://user-images.githubusercontent.com/52409420/116540755-88dcfc00-a925-11eb-867b-cf14bad6036c.png)

 

ⓑ 어떤 기준으로 가지가 나눠지는가?
 그렇다면 중요한 것은 나무가 어떻게 가지를 나누느냐에 대한 점입니다. 여기서 일단 Classification 문제를 다루느냐, Regression 문제를 다루느냐에 따라 Loss Function이 달라집니다. 분류 문제라면 Gini 혹은 Entropy를 사용할 것이고, 회귀 문제라면 RSS(Residual Sum of Square)를 사용할 것입니다.

 

 결국 분리된 각각의 공간에서 Loss Function에 의한 값을 minimize 할 수 있는 방향으로 split point를 잡을 것입니다. 위에서 예를 들었던 야구 선수 연봉 예측 문제를 적용하면 years가 4.5 인 point, hits가 117.5인 point가 가장 잘 분리한 split point일 것입니다.

 

ⓒ 왜 그 가지가 선택되었는가?
 그렇다면 이런 의문점이 생길 것입니다. 각 feature 별로 partition을 나누는 것은 알겠는데, 어떤 가지는 맨 위에 있고, 어떤 가지는 왜 맨 아래에 있는가? 즉 어떤 feature를 먼저 나누고, 어떤 feature를 나중에 나누는가? 이것은 알고리즘마다 조금은 다를 수 있지만, 기본적으로 Tree Based Method의 원형이라고 한다면, 모든 feature에 대한 값을 측정해서 그중 가장 영향력이 높은 feature를 먼저 사용합니다.

 

 즉 위에서 예를 들었던 야구 선수 연봉 예측 문제에서 years에 대한 node가 최상위에 있는 이유는 years를 4.5로 나눈 partition에 대한 RSS가, hits를 117.5로 나눈 partition에 의한 RSS보다 더 낮았기 때문입니다.
![다운로드 (2)](https://user-images.githubusercontent.com/52409420/116540763-8b3f5600-a925-11eb-940f-77214953315e.png)

 


 

 이러한 방식은 모든 feature의 partition을 계산해야 하는 단점이 존재하지만, 단일 Decision Tree를 만들 때는 높은 성능을 가진 모델을 만들 수 있습니다.

 

 

 

 

아래 블로그와 책을 참고했습니다.

quantdare.com/classification-trees-in-matlab/

