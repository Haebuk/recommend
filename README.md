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

  