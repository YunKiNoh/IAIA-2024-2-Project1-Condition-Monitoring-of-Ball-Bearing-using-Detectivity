# Project1: Condition Monitoring of Ball Bearing using Detectivity
본 레포지토리는 24-2 IAIA(Industrial AI & Automation)에서 수행한 첫 번째 팀프로젝트에 대한 가이드라인을 제공하기 위해서 작성되었습니다. 해당 팀프로젝트는 매트랩에서 제공하는 오픈 데이터 셋을 통해 RUL(Remaining Usefule Life)을 예측하는 머신러닝 기법을 다양하게 시도해보기 위해서 수행되었습니다. 특히 매트랩에서는 'Wind Turbine High Speed Bearing Prognosis' 데이터셋을 통해 RUL을 예측하는 기본적인 머신러닝 기법에 대한 가이드라인을 제공하고 있습니다. 저희 팀은 이러한 기본적인 머신러닝 기법과 함께 Detectivity 기법에 대한 논문을 서로 비교하며 RUL 예측에 대한 프로젝트를 진행하였습니다.

<div align="center">
  <img width="940" alt="Structure of Project" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/project.jpg" /><br>
  <p style="margin-top: 10px;">Figure 1. Structure of This Project</p>
</div>

- 논문: Riasi, A., Haghighat, S., & Liang, M. (2021). Detectivity: A combination of Hjorth’s parameters for condition monitoring of ball bearings. Mechanical Systems and Signal Processing, 160, 108247. https://doi.org/10.1016/j.ymssp.2021.108247
- 데이터: MathWorks. (2017). Wind Turbine High Speed Bearing Prognosis Data [Dataset]. GitHub. https://github.com/mathworks/WindTurbineHighSpeedBearingPrognosis-Data
- 매트랩 방식: https://kr.mathworks.com/help/predmaint/ug/wind-turbine-high-speed-bearing-prognosis.html

## 1. Introduction
최단 시간동안 최대한 많은 제품을 생산해야 하는 공장 운영에 있어서 고장 예측은 중요한 과업입니다. 특히 공장 가동에 차질을 일으키는 부분 중에서 볼 베어링의 고장이 많은 부분을 차지하고 있기 때문에 산업 현장에서는 볼 베어링의 고장을 미리 예측하기 위한 방법을 마련하고 있습니다. 이러한 상황 가운데 머신러닝을 통한 RUL 예측은 데이터셋으로부터 볼베어링의 진동 정보를 뽑아내고 이를 머신러닝에 학습시켜 고장을 미리 예측하는 방식으로 정형화되어 있습니다. 매트랩에서는 풍력 발전기 데이터셋을 통해 RUL을 예측하는 전형적인 머신러닝 기법을 제시하고 있는데, 이에 대하여 저희 팀은 Modena and Reggio Emilia University의 논문에서 제시하는 Detectivity 기법을 통하여 RUL을 예측 및 그 결과를 기존 방식과 비교하여 더 나은 RUL 예측에 대한 가능성을 탐구하였습니다.

<div align="center">
  <img width="940" alt="Detectivity" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/detectivity.jpg" /><br>
  <p style="margin-top: 10px;">Figure 2. Detectivity</p>
</div>

Detectivity는 Hjorth's parameter인 Activity, Mobility, 그리고 Complextiy를 조합하여 만든 새로운 파라미터입니다. 특히 볼 베어링의 고장 신호를 담아내기 위해 고안된 파라미터로써, 기존에 RUL 예측을 위한 학습 파라미터를 선정하는 과정이 생략되기 때문에 손쉽게 RUL 예측을 수행할 수 있다는 장점이 있습니다. Activiy는 신호의 에너지를, Movility는 신호의 변동성을, 그리고 Complexity는 신호의 예측 불가능성을 나타내는데 일반적으로 뇌파 분석에 사용되고 있습니다. y의 경우 전처리 되지 않은 진동 데이터로써, 분산 및 미분과 같은 간단한 수학적 과정을 통해 세가지 매개변수를 계산할 수 있습니다. 이 세가지 매개변수를 데시벨화 하여 계산한 매개변수가 detectivity입니다. 해당 매개변수는 고장에 근접할 수록 증가하는 특성을 가지고 있습니다.

