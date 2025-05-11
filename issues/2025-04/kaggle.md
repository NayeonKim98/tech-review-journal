## Competition

### 1. Simple Competition
### 2. Code Competition

- 외부 X 캐글 노트북에서 실행
  - CPU, GPU 런타임 시간제한
  - 인터넷 사용 불가
  - 공개적으로 사용 가능한 외부 데이터 허용 (하단 Dataset& Models upload으로. 미리 전처리한 데이터셋 업로드하면 시간 절약)
  - 사전 훈련 모델 허용

## Competition Type

1. Featured : 기업
2. Research : 연구 목적
3. Analytics : Target 얼마나 맞추는지
4. Gettin Started : 튜토리얼
5. Playground : 재미와 연습
6. Community(In-class) : 캐글러 스스로

## Titanic Competition


### 목표

train.csv / test.csv 데이터를 기반으로,
최적의 전처리와 모델링으로 높은 Public LB 점수 달성

### 개요

| 항목 | 설명 |
| --- | --- |
| 문제 유형 | 이진 분류 (생존 여부) |
| 평가 지표 | Accuracy |
| 데이터 수 | 학습 891건, 테스트 418건 |
| 타깃 변수 | Survived (0 or 1) |

### 전처리 실험 내용

1. 안쓰는 데이터 제거 : 'PassengerID', 'Name', "Ticket', 'Cabin' 제거
2. text로 되어 있는 데이터 인코딩 : 'Sex' Label Encoding ('male':0, 'female':1)
3. 결측치 처리 : 'Embarked', 'Age', 'Fare' (최빈값/중앙값)

4.

| Feature | 설명 | 성능 영향 |
| --- | --- | --- |
| 'FamilySize' | SibSp + Parch + 1 | 긍정적 |
| 'IsAlone' | FamilySize == 1 | 긍정적 |
| 'Title' | 이름에서 Mr, Miss, etc 추출 | 긍정적 (특히 Target Encoding에서) |
| 'Deck' | Cabin의 첫 알파벳 | 제한적 효과 | 
| 'FarePerPerson | Fare / FamilySize | 약간 효과 |
| 'Age*Class' | Age*Pclass | 긍정적 |
| 'AgeBin', 'FareBin' | 연속형 변수 이산화 | 과적합 우려 |
| 'Pclass_Sex' | 계급 + 성별 결합 | 혼합형 인코딩 필요 |

### 모델링 실험 목록

- LogisticRegression
- RandomForest
- XGBoost

XGBoost > RandomForest > LogisticRegression

### 앙상블

1. soft Voting : LR + RF + XGB 조합
2. Stacking : Level 2에 XGB 사용 (성능 하락)

#### 최고 성능 달성 조합

1. Features : FamilySize, IsAlone, Title(target encoding), Age*Class, FarePerPerson
2. Model : VotingClassifier with LB + RF + XGB

#### 점수 하락 원인 분석

- 과도한 피처 추가로 과적합
- Stacking 구조가 오히려 Public LB 기준에서 불안정

