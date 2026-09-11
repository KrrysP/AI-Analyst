# AI Business Investigator --- Approved Schema

## Database

-   Engine: PostgreSQL
-   Hosting: Supabase
-   Dataset: Olist e-commerce
-   Primary analytical table: `olist_summary`

## Verified Grain

`olist_summary` contains:

-   99,441 rows
-   99,441 distinct `order_id`

Therefore:

**One row = one order**

## Approved KPI Definitions

### Orders

``` sql
COUNT(DISTINCT order_id)
```

### Revenue

``` sql
SUM(payment_value)
```

`payment_value` type: `double precision`

### Average Order Value

``` sql
SUM(payment_value) / NULLIF(COUNT(DISTINCT order_id), 0)
```

### Primary Analytical Date

`order_purchase_timestamp`

Type:

`timestamp with time zone`

## Approved `olist_summary` Columns

  Column                               PostgreSQL Type
  ------------------------------------ --------------------------
  order_id                             text
  customer_id                          text
  order_status                         text
  order_purchase_timestamp             timestamp with time zone
  order_approved_at                    text
  order_delivered_carrier_date         text
  order_delivered_customer_date        text
  order_estimated_delivery_date        timestamp with time zone
  dq_delivered_missing_approval        boolean
  dq_delivered_missing_carrier_date    boolean
  dq_delivered_missing_customer_date   boolean
  customer_unique_id                   text
  customer_zip_code_prefix             bigint
  customer_city                        text
  customer_state                       text
  item_count                           text
  distinct_products                    text
  distinct_sellers                     text
  product_value                        text
  freight_value                        text
  order_item_total                     text
  payment_records                      bigint
  payment_value                        double precision
  max_installments                     bigint
  payment_type_summary                 text
  latest_review_score                  text
  review_comment_title                 text
  review_comment_message               text
  review_creation_date                 text
  review_answer_timestamp              text
  has_review_comment                   text
  delivery_days                        text
  estimated_delivery_variance_days     text
  late_delivery_flag                   text
  repeat_customer_flag                 boolean
  customer_lat                         text
  customer_lng                         text
  geo_city_mode                        text
  geo_state_mode                       text
  geo_source_points                    text

## Other Available Source Tables

-   `customers`
-   `geolocation`
-   `order_items`
-   `order_payments`
-   `order_reviews`
-   `orders`
-   `product_category_translation`
-   `products`
-   `sellers`

The current demo should prefer `olist_summary` unless another approved
table is specifically required.

## Product Category Path

Product/category information exists through:

``` text
order_items.order_id
→ order_items.product_id
→ products.product_id
→ products.product_category_name_english
```

Orders may contain multiple categories.

A check found:

-   98,666 orders with category data
-   97,880 single-category orders
-   786 multi-category orders

Therefore category contribution analysis must not blindly assign one
category to every order.

## Important Analytical Warnings

1.  Never use `SELECT *`.
2.  Orders use distinct `order_id`.
3.  `olist_summary` is order-level.
4.  Preserve exact historical periods.
5.  Respect actual PostgreSQL types.
6.  Several analytical fields are stored as text; do not perform numeric
    operations without appropriate validated casting.
7.  Do not invent fields.
8.  Do not call customers churned without an explicit churn definition.
9.  `customer_state` represents the customer geography field used in the
    current demo.
10. Use read-only SQL only.

## Hotdata

The Olist analytical data is also available in Hotdata.

Current table:

`public.olist_summary`

Hotdata query endpoint:

``` text
POST https://api.hotdata.dev/v1/query
```

Typical request configuration:

``` json
{
  "async": false,
  "database_id": "<HOTDATA_DATABASE_ID>",
  "default_catalog": "default",
  "default_schema": "public",
  "dialect": "postgres",
  "sql": "<READ_ONLY_SQL>"
}
```

Do not commit Hotdata IDs/tokens if they are sensitive.
