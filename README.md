# ML_Electricity_Prediction
### SNUKDT 6조

> **목표**: 시계열적으로 주어진 건물별 전력사용량 데이터를 기상 데이터, 건물 데이터 등을 이용하여 예측 
> **프로세스**: EDA&전처리 -> 파생변수 설정 -> 모델링 -> 평가
> **데이터**: [Dacon의 2025 전력사용량 예측 AI 경진대회] https://dacon.io/competitions/official/236531/overview/description

<pre>
.
├── README.md
├── 발표자료.pdf
└── notebooks/
    ├── RandomForest.ipynb
    ├── SVR.ipynb
    └── XGBoost.ipynb ## Final prediction
</pre>
---

## 1. 프로젝트 개요
- **프로젝트 명**: 건물별 전력 사용량 예측
- **프로젝트 소개**:
  - 10개 유형 100개 건물의 전력소비량 데이터(1시간주기, ‘24.6.1～8.31), 기상데이터, 건물개요(면적, 태양광·ESS 용량 등)
  - 건물개요, 기상데이터, 시계열데이터 등 85일 분량의 train 데이터를 이용하여 8.24~8.31 각 건물별 전력사용량 예측

---
## 2. 분석

### 2.1. 평가 지표
- **SMAPE**: $\frac{100}{n} \sum\limits^n \frac{|y_{\text{pred}} - y_{\text{real}}|}{|y_{\text{pred}}|+|y_{\text{real}}|}$
