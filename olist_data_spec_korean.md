
# Olist 브라질 이커머스 데이터셋 — 데이터 명세 (전체 테이블)

## 포함된 소스 파일
- olist_customers_dataset.csv
- olist_orders_dataset.csv
- olist_order_items_dataset.csv
- olist_order_payments_dataset.csv
- olist_order_reviews_dataset.csv
- olist_products_dataset.csv
- olist_sellers_dataset.csv
- olist_geolocation_dataset.csv
- product_category_name_translation.csv

---

# 공통 규칙 (Conventions)

- **PK** = Primary Key (기본키)  
- **FK** = Foreign Key (외래키)  

- CSV 파일의 timestamp는 문자열 형태이므로 분석 시 **datetime으로 변환**해야 함
- ZIP code 필드는 **전체 우편번호가 아니라 앞 5자리 prefix**

---

# customers

**파일:** `olist_customers_dataset.csv`

**Primary Key**
- `customer_id`

### 설명
`customer_unique_id`는 실제 고객을 식별하는 ID이며  
`customer_id`는 주문 단위의 고객 ID라서 동일 고객이라도 여러 개 존재할 수 있음

| 컬럼 | dtype | 설명 |
|---|---|---|
| customer_id | object | 주문 테이블과 조인하는 고객 ID |
| customer_unique_id | object | 실제 고객 식별 ID |
| customer_zip_code_prefix | int64 | 고객 ZIP code prefix |
| customer_city | object | 고객 도시 |
| customer_state | object | 고객 주(state) |

---

# orders

**파일:** `olist_orders_dataset.csv`

**Primary Key**
- `order_id`

**Foreign Key**
- `customer_id → customers.customer_id`

| 컬럼 | dtype | 설명 |
|---|---|---|
| order_id | object | 주문 ID |
| customer_id | object | 주문한 고객 ID |
| order_status | object | 주문 상태 |
| order_purchase_timestamp | object | 주문 생성 시간 |
| order_approved_at | object | 결제 승인 시간 |
| order_delivered_carrier_date | object | 배송사 전달 시간 |
| order_delivered_customer_date | object | 고객 배송 완료 시간 |
| order_estimated_delivery_date | object | 예상 배송일 |

---

# order_items

**파일:** `olist_order_items_dataset.csv`

**Primary Key**
- order_id
- order_item_id

**Foreign Keys**
- order_id → orders.order_id
- product_id → products.product_id
- seller_id → sellers.seller_id

| 컬럼 | dtype | 설명 |
|---|---|---|
| order_id | object | 주문 ID |
| order_item_id | int64 | 주문 내 상품 순번 |
| product_id | object | 상품 ID |
| seller_id | object | 판매자 ID |
| shipping_limit_date | object | 판매자의 배송 마감 시간 |
| price | float64 | 상품 가격 |
| freight_value | float64 | 배송비 |

---

# order_payments

**파일:** `olist_order_payments_dataset.csv`

**Primary Key**
- order_id
- payment_sequential

**Foreign Key**
- order_id → orders.order_id

| 컬럼 | dtype | 설명 |
|---|---|---|
| order_id | object | 주문 ID |
| payment_sequential | int64 | 결제 순서 |
| payment_type | object | 결제 방식 |
| payment_installments | int64 | 할부 개월 |
| payment_value | float64 | 결제 금액 |

---

# order_reviews

**파일:** `olist_order_reviews_dataset.csv`

**Primary Key**
- review_id

**Foreign Key**
- order_id → orders.order_id

| 컬럼 | dtype | 설명 |
|---|---|---|
| review_id | object | 리뷰 ID |
| order_id | object | 주문 ID |
| review_score | int64 | 평점 (1~5) |
| review_comment_title | object | 리뷰 제목 |
| review_comment_message | object | 리뷰 내용 |
| review_creation_date | object | 리뷰 생성일 |
| review_answer_timestamp | object | 리뷰 응답 시간 |

---

# products

**파일:** `olist_products_dataset.csv`

**Primary Key**
- product_id

**Foreign Key**
- product_category_name → product_category_name_translation.product_category_name

| 컬럼 | dtype | 설명 |
|---|---|---|
| product_id | object | 상품 ID |
| product_category_name | object | 상품 카테고리 (포르투갈어) |
| product_name_lenght | float64 | 상품 이름 길이 |
| product_description_lenght | float64 | 상품 설명 길이 |
| product_photos_qty | float64 | 상품 사진 개수 |
| product_weight_g | int64 | 상품 무게 (g) |
| product_length_cm | int64 | 상품 길이 |
| product_height_cm | int64 | 상품 높이 |
| product_width_cm | int64 | 상품 너비 |

---

# sellers

**파일:** `olist_sellers_dataset.csv`

**Primary Key**
- seller_id

| 컬럼 | dtype | 설명 |
|---|---|---|
| seller_id | object | 판매자 ID |
| seller_zip_code_prefix | int64 | 판매자 ZIP prefix |
| seller_city | object | 판매자 도시 |
| seller_state | object | 판매자 주(state) |

---

# geolocation

**파일:** `olist_geolocation_dataset.csv`

설명:
- 동일 ZIP prefix가 여러 번 등장할 수 있음
- 일반적으로 ZIP prefix별로 **median lat/lng**를 계산하여 사용

| 컬럼 | dtype | 설명 |
|---|---|---|
| geolocation_zip_code_prefix | int64 | ZIP prefix |
| geolocation_lat | float64 | 위도 |
| geolocation_lng | float64 | 경도 |
| geolocation_city | object | 도시 |
| geolocation_state | object | 주(state) |

---

# product_category_name_translation

**파일:** `product_category_name_translation.csv`

**Primary Key**
- product_category_name

| 컬럼 | dtype | 설명 |
|---|---|---|
| product_category_name | object | 포르투갈어 카테고리 |
| product_category_name_english | object | 영어 카테고리 |
