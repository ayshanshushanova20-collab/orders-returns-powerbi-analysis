# Orders & Returns Analysis Dashboard

An end-to-end analytics project: SQL data preparation → Power BI data model → DAX measures → a two-page interactive dashboard that shows **which products drive returns and how much profit is lost because of them**.

![Executive summary](screenshots/01_executive_summary.png)

## Business problem

Returns reduce revenue and profit, but a single overall return rate hides where the problem actually is. This project answers:

1. How many orders are returned, and how much revenue and profit do returns cost?
2. Which categories and products are responsible?
3. Why are customers returning them?
4. Where should the business act first?

## Data

| Table | Rows | Description |
|---|---|---|
| `orders` | 200 | order_id, order_date, product_id, quantity, status |
| `products` | 4 | product_id, product_name, category, unit_price, unit_cost |
| `returns` | 32 | return_id, order_id, return_date, reason |

- Orders period: 1–20 September 2026; returns recorded between 8 and 27 September 2026.
- Source data uses Azerbaijani labels. English equivalents used in this document: *Qulaqlıq* = Headphones, *Klaviatura* = Keyboard, *Siçan* = Mouse, *Monitor* = Monitor.
- Return reasons: *Səs gözləntisinə uyğun deyil* = sound does not meet expectations, *Rahat deyil* = uncomfortable, *Sifariş səhvi* = order error.

## Process

1. **SQL: loading.** Created the `products` and `orders` tables in SQL and loaded the data.
2. **SQL: cleaning.** Removed duplicate rows and rows with missing/blank values.
3. **SQL: view.** Created a view that combines the order and product data with a `LEFT JOIN`, so every order is kept and product attributes (name, category, price, cost) are attached to it.
4. **Export to Power BI.** Loaded the view into Power BI as `ORDERS_TABLE`.
5. **Returns data.** Added the returns file (Excel) to Power BI as the `returns` table and related it to `ORDERS_TABLE` on `order_id`.
6. **Measures table.** Created a separate `Hesablamalar` table that holds only DAX measures, keeping the data model clean.
7. **Visuals.** Built the Executive summary and Deep-Dive pages (described below).

### Measures

| Measure | Definition |
|---|---|
| Total Orders | Number of orders |
| Revenue | Σ quantity × unit price |
| Profit | Σ quantity × (unit price − unit cost) |
| Margin | Profit ÷ Revenue |
| Returns | Number of returned orders |
| Returns rate | Returns ÷ Total Orders |
| Returned Revenue | Revenue of returned orders |
| Profit Lost to Returns | Profit of returned orders |
| Net Revenue | Revenue − Returned Revenue |
| Net Profit | Profit − Profit Lost to Returns |

## Dashboard

### Page 1: Executive summary

- KPI cards: Total orders, Revenue, Profit, Returned Revenue, Profit Lost to Returns, Returns rate
- Category slicer that filters the whole page
- Returns rate by category (column chart)
- Returns by reason (donut chart)
- Margin by category (bar chart)
- Key insight sentence
- Navigation button to the Deep-Dive page

### Page 2: Deep-Dive

- KPI cards: Margin, Returns, Net Revenue, Net Profit
- Returns rate by category and product
- Profit lost to returns by category
- Profit by category
- Matrix of returns by category and reason
- Returns over time by return date
- **Product risk map**: scatter of return rate vs. margin per product (bubble size = returned revenue)
- Navigation button back to the Executive summary

![Deep-Dive](screenshots/02_deep_dive.png)

## Key findings

**Overall**

- 200 orders, 32 returned → **16% return rate**
- Revenue 27,600 and profit 9,200 (33.3% margin)
- Returns cost **3,160 in revenue (11.4%)** and **1,120 in profit (12.2%)**
- After returns: net revenue 24,440 and net profit 8,080

**Headphones are the problem category**

- 24 of the 32 returns (**75%**) come from one product, Echo Q1, with a **30% return rate**. All other products sit at 5–10%.
- Echo Q1 causes **720 of the 1,120 profit lost (64%)**.
- Its margin (37.5%) is above the company average, so the product looks profitable until returns are taken into account.
- Reasons for Echo Q1 returns: 12 *sound does not meet expectations* (50%), 8 *uncomfortable* (33%), 4 *order error* (17%). In other words, **20 of 24 returns are product-fit issues** (sound and comfort).

**Order errors affect every category**

- All 8 returns outside Headphones are *order errors*. Across the business, 12 of 32 returns (37.5%) are order errors, so there is also a process problem independent of any single product.

**Monitors carry the business**

- View M1 generates 58% of revenue and 52% of profit with only a 5% return rate.

## Recommendations

1. **Investigate Echo Q1 first.** Compare product descriptions and specifications with the sound complaints, and review ear-cup size/ergonomics for the comfort complaints. Illustrative impact: if the Echo Q1 return rate fell to 10%, about 16 returns (≈480 profit) would be avoided.
2. **Fix the order-error process.** Add order verification (e.g., confirmation of model/variant before shipping) since errors appear in all categories.
3. **Protect the Monitor line.** It is the main profit driver; keep its low return rate under monitoring.
4. **Track the return rate and profit lost by product monthly** using this dashboard to check whether the changes work.

## Limitations

- **Small, short sample:** 200 orders over 20 days and 32 returns. Findings show direction, not statistical proof.
- **Uniform patterns:** every order has quantity 1 and status *completed*, and every return happens exactly 7 days after the order, so a "days to return" analysis would not add information here. There is also one product per category, so category and product views overlap.
- Conclusions should be re-tested on a longer period and larger volume of data before acting on them in a real business.

## Tools

SQL · Power BI · DAX

## Author

**Ayshen Shushanova**  
[LinkedIn](https://www.linkedin.com/in/ayshan-shushanova) · [ayshan.shushanova@gmail.com](mailto:ayshan.shushanova@gmail.com)
