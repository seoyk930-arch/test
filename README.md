# PWM (Pulse Width Modulation)

## 1. 개요
PWM은 펄스의 폭을 조절하여 평균 전압을 제어하는 방식 (디지털 신호를 on, off 반복하여 analog 신호로 만드는 것)

## 2. 주요 변수
- 주파수
- 주기
- 듀티비

## 3. 듀티비
듀티비는 전체 주기에서 HIGH 상태가 차지하는 비율입니다.

```text
Duty Cycle = HIGH 시간 / 전체 주기 × 100
```

| 듀티비 | 출력 특성 |
|---|---|
| 25% | 평균 출력이 낮음 |
| 50% | 평균 출력이 중간 |
| 75% | 평균 출력이 높음 |

mosfet을 on/off하는 스위치로 사용 
| on | off |
|---|---|
| sturated state(fully on) | off: cutoff state(fully off) |

transistor stays at either one or the other extreme states

<img width="303" height="325" alt="image" src="https://github.com/user-attachments/assets/1b378b43-b38c-40b9-843e-99a43506d9bf" />

square wave -> LPF -> dc
```text
DC = t_on/period * amp
```
duty cycle이 변화하면서 dc output이 변화하게 된다.

class D 증폭기는 효율이 높으면서도 아날로그 신호를 선형적으로 증폭할 수 있었다.
### 1. 트랜지스터를 선형 영역에서 사용하지 않는다.
class A 나 AB는 트랜지스터가 중간 정도로 켜진 상태에서도 동작한다.
트랜지스터에는 동시에 전압 V_ds와 I_d 가 모드 인가되므로 P_loss = V_ds I_d가 커진다.
반면 class D에서는 트랜지스터를 거의 두 상태로만 사용한다. 
- on: 전류는 많이 흐르지만 트랜지스터 양단 전압은 거의 0
- off: 전압은 많이 걸리지만 전류는 거의 0

따라서 두 상태 모두 V_ds I_d = 0 이 되어 손실이 작고 효율이 높다.

### 2. on/off 만으로 사인파를 만드는 법
입력 신호를 그대로 트랜지스터에 넣는 것이 아니라 pwm신호로 변환한다.
```text
v_in(t) = V_m sin(wt)
```
class D 내부에서는 사인파의 크기에 따라 펄스 폭(Duty ratio)를 바꾼다. 
```text
D(t) = 1/2 + vin(t)/2Vmax
```
### 3. Low-pass filter가 원래 신호를 복구한다.
1. 우리가 원하는 낮은 주파수 성분의 입력 신호
2. 매우 높은 스위칭 주파수 성분

여기서 높은 스위칭 주파수를 제거하고 낮은 입력 신호만 통과시킨다.
=> 저역통과 필터가 펄스 신호의 **평균값**을 추출한다고 볼 수 있다.

### 4. nice and linear
트랜지스터 자체는 on/off로 비선형적이지만 duty ratio와 입력신호의 관계가 선형이고 필터 뒤의 평균 출력도
그에 비례하므로
```text
vout(t) = A_v vin(t)
```
로 선형 증폭기처럼 보인다.

### 5. wide bandwidth
입력 신호의 대역폭보다 스위칭 주파수가 충분히 높으면 여러 주파수의 입력 신호를 복원할 수 있다.