## 2. Process
매트랩에서 제공하는 기본적인 RUL 예측 기법은 풍력 발전기의 진동 데이터애서 시간 영역 및 주파수 영역 파라미터를 추출한 뒤에 진동 특성을 잘 담아내는 파라미터를 선정하고 이를 바탕으로 머신러닝을 학습하여 RUL 예측에 활용하는 방식입니다. 해당 방식과 Detectivity 기법을 비교하기 위해서 매트랩에서 풍력 발전 데이터셋을 다운로드하여 데이터 전처리 및 RUL 예측을 다음과 같이 진행하였습니다.

### 2.1. Import Dataset
데이터 셋을 다음과 같이 불러옵니다.

```
clear all
% Import WT dataset
hsbearing = fileEnsembleDatastore('Full-Data', '.mat');
timeUnit = 'day';
hsbearing.DataVariables = ["vibration", "tach"];
hsbearing.IndependentVariables = "Date";
hsbearing.SelectedVariables = ["Date", "vibration", "tach"];
hsbearing.ReadFcn = @helperReadData;
hsbearing.WriteToMemberFcn = @helperWriteToHSBearing;
tall(hsbearing); % Show Elements of Selected Variables. 
fs = 97656; % Hz
```

<div align="center">
  <img width="940" alt="Wind Turbine Dataset" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/dataset.jpg" /><br>
  <p style="margin-top: 10px;">Figure 3. Wind Turbine Dataset</p>
</div>

- Hardware: 20-tooth pinion gea / 2MW
- Sampling time: 6s per day
- Samplng period: 50 days
- Sampling frequency: 97656Hz
- Data number: 585,936 data of each 50 files

해당 데이터는 풍력 발전기 터빈의 진동정보와 회전속도정보[tachometer]로 구성되어 있으며, 각 데이터들은 하루에 6초씩 총 50일동안 수집되었습니다. 샘플링 주파수는 97656Hz로써 매일 총 585,936개의 데이터가 수집된 것을 확인할 수 있습니다.

### 2.2. Check Raw Data
진동 데이터의 양상을 그래프를 그려서 확인합니다.
```
% Reset hsbearing
reset(hsbearing);
tstart = 0;
% Create and center figure
figure('Units', 'normalized', 'Position', [0.3, 0.3, 0.4, 0.6]); % Center the figure
hold on;
while hasdata(hsbearing)
 data = read(hsbearing);
 v = data.vibration{1};
 t = tstart + (1:length(v)) / fs;
 plot(t(1:10:end), v(1:10:end));
 tstart = t(end);
end
hold off;
% Labeling and formatting the plot
xlabel('Time (s), 6 seconds per day, 50 days in total');
ylabel('Acceleration (g)');
title('Vibration Data Over Time');
grid on;
```

<div align="center">
  <img width="940" alt="vibration" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/vibration.jpg" /><br>
  <p style="margin-top: 10px;">Figure 4. Vibration Singal</p>
</div>

```
reset(hsbearing);
tstart = 0;
% Plot tachometer's dataset
figure;
hold on;
while hasdata(hsbearing)
 data = read(hsbearing);
 tach = data.tach{1};
 t = tstart + (1:length(tach))/fs;
 plot(t, tach);
 tstart = t(end);
end
hold off;
xlabel('Time (s), 6 seconds per day, 50 days in total');
ylabel('RPM');
title('Tachometer Data Over Time');
grid on;
```

<div align="center">
  <img width="940" alt="Tachometer" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/tach.jpg" /><br>
  <p style="margin-top: 10px;">Figure 5. Tachometer Singal</p>
</div>

해당 데이터셋은 50일안에 고장이 발생한 경우인데, 진동 신호를 살펴보면 시간이 지날 수록 그 값이 증가함을 확인할 수 있습니다. 하지만 tach 신호의 경우 그 양상이 일정하게 유지되고 있기 때문에 고장 진단을 위한 데이터로써 진동 신호를 채택하였습니다.

### 2.3. Data Processing
<div align="center">
  <img width="940" alt="extract" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/extract.jpg" /><br>
  <p style="margin-top: 10px;">Figure 6. Extract Detectivity from Vibration Signal</p>
