---
title: SCM & Logistics
category: Domain
order: 3
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---


## Reference 
[유통]
- [삼성SDS블로그](https://m.post.naver.com/viewer/postView.nhn?volumeNo=10237032&memberNo=36733075)
- [뉴스기사](https://byline.network/2019/05/11-58/)

[물류]
- [삼성SDS사이트](https://www.samsungsds.com/kr/insights/102817_L_whitepaper7.html)


# 1. 유통 용어 정리
프로젝트 관리는 일반적으로 **지정된 기간 내**에 특정 제품, 서비스 또는 결과를 생산하기 위해 수행되는 일련의 작업

1. **PPM(Project portfolio management)**: 조직의 모든 프로젝트를 높은 관점에서 관리/ PPM의 목적은 사업의 우선순위를 정하고, 자격 있고 가용한 직원(자원 관리)과 함께 현실적으로 기획하고, 이를 감시하고, 모든 관련 당사자에게 그 현황을 알리는 것<br/>
 - [ ] 프로젝트 포트폴리오 관리 태스크
전략적 조정: 프로젝트 선택 및 우선순위 지정  
리소스 관리: 인력 계획 및 인력 배치(역량,  일정, 예산 고려)
대략적인 마일스톤 계획: 프로젝트 시작, 계획, 구현 및 완료  
프로젝트 관리자 및 팀 교육 및 코칭  
PM 방법, 툴 및 기술 도입 및 유지관리  
프로젝트 포트폴리오 제어: 프로젝트 진행 상황 모니터링 및 평가  
프로젝트 지원: 프로젝트 팀을 위한 커뮤니케이션 및 지원  

2. **PMO(Project Management Office)**: PMO는 비즈니스 내 모든 프로젝트의 중심 허브로서 PPM을 전략적인 차원에서 주도
3. **Lead Time**: 상품의 주문일시와 인도일시 사이에 경과된 시간/ 생산 리드타임은 생산을 계획하여 공정에 착수하는 시점부터 제품이 완성되어 완제품 창고에 입고되는 시점까지의 기간을 의미
4. **RPA** (Robotic Process Automation): 사람이 수행하던 규칙 기반의 단순 반복적인 업무 프로세스를 소프트웨어를 적용하여 자동화하는 것
cf. RPA 적용을 위한 주요 프로세스 평가 요소  
– 수작업 여부  
– 반복 여부  
– 규칙 기반(Rule-Based) 업무  
– 업무 수행 건수  
– 예외 처리 비중
5. **Cargo Ready Date**: 화물 준비일, 화물이 공급업체 또는 기타 지정된 위치(창고 등)에서 제공될 것으로 예상되는 날<br/>
[기기, 자재에 대한 사전검증: TBE, CBE]
7. **TBE**(Technical Bid Evaluation): (견적서를 제출한 입찰자들에 한해) 기술적인 평가
8. **CBE**(Commercial Bid Evaluation): 견적가
9. **Procurement**: 직역하면 '조달'이지만 현업에서는 그 의미가 약간 다름
 - [ ] 모르는 용어
MR
PSR, ESR
VP issue



# 2. 분석 방향
1. 표준 리드 타임
2. 미래 예측 모델
3. Risk assessment
- 어떤 리스크를 의미하는지...

## 2.1. 참고. SDS SCM
[SDS]
**디지털 SCM 구현을 위한 핵심역량**
1. Intelligent Sensing: 데이터에 근거한 SCM 의사결정
- 영업, 마케팅(내부 데이터) + 판매채널과 고객(외부 데이터) ->시장전망과 마케팅 투자를 최적화
- 수요예측
2. Real Time Planning: 실시간 공급망 계획
- 자원탐색 하이브리드 알고리즘과 병렬처리 이용
-  프로세스를 동시에 여러 개의 시나리오를 사전 정의하고 최적의 시나리오를 선정
3. Autonomous Fulfillment: 실시간으로 들어오는 고객의 요구에 대응
- 예컨대 고객에게 신뢰할 수 있는 납기를 제시하고, 현시점 주문 처리 상황을 확인하고, 긴급 주문이나 변경에 빠른 응대
4. End-to-End Control Tower: 사용자의 업무와 다루는 정보의 특성에 맞춰 개인화, 시각화

# 3. 물류 용어&개념 정리

1. 물동량: 물자의 움직이는 양

2. 물류 네트워크 유형

3.1. Point-to-Point vs. Hub-and-Spoke
- Point-to-Point: 단일 거점에서 모든 수요를 대응<br/> 
단점: 납기 리드타임이 길뿐만 아니라 다수의 다양한 고객의 제품 Mix를 생산계획에 반영하기 어렵다.
but 공장에서 고객사로 바로 선적하는 경우 P2P 선호
-> P2P의 효율성 보완: H&S  방식
- Hub-and-Spoke:  배송 권역별로 나누어 각 권역별 Hub에서 해당 지역의 고객에게 서비스

3.2.  수평적 구조 vs. 수직적 구조 
문량과 주문 빈도를 기준으로 분류하여, 재고보유 부담을 최소화하는 방향으로 전략을 수립한다. 
- 수평적 구조: RDC(Regional Distribution Center - 고객사로 가는
-  수직적 구조: CDC(Central Distribution Center)-RDC-고객사

참고
- RDC(Region Distribution Center), 지역물류센터, 특정 지역에 제품을 공급하는 것을 목적으로 만들어진 DC
- CDC(Central Distribution Center),중앙물류센터
- 라우팅 최적화> 경로 최적화 정도로 생각하면 됨

3.3. 네트워크 최적화방법

3.3.1. Mathematical Modeling
흔히 말하는 최적안

3.3.2. Simulation Modeling
실제 비즈니스 환경을 수학공식에 다 적용할 수 없기에 다양한 현실적인 제약조건들을 반영하여 의사결정을 내릴 수 있는 방식
- step1. 현황분석 - 영업 및 마케팅 전략 이해, 프로세스 및 데이터 분석, 목표 물동량 산정
-> Aggregation: 물동량을 제품 Hierarchy 기준으로 분류하여 분석 후 취급특성에 따라 제품군별로 Grouping


- step2. Modeling 및 대안평가 - Modeling 제약조건 및 Simulation 기준 정의, 거점 기능, 후보 거점 및 평가방법 정의, 그리고 시나리오 작성 및 대안 비교 평가를 통한 최적안을 선정
-> Baseline Modeling을 통해 Simulation 방식의 적합성을 객관적으로 검증

- step3. 이행계획수립








