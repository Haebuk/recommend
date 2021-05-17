# 추천시스템



- 파레토의 법칙

  > 상위 20%가 80%의 가치를 창출한다.

- 롱테일의 법칙 - 인터넷의 발전

  > 하위 80%가 상위 20%의 가치보다 크다.

## Contents

- [추천 시스템 이해](# 추천-시스템-이해)

  - [연관분석](# 연관분석(Association-Analysis))
  - [Apriori](# Apriori-알고리즘)
  - [FP-Growth](# FP-Growth-알고리즘)

- [컨텐츠 기반 추천](# 컨텐츠-기반-추천)

  - [유사도 함수](# 유사도-함수)
  - [TF-IDF](# TF-IDF)

- [평가함수](# 평가함수)

  - [Accuracy](# Accuracy)

  - [MAP](# MAP)
  - [NCDG](# NDCG)

  

## 추천 시스템 이해

### 연관분석(Association Analysis)

- 상품과 상품사이에 어떤 연관이 있는지 찾아내는 알고리즘

  - 얼마나(frequency) 같이  구매하는가
  - A를 구매하면 B를 구매하는가

  같은 규칙을 찾아내는 형태. 장바구니 분석이라고도 함

#### 연관분석 - 규칙평가지표

- Support(지지도)

  ![image](https://user-images.githubusercontent.com/68543150/115555788-e3a1a280-a2ea-11eb-8754-90b79ad58db6.png)

  - 빈발 아이템 집합을 판별
  - 사건 A가 일어날 확률

  

- Confidence(신뢰도)

  ![image](https://user-images.githubusercontent.com/68543150/115555911-0633bb80-a2eb-11eb-945a-4ea5b53b78e2.png)

  - 아이템 집합 간의 연관성 강도를 측정
  - A가 주어졌을 때 B가 일어날 조건부확률
  - 값이 클 수록 그만큼 A와 B가 연관성이 강함

  

- Lift(향상도)

  ![image](https://user-images.githubusercontent.com/68543150/115555868-f87e3600-a2ea-11eb-9fcb-3c58f3b91c4c.png)

  - 생성된 규칙이 실제 효용가치가 있는지 판별
  - 두 사건이 독립인가(분모) vs 두 사건이 동시에 얼마나 발생하는가(분자)의 비율
  - 값이 1이면 A와 B는 서로 독립, 값이 2라면 두 사건이 독립임을 가정했을 대비 2배의 긍정적인 연관성

#### 연관분석 - 규칙 생성

- 가능한 모든 경우의 수를 탐색 -> 지지도, 신뢰도, 향상도가 높은 규칙 찾기

![image](https://user-images.githubusercontent.com/68543150/115557489-b6ee8a80-a2ec-11eb-9050-663d49de7a81.png)

- 위처럼 상품이 4개일 때, 전체 경우의 수는 4C1 + 4C2 + 4C3 + 4C4 = 15

#### 연관분석 - 문제점

![image](https://user-images.githubusercontent.com/68543150/115557722-f61cdb80-a2ec-11eb-993f-93ad5b05acda.png)

- 아이템의 증가에 따른 규칙의 수의 증가가 기하급수적으로 증가

---

### Apriori 알고리즘

#### Apriori

- 최소 지지도 혹은 신뢰도 이상의 아이템만 선택
  1. k 개의 item을 가지고 단일항목집단 생성 (one-item frequent set)
  2. 단일항목집단에서 최소 지지도(support) 이상의 항목만 선택
  3. 2에서 선택된 항목만을 대상으로 2개항목집단 생성
  4. 2개항목집단에서 최소 지지도 혹은 신뢰도 이상의 항목만 선택
  5. 위의 과정을 k개의 k-item frequent set을 생성할 때까지 반복

- 예) 최소 지지도 0.5인 경우

![image](https://user-images.githubusercontent.com/68543150/118401045-172fdc80-b69f-11eb-9c49-0766060e0bb8.png)

![image](https://user-images.githubusercontent.com/68543150/118401075-33337e00-b69f-11eb-99ac-e2d3c41d8e1c.png)

#### Apriori - 장점 

- 원리가 간단하여 사용자가 쉽게 이해할 수 있고 의미를 파악할 수 있음
- 유의한 연관성을 갖는 구매패턴을 찾아줌

#### Apriori - 단점

- 데이터가 클 경우(item이 많은 경우) 속도가 느림
- 실제 사용시 많은 연관상품들이 나타나는 단점

---

### FP-Growth 알고리즘

#### FP-Growth

- Apriori 속도 개선 알고리즘
  1. 모든 거래 확인 -> 각 아이템마다 지지도 계산 -> 최소 지지도 이상 아이템 선택
  2. 모든 거래에서 빈도가 높은 아이템 순서대로 정렬
  3. 부모 노드를 중심으로 거래를 자식노드에 추가해주면서 tree 생성
  4. 새로운 아이템이 나올 경우 부모노드에서 시작, 아닌 경우 기존 노드에서 확장
  5. 위 과정을 모든 거래에 대해 반복하여 FP tree 만들고 최소 지지도 이상의 패턴 추출

#### FP-Growth - 장점

- Apriori 보다 빠르고 2번의 탐색만 필요로 함
- 후보 아이템셋을 생성할 필요 없이 진행 가능

#### FP-Growth - 단점

- 대용량 데이터셋에서 메모리를 효율적으로 사용하지 않음
- Apriori 알고리즘에 비해 설계 어려움
- 지지도 계산이 FP-Tree 만들어진 후에야 가능

---

## 컨텐츠 기반 추천

### 컨텐츠 기반 모델

- 사용자가 이전에 구매한 상품중 좋아하는 상품과 유사한 상품을 추천
- item을 벡터 형태로 표현(도메인에 따라 다른 표현 방법 적용)

### 유사도 함수

#### 유클리디안 유사도

![image](https://user-images.githubusercontent.com/68543150/118401680-88708f00-b6a1-11eb-8176-04f531d335f4.png)

- 유클리디안 유사도 = 1 / (유클리디안 거리 + 1e-05)
- 장점
  - 계산하기가 쉬움
- 단점
  - p와 q의 분포가 다르거나 범위가 다른 경우 상관성을 놓침

---

#### 코사인 유사도

![image](https://user-images.githubusercontent.com/68543150/118401728-bfdf3b80-b6a1-11eb-95e4-0711551d4f38.png)

- 장점
  - 벡터의 크기가 중요하지 않은 경우에 사용(예: 빈도 수,(얼마나 나왔는지 비율))
- 단점
  - 벡터의 크기가 중요한 경우에 대해 잘 작동하지 않음

---

#### 유클리디안 유사도 vs 코사인 유사도

![image](https://user-images.githubusercontent.com/68543150/118401828-28c6b380-b6a2-11eb-9217-1c9ff8d61cd6.png)

---

#### 피어슨 유사도

![image](https://user-images.githubusercontent.com/68543150/118401850-3f6d0a80-b6a2-11eb-91f1-a2a38be1ecf4.png)

---

#### 자카드 유사도

![image](https://user-images.githubusercontent.com/68543150/118401857-4c89f980-b6a2-11eb-8e81-b3557bf658e3.png)

---

### TF-IDF

- 특정 문서 내 특정 단어의 빈도(TF)와 전체 문서에서 특정 단어의 빈도(DF)를 통해 특정 문서에서만 자주 등장하는 단어를 찾아 문서 내 단어의 가중치를 계산하는 방법
- 문서의 핵심어 추출, 문서간 유사도 계산, 검색 결과의 중요도를 정하는 작업등에 활용
- TF(d, t): 특정 문서 d에서 특정 단어 t의 등장 횟수
- DF(t): 특정 단어 t가 등장한 문서의 수
- IDF(d, t): DF(t)에 반비례하는 수
- ![image](https://user-images.githubusercontent.com/68543150/118434675-6a924100-b718-11eb-86bf-14cc779f4638.png)
- TF(d, t) * IDF(d, t) = TF-IDF(d, t)

#### TF-IDF - 장점

-  직관적인 해석 가능

#### TF-IDF - 단점

- 대규모 말뭉치를 다룰 때 메모리상의 문제 발생
  - 높은 차원
  - very sparse data

---

### Word2Vec

- 비슷한 위치에 등장하는 단어들은 비슷한 의미를 가진다라는 가정을 통해 학습 진행
- sparse matrix가 가지는 단점을 해소하고자 저차원의 공간에 백터로 매핑

#### Word2Vec - CBOW

- 주변의 단어들을 가지고 중간의 단어를 예측

#### Word2Vec - Skip-gram

- 중간에 있는 단어로 주변 단어 예측

---

### 컨텐츠 기반 모델 - 장점

- 자신의 평점만을 가지고 추천시스템을 만들 수 있음
- 아이템의 피쳐를 통해 추천 -> 추천한 이유를 설명하기 쉬움
- 사용자가 평점을 매기지 않은 새로운 아이템에도 추천이 가능

#### 컨텐츠 기반 모델 - 단점

- 아이템의 피쳐를 추출하고 이를 통해 추천 -> 피쳐를 제대로 추출하지 못하면 정확도 낮음 - 도메인 지식 필요
- 기존의 아이템과 유사한 아이템 위주로 추천 -> 새로운 장르 추천 어려움
- 새로운 사용자에 대해 충분한 평점이 쌓이기 전까지 추천 힘듦

## 협업 필터링

- 사용자의 구매 패턴이나 평점을 가지고 다른 사람들의 구매 패턴, 평점을 통해 추천하는 방법
- 장점
  - 도메인 지식 필요 없음
  - 사용자의 새로운 interests 발견하기 좋음
  - 시작단계 모델로 선택하기 좋음(추가적인 문맥정보 필요 없음)
- 단점
  - 새로운 아이템에 대해 다루기 힘듦
  - 사이드 피쳐(개인정보, 아이템 추가정보) 포함시키기 어려움

### Neighborhood based method

- 협업 필터링을 위해 개발된 초기 알고리즘
- User-based collaborative filtering
  - 사용자의 구매 패턴(평점)과 유사한 사용자를 찾아 추천 리스트 생성
- Item-based collaborative filtering
  - 특정 사용자가 준 점수간의 유사한 상품을 찾아 추천 리스트 생성
- ![image](https://user-images.githubusercontent.com/68543150/118435419-e771ea80-b719-11eb-83c0-e2633f053f4c.png)



#### KNN(K Nearest Neighbours)

- 가장 근접한 K명의 neighbors를 통해 예측
- ![image](https://user-images.githubusercontent.com/68543150/118435536-1ee09700-b71a-11eb-9eac-c37cdeec1f83.png)

#### Neighborhood based method - 장점

- 간단하고 직관적 -> 구현 및 디버그 쉬움
- 특정 아이템을 추천하는 이유를 정당화하기 쉬움
- 추천 리스트에 새로운 아이템과 유저가 추가되어도 상대적 안정성

#### Neighborhood based method - 단점

- 유저 기반 방법의 시간, 속도, 메모리가 많이 필요
- 희소성 때문에 제한된 범위가 있음
  - 유저의 Top K 만 관심있음
  - 유저의 비슷한 이웃 중 아무도 해리포터를 평가하지 않으면, 이 유저의 해리포터에 대한 등급 예측 할 수 없음

### Latent Factor Collaborative Filtering

- Rating Matrix에서 빈 공간을 채우기 위해 사용자와 상품을 잘 표한하는 차원(Latent Factor)을 찾는 방법
- ![image](https://user-images.githubusercontent.com/68543150/118435971-e55c5b80-b71a-11eb-809f-d92cfea56499.png)
- 사용자의 잠재요인과 아이템의 잠재요인을 내적해서 평점 매트릭스 계산 

#### SVD

- 고유값 분해와 같은 행렬을 대각화 하는 방법
- 한계
  - 데이터에 결측치가 없어야 함
  - 대부분의 현실 데이터는 sparse

#### SGD

- Gradient Descent을 통한 Latent Factor 찾는 방법
- 장점
  - 매우 유연한 모델로 다른 Loss function 사용 가능
  - parallelized 가능
- 단점
  - 수렴 속도 느림

#### ALS

- 두 행렬(User Latent, Item Latent) 중 하나를 고정하고 다른 하나의 행렬을 순차적으로 반복하며 최적화하는 방법
- 수렴된 행렬을 찾을 수 있는 장점
- 장점
  - SGD보다 수렴속도 빠름
  - parallelized 가능
- 단점
  - 오직 square loss만 사용 가능

## 평가함수

### 평가함수를 다양하게 알아야 하는 이유

- 도메인이나 목적에 따라 다른 평가 함수를 도입해서 얼마나 잘 추천이 되는지 평가하는게 중요하기 때문
  - 내가 추천해준 영화를 고객이 봤는가
  - 내가 추천해준 영화를 고객이 높은 점수로 평점을 줬는가
- 1번의 경우 추천은 성공했지만 만족도는 낮을 수 있음
- 2번의 경우 고객의 만족도까지 고려해서 평가를 한 것
- 체류시간을 파악하는 것도 중요할 때가 있음

### Accuracy

![image](https://user-images.githubusercontent.com/68543150/118085102-c5682780-b3fc-11eb-9b41-6cd7064e9309.png)

- 내가 추천해준 영화를 봤는가 vs 보지 않았는가
- 내가 추천해준 영화를 많이 볼 수록, 추천하지 않은 영화를 보지 않을수록 정확도 상승
- But 추천하지 않은 영화의 수는 추천한 영화의 수에 비해 굉장히 많고 편향된 결과를 야기할 수 있음

따라서 추천한 영화 중 본 영화로만 평가를 매겨야함

- 이 경우 모든 상품을 추천해주면 정확도가 무조건 1이 나오는 문제점이 있음
- 상위 n개의 상품을 추천했을 때 정확도가 얼마나 나오는지 판단하는게 제일 정확할 수 있음

### MAP

![image](https://user-images.githubusercontent.com/68543150/118085656-b59d1300-b3fd-11eb-8209-8d4fd4a6abd0.png)

![image](https://user-images.githubusercontent.com/68543150/118085696-c483c580-b3fd-11eb-8548-2534f8252a76.png)

- Recommendations: 추천을 했을때 맞은 경우 1, 틀린 경우 0
- AP: Precision @k's를 평균낸 값(추천한 K개의 영화의 Precision을 평균)
  - 추천한 순서에 따라 가중치가 붙음
  - 1번째 케이스는 3번째 추천한 영화를 봤으므로 Precision = [0,0,1/3]
- MAP@4: 4명의 사용자의 AP를 평균낸 값(Precision을 평균낸 AP를 4명의 사용자에 대해 평균)
  - 이 경우 (1/4)*(0.11+0.38+0.89+1)=0.595
  - 추천의 순서에 따라 값이 차이가 나게 된다.
  - 상위 k개의 추천에 대해서만 평가하기에 k를 바꿔가며 상위 몇 개를 추천하는게 좋은지도 결정할 수 있음

### NDCG

NDCG(Normalized Discounted Cumulative Gain): ranking quality measure, 검색 알고리즘에서 성과를 측정하는 평가 메트릭

- 추천엔진은 유저와 연관있는 문서의 집합을 추천해주기 때문에, 단순히 문서 검색 작업을 수행한다고 생각할 수 있음

#### CG(Cumulative Gain)

![image](https://user-images.githubusercontent.com/68543150/118086727-5dffa700-b3ff-11eb-8ae6-3d87ba69e71e.png)

- relevance (scores): 유저에게 중요도 순으로 추천했을 때 유저의 피드백을 나타냄
- 여기서는 3>2>1 순으로 유저의 긍정적인 피드백을 나타냄 (3이 very positive) 

![image](https://user-images.githubusercontent.com/68543150/118087016-d6666800-b3ff-11eb-9a52-9cc509b295ae.png)

- CG_A = 2 + 3 + 3 + 1 + 2 = 11
- CG_B = 3 + 3 + 2+ 2 + 1 = 11
  - B가 A보다 나은 추천 결과이지만 CG의 관점에서는 동일함.
  - 이러한 점을 보완하기 위해 문서의 위치에 따른 점수를 반영할 필요가 있음

#### DCG

![image](https://user-images.githubusercontent.com/68543150/118087279-470d8480-b400-11eb-8986-8c21b87134d3.png)

- CG를 보완: 앞에 있을 수록 분모값이 작음

![image](https://user-images.githubusercontent.com/68543150/118087339-5e4c7200-b400-11eb-8cdf-7562c4f0f524.png)

- DCG는 좋은 척도로 보이지만 완벽하지 않음
  - 다양한 요인에 따라 권장 사항 수가 사용자마다 다를 수 있음
  - DCG는 권장 사항의 수에 따라 결과가 달라지기 때문

#### NDCG

- 사람마다 추천해주는 개수가 다를 수 있음(DCG가 변하는 문제점) -> 보완
- DCG에 정규화를 수행

![image](https://user-images.githubusercontent.com/68543150/118087720-f9dde280-b400-11eb-9b1e-bc2b38f1ecf4.png)

![image](https://user-images.githubusercontent.com/68543150/118087736-006c5a00-b401-11eb-83dd-a77056fc8bac.png)

![image](https://user-images.githubusercontent.com/68543150/118087785-111cd000-b401-11eb-9842-e1f3b9170e79.png)

![image](https://user-images.githubusercontent.com/68543150/118087818-19750b00-b401-11eb-9f11-6385ff0b4fda.png)

