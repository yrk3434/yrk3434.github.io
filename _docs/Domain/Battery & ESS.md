---
title: Battery & ESS
category: Domain
order: 1
use_math: true
comments: true
---

# 리튬이온배터리와 에너지 저장장치

## eference
- 리튬이차전지의 원리 및 응용(박정기)
- [An Online SOC and SOH Estimation Model for Lithium-Ion Batteries(2017)](https://www.mdpi.com/1996-1073/10/4/512/htm)
- [Applications of Voltammetry in Lithium Ion Battery Research(2020)](https://www.jecst.org/journal/view.php?doi=10.33961/jecst.2019.00619)
- [Electrochemistry Data Hidden In Your Time Series Data: dQ/dV(2016)](https://blog.voltaiq.com/electrochemistry-data-hidden-in-your-time-series-data-dq/dv)


# 1. 리튬이온전지

## 1.1. 전지 구성과 전류, 전압
### 1.1.1. 전지의 구성
- 전지의 구성: 음극> 분리막&전해질 < 양극
- 이차전지는 산화, 환원 반응이 가역적이며 반복사용 가능
<center><img class="center" src="https://www.samsungsdi.co.kr/upload/editor/1516259713027.jpg" width=500px/></center>


### 1.1.2. 전류, 전압, 분극
- 전류: 단위시간당 전하의 이동량, 전극반응의 속도와 밀접(전류, 속도 간 식은 Nernst 식 참조)
- 전압: 전기 회로 두 지점 간 전위 차이, 자발적 산화, 환원 반응을 가지면 +, 비자발적이면 -
	- 방전 시 분극현상: 개방회로 전압(외부회로 접속안 된 장치의 전기적 전위차)보다 실제 전압이 낮아지는 원인
	- 분극현상: 전해질을 통해 전극과 전해질 사이에 전류가 흐르면 원래 전류방향과 반대방향으로 움직이는 기전력이 생겨 마치 저항처럼 작용해 전류 감소
- 분극: 전극전위 값이 평형상태에서 과하거나 부족한 현상, 평형전압보다 크거나(충전시) 작게(방전시) 나타남
	- 과전압=양 단자의 전압-평형전압

	[분극의 종류]: 실제는 세 종류가 혼재해 나타남
	- IR drop: 방전 초기에 매우 빠른 시간 내 나타남
	- 활성화 분극
	- 농도 분극
	
## 1.2. 전지의 특성
- **전지 용량(Capacity)**: 전지를 완전히 방전시켰을 때 얻을 수 잇는 전하량, 전류x시간
	- 이론 용량: $C_T=xF$
	- 실제 용량: $C_p$
	- 이론용량>실제용량(충방전 속도 증가>iR drop>용량 감소)
- **C rate**: 충방전 속도
	- $h=\frac{C_p}{i}=\frac{1}{C-rate}$
	-  $h$는 전지를 전히 방전 또는 충전하는 데 걸리는 간, $i$는 방전 전류(A), $C_p$는 전지용량(Ah)
- **출력(Power)**: 단위시간간 당 생산할 수 있는 에너지
	- $P(W)=i(A)E(V)$

[충전과 방전]
- 충전: 비자발젹 현상(전지내 방향 역방향)
- 방전: 자발적 현상
	
[추가 개념]

- 충방전 수명: 일정 용량 이상의 성능을 구현할 수 있는 전지의 충방전 횟수
- 비가역 용량: 충전된 전하량 중 방전동안 방전되지 못한 전하량, 초,중기 충방전 싸이클에서 큰 값
- low DOD에서 수명이 길다.
	- 참고. 방전 깊이(Depth of Discharge, DOD): SOC(State of Charge)의 또다른 표현
- **SOC**: State of Charge, 전지용량대비 충전 비율
- **SOH**: State of Health, 전지의 수명
- 참고. **온도와 Capacity**: 온도가 낮음> 리튬이온 이동 속도 느려짐> 수명과 관련은 없으나 Capacity 저하에 영향을 줌, 단, 저온으로 인해 Li plating 현상이 발생하면 간접적으로 전지 수명 감소에 영향을 줄 수는 있음.

## 1.3. 물성분석
- 임피던스

## 1.4. 충방전을 통한 수명 & 상태
### 1.4.1. 충방전 곡선
- 충방전 사이클이 반복됨에 따라 전지의 방전 특성이 달라짐
- 전압이 높을수록 출력이 좋고, capacity가 높을수록 오랜 시간 구동이 가능
- capacity와 방전 전압은 우하향 관계
- 전지가 노화되면 capa가 낮아지고, 전지 내부저항도 높아져 동일전류에서 셀 전압이 낮아짐
- 단, 방전 곡선은 전규, 출력, 인가 시간등에 따라 상이

<center><img class="center" src="https://www.mdpi.com/energies/energies-10-00512/article_deploy/html/images/energies-10-00512-g001.png" width=400px/></center>

### 1.4.2. 과충전
- 과충전: 리튬 이차전지의 경우 4.3V 이상이면 과충전
	- 과충전 > 온도 상승 > 음극이 수용 가능한 리튬보다 많은 양이 들어옴  > 잉여 리튬이 음극 표면에 금속 형태로 석출 > 양극 구조 붕괴 
	- 내부막이 녹으면 내부단락(shot) 가능 > 폭발 가능성
	- 주로 전지 자체가 아닌 오작동에 의해 발생(예. 정품이 아닌 충전기)
	
### 1.4.3. 방전속도 별 방전 특성
- 율속특성: C-rate과 관련, 용량/시간, 예. 1C는 한 시간 내 사용하는 용량, 2C는 30분, C/2는 2시간
- 전지 노화 >  전지 내 저항 증가 > 율속특성 하락


### 1.4.3. dQ/dV vs V 그래프

- [그림1] 충방전에 따른 dQ/dV vs V 그래프는 다음과 같다. 충전의 경우 y축 기준 양수 부분에 해당하고, 방전의 경우 y축 기준 음수 부분에 해당한다. 시계방향으로 충전 > 방전 한 사이클이 진행된 그래프라고 보면 된다.
- 이 그래프를 통해 확인할 수 있는 것은 degradation, failure mechanisms, changes in chemistry 등이다.
- [그림2] 동일전지에 대해서 피크점은 전지의 수명으로 간주할 수 있다. 충방전 사이클을 많이 진행할수록 붉은색 그래프처럼 피크점의 절대값이 작아지며, 피크점이 발생하는 전압값이 서서히 이동한다.
- [그림3] 상이한 전지에 대해서 피크점은 전지의 Capacity라고 볼 수 있다.

[그림1]
<center><img class="center" src="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=640&amp;name=dq-dv-vs-v-1.png" srcset="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=320&amp;name=dq-dv-vs-v-1.png 320w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=640&amp;name=dq-dv-vs-v-1.png 640w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=960&amp;name=dq-dv-vs-v-1.png 960w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=1280&amp;name=dq-dv-vs-v-1.png 1280w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-1.png?width=1600&amp;name=dq-dv-vs-v-1.png" width=500px/></center>
[그림2]
<center><img class="center" src="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=640&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png" srcset="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=320&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png 320w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=640&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png 640w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=960&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png 960w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=1280&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png 1280w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=1600&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png 1600w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v-cycles-100-200-300-400.png?width=1920&amp;name=dq-dv-vs-v-cycles-100-200-300-400.png" width=500px/></center>
[그림3]
<center><img class="center" src="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=640&amp;name=dq-dv-vs-v_1-1.png" srcset="https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=320&amp;name=dq-dv-vs-v_1-1.png 320w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=640&amp;name=dq-dv-vs-v_1-1.png 640w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=960&amp;name=dq-dv-vs-v_1-1.png 960w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=1280&amp;name=dq-dv-vs-v_1-1.png 1280w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=1600&amp;name=dq-dv-vs-v_1-1.png 1600w, https://blog.voltaiq.com/hs-fs/hubfs/dq-dv-vs-v_1-1.png?width=1920&amp;name=dq-dv-vs-v_1-1.png" width=500px/></center>



# 2. 에너지저장장치(Energe Storage System)

## 2.1. ESS 구성단위

<center><img class="center" src="https://cdn.buttercms.com/02O0WGJmSriXUzq7NvpE" width=500px/></center>

- Cell > Module > Rack > ESS
	- 앞서 설명한 리튬이온배터리가 셀에 해당하며, 셀 몇 개를 전기적으로 연결한 것이 모듈, 모듈 몇 개를 연결한 것이 랙에 해당한다.
	- 각 단위는 직렬로 연결될수도, 병렬로 연결될 수도 있다. 예컨대, 셀을 직렬로 연결해 모듈 팩을 만들고, 모듈 팩을 병렬로 연결하여 적층해 랙을 만든다.

## 2.2. ESS 제어 단위
<center><img class="center" src="https://cdn.buttercms.com/QLhtLHBnT6685CqKKwFq" width=700px/></center>

- **BMS:** Battery Management System, 배터리의 전압과 전류, 온도를 실시간으로 측정하여 과충전 또는 방전 등의 보호, 배터리 상태 데이터를 PCS, 운영 시스템에 제공
- **PCS:**  Power Conversion System, 전력 변환 장치 
	-  ESS에 저장되는 전기는 직류, 사용하는 전기는 교류 > 전력공급 시 or 전류 저장시 전력 변환
- **EMS:** Energy Management System, 실시간으로 배터리와 PCS의 상태를 모니터링 및 제어

