# Manufacturing Performance Dashboard — Norway

A Power BI dashboard answering one question: **are we meeting production targets, and what is driving production losses?**

Built on a **synthetic** dataset modelled on a Norwegian manufacturing plant (4 lines, 8 machines, 5 products, Jan–Sep 2026). It does not represent a real company.

![Overview](PBI%20Images/Overview.jpg)

## The five stories

| # | Question | Page | Finding |
|---|----------|------|---------|
| 1 | Above or below target? | Overview | Plant output is 98.8% of target; Line 3 is at 94.0% and fell to about 85% in September |
| 2 | Which line, machine and shift cause the largest losses? | Production Losses | Line 3 causes 43% of shortfall units; its welders WLD-01 and WLD-02 miss target by about 6% |
| 3 | What explains downtime? | Downtime | 1,680 h in total; machine breakdown, changeover and material shortage lead; about 64% is unplanned |
| 4 | Where are scrap and quality problems? | Scrap & Quality | Scrap 2.37% vs a 2.00% target; Line 3 and both welders about 3.3% |
| 5 | What does it cost in NOK? | NOK Impact | About 48.9M NOK estimated loss (about 2.7% of target revenue) |

Plus a **Machine Detail** drill-through page and a **Summary & Notes** page.

![Production losses](PBI%20Images/Production%20Line%20Loss.jpg)

![NOK impact](PBI%20Images/NOK%20Impact.jpg)

## Data model

Star schema: `Dim_Calendar` (date table), `Dim_Line`, `Dim_Machine`, `Dim_Shift`, `Dim_Product` → `Fact_Production`, `Fact_Downtime`, `Fact_Quality`, `Fact_Cost` (single-direction, many-to-one). All measures sit in `_Measures`, grouped in display folders.

![Model view](PBI%20Images/Model%20View.jpg)

## Skills demonstrated

- **Power Query:** typed columns, merges, custom columns (planned vs unplanned downtime, unit margin, total cost)
- **DAX:** `VAR`, `CALCULATE`, `DIVIDE`, `SUMX` / `RELATED`, `TOPN`, `DATEADD`, `TOTALYTD`, `DATESINPERIOD`, `REMOVEFILTERS`
- **Report design:** synced slicers, drill-through, calculated headline text, consistent layout

## NOK impact assumptions

- Lost margin = units below target × (sales price − standard cost)
- Scrap cost = scrap units × standard cost
- Rework cost = rework units × standard cost × 25% (editable measure)
- Idle cost of downtime = downtime hours × (labour + overhead per scheduled hour, 8 h shifts). Shown separately because it overlaps the shortfall
- Total estimated loss = lost margin + scrap + rework

These are estimates built on stated assumptions, not accounting figures.

## How to open

**Quickest:** download the `.pbix` from the `Project Manufacturing Dashboard` folder and open it in Power BI Desktop. The data is embedded in the model.

**Developer version:** the `.pbip` project (text-based report and model definitions, easy to diff) is in the same folder. Open `Manufacturing_Dashboard.pbip` in Power BI Desktop and click **Home → Refresh**.

## Repository layout

```
Project Manufacturing Dashboard/   .pbix, semantic model (TMDL) and report definition
PBI Images/                        dashboard screenshots
data/                              original Excel dataset
README.md
```
