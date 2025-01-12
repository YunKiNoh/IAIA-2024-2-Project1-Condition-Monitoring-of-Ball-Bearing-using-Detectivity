# Project1: Condition Monitoring of Ball Bearing using Detectivity
본 레포지토리는 24-2 IAIA(Industrial AI & Automation)에서 수행한 첫 번째 팀프로젝트에 대한 소개를 위해 작성되었습니다. 해당 팀프로젝트는 오픈 데이터 셋을 통해 RUL(Remaining Usefule Life)을 예측하는 머신러닝 기법을 다양하게 시도해보기 위해서 수행되었습니다. 특히 매트랩에서는 오픈 데이터셋인 'Wind Turbine High Speed Bearing Prognosis' Dataset을 통해 RUL을 예측하는 기본적인 머신러닝 기법에 대한 가이드라인을 제공하고 있습니다. 이때, 저희 팀은 이러한 기본적인 머신러닝 기법을 넘어서서 Hjorth's parameter를 통해 생성한 Detectivity를 통해 RUL을 예측하는 더 효율적인 방식을 논문을 통해 발견하였고 이를 매트랩을 통하여 구현 및 해당 데이터 셋에 적용하였습니다.
- 논문: Riasi, A., Haghighat, S., & Liang, M. (2021). Detectivity: A combination of Hjorth’s parameters for condition monitoring of ball bearings. Mechanical Systems and Signal Processing, 160, 108247. https://doi.org/10.1016/j.ymssp.2021.108247
- 데이터: MathWorks. (2017). Wind Turbine High Speed Bearing Prognosis Data [Dataset]. GitHub. https://github.com/mathworks/WindTurbineHighSpeedBearingPrognosis-Data

## Introduction
최단 시간동안 최대한 많은 제품을 생산해야 하는 공장 운영에 있어서 고장 예측은 중요한 과업입니다. 특히 공장 가동에 차질을 일으키는 부분 중에서 볼 베어링의 고장이 많은 부분을 차지하고 있기 때문에 산업 현장에서는 볼 베어링의 고장을 미리 예측하기 위한 방법을 마련하고 있습니다. 이러한 상황 가운데 머신러닝을 통한 RUL 예측은 데이터셋으로부터 볼베어링의 진동 정보를 뽑아내고 이를 머신러닝에 학습시켜 고장을 미리 예측하는 방식으로 정형화되어 있습니다. 매트랩에서는 풍력 발전기 데이터셋을 통해 RUL을 예측하는 전형적인 머신러닝 기법을 제시하고 있는데, 이에 대하여 저희 팀은 Modena and Reggio Emilia University의 논문에서 제시하는 Detectivity 기법을 통하여 RUL을 예측 및 그 결과를 기존 방식과 비교하여 더 나은 RUL 예측에 대한 가능성을 탐구하였습니다.

<div align="center">
  <img width="940" alt="detectivity" src="Detectivity" src="https://github.com/YunKiNoh/24-2_IAIA_Project2-Automatic-Hamburger-Stacking-System/blob/main/image/detectivity.png" /><br>
  <p style="margin-top: 10px;">Fig 1. Detectivity</p>
</div>
