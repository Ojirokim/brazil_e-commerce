# Olist Brazilian E-Commerce Dataset — Data Specifications (All Tables)

Source files included in this spec:
- `olist_customers_dataset.csv`
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_products_dataset.csv`
- `olist_sellers_dataset.csv`
- `olist_geolocation_dataset.csv`
- `product_category_name_translation.csv`

---

## Conventions
- **PK** = Primary Key, **FK** = Foreign Key
- Data types below reflect what appears in the CSVs (pandas dtypes from a sample read). Timestamps are stored as strings in CSV and should be parsed to datetime.
- ZIP code fields are **ZIP code prefixes (first 5 digits)**.

---

## customers

**File:** `olist_customers_dataset.csv`

**PK:** `customer_id`

**Notes:**
- `customer_unique_id` identifies the *real* customer across multiple orders; `customer_id` can repeat per unique customer (an order-level customer identifier).

| Column | CSV dtype | Description |
|---|---:|---|
| `customer_id` | object | Customer identifier used to join to `orders`. |
| `customer_unique_id` | object | Stable customer identifier (same person across multiple orders). |
| `customer_zip_code_prefix` | int64 | Customer ZIP code prefix (first 5 digits). |
| `customer_city` | object | Customer city (as provided). |
| `customer_state` | object | Customer state (UF). |

---

## orders

**File:** `olist_orders_dataset.csv`

**PK:** `order_id`

**FKs:**
- `customer_id` → `customers.customer_id`

**Notes:**
- Timestamps are strings in the CSV; parse to datetime for analysis.
- `order_status` commonly includes values like delivered, shipped, canceled, unavailable, etc.

| Column | CSV dtype | Description |
|---|---:|---|
| `order_id` | object | Order identifier. |
| `customer_id` | object | Customer identifier placing the order. |
| `order_status` | object | Order status in the lifecycle. |
| `order_purchase_timestamp` | object | When the order was placed. |
| `order_approved_at` | object | When payment was approved. |
| `order_delivered_carrier_date` | object | When the order was handed to the logistics carrier. |
| `order_delivered_customer_date` | object | When the customer received the order (delivery date). |
| `order_estimated_delivery_date` | object | Estimated delivery date promised to customer. |

---

## order_items

**File:** `olist_order_items_dataset.csv`

**PK:** `order_id`, `order_item_id`

**FKs:**
- `order_id` → `orders.order_id`
- `product_id` → `products.product_id`
- `seller_id` → `sellers.seller_id`

**Notes:**
- One order can have multiple items (multiple rows per `order_id`).
- `price` is the item price; `freight_value` is the shipping cost allocated to that item.

| Column | CSV dtype | Description |
|---|---:|---|
| `order_id` | object | Order identifier. |
| `order_item_id` | int64 | Item sequence number within the order. |
| `product_id` | object | Purchased product. |
| `seller_id` | object | Seller fulfilling this item. |
| `shipping_limit_date` | object | Seller shipping deadline date/time. |
| `price` | float64 | Item price. |
| `freight_value` | float64 | Freight/shipping value for this item. |

---

## order_payments

**File:** `olist_order_payments_dataset.csv`

**PK:** `order_id`, `payment_sequential`

**FKs:**
- `order_id` → `orders.order_id`

**Notes:**
- An order can have multiple payment records (e.g., split payments).

| Column | CSV dtype | Description |
|---|---:|---|
| `order_id` | object | Order identifier. |
| `payment_sequential` | int64 | Payment sequence within the order. |
| `payment_type` | object | Payment method (e.g., credit_card, boleto, voucher, debit_card). |
| `payment_installments` | int64 | Number of installments (if applicable). |
| `payment_value` | float64 | Payment amount for this record. |

---

## order_reviews

**File:** `olist_order_reviews_dataset.csv`

**PK:** `review_id`

**FKs:**
- `order_id` → `orders.order_id`

**Notes:**
- Usually there is at most one review per order, but treat it as 1:N unless you verify uniqueness on your data.
- Review text fields can be missing (null/empty).

| Column | CSV dtype | Description |
|---|---:|---|
| `review_id` | object | Review identifier. |
| `order_id` | object | Order being reviewed. |
| `review_score` | int64 | Rating score (1–5). |
| `review_comment_title` | object | Review title (optional). |
| `review_comment_message` | object | Review message (optional). |
| `review_creation_date` | object | When the review was created. |
| `review_answer_timestamp` | object | When the review was answered/responded (if applicable). |

---

## products

**File:** `olist_products_dataset.csv`

**PK:** `product_id`

**FKs:**
- `product_category_name` → `product_category_name_translation.product_category_name` (optional join)

**Notes:**
- `product_category_name` is Portuguese; join to `product_category_name_translation` for English.
- Some products may have missing category.

| Column | CSV dtype | Description |
|---|---:|---|
| `product_id` | object | Product identifier. |
| `product_category_name` | object | Product category name (Portuguese). |
| `product_name_lenght` | float64 | Length of product name (character count). |
| `product_description_lenght` | float64 | Length of product description (character count). |
| `product_photos_qty` | float64 | Number of product photos. |
| `product_weight_g` | int64 | Product weight (grams). |
| `product_length_cm` | int64 | Product length (cm). |
| `product_height_cm` | int64 | Product height (cm). |
| `product_width_cm` | int64 | Product width (cm). |

---

## sellers

**File:** `olist_sellers_dataset.csv`

**PK:** `seller_id`

**Notes:**
- Seller location can be joined to geolocation by ZIP prefix (not guaranteed 1:1).

| Column | CSV dtype | Description |
|---|---:|---|
| `seller_id` | object | Seller identifier. |
| `seller_zip_code_prefix` | int64 | Seller ZIP code prefix (first 5 digits). |
| `seller_city` | object | Seller city. |
| `seller_state` | object | Seller state (UF). |

---

## geolocation

**File:** `olist_geolocation_dataset.csv`

**Notes:**
- This table is not a strict dimension table: the same ZIP prefix can appear multiple times (multiple lat/lng rows).
- Common practice: aggregate to one coordinate per ZIP prefix (e.g., median lat/lng) before joining to customers/sellers.

| Column | CSV dtype | Description |
|---|---:|---|
| `geolocation_zip_code_prefix` | int64 | ZIP code prefix (first 5 digits). |
| `geolocation_lat` | float64 | Latitude. |
| `geolocation_lng` | float64 | Longitude. |
| `geolocation_city` | object | City (as recorded for the ZIP prefix row). |
| `geolocation_state` | object | State (UF). |

---

## product_category_name_translation

**File:** `product_category_name_translation.csv`

**PK:** `product_category_name`

**Notes:**
- Maps Portuguese category names to English.

| Column | CSV dtype | Description |
|---|---:|---|
| `product_category_name` | object | Category name in Portuguese (join key to `products.product_category_name`). |
| `product_category_name_english` | object | English translation of the category. |

---

