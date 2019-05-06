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
- 일시: 5/4 17:00~17:40  
- 작성자: 오동건  
- 참석자: 이현아, 김종찬, 최보경, 류승우  

#### pre-shared

<details>
  <summary>Click to expand!</summary>
  
동건: 
1. Dynamic pricing 대상.  
조건(성별,나이,상품군,세션,검색)에 따라 상품을 구매할 가중치를 예측한 뒤, 다이나믹 프라이싱 전략 기획     
ex) 0~1사이의 값을 갖는 가중치를 만든다면, 0.5 근처의 사람들을 dynamic pricing 대상으로! 

2. 분석범위.  
소분류로 분석하면, 데이터가 너무 sparse해지므로 상품군은 중분류까지만 사용했으면 좋겠다. 단 고객범위는 최대한 다양하게

3. 데이터 구조 질문.  
Search1과 Search2의 search_cnt가 서로 다른 의미? 확인했는 데 수치가 서로 다르다.   
Session은 클라이언트의 전체 세션에 대한 정보는 아니고 구매행위가 이루어진 세션에 대한 정보만?  
Seseion dastaset에 포함된 Session의 기준이 무엇?

보경:

1. 분류: Master 데이터셋에 class2(원래 얘기한 중분류)로 분류하면 좋을 듯  
2. 최종 목표: 다이나믹 프라이싱을 위한 개인별 가중치 도출  

하위 지표: 
- 검색 - 많이 검색할수록(frequency)
- 상품 및 상세 페이지 - 많이 방문할수록(frequency)
세션 - 세션간 시간간격이 좁을수록(고려를 여러 번 한다), 세션의 길이가 짧을수록(구매 고민에 시간을 많이 쓰지 않는다)

3. 데이터 구조 질문
TOT_session_HR_V가 어느 단위의 시간이지
Session sequence가 무엇이지

승우:

1. 중요 지표
- Session.csv: TOT_PAG_VIEW_CT,  TOT_SESS_HR_V(애매, 온전히 쇼핑에만 쏟을 가능성?) / DVC_CTG_NM(애매, 기기 특성?)
- Product.csv: HITS_SEQ, PD_BUY_AM, PD_BUY_CT
- Custom.csv: CLNT_GENDER, CLNT_AGE

2. 분류
- 상품범위: CLAC2_NM

3. 의문점:
- 1. Pricing 방향성
   - 많이 산 사람들에게 많은 할인을 해 줄 것인가? / 고민을 많이 한 사람들(지표 ex. 클릭 수)들로 하여금 구매를 유도할 것인가?
   (:방향성3와 관련하여 모델링의 개괄적인 방향성은 확실히 하고 가고 싶다)
- 2. Session.csv의 SESS_SEQ가 정확히 무엇인가? 
   - 예를 들어, CLNT_ID 5874829 동일한 날짜 20180903 세션ID는 늦게 발급 받았음에도 SESS_SEQ가 앞서는 경우가 생김. 
- 3. 성별에 따라 모델링을 달리할 것인가? (Male: 101063, Female: 570616)
- 4. E-Commerce 연령대 선택은 어떻게? (30대가 가장 많고, 2,3,40대에 몰려 있음.)
- 5. 괴상한 행동 양태를 갖고 있는 소비자에 대한 고려? (Pricing에 혜택을 주나?)
   - ex. CLNT_ID 4736937 비슷비슷하고 가격은 똑같은 제품군에 대해 3154번 구매? 4월부터 9월까지? ???

4. 방향성:
- CLNT_ID, SESS_ID 를 Key값으로 Product, Session Data Merging
- Product.csv, Session.csv를 통해 Key값들을 바탕으로 실제 검색 활동이 구매로 이어지고 있는 가에 대한 고찰 
   - (-> 이것에 대한 객관적인 지표도 결정)
- 모델링 시작 이전에, Input, Layer, Output에 대한 개괄적인 방향성은 잡아놓고 시작했으면 좋겠음
- 추후에 동일 상품군에 대한 타사 제품 가격을 이용할 것인지 상의

