## 고객 행동 기반 대출 상환 예측

> 대출 상품에 대해 채무 불이행 가능성이 있는 소비자 예측하기

해당 프로젝트는 kaggle Datasets 을 이용하여 작성된 머신러닝/딥러닝 스터디 프로젝트 입니다.

Datasets link - (https://www.kaggle.com/subhamjain/loan-prediction-based-on-customer-behavior)



### 평가척도

loss (cross-entropy) 를 최소화되는 모델로 접근해보자.

### **데이터 소개**

- Rows 252,000, Columns 12
  - Categorical 6, Numeric 5

![DF_info](https://user-images.githubusercontent.com/84762599/153588655-b16a0ffe-d100-4173-921f-e5a134693d60.png)



## EDA

**train/test**

- train 에는 고객에 대한 전반적인 정보와 대출 상환 여부에 대한 데이터가 있다. (결측치 없음)
- test 에는 고객에 대한 전반적인 정보만 있다. (결측치 없음)

**CITY**

- 카테고리 데이터지만 데이터간 중복이 거의 없다 (피쳐 제거)



## Making feature & Model

> 학습에 의미가 있을법한 피쳐를 추가하고, 모델을 구축하기



###  **EDA를 통한 새로운 피쳐 생성**

- Changed_profession

  Experience와 CURRENT_JOB_YRS 열의 차이에서 이직여부를 확인가능 (즉, 실 업무 기간에 대한 유추 가능)

  

### **train data -  Risk_Flag 피쳐의 클래스 불균형 해소**

- 반응변수 Risk_Flag의 데이터 불균형이 크므로 향후 test data 결과물 도출 시 편향된 모델이 작성 될 문제가 있을 것이므로 sampling 작업 진행.
- 불균형 해소 전 Risk_Flag 피쳐의  비율 (약 23만 : 2만)
- Down sampling을 통해 23만개의 data 양 축소, Up sampling을 통해 2만개의 data 양 증가

![불균형](https://user-images.githubusercontent.com/84762599/153588831-ed2f59e3-c252-4860-91c5-005dc320f022.png)

### label 인코딩과 ordinal 인코딩

- 머신러닝에서 x 데이터를 활용하려면 변수는 숫자여야 한다. 고로 value 가 문자열 타입인 변수들을 label 인코딩과 ordinal 인코딩을 통해 정수, 실수 타입으로 변경하였다.

### 모델 적용 전 데이터 표준화 (Standard, OneHot)

- numeric_features와 categorical_features를 구분하여 데이터 표준화 작업 진행.



# 적용한 분석 기법 및 모델

### machine learning

- 반응변수 Y에 따른 머신러닝 모델의 타입은 Classification.
- pycaret 을 이용하여 가장 결과물이 좋은 best 1에 대한 결과를 선정하였습니다.
- RandomForestClassifier 모델이 가장 최적화된 성능을 보여주었습니다.

![ML result](https://user-images.githubusercontent.com/84762599/153588906-201e1f3e-ab1c-4b80-813b-c528c372019e.png)

AUC score : 0.935

Model score : 0.872



### machine learning 모델 척도 확인

![모델 척도](https://user-images.githubusercontent.com/84762599/153589247-5dde8df6-a72b-4bc7-933c-99c097ea57ef.png)

### Deep learning

- colab 클라우드 서비스 사용 
- tensorflow 이용
- Keras-tuner for Bayesian HPO 이용하여 파라미터 최적값을 도출

![DL result](https://user-images.githubusercontent.com/84762599/153588982-fd525187-e970-4946-aad4-6942bfa2df90.png)

loss (cross-entropy) : 0.3365

test accuracy : 0.8716

분석결과 epoch 6 부터 오버피팅 현상이 심해짐을 확인하여 epoch 값은 5로 설정.

