---
title: FEMS
category: Domain
order: 2
---

# 공장에너지관리시스템(FEMS)

## Reference 
- [EMS: Factory Energy Management System based on Production Information, MITSUYASU KACHI et al.](http://www.wseas.us/e-library/conferences/2013/Morioka/ISMA/ISMA-05.pdf)
- [MES/POP | 스마트팩토리 | (주)TIS (tisys.co.kr)](http://www.tisys.co.kr/sub2_1.html)
- [적정스마트팩토리포럼_코에버_.pdf (snu.ac.kr)](http://fab.snu.ac.kr/smart/%EC%A0%81%EC%A0%95%EC%8A%A4%EB%A7%88%ED%8A%B8%ED%8C%A9%ED%86%A0%EB%A6%AC%ED%8F%AC%EB%9F%BC_%EC%BD%94%EC%97%90%EB%B2%84_.pdf)


# 1. 개요

*FEMS란 생산 공정에 대한 에너지 정보 수집을 통하여 공장 설비의 에너지 소비 및 성능을 분석하고, 나아가 공장의 가동 및 운전 방법 개선, 설비 교체 등의 방법으로 에너지 소비를 최소화하는 시스템으로서 체계적이고 지속적인 에너지 관리 활동을 의미*
## 1.1. 공장 에너지 사용 영역
-  생산 시스템: 생산 장비를 사용하여 실제 생산이 이루어 지는 영역
-  유틸리티: 장 인프라의 일부로 기능하는 영역

## 1.2. 에너지 감소 방안 예시
- 장비 이용률 개선 -> downtime 및 대기시간 감소 -> 에너지 절감
- takt time 감소 -> 동일 수량 생산 시 장비 운영 시간 줄어 에너지 절감
- 생산 스케줄링

## 1.3. 참고.
- takt time: 한 제품을 만드는 데 걸리는 시간, 생산속도
- downtime: 시스템을 이용할 수 없는 시간
- Manufacturing Execution System (MES)
Factory Automation (FA)
Plan-Do-Check-Action (PDCA)
Programmable Logic Controller (PLC)

# 2.  FEMS 주요 기능
에너지관리 세분화 
- 원단위 분석을 통한 에너지 효율 분석 

설비별 에너지 검침 자동화 
- 지동원격 검침 시스템을 통한 관리적 효과 
- 기기별/공정별 세분화된 추적과 분석 

에너지 사용량의 분석
- 에너지 사용량의 통계분석 체계 구축 
- 공정별/설비별 에너지관리, 에너지 소비량 예측 

실시간 모니터링 
- 전력피크 사전대응 

에너지 통합관리 
- 에너지관리 상황을 총체적으로 제어 

장비운전 트렌드 분석 
- 효율적인 운전방법 제시 
- 장비 수명연장 및 비용절감 

에너지MAP 확인 
- 에너지 흐름의 가시화로 소비분야 관리 시스템의 명확화 
- 생산흐름과 에너지 흐름의 통합관리

# 3. 에너지 절감 관리 절차
1. Measurement: 생산 정보(예. 생산 수량)와 관련된 에너지 데이터 수집 -> PLC에 데이터 전달
- 계측 데이터: CC-Link로부터 전류, 전압, 전력, 전류 누수 등 수집
(CC-Link: Control & Communication Link)
- 생산 데이터: 생산 수량
- 질적 데이터: 에러 또는 breakdown의 빈도, 지속 시간, 종류, 복구시간

2. Visualization: MES 인터페이스에서 정보 시각화 -> 정보 시스템으로 데이터 전송

3. Reduction: 설비, 장치의 에너지 최적화, 고효율 장치 사용
4. Management: specific consumption(제품의 한 단위를 생산하기 위해 소비되는 에너지의 양)과 에너지 연관지어 모니터링
- 예. 설비 상태(운영, standby, breakdown) 별 누수 에너지 감지 -> standby 시간 설정 변경
- specific consumption은 line-by_line으로 관리

<center><img class="center" src="http://www.tisys.co.kr/images/sub/sub0201_img01.jpg" style="float:right" width=500px></center>


