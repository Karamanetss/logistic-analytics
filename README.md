# Trucking Operations — EDA

This is an exploratory data analysis of a trucking company's operational data. I went through it looking at three things: why deliveries run late, which routes actually make money, and which drivers cost the company the most.

I come from freight forwarding (worked as Speditionskaufmann in Hamburg), and I'm moving into data analytics. Picked this dataset because I already understand the domain, so instead of just running numbers, I could actually tell when a result made sense and when it didn't.

## The data

It's the [Logistics Operations Database](https://www.kaggle.com/datasets/yogape/logistics-operations-database) from Kaggle. 14 tables, around 85,000 trips, covering 2022–2024. The main ones I used were trips, loads, delivery_events, fuel_purchases, routes, safety_incidents and driver_monthly_metrics. The rest are in the notebook.

It's a synthetic dataset, which matters some variables turned out almost perfectly uniform, which you wouldn't see in real operations. I've flagged that where it came up.

## What I looked at

**Delivery delays.** Only 44.6% of deliveries arrive on time, so I wanted to know why. Tested about a dozen ideas routes, drivers, distance, time of day, season, booking type. Almost none of them explained much. The one thing that did stand out was the customer: on-time rates vary a lot depending on who you're delivering to. My read is that the real cause sits outside this dataset appointment windows, customer scheduling, that kind of thing.

**Route profitability.** Built a per-trip cost model (fuel, driver pay, maintenance) and rolled it up by route. Every route is technically profitable, but the spread is huge from $120 to $4,573 profit per trip. The reason is basically just distance. Long routes spread their fixed costs over more miles, so they earn way more per trip. The short city-to-city lanes barely break even, and once you add tolls and overhead they're probably losing money.

**Driver cost and risk.** Two angles here fuel and incidents. On fuel, there's nothing: every driver burns almost exactly the same per mile. On incidents, there's more going on, and the interesting part is that the drivers who _cost_ the most in damage aren't the same people as the drivers who are most often _at fault_. Those are two different problems and they need different fixes one's a training issue, the other's probably about which routes those drivers get assigned.

Each section in the notebook ends with a fuller writeup findings, what it means in practice, and where the analysis falls short.

## Running it

```bash
git clone https://github.com/Karamanetss/logistic-analytics
pip install -r requirements.txt
```

Download the CSVs from the Kaggle link and drop them in `/data/`, then open `notebooks/eda_trucking.ipynb`.

Built with Python and Pandas, charts in Matplotlib, written in VS Code.

## Notes

This is a learning project. The cost models are deliberately simple I left out depreciation, insurance, tolls and overhead because the data isn't there for them, so the absolute numbers run high. The comparisons between routes and between drivers still hold up though, and that's what the analysis is really about.
