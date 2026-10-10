# Airline Passenger Satisfaction Dashboard

A Power BI project exploring passenger satisfaction across cabin classes, travel purposes, customer types, and age groups. The interactive report summarizes 103,904 passenger survey records and lets users compare segments using Class and Gender slicers.

## Dashboard preview

![Airline Passenger Satisfaction Dashboard](dashboard.png)

Upload your final dashboard screenshot as `dashboard.png` in the same folder as this README to display the preview.

## Project objective

Build an interactive dashboard that makes passenger satisfaction patterns easy to explore. The report compares customer segments and summarizes arrival delays to support further investigation of the passenger experience.

## Tools and skills

- Microsoft Power BI Desktop: report development and visual formatting.
- DAX: passenger counts, satisfaction rate, average arrival delay, and age-group sorting.
- Interactive visualization: KPI cards, donut chart, column charts, bar charts, and slicers.
- Data validation: checking totals, segment results, filter behavior, and category order.

## Dashboard features

| Component | What it shows |
|---|---|
| Total Passengers card | Number of passenger survey records in the current filter context |
| Satisfaction Rate card | Percentage of records classified as satisfied |
| Average Arrival Delay card | Mean arrival delay in minutes |
| Satisfaction distribution donut | Satisfied versus neutral or dissatisfied passengers |
| Satisfaction by Class | Comparison of Business, Eco, and Eco Plus |
| Satisfaction by Type of Travel | Comparison of business and personal travel |
| Satisfaction by Customer Type | Satisfaction composition for loyal and disloyal customers |
| Satisfaction by Age Group | Satisfaction rates across five ordered age bands |
| Class and Gender slicers | Filter the cards and charts interactively |

## Main results

The following values apply with all filters cleared:

| Metric | Result |
|---|---:|
| Passenger survey records | 103,904 |
| Satisfied passengers | 45,025 |
| Neutral or dissatisfied passengers | 58,879 |
| Satisfaction rate | 43.33% |
| Average arrival delay in the supplied cleaned data | 15.13 minutes |

### Segment comparisons

| Segment | Satisfaction rate |
|---|---:|
| Business class | 69.43% |
| Eco Plus class | 24.61% |
| Eco class | 18.61% |
| Business travel | 58.26% |
| Personal travel | 10.17% |
| Loyal customers | 47.73% |
| Disloyal customers | 23.67% |

Business-class passengers and business travelers have higher satisfaction rates than their comparison groups in this dataset. Loyal customers also report higher satisfaction than disloyal customers. These are descriptive associations; they do not establish that class, travel purpose, or loyalty causes satisfaction.

## Repository files

| File | Purpose |
|---|---|
| `Airline Passenger Satisfaction Analysis and Prediction.pbix` | Completed Power BI report; the existing filename is retained |
| `airline_passenger_cleaned.csv` | Cleaned CSV used by the report, if redistribution is permitted |
| `dashboard.png` | Final dashboard screenshot for the preview |
| `README.md` | Project description and instructions |

Rename the uploaded `airline_passenger_cleaned(1).csv` to `airline_passenger_cleaned.csv` when adding it to the repository.

## How to open and explore the report

1. Download the `.pbix` file from this repository.
2. Open it in Microsoft Power BI Desktop.
3. Review the dashboard with filters cleared.
4. Select a Class or Gender option to explore a passenger segment.
5. Clear the slicer selection to return to the overall results.

The imported data is stored in the Power BI file. To refresh from the CSV, download the dataset and update the local CSV source path in Power Query before refreshing; the original author's local path will not necessarily exist on another computer.

## Core DAX calculations

The report uses the table name `airline_passenger_cleaned`.

```DAX
Total Passengers =
COUNTROWS('airline_passenger_cleaned')
```

```DAX
Satisfaction Rate =
AVERAGE('airline_passenger_cleaned'[satisfaction_numeric])
```

Here, `satisfaction_numeric` is 1 for satisfied and 0 for neutral or dissatisfied. Format this measure as a percentage.

```DAX
Average Arrival Delay =
AVERAGE('airline_passenger_cleaned'[Arrival Delay in Minutes])
```

The age-group sorting column is calculated from Age so it does not depend on the category it sorts:

```DAX
Age Group Order =
SWITCH(
    TRUE(),
    'airline_passenger_cleaned'[Age] <= 18, 1,
    'airline_passenger_cleaned'[Age] <= 30, 2,
    'airline_passenger_cleaned'[Age] <= 45, 3,
    'airline_passenger_cleaned'[Age] <= 60, 4,
    5
)
```

Set `Age Group` to sort by `Age Group Order`, then sort the visual by Age Group ascending.

## Data notes and limitations

- Each row represents a passenger survey record. Counts are row counts, not a deduplicated count of unique individuals.
- The satisfaction field combines neutral and dissatisfied responses into one category; the report cannot distinguish them.
- The source contains no date field, so the report does not analyze monthly or yearly trends.
- The cleaned file replaces 310 missing arrival-delay values from the raw training data with zero. The current report's 15.13-minute mean includes those values. Retaining the missing values as blanks produces an observed-value mean of approximately 15.18 minutes; that correction is not implemented in this report.
- The existing age labels are imperfect: `Under 18` includes ages up to and including 18, and `18-30` contains ages 19–30. More accurate labels would be `18 and under`, `19-30`, `31-45`, `46-60`, and `61+`.
- Although the Power BI filename includes “Prediction,” the completed report is descriptive and contains no predictive model or model evaluation.
- The original dataset source and license have not been verified in this project documentation. Add the source attribution and confirm redistribution permission before publishing the dataset or the report containing it.

## Future improvements

- Preserve raw arrival-delay missingness and show a data-quality indicator.
- Correct the age-band labels while retaining their numeric order.
- Add service-rating comparisons and delay-band analysis.
- Add a reset-filters button and improve visual accessibility.
- If prediction is implemented, document the training process and evaluate it using a separate held-out test set.

The additional preparation script, generated CSV files, theme, DAX file, and multi-page build guide supplied separately describe an expanded workflow. They are not required to open this completed single-page report and should be documented separately if included.
