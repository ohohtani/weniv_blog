# LG Aimers 6기 온라인 해커톤 수료 후기

LG Aimers 6기에 참여하였고, 

![alt text](img/에이머스/에이머스_최고순위.png)

public 최고 순위 33위까지 달성하며 100명 안에 들 줄 알았으나 (100명까지만 오프라인 본선 진출)
아쉽게도 최종 private 순위는 98위까지 내려가며 첫 Aimers 수료가 이렇게 끝이 났다.

그래도 첫 참여에 엄청 낮은 순위라고 생각하지는 않기 때문에, 어떤 방식으로 점수를 올렸었는지, 어떤 것이 효과가 있고 없었는지 공유하며
최종적으로 최상위권 분들과의 코드를 비교해보며 어떤 식으로 하는 게 비슷한 종류의 대회에서 큰 효과를 볼 수 있는지 공부하고자 한다.

먼저 대회 개요는 다음과 같다.

![alt text](img/에이머스/image.png)

평가산식은 roc-auc 를 사용하기 때문에 0 혹은 1로 나타내는 것이 아닌 예상 확률값 그대로를 제출하고 점수를 받는다.

#### 1. 베이스라인 구축 & 기본 전처리

전처리도 중요하지만 일단 적절한 방식으로 실행 되어 점수가 제대로 나오는 지 확인할 수 베이스라인 코드를 먼저 만들었다.
물론 데이콘 측에서 베이스라인 코드를 제공해주긴 하지만 점수를 기대할 수 있는 코드는 아니기에 참고용으로만 사용하기에 좋다.
아무 것도 하지 않고 데이콘에서 제공하는 베이스라인 코드를 실행하여 제출하면 0.689 점이 나온다 (1점 만점)

점수가 적절히 나오는 베이스라인 코드를 만들어야 했고, 대회 초반 당시 최상위권 팀들의 점수가 0.741~2 정도였기에
0.74정도가 나오는 베이스라인 코드를 만들면 되겠다고 생각했다. 

일단 우리 팀의 경우 팀원이 5명이라서, 초반에 전처리와 베이스라인 코드 구축을 나눠서했는데,
베이스라인 코드의 경우 optuna 파라미터 튜닝 없이 XGBoost 기본 파라미터를 사용하였고, 전처리의 경우 컬럼 삭제 없어
범주형 데이터를 모두 수치형으로 변환하여 수치형 컬럼만 남긴 채로 진행하였다. -> 점수 : 0.7395

optuna 튜닝만 진행하면 0.74라인은 넘길 것 같았고, 역시나 튜닝 진행후 제출하니 0.74092가 나오며 나쁘지 않은 출발을 했다.
하지만 이 점수대는 어느 팀이든 만들 수 있었기에 매일매일 점수를 조금이라도 더 올리기 위한 노력을 해야만 했다.


#### 2. 모델 변경 & K-Fold 교차 검증 사용

데이터 분석 & 예측 관련 대회가 처음이다 보니, 모델의 종류는 어떤 것이 있는지부터 제대로 찾아보았다.
다양한 모델이 존재했지만, 성능과 사용 빈도 면에서 현재 가장 많이 사용하는 모델은 Catboost, XGBoost, LightGBM 이 세 가지 인 것 같았다.
각 모델의 대한 논문 리뷰는 블로그 다른 페이지에 작성 예정.

추가적으로, 5기에 참여했던 지인 분께서 범주형이 있는 경우 무조건 Catboost를 쓰라는 조언을 해주셨다. 범주형 데이터가 꽤나 많이 존재했던 데이터셋이었기에
지인의 말을 듣고 catboost로 변경하게 되었다. 모델만 변경하였을 때의 점수는 XGBoost를 사용할 때와 거의 동일했다. 다만 범주형 데이터를 따로 인코딩 하지 않아도 된다는 편리함 때문에
Catboost를 계속해서 사용하기로 했다.

그리고 가장 점수 변화 폭이 컸던 'K-Fold 교차 검증 사용'이다. 이 방식의 경우 0.74092였던 점수를 한 방에 0.74155까지 올려주었는데 순식간에 등수가 엄청나게 올라갔었다.
K-Fold 교차검증의 경우 아래의 사진처럼 Optuna 튜닝 코드 내에 들어가게 된다.

![alt text](img/에이머스/image1.png)

아래의 세 가지 방식을 모두 사용해보았다.

1. Optuna 내에만 k-fold 교차 검증을 넣기
2. optuna 밖의 학습 구간에서 k-fold 교차 검증을 넣기
3. 둘다 넣기

결과는 1번일 때가 가장 좋은 점수가 나왔다. 하지만 이유를 정확히 모르겠어서 상위권분들의 코드 공유를 보고나면 이해가 될 것 같다.


#### 3. 전처리 심화 과정

