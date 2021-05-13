# 추천시스템

- 파레토의 법칙

  > 상위 20%가 80%의 가치를 창출한다.

- 롱테일의 법칙 - 인터넷의 발전

  > 하위 80%가 상위 20%의 가치보다 크다.

## 여러 추천 시스템

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