* 지금 하고 있는 고민들이, NN이 실제로 Dynamic Pricing에 어떻게 활용되는지에 대한 Reference(개괄적 코드)를 찾지 못해
활용 양상을 잘 알지 못하기에 어쩌면 현재 스스로가 불필요한 고민을 하고 있는 것인지도 모른다는 생각. 
모델링에 대한 개괄적인 내용은 꼭 한 번 짚고 넘어갔으면 좋겠음
(현재 수동으로 고려하고 있는 내용들이 모델 내 weight에서 자동으로 고려된다면 몇 가지 불필요한 고민을 하고 있다고 생각 됨)*

</details>

#### Contents and Decisions
1. 데이터 구조
- 변수 설명서를 확인하고 함께 논의. 

2. 분석 범위
- 상품 분석으로 중분류까지 사용
- e-commerce 구매력에 대한 통계 자료를 통해, 핵심고객층을 선정. 10~60대를 대상으로 분석.   
  - (기준 관련 참조 )

3. 협업 방식
- 전처리단계: 데이터셋을 나눠서 전처리 방식을 고민
- 모델단계: 동일한 테스트/트레이닝 셋에 대해서 개별적으로 모델 만들고 최종적으로 . 

#### Forward plans

1. 데이터 구조 숙지, 질문 사항 공유
2. 다이나믹 프라이싱을 어떻게 할지 ML모델 outline 그려오기 또는 레퍼런스 찾아서 요약하기.



---

###  다이나믹 프라이싱 Outline 
#### Overview
#### pe-shared

<details>
  <summary>Click to expand!</summary>
	
동건  
- 특정 시점의 데이터만 존재하기 때문에, 실시간 프라이싱은 불가능
- 단, 데이터 집적단위가 개인이기 때문에 개인맞춤화는 가능

제안1  
- 사용자 행동에 대한 데모데이터 만든 후 구매확률 예측
1. 현재 데이터는 구매를 한 사람들의 온라인 행동 데이터만 존재.
2. 구매를 안 한 사람들의 온라인 행동의 분포를 유추하여 데모데이터를 만듬
3. 구매 여부에 따라 0,1 라벨을 만들고 지도학습 모델 생성
4. 0~1사이의 가중치를 예측하여 사용자 온라인 행동에 따른 가격차별화!
-  데모데이터의 비율을 어떻게 할 것인가에 대한 레퍼런스 필요! 즉 고객전환율!!
	
제안2
- 데이터에서 새롭게 지불용의 y를 정의하고 이를 예측
1. 지불용의에 대한 세부 정의는 데이터 탐색 단계에서 결정
2. 상품 구매 소요시간, 구매빈도, 구매량 등으로 y를 정의할 수 있을 듯? 
3. 단 구매를 한 사람들만 있기 때문에, 데이터는 모두 지불용의가 상품의 가격보다 높은 상태. 
-  다이나믹 프라이싱을 하는 데 한계가 될 수 있음

승우
- 다이내믹 프라이싱 -> 초/분 단위로 업데이트 되는 진정한 의미의 Real Time Data가 아니라면 결국 개별 Customizing 문제

제안
- 사용자들의 소비 행태에 대한 새로운 지표를 만든다.
[단순한 하나의 시나리오]
1. ex. PD_BUY_AM*PD_BUY_CT ~ TOT_PAG_VIEW_CT + TOT_SESS_HR_V + CLNT_GENDER + CLNT_AGE (+다른 변수들) MSE를 Minimize 하도록 가중치 학습
2. Softmax -> 새로운 prediction variable 생성
3. 확률값의 분포를 보고, 특정 확률값에 따라 소비자군 분리 -> 소비자군 별로 다른 할인폭 적용 등의 Strategy

문제는, 
1. 높은 할인폭을 적용 받은 사람들이 가격이 낮아졌다고 해서 구매를 늘릴지는 또 생각해봐야 할 문제.
   필요 이상으로 살 것 같지는 않고, 또 그렇다고 다른 상품군으로 넘어갈까에 대해서도 의문이 듦.
2. 개별 Customizing이 아니라 Clustering의 의미가 짙어 보이는데, 과연 넓은 의미에서 Dynamic Pricing으로 볼 수 있겠는가. 

</details>

#### Contents and Decisions
#### Forward plans

