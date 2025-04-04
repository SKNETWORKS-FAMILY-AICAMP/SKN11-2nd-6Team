**📅개발 기간: 2025.03.31 ~ 2025.04.01**
# 🎓 대학교 수강 이탈 예측 모델링

## 🏃‍♂️ 팀원 소개
- **SK 네트웍스 Family AI 캠프 11기**

- **팀명:** **6조**
---

|[@백미송](https://github.com/misong-hub)|[@김성지](https://github.com/kimseoungji0801)|[@이채은](https://github.com/chaeeunlee05)|[@이혜성](https://github.com/comet39)|[@홍성욱](https://github.com/Sung-WookHong)|
|------|------|------|------|------|
| <img src="https://github.com/user-attachments/assets/a1ac1e5d-1ebf-4a76-b415-60b718b26c8c" width="200"/> | <img src="https://github.com/user-attachments/assets/86a6b099-43f7-4b43-86f0-e0738ac5b7a0" width="200"/> | <img src="https://github.com/user-attachments/assets/6aa27c80-d308-4ed1-a3ee-526f17e149be" width="200"/> | <img src="https://github.com/user-attachments/assets/dbb903a4-d079-4671-b257-606920967396" width="200"/> | <img src="https://github.com/user-attachments/assets/a4554b6a-e7ef-44d2-9799-09cc7bb78402" width="200"/> |


---

## 🔧 기술 스택

  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=&logoColor=white" style="display: inline-block; margin: 5px;">

## 데이터 분석
<p align="">
  <img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" style="display: inline-block; margin: 5px;">
  <img src="https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=NumPy&logoColor=white" style="display: inline-block; margin: 5px;">
  <img src="https://img.shields.io/badge/scikitlearn-0FAAFF?style=for-the-badge&logo=&logoColor=white" style="display: inline-block; margin: 5px;">
</p>

## 시각화
<p align="">
  <img src="https://img.shields.io/badge/Matplotlib-E95420?style=for-the-badge&logo=&logoColor=white" style="display: inline-block; margin: 5px;">
  <img src="https://img.shields.io/badge/Seaborn-6D00CC?style=for-the-badge&logo=&logoColor=white" style="display: inline-block; margin: 5px;">
</p>

---

## 목차   
1. 프로젝트 개요   
2. 탐색적 데이터 분석 (EDA)   
3. 학생 이탈 예측 모델 학습   
4. 학생 정보 기반 맞춤형 교육과정 설계
5. Appendix
   
<br>

---

## 1. 🔍 프로젝트 개요
### 1) 📌 프로젝트 목표
 * Open University의 실제 데이터셋을 활용하여 수강 철회 결정에 영향을 미치는 주요 요인 분석
 * 수업 난이도, 학생의 기본 정보, 과제 제출 패턴 등을 종합적으로 분석해 수강 철회 확률 예측
 * 신규 수강생의 데이터를 기반으로 수강 철회 가능성을 조기에 예측하고, 맞춤형 교육과정 설계방안 제시

### 2) 📂 데이터셋    
 * 데이터 개요   
   영국 Open University에서 공개한 대표적인 공개 데이터: [Open University Learning Analytics Dataset (OULAD)](https://analyse.kmi.open.ac.uk/#open-dataset)
 * 데이터 구성
    1. `assessments.csv` : 각 강의에서 제공되는 과제 및 평가 관련 세부 정보 <br/>
    2. `courses.csv`: 개설된 각 강의의 식별자, 강의명 등 강의 기본 정보 <br/>
    3. `studentAssessment.csv`: 학생들의 과제 제출 이력과 성적 데이터를 포함한 학습 성과 정보 <br/>
    4. `studentInfo.csv` : 각 학생의 인구통계학적 특성(성별, 연령, 최종 학력 등) <br/>
    5. `studentRegistration.csv`: 학생들의 수강 신청 내역과 각 강의에 대한 철회 여부 <br/>
 * 5개의 csv 파일을 `학생 ID(id_student)`, `과목 코드(code_module)`, `학기(code_presentation)`를 기준으로 병합
 * 최종 병합된 데이터에서 불필요한 컬럼 제거  cf.) [제거된 컬럼 명 확인](#불필요한-컬럼-제거)   

<br>

## 2. 📊 탐색적 데이터 분석 (EDA)
### 1) 결측치 처리
#### `assessment_type`의 `Exam`   : 시험일
 * 시험일은 일반적으로 종강일과 동일하므로, 시험일을 강의 마지막 날짜로 수정

#### `score`, `imd_band`   : 각 과제 성적, 학생이 거주하는 지역의 소득 수준
 * 전체 데이터에서 결측치 비율이 낮아 해당 항목은 삭제

#### `date_unregistration`   : 수강철회일
 * 결측치 160,857건 : 수업을 철회하지 않은 수료자
 * 수강 철회일 정보를 라벨 데이터로 변환하고, 결측치는 철회하지 않음을 의미하는 0으로 대체함

#### `final_result` : 수강 철회 여부
 * 라벨 데이터에 해당하므로, 결측치는 삭제

### 2) 데이터 인코딩   
 * 라벨형 데이터로 변환 : `age_band`, `education_to_income`, `gender_to_income`, `disabil_to_income`, `imd_band_to_imcome`  cf.) [데이터 변환 코드 확인](#라벨형-데이터-인코딩)    

 * 원핫 인코딩 : `code_module`, `code_presentation`

### 3) 특성 엔지니어링   
 * 학생의 성적 관련 특성
    - 각 학생의 평균 점수, 최고 점수, 최저 점수, 점수의 표준편차
    - 점수 추세 (상승 또는 하락)
    - 과제 난이도에 따른 가중 점수 부여

 * 코스 관련 특성
    - 코스별 평균 점수, 최고 점수, 최저 점수, 점수의 표준편차
    - 코스별 과제 개수

 * 행동 패턴 특성
    - 과제 제출률
    - 지각 제출 비율
    
 * 원본 데이터에 포함된 주요 컬럼 항목

| 컬럼명                     | 설명                                                               |
|----------------------------|-------------------------------------------------------------------|
| gender                     | 학생의 성별 (남성, 여성)                                           |
| highest_education          | 학생의 최고 학력 수준 (고등학교, 학사, 석사)                       |
| imd_band                   | 학생이 속한 지역의 사회경제적 지위                                 |
| age_band                   | 학생의 연령대                                                     |
| disability                 | 학생의 장애 여부                                                  |
| studied_credits            | 학생이 현재까지 이수한 학점 수                                    |
| num_of_prev_attempts       | 해당 과목의 과거 수강 이력 횟수                                   |
| date_registration          | 학생이 등록한 날짜                                               |
| module_presentation_length | 강의의 전체 길이                                                 |
| assessment_weight          | 각 평가 항목이 성적에 미치는 비중                                 |
| final_result               | 최종 결과 (Pass, Fail, Withdrawn)                                |

<br>

 * 초기 데이터 컬럼을 기반으로 계산된 신규 컬럼 항목

| 컬럼명                     | 설명                                                                 |
|----------------------------|----------------------------------------------------------------------|
| my_average_score           | 특정 학생의 평균 점수                                               |
| my_score_std               | 특정 학생의 점수 표준편차                                           |
| my_score_trend             | 특정 학생 점수의 변화 추이 (상승, 하락)                             |
| weighted_score             | 평가 가중치를 반영한 환산 점수                                      |
| course_avg_score           | 특정 과목 수강생들의 평균 점수                                      |
| course_max_score           | 특정 과목 수강생 중 최고 점수                                       |
| course_min_score           | 특정 과목 수강생 중 최저 점수                                       |
| course_std_score           | 특정 과목 수강생들의 점수 표준편차                                  |
| course_late_rate           | 해당 과목에서 전체 학생의 지각 제출 비율                            |
| days_early_submission      | 마감일 기준으로 과제를 조기 제출한 일 수                            |
| my_late_rate               | 해당 학생의 전체 과제 중 지각 제출한 과제 비율                       |

<br>

### 4) 피어슨 상관계수 히트맵 시각화
  * 특정 변수들 간의 선형적 상관관계가 낮게 나타남
  * 피어슨 상관계수는 변수 간 선형 관계에만 민감하게 반응하므로, 보다 유연한 관계 파악을 위해 추가적인 상관관계 분석을 수행함
![image](https://github.com/user-attachments/assets/178ab026-38cf-4d00-a9b9-199194607474)

### 5) Spearman 상관계수 시각화
  * 변수 간 순위 기반의 비선형 상관관계를 측정
  * 변수간 비례/반비례 관계 확인 가능
  * 수강 철회 여부와 상관관계를 가지는 특성 컬럼 부재
![image](https://github.com/user-attachments/assets/a6e90f34-9fd3-4c24-bb93-406e84c1d3aa)

<br>

  * 변수별 행동 분석 시각화: 과제 점수 비중

![스크린샷 2025-04-01 130619](https://github.com/user-attachments/assets/676ca479-e52a-4f8a-a94f-4718dc98e213)   

<br>

  * 데이터가 수직 방향으로 무작위 분포를 보여, 변수 간에 뚜렷한 경향성이 없음을 시각적으로 확인할 수 있음   
  * 학생의 이탈 여부는 하나의 수치 변수로 설명하기 어렵고, **학생의 행동 기반 요인들은 불규칙하게 작용하여 예측에 복잡성을 더함**   
  * 트리 기반 분류 모델은 변수 간 상관관계가 낮더라도 효과적인 분류가 가능하므로, **통합적인 분석을 위해 학습 모델 활용이 적합함**

---

## 3. ⚙️ 학생 이탈 예측 모델 학습   

### **모델링 개요**
#### **클래스 불균형 문제**
![image](https://github.com/user-attachments/assets/88a00bb3-78b2-4786-8298-b86bcd0f02d2)
  * 데이터 불균형 존재 (1: 철회 / 0: 수강완료)
  * XGBoost 모델에 오버샘플링과 언더샘플링을 적용하여 정확도 비교

   ### ⬆️ 오버샘플링
   - 클래스 불균형 처리: SMOTE를 통해 **이탈자 수(1)** 를 오버샘플링하여 균형 잡힌 학습 데이터셋 구성
     
     ![image](https://github.com/user-attachments/assets/8626ee39-ce18-4a18-a17c-3b2e0d9d26e8)
   
   - recall이 0.28 → 0.52로 크게 상승 → **이탈자를 훨씬 더 많이 잡아냄.**
   - precision은 줄었지만 이는 이탈자 예측을 더 시도했기 때문에 자연스러운 현상.
   - f1-score도 올라서 **균형 잡힌 예측 성능 향상.**
   
   
   ### ⬇️ 언더샘플링
   - 클래스 불균형 처리: **언더샘플링**으로 **이탈자 수(1)** 에 맞춰 비이탈자 수 조정
 
     ![image](https://github.com/user-attachments/assets/75e3c05c-d535-4de5-92af-4f6fe19beb53)
    - Recall이 0.28 → 0.87로 크게 상승 **이탈자를 훨씬 더 잘 잡아냄 (실제 이탈자 중 예측된 비율이 상승)** <br/>
    - Precision은 감소했지만 자연스러운 현상 <br/>
    - F1-score는 약간 하락했지만, 전체적인 목표가 **‘이탈자를 놓치지 않는 것’** 이라면 매우 유의미한 개선 <br/>
    - Accuracy는 줄었지만 이는 클래스 불균형 완화로 인한 변화 → 전체 정확도보다 이탈자 예측 성능이 더 중요한 상황에서는 좋은 방향 <br/>

  ### 결론
  - 언더샘플링 기법을 통해 클래스 불균형 문제를 해결 하는 것이 모델 성능 향상율이 높으므로 앞으로의 모델 학습에 언더샘플링 기법을 사용합니다.

## 1. 앙상블 모델

### 1. 앙상블 (기본)
   - Voting 방식: Hard Voting
   → 로지스틱 회귀, DecisionTreeClassifier, XGBoost의 다수결 투표로 최종 예측을 결정하는 기본 앙상블 방식.
   ```python
   VotingClassifier(
       estimators=[
           ('lr_clf', LogisticRegression()),
           ('dt_clf', DecisionTreeClassifier()),
           ('xgb_clf', XGBClassifier())
       ],
       voting='hard'
   )
   ```
   ![image](https://github.com/user-attachments/assets/41c0477d-97c2-4ace-9722-1390e96d9634)

### 2. 앙상블 + GridSearchCV 
   - Voting 방식: Soft Voting + 하이퍼파라미터 튜닝(GridSearchCV)
   → 각 모델을 GridSearchCV로 튜닝한 후 soft voting 방식으로 예측 확률 평균을 기반으로 최종 예측을 수행.
   ```python
   # 4. Logistic Regression 튜닝
   lr_param_grid = {
       'C': [0.01, 0.1, 1, 10],
       'penalty': ['l2'],
       'solver': ['lbfgs'],
       'max_iter': [100, 500, 1000]
   }
   lr_grid = GridSearchCV(LogisticRegression(), lr_param_grid, scoring='f1', cv=3, verbose=1, n_jobs=-1)
   lr_grid.fit(X_train, y_train)
   best_lr = lr_grid.best_estimator_
   
   # 5. Decision Tree 튜닝
   dt_param_grid = {
       'max_depth': [5, 10, 15],
       'min_samples_split': [2, 5, 10],
       'criterion': ['gini', 'entropy']
   }
   dt_grid = GridSearchCV(DecisionTreeClassifier(random_state=42), dt_param_grid, scoring='f1', cv=3, verbose=1, n_jobs=-1)
   dt_grid.fit(X_train, y_train)
   best_dt = dt_grid.best_estimator_
   
   # 6. XGBoost 튜닝
   xgb_param_grid = {
       'n_estimators': [50, 100, 150],
       'max_depth': [3, 5, 7],
       'learning_rate': [0.01, 0.05, 0.1],
       'subsample': [0.7, 0.8, 1.0],
       'colsample_bytree': [0.7, 0.8, 1.0]
   }
   xgb_grid = GridSearchCV(XGBClassifier(use_label_encoder=False, eval_metric='logloss', random_state=42), xgb_param_grid, scoring='f1', cv=3, verbose=1, n_jobs=-1)
   xgb_grid.fit(X_train, y_train)
   best_xgb = xgb_grid.best_estimator_
   ```
   ![image](https://github.com/user-attachments/assets/3bb140d3-47cf-4bc7-a588-c5ac61934c04)

### 3. 앙상블 + RandomizedSearchCV
   - Voting 방식: Soft Voting + 하이퍼파라미터 튜닝(RandomizedSearchCV)
   → 모델별로 랜덤 탐색 기반 튜닝(RandomizedSearchCV) 후 soft voting으로 예측 확률 평균을 활용하여 예측.
   ```python
   # 4. RandomizedSearchCV - Logistic Regression
   lr_param_dist = {
       'C': uniform(0.01, 10),
       'penalty': ['l2'],
       'solver': ['lbfgs'],
       'max_iter': [100, 300, 500, 1000]
   }
   lr_random = RandomizedSearchCV(
       estimator=LogisticRegression(),
       param_distributions=lr_param_dist,
       n_iter=20,
       scoring='f1',
       cv=3,
       verbose=1,
       n_jobs=-1,
       random_state=42
   )
   lr_random.fit(X_train, y_train)
   best_lr = lr_random.best_estimator_
   
   # 5. RandomizedSearchCV - Decision Tree
   dt_param_dist = {
       'max_depth': randint(3, 20),
       'min_samples_split': randint(2, 20),
       'criterion': ['gini', 'entropy']
   }
   dt_random = RandomizedSearchCV(
       estimator=DecisionTreeClassifier(random_state=42),
       param_distributions=dt_param_dist,
       n_iter=30,
       scoring='f1',
       cv=3,
       verbose=1,
       n_jobs=-1,
       random_state=42
   )
   dt_random.fit(X_train, y_train)
   best_dt = dt_random.best_estimator_
   
   # 6. RandomizedSearchCV - XGBoost
   xgb_param_dist = {
       'n_estimators': randint(50, 300),
       'max_depth': randint(3, 15),
       'learning_rate': uniform(0.01, 0.3),
       'subsample': uniform(0.7, 0.3),
       'colsample_bytree': uniform(0.7, 0.3)
   }
   xgb_random = RandomizedSearchCV(
       estimator=XGBClassifier(use_label_encoder=False, eval_metric='logloss', random_state=42),
       param_distributions=xgb_param_dist,
       n_iter=30,
       scoring='f1',
       cv=3,
       verbose=1,
       n_jobs=-1,
       random_state=42
   )
   xgb_random.fit(X_train, y_train)
   best_xgb = xgb_random.best_estimator_
   ```
   ![image](https://github.com/user-attachments/assets/f4610204-6c05-45a9-9fbe-74a4def9736b)

### 4. 앙상블 + RandomizedSearchCV + LogisticRegression 정규화

   ```python
   lr_pipeline = Pipeline([
       ('scaler', StandardScaler()),
       ('model', LogisticRegression())
   ])
   lr_param_dist = {
       'model__C': uniform(0.01, 10),
       'model__penalty': ['l2'],
       'model__solver': ['lbfgs'],
       'model__max_iter': [100, 300, 500, 1000]
   }
   ```
![image](https://github.com/user-attachments/assets/72e7d5b1-d06e-4c7b-a7fa-045eb5446f54)

## 📈 모델 평가 (최종) - 각 모델별 예측값 비교 (랜덤 샘플 15개)
![output](https://github.com/user-attachments/assets/ae6858e2-c59e-4c1f-873a-cc3f8389164d)


- 학습 정확도 / 테스트 정확도
- classification_report: Precision / Recall / F1-score
- 오버피팅 여부 확인

## 2. 클러스터 기반 분류 모델

### 📌 개요
클러스터 기반의 이진 분류 모델을 사용하여 새로운 학생의 데이터가 들어왔을 때 이탈을 예측합니다. 비지도 학습(클러스터링)과 지도 학습(분류 모델)을 결합하여, 서로 다른 학생 그룹에 특화된 예측 모델을 생성하는 방식으로 설계되었습니다.

### 💡 클러스터링 모델 개선 

데이터의 특성과 클러스터링 알고리즘에 따라 클러스터 개수 및 PCA 컴포넌트 개수가 클러스터링 성능에 영향을 미칠 수 있어서 다음과 같은 시각화를 통해 최적의 파라미터를 탐색

#### 최적의 PCA 컴포넌트 개수 찾기(실루엣 분석 활용)
![](img/PCA_components.png)
- 2개의 PCA 컴포넌트를 사용할 때 가장 높은 Silhouette Score를 기록하므로, 이 경우가 최적의 선택

#### 최적의 클러스터 개수 찾기(엘보우 메서드, 실루엣 분석 활용)
![](img/elbow_search.png)
- 4~5개의 클러스터가 데이터 분류에 가장 적합

### 👥 클러스터링 + 이진 분류 모델

```
1. 데이터 스케일링 및 PCA를 통해 데이터를 2차원으로 축소
2. K-means 클러스터링을 통한 군집화
3. 각 클러스터에 대해 별도의 앙상블 모델 
4. 각 클러스터 id 기준으로 모델과 스케일러를 저장
```

### 🪄 이탈 예측 프로세스
```
1. 새로운 학생 유입
2. 내 클러스터은 어디? (id)
3. 내 클러스터 id에 해당하는 모델을 사용
4. 새로운 학생의 최종 이탈 예측 확인
```

### 📊 결과 분석
#### 클러스터 분포 통계 확인

![](img/clustering.png)

#### 클러스터 별 모델 학습
````
🚀 클러스터 4 모델 학습 시작...
리샘플링 후 클래스 분포:
0: 2573개 | 1: 2573개

✅ 학습 정확도: 0.9951409135082604
✅ 테스트 정확도: 0.829126213592233

📋 분류 성능 보고서 (테스트셋):

              precision    recall  f1-score   support

           0       0.85      0.79      0.82       515
           1       0.81      0.86      0.83       515

    accuracy                           0.83      1030
   macro avg       0.83      0.83      0.83      1030
weighted avg       0.83      0.83      0.83      1030


🚀 클러스터 0 모델 학습 시작...
리샘플링 후 클래스 분포:
0: 4894개 | 1: 4894개

✅ 학습 정확도: 0.9948914431673053
✅ 테스트 정확도: 0.839632277834525

📋 분류 성능 보고서 (테스트셋):

              precision    recall  f1-score   support

           0       0.87      0.80      0.83       979
           1       0.81      0.88      0.85       979

    accuracy                           0.84      1958
   macro avg       0.84      0.84      0.84      1958
weighted avg       0.84      0.84      0.84      1958


🚀 클러스터 2 모델 학습 시작...
리샘플링 후 클래스 분포:
0: 887개 | 1: 887개

✅ 학습 정확도: 0.9950669485553206
✅ 테스트 정확도: 0.856338028169014

📋 분류 성능 보고서 (테스트셋):

              precision    recall  f1-score   support

           0       0.90      0.80      0.85       178
           1       0.82      0.92      0.86       177

    accuracy                           0.86       355
   macro avg       0.86      0.86      0.86       355
weighted avg       0.86      0.86      0.86       355


🚀 클러스터 3 모델 학습 시작...
리샘플링 후 클래스 분포:
0: 2545개 | 1: 2545개

✅ 학습 정확도: 0.9945972495088409
✅ 테스트 정확도: 0.7907662082514735

📋 분류 성능 보고서 (테스트셋):

              precision    recall  f1-score   support

           0       0.81      0.75      0.78       509
           1       0.77      0.83      0.80       509

    accuracy                           0.79      1018
   macro avg       0.79      0.79      0.79      1018
weighted avg       0.79      0.79      0.79      1018


🚀 클러스터 1 모델 학습 시작...
리샘플링 후 클래스 분포:
0: 1055개 | 1: 1055개

✅ 학습 정확도: 0.9869668246445498
✅ 테스트 정확도: 0.8080568720379147

📋 분류 성능 보고서 (테스트셋):

              precision    recall  f1-score   support

           0       0.87      0.73      0.79       211
           1       0.77      0.89      0.82       211

    accuracy                           0.81       422
   macro avg       0.82      0.81      0.81       422
weighted avg       0.82      0.81      0.81       422


🎉 모든 클러스터 학습 완료!
`````

##### 예측 실행 시 정확도

![](img/predict_result.png)

새로운 학생 데이터 7개에 대한 예측 정확도는 85.71%


## 4. 학생 정보 기반 맞춤형 교육과정 설계
### 🧑‍🧑‍🧒‍🧒 협업 필터링을 활용한 신규 학생 이탈 예측
협업 필터링을 통해 유사한 패턴을 보이는 학생들을 추출하고 앞서 개발한 앙상블 모델을 활용하여 새로운 학생의 이탈 예측합니다.

### 개인정보 여부에 따른 수강 이탈자 비율 (Withdrawn)

1. **Gender (성별)**: 성별에 따른 "withdrawn" 상태의 비율
2. **Age Band (연령대)**: 연령대별로 "withdrawn" 상태의 비율
3. **Disability (장애 여부)**: 장애 여부에 따른 "withdrawn" 상태의 비율
4. **Highest Education (최고 학력)**: 최고 학력에 따른 "withdrawn" 상태의 비율

![image](https://github.com/user-attachments/assets/874691a0-828d-4dff-bbb7-257ee95a6180)
![image](https://github.com/user-attachments/assets/aa93f506-ff12-44ac-97d9-533cd873c72d)

#### 이탈 확률이 높은 신입생 예측
1. 신입생 입학 초기에는 인적 정보만 활용 가능함
2. 해당 인적 정보를 기반으로 데이터베이스 내 유사 학생을 선별함
3. 유사 학생 집단에서의 이탈자 비율을 분석함
4. 이탈 비율이 일정 기준을 초과할 경우, 해당 신입생을 중점 관찰 대상으로 지정함

#### 이탈 확률이 높은 신입생의 과제 제출 후 이탈확률 재예측
1. 학생의 제출 과제가 3개 이상일 경우, 점수의 표준편차를 기반으로 행동 특성을 분석하고, 이를 바탕으로 데이터베이스 내 유사 학생을 선별함
2. 선별된 유사 학생들의 데이터를 활용해 해당 학생의 이탈 확률을 예측함
3. 이탈 확률이 일정 기준을 초과할 경우, 이탈 방지를 위한 조건을 검토하고 과제 및 시험의 난이도를 조정함

#### 이탈하지 않을 조건 확인   
  * 과제 난이도 (코스 평균 성적, 최고 성적 표준편차를 조정해가며, 이탈 하지 않을 조건 확인)
```
t = True
result_info = {}

for idx, row in df[df['final_result'] == 1].iterrows():
    base_input = row[X.columns].copy()
    for avg in np.linspace(40, base_input['course_avg_score'], 1):
        for max_ in np.linspace(60, base_input['course_max_score'], 1):
            for std in np.linspace(5, base_input['course_std_score'], 1):
                modified = base_input.copy()
                modified['course_avg_score'] = avg
                modified['course_max_score'] = max_
                modified['course_std_score'] = std

                modified = pd.DataFrame(modified)
                modified = modified.T
                modified = standard_sc.transform(modified)
                pred = voting_clf.predict(modified)[0]
                
                if pred == 0:
                    result_info = {
                        'original_student_id': idx,
                        'original_final_result': row['final_result'],
                        'modified_course_avg_score': avg,
                        'modified_course_max_score': max_,
                        'modified_course_std_score': std
                    }
                    t = False
                    break
            if not t:
                break
        if not t:
            break
    if not t:
        break
```

![](img/id_21)
  * 'modified_course_max_score': np.float64(88.5), 'modified_course_std_score': np.float64(5.0)



## 💡 인사이트 및 결론
### 이탈 위험 학생 조기 식별 시스템
- 학생의 과제 제출률, 점수 편차, 지각 제출 비율, 성적 추세 등을 조합하여
→ 수강 초반부터 이탈 위험군을 사전 탐지 가능

실시간으로 이탈 위험도를 모니터링하는 경보 시스템 도입 가능 (예: "이 학생은 철회 가능성이 높음")


### 개인 맞춤형 학습 개입 전략
- 클러스터링 기반 분류 모델을 통해 서로 다른 학습 유형을 가진 학생들을 그룹화
→ 각 그룹별 맞춤형 개입 전략 제안 가능:

ex) 평균 점수는 낮지만 과제 제출률 높은 학생군 → 튜터링 또는 난이도 조정

협업 필터링을 통해 **유사 학생**과의 비교를 통해 행동 유도 가능

### 학생 개인 정보 기반 이탈 특성 분석

-성별, 연령대, 학력, 장애 여부에 따른 이탈 비율 분석 수행
-예: 연령대가 높을수록, 장애가 있는 경우 이탈 비율 증가하는 패턴 존재

-사회적 취약 계층 대상 학업 유지 프로그램 설계
-특정 연령군을 위한 적응형 콘텐츠 제공 검토

 
## 한줄 회고
 - 이혜성 : 이번 프로젝트를 통해 흥미로운 데이터셋을 바탕으로 데이터 전처리 과정에서 EDA에 유의미한 특성을 탐색하고 추가하는 과정이 재밌었습니다. 또한, 비선형적인 데이터에서 관계성을 찾기 위한 다양한 접근 방식을 시도할 수 있는 시간이 되었습니다.
더불어, 데이터 불균형이 모델 성능에 미치는 영향을 실감할 수 있었고, 이를 극복하기 위한 전략을 고민하는 과정에서 학습이 더욱 깊어졌습니다. 특히, 앙상블 모델과 클러스터링을 조합하여 예측 모델을 구축하면서 각 기법에 대한 이해도를 높일 수 있었던 점이 의미 있었습니다.

- 백미송: 많은 모델들을 접해보고 하이퍼 파라미터 튜닝의 중요성을 느꼈고, 팀원 간의 원활한 커뮤니케이션이 프로젝트 성공에 큰 도움이 되었습니다. 이러한 경험은 앞으로의 데이터 분석 프로젝트에 큰 도움이 될 거 같습니다.

 - 김성지: 이번 프로젝트를 통해서 전처리의 중요성을 다시 한 번 깨닫게 되었으며, 오버샘플링과 언더샘플링을 적용하며 머신러닝 개념을 복습할 수 있었다. 또한, 다양한 모델을 실험하며 데이터 균형이 모델 성능에 미치는 영향을 직접 경험한 값진 기회였다.

 - 이채은: 이번 프로젝트를 통해 모델 성능 향상을 위한 다양한 샘플링 기법과 앙상블 전략을 실험해볼 수 있었습니다. 데이터 전처리 과정에서 컬럼들과 타겟 변수에 대한 해석이 쉽지 않았지만, 복합적인 이탈 요인을 고민하고 이를 모델에 적용해보며 유의미한 인사이트를 얻을 수 있었습니다. 각 모델의 특성과 성능 차이를 직접 비교하며, 모델에 대한 이해도를 한층 더 높일 수 있는 시간이었습니다.

## 5. Appendix

### 불필요한 컬럼 제거   
   - 분석에 불필요하거나 중복되는 정보를 가진 열 제거   
![image](https://github.com/user-attachments/assets/6bfea0d2-d3ab-43d1-ae69-88ef89b54229)

### 라벨형 데이터 인코딩  
![image](https://github.com/user-attachments/assets/bdd3bec7-c29f-49b2-a7d5-a0108f37bbef)




<p align="center">
  <img src="./readme_image/언어의 연관성.jpg" height="450" width="450">
</p>

<div align="center">
  그림 1.1 언어 데이터가 가지는 단어간 상호관계 예시
</div>
<br>
  
* 단어 빈도 속성: K-means 클러스터링<sup>*</sup>을 활용하여 48개의 속성을 5개의 그룹으로 구분
* 군집화된 단어들을 시각화하기 위해 주성분 분석 (PCA)<sup>**</sup>으로 데이터 속성을 2개로 축소 후 산점도(scatter) 그래프 작성
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<sup>*</sup>K-means 클러스터링: 데이터를 K개의 군집으로 나누어 중심점을 기준으로 반복적으로 최적화  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<sup>**</sup>주성분 분석 (PCA): 고차원 데이터를 저차원으로 변환하여 주요 정보만 보존