</div>

Detectivity를 구하기 위해서는 기본적인 Hjorth의 파라미터들인 Activitiy, Mobility, 그리고 Complexitiy를 구해야 합니다. 다만 해당 값들은 신호의 변화 양상에 대한 정보를 담기 때문에 매일 6초동안 수집한 진동 신호들을 하나의 단위로 나누어서 각각 50개씩의 파라미터를 추출하였습니다. 그렇게 수집한 50개씩의 Activitiy, Mobility, Complexitiy들을 바탕으로 50개의 Detectivity를 다음과 같이 추출하였습니다.
```
% Update DataVariables to include detectivity
hsbearing.DataVariables = [hsbearing.DataVariables; "detectivity"];
hsbearing.SelectedVariables = "vibration";
reset(hsbearing);

% Total number of detectivity calculations
num_cycles = 50;

% Initialize an array to store 50 four parameter values
activities = zeros(1, num_cycles);
mobilities = zeros(1, num_cycles);
complexities = zeros(1, num_cycles);
detectivitiy = zeros(1, num_cycles);

% Repeat 50 times to extract detectivity
for cycle_index = 1:num_cycles
  if ~hasdata(hsbearing)
    reset(hsbearing); % If no more data, reset to read again
  end
 
  features = table;
 
  % Extract 50 numbers of activity, mobility, and complexity
  for j = 1:num_cycles
    % Calculate activity
    v = vibration_data(:,j);
    activities(j) = var(v);

    % Calculate mobility
    first_derivative = diff(v);
    mobilities(j) = sqrt(var(first_derivative) / activities(j));

    % Calculate complexity
    second_derivative = diff(first_derivative);
    complexities(j) = sqrt(var(second_derivative) / var(first_derivative))/sqrt(var(first_derivative) / var(v));
  end

  % Reference values
  A_ref = mean(activities);
  M_ref = mean(mobilities);
  C_ref = mean(complexities);

  % Calculate dB values of each three parameters
  activity_j_dB = 10 * log10(activities / A_ref);
  mobility_j_dB = 10 * log10(mobilities / M_ref);
  complexity_j_dB = 10 * log10(complexities / C_ref);
 
  % Calculate detectivity
  detectivity = activity_j_dB - mobility_j_dB + complexity_j_dB;
end

% Store all 50 detectivity values in the features table
features.detectivity = detectivity
```

<div align="center">
  <img width="940" alt="table" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/table.jpg" /><br>
  <p style="margin-top: 10px;">Figure 8. Detectivity Dataset</p>
</div>

그러고 나서 매트랩에서 제공하는 RUL 예측 평가 함수를 사용하기 위해 Detectivity 정보와 Date 정보를 다음과 같이 결합합니다.

```
% Write the derived features to the corresponding file in hsbearing
data = read(hsbearing);
writeToLastMemberRead(hsbearing, features);
% Select the "Date" variable from hsbearing
hsbearing.SelectedVariables = ["Date"];
reset(hsbearing);
% Gather the date data
dateTable = gather(tall(hsbearing));
% Create a table combining Date and detectivity
combinedTable = table(dateTable.Date, features.detectivity', 'VariableNames', 
{'Date', 'Detectivity'});
featureTable = table2timetable(combinedTable)
```

<div align="center">
  <img width="940" alt="timetable" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/timetable.jpg" /><br>
  <p style="margin-top: 10px;">Figure 9. Detectivity Timetable</p>
</div>

Detectivity의 그래프를 살펴보면 다소 위아래로 불안정하게 변동하는 모습을 보이지만 시간이 지날 수록 증가하는 양상을 보입니다. 급격한 변동을 줄이기 위해 smooth 필터를 적용합니다.

```
variableNames = featureTable.Properties.VariableNames;
featureTableSmooth = varfun(@(x) movmean(x, [5 0]), featureTable);
featureTableSmooth.Properties.VariableNames = variableNames;
figure
hold on
plot(featureTable.Date, featureTable.Detectivity)
plot(featureTableSmooth.Date, featureTableSmooth.Detectivity)
hold off
xlabel('Time')
ylabel('Feature Value')
legend('Before smoothing', 'After smoothing')
title('Detectivity') 
```

