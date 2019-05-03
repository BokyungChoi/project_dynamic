##### Meeting log & Timeline
---

### 프로젝트 주제 및 데이터 선정. 
#### Overview
- 일시: 4/30 20:00~21:00  
- 작성자: 오동건  
- 참석자: 이현아, 김종찬, 최보경, 류승우  

#### Contents and Decisions
1. 주제: 사용자 온라인 행동 데이터를 활용한 다이나믹 프라이싱.  
- 다이나믹 프라이싱이란?  
: 개별 고객의 특성 및 상황에 따라 **실시간**으로 **개인맞춤화** 가격을 도출.  

  + [정의 출처](http://blog.naver.com/PostView.nhn?blogId=mosfnet&logNo=221320806418)

2. 사용 데이터 
- 기업 사용자 로그 데이터 및 검색 데이터

3. 타임라인
- 5/14 (화) – 연사 강연.    
- 5/21 (화) - 데이터 전처리 마무리, 모델링 시작.  
- 6/4 (화) - 1차 완료 예정일.  

#### Forward plans
- 데이터 구조 파악  
- 분석 범위 결정  
- 협업 방식 고민  

---
### 데이터구조 파악 및 분석범위 결정.
#### Overview
#### pre-shared
동건: 
1. Dynamic pricing 대상 선정 방법.  
다양한 조건(성별,나이,상품군,세션,검색)에 따라 특정 상품을 구매할 가중치를 예측한 뒤, 이를 바탕으로 프라이싱 전략 마련
ex) 0~1사이의 값을 갖는 가중치를 만든다면, 특정 기준 근처의 조건의 사람들을 dynamic pricing 대상으로! -> 특정 기준은 가중치의 분포를 보고 사후적으로 결정 (대략 0.5사이의 값으로?)

2. 분석범위
소분류로 분석하면, 데이터가 너무 sparse해지므로 상품군은 중분류까지만 사용했으면 좋겠다. 단 고객범위는 최대한 다양하게 하는 것이 개인별 가격을 책정하는 데 가치 있을 것 같다.  

#### Contents and Decisions
1. 데이터 구조
2. 분석 범위
3. 협업 방식
#### Forward plans
