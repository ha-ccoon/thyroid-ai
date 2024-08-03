## 2024년 서울 바이오 허브 DNA 특화 교육 프로젝트 실습

### 프로젝트 설명

- 교육 수료 후 마무리 프로젝트 후 발표 (70 시간)
- 주제: 원하는 데이터와 원하는 유형의 모델을 학습 (자유 주제)
- 기간: 24 시간

### 목표 Task

갑상선 질환 (Target classes)을 인공지능 모델을 통해 분류해보자

### 데이터 출처

UCI (University of California, Irvine)
https://archive.ics.uci.edu/dataset/102/thyroid+disease

### 데이터 설명

- 사용 파일: allhyper.data / allhyper.names / allhyper.test
- 2800개의 training set, 972개의 test set

##### Target classes

- Hyperthyroid: 갑상선 기능 항진증 / 갑상선 호르몬이 과다하게 분비되는 상태
- T3 toxic: T3 독성 / T3(삼요오드티로닌) 호르몬이 과다하게 분비되는 상태
- Goitre: 갑상선종 / 갑상선이 비대해진 상태
- Secondary toxic: 이차성 독성 / 이차적인 원인에 의한 갑상선 기능 항진증
- Negative: 정상 상태

### 모델 설명

- 다항 로지스틱 회귀 모델 (SoftMax Regression) 사용

#### 시도 매개 변수

| 차시  | 매개변수                                                                |
| ----- | ----------------------------------------------------------------------- |
| 1차시 | solver='lbfgs', max_iter=500                                            |
| 2차시 | solver='lbfgs', max_iter=1000                                           |
| 3차시 | solver='newton-cg', max_iter=1000, penalty='l2'                         |
| 4차시 | solver='newton-cg', max_iter=500, penalty='l2', class_weight='balanced' |

### 프로젝트 평가

- 모델의 정확도 (accuracy)는 98%로 높은 성능 유지
- `negative` 클래스에 대해 높은 정확도와 재현율을 가지지만, 나머지 `T3 toxic, goitre, hyperthyroid` 클래스에 대한 성능이 매우 낮음
- 이는 데이터 불균형으로 인한 소수 클래스에 대한 성능 저하로 보임. 어떻게 개선할 수 있을까?
  - `class_weight='balanced` 매개변수를 추가 시도
  - 그러나 모델의 정확도는 91%로 저하. 왜 그럴까?
  - 데이터 불균형이 아주 심각한 경우 데이터의 품질(대표성)이 떨어지기 때문에 해당 매개변수로도 커버 불가능
- 개선 방안
  - 오버샘플링(SMOTE) 또는 언더샘플링: 소수 클래스의 샘플 추가 또는 다수 클래스 샘플 무작위 삭제
  - 언더 샘플링의 경우, 모집단의 샘플 수를 감소 시키기 때문에 오버샘플링을 보통 사용

### 프로젝트 회고

- 도메인 이해의 중요성
  - 어떤 데이터가 중요한지, 어떤 요소가 feature가 될 수 있는지 도메인에 대한 이해가 부족하다 보니 전반적인 과정에 어려움을 겪음
- 양질의 데이터의 중요성
  - 데이터가 아무리 많아도 데이터의 품질이 떨어진다면 좋은 모델을 만들 수 없음 (garbage in, garbage out)
- 다른 분류 모델 시도
  - 랜덤 포레스트 (random forest)를 사용해 봤으면 어땠을까 고민 (시간 부족....)

**강사님 피드백**

- 데이터가 한 쪽으로 많이 치우친 경우 랜덤 포레스트 모델을 사용하기 어려움
- 랜덤 포레스트의 경우, 부트스트래핑(bootstrapping)으로 여러 결정 트리(Decision tree)를 만들기 때문에 데이터에 불균형이 있는 경우 편향된 모델을 만들기 쉬움
- 따라서 랜덤 포레스트를 위한 세심한 데이터 전처리가 필요
