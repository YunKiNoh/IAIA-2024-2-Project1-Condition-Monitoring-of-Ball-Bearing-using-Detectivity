# Project1: Condition Monitoring of Ball Bearing using Detectivity
본 레포지토리는 24-2 IAIA(Industrial AI & Automation)에서 수행한 첫 번째 팀프로젝트에 대한 가이드라인을 제공하기 위해서 작성되었습니다. 해당 팀프로젝트는 매트랩에서 제공하는 오픈 데이터 셋을 통해 RUL(Remaining Usefule Life)을 예측하는 머신러닝 기법을 다양하게 시도해보기 위해서 수행되었습니다. 특히 매트랩에서는 'Wind Turbine High Speed Bearing Prognosis' 데이터셋을 통해 RUL을 예측하는 기본적인 머신러닝 기법에 대한 가이드라인을 제공하고 있습니다. 저희 팀은 이러한 기본적인 머신러닝 기법과 함께 Detectivity 기법에 대한 논문을 서로 비교하며 RUL 예측에 대한 프로젝트를 진행하였습니다.

<div align="center">
  <img width="940" alt="Structure of Project" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/project.jpg" /><br>
  <p style="margin-top: 10px;">Fig 1. Structure of This Project</p>
</div>

- 논문: Riasi, A., Haghighat, S., & Liang, M. (2021). Detectivity: A combination of Hjorth’s parameters for condition monitoring of ball bearings. Mechanical Systems and Signal Processing, 160, 108247. https://doi.org/10.1016/j.ymssp.2021.108247
- 데이터: MathWorks. (2017). Wind Turbine High Speed Bearing Prognosis Data [Dataset]. GitHub. https://github.com/mathworks/WindTurbineHighSpeedBearingPrognosis-Data

## 1. Introduction
최단 시간동안 최대한 많은 제품을 생산해야 하는 공장 운영에 있어서 고장 예측은 중요한 과업입니다. 특히 공장 가동에 차질을 일으키는 부분 중에서 볼 베어링의 고장이 많은 부분을 차지하고 있기 때문에 산업 현장에서는 볼 베어링의 고장을 미리 예측하기 위한 방법을 마련하고 있습니다. 이러한 상황 가운데 머신러닝을 통한 RUL 예측은 데이터셋으로부터 볼베어링의 진동 정보를 뽑아내고 이를 머신러닝에 학습시켜 고장을 미리 예측하는 방식으로 정형화되어 있습니다. 매트랩에서는 풍력 발전기 데이터셋을 통해 RUL을 예측하는 전형적인 머신러닝 기법을 제시하고 있는데, 이에 대하여 저희 팀은 Modena and Reggio Emilia University의 논문에서 제시하는 Detectivity 기법을 통하여 RUL을 예측 및 그 결과를 기존 방식과 비교하여 더 나은 RUL 예측에 대한 가능성을 탐구하였습니다.

<div align="center">
  <img width="940" alt="Detectivity" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/detectivity.jpg" /><br>
  <p style="margin-top: 10px;">Fig 2. Detectivity</p>
</div>

Detectivity는 Hjorth's parameter인 Activity, Mobility, 그리고 Complextiy를 조합하여 만든 새로운 파라미터입니다. 특히 볼 베어링의 고장 신호를 담아내기 위해 고안된 파라미터로써, 기존에 RUL 예측을 위한 학습 파라미터를 선정하는 과정이 생략되기 때문에 손쉽게 RUL 예측을 수행할 수 있다는 장점이 있습니다. Activiy는 신호의 에너지를, Movility는 신호의 변동성을, 그리고 Complexity는 신호의 예측 불가능성을 나타내는데 일반적으로 뇌파 분석에 사용되고 있습니다. y의 경우 전처리 되지 않은 진동 데이터로써, 분산 및 미분과 같은 간단한 수학적 과정을 통해 세가지 매개변수를 계산할 수 있습니다. 이 세가지 매개변수를 데시벨화 하여 계산한 매개변수가 detectivity입니다. 해당 매개변수는 고장에 근접할 수록 증가하는 특성을 가지고 있습니다.

## 2. Process
매트랩에서 제공하는 기본적인 RUL 예측 기법은 진동 데이터 셋을 통해서 시간 영역 및 주파수 영역 파라미터를 추출하여 진동 특성을 잘 담아내는 파라미터를 선정 및 머신러닝 학습으로 정리될 수 있습니다. 
[Link: https://kr.mathworks.com/help/predmaint/ug/wind-turbine-high-speed-bearing-prognosis.html]. 해당 방식과 Detectivity 기법을 비교하기 위해서 풍력 발전 데이터셋을 다운로드하여 데이터 전처리 및 RUL 예측을 다음과 같이 진행하였습니다.

### 2.1. Download Dataset
<div align="center">
  <img width="940" alt="Wind Turbine Dataset" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/dataset.jpg" /><br>
  <p style="margin-top: 10px;">Fig 2. Wind Turbine Dataset</p>
</div>
데이터 셋은 상단 데이터 링크를 통해서 다운로드 할 수 있습니다.


