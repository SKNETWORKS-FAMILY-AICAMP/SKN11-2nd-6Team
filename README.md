# 온라인 학습 플랫폼 회원 이탈 예측

## 1️⃣ 팀원 소개
- **SK 네트웍스 Family AI 캠프 11기**

- **팀명:** **6조**

- **기간:** **2025년 3월 31일** ~ **2025년 4월 1일**

---

|[@백미송](https://github.com/misong-hub)|[@김성지](https://github.com/kimseoungji0801)|[@이채은](https://github.com/chaeeunlee05)|[@이혜성](https://github.com/comet39)|[@홍성욱](https://github.com/Sung-WookHong)|
|------|------|------|------|------|
| <img src="https://github.com/user-attachments/assets/2dc83746-b3a4-45a8-96d3-36a458222cc1" width="200"/> | <img src="https://github.com/user-attachments/assets/108ea96c-cb56-42fc-90cb-0d2c833c0fd2" width="200"/> | <img src="https://github.com/user-attachments/assets/9c95cb5d-a8e2-4716-a7a4-7278aee484d1" width="200"/> | <img src="https://github.com/user-attachments/assets/df0d3172-e97e-4909-8c53-dbb5ee137713" width="200"/> | <img src="https://github.com/user-attachments/assets/27d29763-0f6a-4b17-8151-e5654c407e7d" width="200"/> |

## 🔧 기술 스택

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white" style="display: inline-block; margin: 5px;">
  <img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" style="display: inline-block; margin: 5px;">
  <img src="https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=NumPy&logoColor=white" style="display: inline-block; margin: 5px;">
</p>


## 2️⃣ 데이터 셋
# Merging Dataframes

이 프로젝트에서는 여러 데이터 파일을 병합하여 학생들의 학습 활동과 관련된 다양한 분석을 수행합니다. 각 데이터 파일은 다음과 같은 정보를 포함하고 있습니다:

## 1. 학생 정보 관련 파일
- **studentInfo.csv**: 학생의 인구 통계 정보, 등록 정보 등을 담고 있습니다.

## 2. 과정 정보 관련 파일
- **courses.csv**: 각 코스(모듈)에 대한 정보를 담고 있습니다.
- **assessments.csv**: 코스 내의 평가 정보 (과제, 시험 등)를 담고 있습니다.

## 3. 학생 성적 관련 파일
- **studentAssessment.csv**: 학생들의 평가 점수를 담고 있습니다.
- **studentRegistration.csv**: 학생의 코스 등록 정보를 담고 있습니다.

## 4. VLE 활동 관련 파일
- **vle.csv**: VLE 활동에 대한 설명 정보를 담고 있습니다.
- **studentVle.csv**: 학생의 VLE(Virtual Learning Environment) 활동 데이터를 담고 있습니다.

이 데이터들은 다양한 분석을 위해 병합될 예정이며, 이를 통해 학생들의 성적, 학습 활동, 평가 등 다양한 측면에서 통찰을 얻을 수 있습니다.

![image](https://github.com/user-attachments/assets/2cfa138d-a3aa-44d0-86ed-743312566af0)

---
### 데이터 병합 과정
![image](https://github.com/user-attachments/assets/30c486af-4ccf-4940-9960-68d6e28fa84f)
- 각각 사진 병합 

![image](https://github.com/user-attachments/assets/4628b792-cfee-41a1-8ace-aa20a437b605)
- 한번에 사진 병합

## 3️⃣ 전처리(Data Preprocessing)

### 1. 결측치 처리

### `date`

- 대부분 `assessment_type = "Exam"`에 해당
- 시험은 일반적으로 강의 종료일에 시행됨
→ `module_presentation_length`를 활용해 **강의 마지막 날짜로 채움**

![image](https://github.com/user-attachments/assets/60390ed7-d70f-4dd2-8733-7f74884ec728)

### 🔹 결측치 제거
### `score`

- 총 173건 결측
    - `Withdrawn`(이탈자): 72개
    - `Pass`, `Fail`, `Distinction`(수료자): 101개
- 과제를 제출했는데도 점수가 없는 경우로 추정… 수가 적으므로 **삭제 처리**
![image](https://github.com/user-attachments/assets/c3ae7c6c-3927-4db9-bcdf-17afcacc547e)

### `imd_band`

- 총 7697건 결측
- 전체 28,785명 중 약 **971명**의 `imd_band`(지역 기반 사회경제 수준 지표)가 없음
- 'imd_band' 컬럼에서 결측치가 있는 행을 삭제
![image](https://github.com/user-attachments/assets/367bcdcb-e5ce-48cb-b883-ebbc0efe3234)

### `date_unregistration`

- 결측치 160,857건
- 해당 컬럼은 **"언제 수강을 중도 이탈했는가"**를 의미
- - 수강 등록일이 없는 7건 → **삭제**
→ **결측 = 중도 이탈하지 않은 수료자**
→ 즉, 의미 있는 결측이므로 **삭제하지 않음** → 인코딩 과정에서 결측치를 처리 한다!!!

### `final_result 이상치 처리 코드`
<<<<<<< HEAD
=======
<img src="https://github.com/user-attachments/assets/82e61b3a-c481-4d34-8b4d-d228749b24b0" style="display: inline; margin-right: 10px;">

![image](https://github.com/user-attachments/assets/f5cc97f2-0d1e-41f3-adfd-badd28856ce0)


>>>>>>> a7aeb3cc40012ac5f44d9d35474d7f765a271b77
- **`date_unregistration`**: 이 필드는 학생이 온라인 학습을 이탈한 날짜를 나타냅니다. 학생이 학습을 중단하거나 수업을 탈퇴한 시점을 추적할 수 있습니다.

- **`final_result`**: 이 필드는 학생의 최종 성적 결과를 나타냅니다. 다만, **`date_unregistration`**이 존재하는 경우(즉, 학생이 온라인 학습을 이탈한 경우), 해당 학생은 **`Fail`** 상태로 결과가 나오지 않도록 처리되었습니다. 이유는 학습 이탈이 성적에 영향을 미치므로, 이탈 학생에게는 `Fail` 값을 부여할 수 없기 때문입니다.

### 처리 방식

- **학습 이탈**: `date_unregistration` 값이 존재하는 학생은 학습을 중단한 것으로 간주되며, 이들은 더 이상 학습을 지속하지 않으므로 **`Fail`** 상태로 처리될 수 없습니다.
  
- **이탈 학생 제외**: `final_result` 값이 `Fail`로 설정될 수 없는 이유는, `date_unregistration`에 의해 이미 이탈한 학생은 성적이 `Fail`로 기록되지 않기 때문입니다.

따라서, **`date_unregistration`** 값이 존재하는 학생은 `final_result` 값이 **`Fail`** 이 아니도록 예외 처리가 이루어집니다.

<<<<<<< HEAD
<img src="https://github.com/user-attachments/assets/82e61b3a-c481-4d34-8b4d-d228749b24b0" style="display: inline; margin-right: 10px;">

![image](https://github.com/user-attachments/assets/f5cc97f2-0d1e-41f3-adfd-badd28856ce0)

### 2.인코딩
![image](https://github.com/user-attachments/assets/bdd3bec7-c29f-49b2-a7d5-a0108f37bbef)

### 불필요한 특성 제거
   - 분석에 불필요하거나 중복되는 정보를 가진 열 제거
![image](https://github.com/user-attachments/assets/6bfea0d2-d3ab-43d1-ae69-88ef89b54229)


=======
### 2.인코딩
 ![image](https://github.com/user-attachments/assets/bdd3bec7-c29f-49b2-a7d5-a0108f37bbef)
 
 ### 불필요한 특성 제거
    - 분석에 불필요하거나 중복되는 정보를 가진 열 제거
 ![image](https://github.com/user-attachments/assets/6bfea0d2-d3ab-43d1-ae69-88ef89b54229)

### 특성 엔지니어링
#### 학생의 성적 관련 특성
  - 각 학생의 평균 점수, 최고 점수, 최저 점수, 점수의 표준편차
    - 각 학생당 코스별 성적 편차 필요할까?
  - 점수 추세 (상승 또는 하락)
  - 과제 난이도에 따른 가중 점수 -> 난이도 기준을 뭘로 잡아야하나?
 
##### my_average_score, my_max_score, my_min_score, my_score_std, my_score_trend, assesment_weight, weighted_score

- my_avg_score : 개인 학업 성취도 수준 파악
- my_max/min_score/my_score_std : 특정 과목 강점/약점 식별 및 극단적 편차 분석
- my_score_trend	: 학습 효과성 평가 (지속적 상승=효율적 학습법, 하락=개입 필요)
- weighted_score :	난이도 대비 성취도 → "B과제는 고난이도지만 고가중점수 → Distinct 학생" 
![image](https://github.com/user-attachments/assets/832ced68-6613-4d84-a1f0-be2de8745883)

#### 코스 관련 특성
  - 코스별 평균 점수, 최고 점수, 최저 점수, 점수의 표준편차 -> 최저 점수 다 0임, 최고점이 필요한가?, 중간값이 필요한가?
  - 코스별 과제 개수

##### 'course_avg_score', 'course_std_score', 'assignment_count'

- course_avg_score, course_std_score :	평가 방식 적정성 → 편차↑=과도한 변별력 / 강좌 난이도 레벨링
- assignment_count :	과제량-성적 상관관계 분석 → 최적 과제량 도출
![image](https://github.com/user-attachments/assets/a4ff2c22-fb04-42aa-b6a9-95ffa96c0d95)

##### 코스별 지각 제출 비율
![image](https://github.com/user-attachments/assets/41aa4ff8-0aad-485d-bae9-8e7cd983ec87)
![image](https://github.com/user-attachments/assets/eebfb32f-f52b-42c3-adbf-85de664f4573)






