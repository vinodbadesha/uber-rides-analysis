# Uber Rides Analysis (2024)

Analysis of 2024 Uber booking data, covering ride completion and cancellation rates, demand patterns by hour and weekday, vehicle and payment preferences, ratings, and the relationship between price and ride distance.

## Dataset

The dataset contains all Uber rides booked in 2024, including completed, cancelled, and incomplete rides, with fields covering booking status, ride distance, booking value, vehicle type, payment method, cancellation reasons, and driver/customer ratings.

- **Source:** Kaggle — https://www.kaggle.com/code/mueezraja/ola-ride-booking-data-eda/input
- **Size:** 150,000 rows

| Statistic | Booking Value (₹) | Ride Distance (km) |
|---|---|---|
| Count | 102,000 | 102,000 |
| Mean | 508.30 | 24.64 |
| Std Dev | 395.81 | 14.00 |
| Min | 50.00 | 1.00 |
| 25% | 234.00 | 12.46 |
| 50% (Median) | 414.00 | 23.72 |
| 75% | 689.00 | 36.82 |
| Max | 4277.00 | 50.00 |

*Booking Value and Ride Distance are only populated for rides that progressed far enough to have fare/distance data (102,000 of 150,000 total rides) — cancelled and no-driver-found rides have no value here by design.*

No missing values were found outside of cancellation- and incomplete-reason columns, which are structural rather than missing data (e.g. a completed ride has no cancellation reason by definition) — these were retained rather than dropped or imputed. `Booking Status` categories were verified consistent via `value_counts()`, with totals reconciling exactly to 150,000 rows. `Booking Value` and `Ride Distance` were checked for outliers or invalid entries (negative/zero values) — none found. Stray quotation marks embedded in `Customer ID` (a CSV export artifact) were cleaned, and inconsistent category labels in `Vehicle Type` and `Payment Method` were standardized.

## Tech Stack

- Python (pandas, numpy)
- Matplotlib for visualization
- Jupyter Notebook
- Tableau for dashboarding (development only; not included in repo)

## How to Run

1. Clone the repo:
   ```bash
   git clone https://github.com/vinodbadesha/uber-rides-analysis.git
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `rides-data.ipynb` in Jupyter and run all cells.

## Key Insights

1. Only 62% of booked rides were completed. 7% failed due to "No Driver Found," 25% were cancelled (driver-side cancellations 2.5x higher than customer-side), and 6% remained incomplete.

   ![Booking status distribution](booking-status-distribution.png)

2. Drivers cancelled far more rides than customers (27,000 vs 10,500). Customer cancellation reasons were spread evenly (wrong address, driver unresponsive, change of plans), while driver reasons were vague and dominated by a single catch-all category ("Customer related issue").

   ![Cancelled rides distribution](canceled-rides-distribution.png)
   ![Driver cancellation reasons](driver-cancellation-reasons.png)
   ![Customer cancellation reasons](customer-cancellation-reasons.png)

3. Bookings peaked during evening hours (17:00–19:00), with the 18:00 hour alone recording 12,397 rides. Demand was consistent across all 7 days — no meaningful weekday vs. weekend variation.

   ![Demand by hour](demand-by-hour.png)
   ![Demand by weekday](demand-by-weekday.png)

4. Budget options dominate vehicle choice: Auto (24.9%), Go Mini (19.9%), and Go Sedan (18.1%) together account for ~63% of bookings. Premium vehicles make up only ~3%.

   ![Vehicle type distribution](vehicle-type-distribution.png)

5. UPI leads payment methods at 45%, followed by cash at 24.9% — together nearly 70% of all transactions.

   ![Payment methods distribution](payment-methods-distribution.png)

6. Customers rate drivers higher on average (4.4) than drivers rate customers (4.23), suggesting drivers may experience more friction during rides than is reflected in customer feedback.

   ![Driver vs customer ratings](driver-customer-ratings.png)

7. Booking value shows almost no correlation with ride distance (r = 0.01), consistent even after controlling for vehicle type and excluding incomplete rides. This suggests fares are driven more by zone, time, and surge pricing than by distance travelled.

## Recommendations

1. Improve driver availability during 17:00–19:00 through peak-hour incentives to close the demand-supply gap.
2. Reduce cash dependency by promoting UPI cashback offers or requiring drivers to maintain adequate change, reducing payment friction.
3. Audit the fare calculation model — the weak distance-price correlation suggests either pricing inconsistency or an opportunity for more transparent, distance-aware pricing.
4. Maintain strong Auto/Go Mini/Go Sedan fleet supply given consistently high demand for budget vehicle types.

## Limitations

- Dataset covers a single year (2024) — trends may not generalize to other periods
- "Customer related issue" from driver side as a dominant cancellation reason lacks granularity; root cause unclear
- Correlation analysis is limited to price vs. distance; other variables (e.g., time of day, surge multiplier) were not fully modeled
- No statistical significance testing was performed on differences (e.g., cancellation rate by vehicle type)

## Author
Vinod Badesha - https://www.linkedin.com/in/vinodbadesha/
