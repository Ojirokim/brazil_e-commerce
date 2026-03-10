## Olist E-commerce 데이터 분석
고객 경험(Customer Experience)과 판매자 성과(Seller Performance) 분석

### 0. 팀 소개
| Name | Role                                           |
| ---- | ---------------------------------------------- |
| 김관후  | Seller Performance Analysis, Machine Learning |
| 김규열 | Customer Experience Analysis, Machine Learning         |
| 유승희 | Customer Experience Analysis, Data Preprocessing, EDA       |
| 유현규 | Customer Experience Analysis, Data Preprocessing, EDA       |
| 김하은 | Seller Performance Analysis, Data Preprocessing, EDA         |

### 1. 프로젝트 개요

본 프로젝트는 브라질 Olist 이커머스 데이터셋을 활용하여 고객 경험과 판매자 성과 간의 관계를 분석하는 것을 목표로 한다.

Olist는 판매자와 소비자를 연결하는 **마켓플레이스 플랫폼(B2B2C 구조)**이며, 이 환경에서는 판매자의 운영 역량(배송 성능, 가격 전략, 주문 처리 안정성 등)이 고객 경험과 판매 성과에 중요한 영향을 미친다.

본 연구는 단순히 매출이나 리뷰를 개별적으로 분석하는 것이 아니라,

고객 경험(Customer Experience)

판매자 성과(Seller Performance)

두 가지 관점을 동시에 분석하여 다음 질문에 답하고자 한다.

연구 질문

고객 리뷰는 어떤 요인에 의해 결정되는가?

어떤 판매자가 높은 성과(매출)를 달성하는가?

고객 경험과 판매자 성과는 어떤 구조로 연결되는가?

본 프로젝트는 일반적인 Top-down 방식이 아니라 EDA를 통해 데이터에서 문제를 발견하는 Bottom-up 방식으로 진행되었다.

프로젝트 전체 연구 보고서는 아래 문서를 기반으로 작성되었다.

### 2. 데이터셋

분석에는 Olist Brazilian E-commerce Dataset을 사용하였다.

### 3. 핵심 결과

분석 결과 고성과 판매자를 결정하는 핵심 요인은 다음과 같다.

- 낮은 배송 지연률
- 효율적인 배송비 구조
- 전략적인 가격 설정
- 안정적인 고객 리뷰
- 다양한 상품 카테고리

즉 판매자 성과는 단일 요인이 아니라

물류 안정성 + 가격 전략 + 상품 전략 + 운영 역량

이 결합된 결과로 나타난다.