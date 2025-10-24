# ML_Electricity_Prediction
### SNU_KDT 6조
### 권서영 이소민 이유림 조성우 지희선 홍예나

> **목표**: 시계열적으로 주어진 건물별 전력사용량 데이터를 기상 데이터, 건물 데이터 등을 이용하여 예측  
> **프로세스**: EDA&전처리 -> 파생변수 설정 -> 모델링 -> 평가  
> **데이터**: [Dacon의 2025 전력사용량 예측 AI 경진대회](https://dacon.io/competitions/official/236531/overview/description) 

<pre>
.
├── README.md
├── 발표자료.pdf
└── notebooks/
    ├── EDA1.ipynb
    ├── EDA2.ipynb
    ├── RandomForest_Per_type.ipynb
    ├── XGBoost_Per_type.ipynb
    ├── SVR_Per_building.ipynb
    └── XGBoost_Per_building.ipynb ## Final prediction
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
- **SMAPE**: $$\frac{100}{n} \sum\limits^n \frac{|y_{\text{pred}} - y_{\text{real}}|}{\left(|y_{\text{pred}}|+|y_{\text{real}}|\right)/2}$$

### 2.2. 분석 Flow
- **EDA**: 상관계수, ACF/PACF, time-series decomposition의 유효성 검증, 이상치 확인
- **전처리**: datetime type 변환, 파생변수 생성(시점관련, 기상관련), Min-Max normalization
- **모델링**:
  - `TimeSeriesSplit`을 사용해 Forward-looking Leakage 방지
  - 건물별 병렬처리를 사용한 Optuna Tuning with TPESampler로 최적 하이퍼파라미터 탐색
  - 건물 유형별로 분석: Time-series decomposition에 기반한 linear 모델에서 한계를 발견하고 앙상블 모델 채택
    1. RandomForest: 8.38%
    2. XGBoost: 22.56%
  - 건물별 분석: 같은 유형이라도 다른 시계열 패턴을 보이는 건물에 대한 성능 향상을 위해 건물별 모델링 실행
    1. SVR: 9.30%
    2. XGBoost: 7.97%

---
## 3. 결론
최종 모델:
 - XGBoost(건물별 분석): SMAPE 점수 7.97%
  
한계/개선 방향:
 - 기상,시점 관련 파생변수에 비해 인문학적인 요인을 고려하지 못함
 - 시계열 이상치 처리 방안 
 - XGBoost Prediction의 잔차에 시계열 모델링을 통한 자기상관성 반영
