# Deep-Learning-Approaches-to-RUL-Prediction-of-Lithium-ion-Batteries

## NMC-LCO 18650 배터리 RUL 예측

####    0. Hawaii Natural Energy Institute에서 공개한 실제 NMC-LCO 18650 배터리 시험 데이터를 활용해서 데이터 분석을 진행함
    
####    1.배터리 RUL에 영향을 주는 여러 인자들 중 충방전 할때 변화하는 값들의 의미를 이해하였고, 실제 분석도 진행했음 </span>
    
    
####    2. 데이터의 이상치를 다루는데 있어서 통계적 방법을 적용함 </span>

    
    
#### 3. 실제 현업에서도 여러 가지 이유로 이상치가 나올 수 있음. 대표적인 것들이 노이즈, 센서 이상, 데이터 계측 시스템 이상 등이 있음
   - 이러한 상황에 대비하여 통계적 접근을 통해 데이터를 지우거나, 현업 지식을 사용하는 것이 중요하단 것을 알았음

#### 4. 또한, 데이터들간의 상관성 분석을 진행하였는데, 상관성이 높은 데이터들이 있었음. 이럴 때도 여러 가지 판단 요건에 의해 데이터를 가공함 (실제로 연관이 있는지, 데이터의 형태는 어떤지, 현업 지식으로 판단하였을 때 실제로 연관성이 서로 있는 데이터인지 등 종합적인 판단이 필요함)

   

## 결론

#### 변수 해석 관점
🤚 결론

**1. Discharge Time(초):**

- **현재 관찰:** 배터리 상태와 양의 상관관계가 있습니다. 즉, 방전 시간이 길어질수록 배터리 상태가 좋아집니다.
- **관리:** 배터리 사용 시 방전 속도를 늦출 수 있는 시스템을 설계하고 운영하기 위한 노력이 이루어져야 합니다. 여기에는 사용 패턴을 최적화하거나 방전율을 제어하는 기술 구현이 포함될 수 있습니다.

**2. Time constant current(초):**

- **전류 관찰:** 배터리 상태와 음의 상관관계가 있습니다. 즉, 정전류 충전 시간이 길어지면 배터리 상태에 부정적인 영향을 미칩니다.
- **관리:** 전략은 정전류로 배터리를 충전하는 데 소요되는 시간을 최소화하는 데 중점을 두어야 합니다. 여기에는 충전 알고리즘을 최적화하거나 이 단계의 지속 시간을 줄이는 고속 충전 기술을 채택하는 것이 포함될 수 있습니다.

**3. 고속 충전:**

- **현재 관찰:** 고속 충전은 더 많은 열을 발생시키며 배터리 성능과 수명에 영향을 미칠 수 있습니다.
- **관리:** 열 관리 시스템을 구현하는 것은 고속 충전 중에 안전한 온도를 유지하는 데 중요합니다. 이러한 시스템은 과열을 방지하기 위해 배터리 온도를 모니터링하고 필요에 따라 충전 속도를 조정해야 합니다.

#### 모델 성능 관점

  - 3가지의 모델 비교 해본 결과 다음과 같은 MAPE 수치가 기록되었습니다. <br>
    LSTM(window size를 지정): 99.63772567056276 <br>
    EMD-LSTM: 4.118588901373055 <br>
    EMD-CNN-LSTM: 5.727459644136884 <br>
    단순 LSTM을 적용한 방식은 성능이 거의 예측을 못하는 것 처럼 보였습니다. <br>
    하지만 EMD라는 시계열 분해 기법을 사용하니 모델의 성능이 개선되었습니다. <br>
    
 **EMD-LSTM이 EMD-CNN-LSTM보다 더 나은 성능을 발휘할 수 있다고 생각하는 몇 가지 이유**

    1. 데이터 특성: 데이터에 LSTM 레이어만으로 효과적으로 캡처할 수 있는 순차적 종속성과 패턴이 있는 경우 CNN 레이어를 추가해도 큰 이점을 제공하지 못할 수 있으며 잡음이나 중복성이 발생할 수 있습니다. LSTM 레이어는 순차 데이터에 매우 적합합니다. <br>
    2. 데이터 전처리: EMD(경험적 모드 분해)를 사용한 분해를 포함한 데이터 전처리는 모델 성능에 영향을 미칠 수 있습니다. EMD 분해로 인 해 LSTM이 활용할 수 있는 잘 분리되고 의미 있는 구성 요소가 생성되는 경우 추가 컨벌루션 계층이 필요하지 않을 수 있습니다. <br>
    3. 하이퍼파라미터 조정: 신경망 모델의 성능은 레이어 수, 레이어당 단위, 학습 속도 등과 같은 하이퍼파라미터에 민감합니다. 각 모델 아 키텍처에 대해 이러한 하이퍼파라미터를 적절하게 조정하는 것은 우수한 성능을 달성하는 데 중요합니다. <br>
    4. 과적합: CNN 레이어의 경우처럼 모델에 더 많은 레이어와 복잡성을 추가하면 과적합 위험이 높아질 수 있으며, 특히 훈련 데이터의 양  이 제한되어 있는 경우 더욱 그렇습니다. 순차 패턴을 캡처하는 기능을 갖춘 LSTM 모델은 과적합 가능성이 낮을 수 있습니다. <br>
    5. 작업 복잡성: 예측 작업의 복잡성도 중요한 역할을 합니다. 어떤 경우에는 단순한 모델로도 데이터의 기본 패턴을 포착하는 데 충분할   수 있지만, 더 복잡한 모델에서는 과적합이 발생할 수 있습니다.