<div align="center">
  <img width="940" alt="smoothing" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/smoothing.jpg" /><br>
  <p style="margin-top: 10px;">Figure 10. Smoothing Detectivity</p>
</div>

### 2.4. Predict RUL(Remain Useful Life)
Detectivity를 Health Indicator(머신러닝 학습 데이터 또는 건강지표데이터)로 활용하기 위해 Exponential Degradation Mode(EDM, 지수열화모)을 활용하였습니다. 해당 모델은 매트랩에서 제공하는 방식을 차용하였습니다.

<div align="center">
  <img width="940" alt="EDM" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/EDM.jpg" /><br>
  <p style="margin-top: 10px;">Figure 11. Exponential Degradation Mode(EDM)</p>
</div>

\[
h(t) = \phi + \theta \exp\left( \beta t + \epsilon - \frac{\sigma^2}{2} \right)
\]

해당 모델을 학습하기 위하여 학습 데이터 셋을 구분하여 하고 모델을 정의합니다.
```
% Divide Detectivity dataset into train and test.
breaktime = datetime(2013, 3, 27);
breakpoint = find(featureTableSmooth.Date < breaktime, 1, 'last');
trainData = featureTableSmooth(1:breakpoint, :);

% healthIndicator of detectivity
healthIndicator = featureTableSmooth{:,:};
healthIndicator = healthIndicator - healthIndicator(1);
% Threshold for failure
threshold = healthIndicator(end);

% Fitting model
mdl = exponentialDegradationModel(...
 'Theta', 1, ...
 'ThetaVariance', 1e6, ...
 'Beta', 1, ...
 'BetaVariance', 1e6, ...
 'Phi', -1, ...
 'NoiseVariance', (0.1*threshold/(threshold + 1))^2, ...
 'SlopeDetectionLevel', 0.05);
```

그러고 난 다음에 RUL 예측을 진행합니다.
```
% Detectivity
% Keep records at each iteration
totalDay = length(healthIndicator) - 1;
estRULs = zeros(totalDay, 1);
trueRULs = zeros(totalDay, 1);
CIRULs = zeros(totalDay, 2);
pdfRULs = cell(totalDay, 1);

% Create figures and axes for plot updating
figure
ax1 = subplot(2, 1, 1);
ax2 = subplot(2, 1, 2);
for currentDay = 1:totalDay

  % Update model parameter posterior distribution
  update(mdl, [currentDay healthIndicator(currentDay,:)])

  % Predict Remaining Useful Life
  [estRUL, CIRUL, pdfRUL] = predictRUL(mdl, ...
                                        [currentDay healthIndicator(currentDay)], 
...
                                        threshold);
  trueRUL = totalDay - currentDay + 1;

  % Updating RUL distribution plot
  helperPlotTrend(ax1, currentDay, healthIndicator, mdl, threshold, timeUnit);
  helperPlotRUL(ax2, trueRUL, estRUL, CIRUL, pdfRUL, timeUnit)

  % Keep prediction results
  estRULs(currentDay) = estRUL;
  trueRULs(currentDay) = trueRUL;
  CIRULs(currentDay, :) = CIRUL;
  pdfRULs{currentDay} = pdfRUL;

  % Pause 0.1 seconds to make the animation visible
  pause(0.1)
end
```

<div align="center">
  <img width="940" alt="RUL" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/RUL.jpg" /><br>
  <p style="margin-top: 10px;">Figure 12. RUL Result from Detectivity</p>
</div>

### 2.5. Evaluate RUL
RUL 평가를 위해 매트랩에서 제공하는 α-λ plot 함수를 활용하였습니다. 해당 함수를 통해서 RUL 예측 결과의 정확도를 시작적으로 확인할 수 있습니다.

<div align="center">
  <img width="940" alt="alpha" src="https://github.com/YunKiNoh/IAIA-2024-2-Project1-Condition-Monitoring-of-Ball-Bearing-using-Detectivity/blob/main/image/alpha.jpg" /><br>
  <p style="margin-top: 10px;">Figure 13. α-λ Plot for RUL Evaluation</p>
</div>
