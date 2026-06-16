한국어 | [English](README.en.md)

# Water Potability Classification

수질 측정값으로 물의 음용 가능 여부를 분류하는 머신러닝 학습 프로젝트입니다. 2022학년도 1학기 동서대학교 AI시스템설계 수업 과제로 진행했으며, 여러 분류 모델을 같은 데이터에 적용해 성능을 비교하는 데 목적을 두었습니다.

개발자는 박민건, 김채호입니다.

## 데이터

`data/water_potability.csv`를 사용합니다. ph, Hardness, Solids, Chloramines, Sulfate, Conductivity, Organic_carbon, Trihalomethanes, Turbidity 9개의 수질 지표를 입력으로 받고, 마지막 열 `Potability`(음용 가능 1 / 불가능 0)가 분류 대상입니다. 전체 약 3,276개 행으로 이루어져 있습니다.

ph, Sulfate, Trihalomethanes 열에 결측치가 있는데, 노트북에서는 중앙값으로 채우는 방법과 결측 행을 제거하는 방법을 모두 시도했고 최종적으로는 `dropna`로 결측 행을 삭제한 뒤 분석을 진행합니다.

## 진행 흐름

전처리를 마친 뒤 다음 순서로 작업합니다.

1. `Potability` 클래스 분포, 각 지표의 분포(KDE), 상관 히트맵을 plotly와 seaborn으로 그려 데이터를 살펴봅니다.
2. 데이터를 7:3으로 학습/테스트 분할하고 `StandardScaler`로 표준화합니다.
3. 임의의 파라미터로 여러 분류기를 학습시켜 1차 비교합니다.
4. `GridSearchCV`로 모델별 하이퍼파라미터를 탐색합니다.
5. 찾은 파라미터로 `RepeatedStratifiedKFold` 교차 검증을 돌려 정확도 평균을 비교합니다.

## 사용한 모델

scikit-learn과 Keras로 구성한 분류기들을 비교합니다.

- LinearSVC, SVC (서포트 벡터 머신)
- KNeighborsClassifier (KNN)
- MLPClassifier (사이킷런 다층 퍼셉트론)
- Keras로 직접 쌓은 심층 신경망(DMLP). `KerasClassifier`로 감싸 같은 파이프라인에서 비교합니다.

평가는 `precision_score`와 `accuracy_score`로 합니다.

## 노트북

| 파일 | 내용 |
|---|---|
| `src/WPC.ipynb` | 전처리부터 모델 비교까지 메인 분석 |
| `src/WPC_plot.ipynb` | 시각화 결과가 함께 저장된 버전 |

## 실행

주요 의존성은 numpy, pandas, scikit-learn, tensorflow/keras, matplotlib, seaborn, plotly, missingno입니다. 설치 후 Jupyter에서 `src/WPC.ipynb`를 열어 위에서부터 실행하면 됩니다.

```bash
pip install numpy pandas scikit-learn tensorflow matplotlib seaborn plotly missingno
jupyter notebook src/WPC.ipynb
```
