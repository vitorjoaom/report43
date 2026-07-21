# Sleep Health & Lifestyle — Wellness Analytics Report

Interactive Power BI report analyzing the Sleep Health and Lifestyle dataset — sleep duration, sleep quality, stress, physical activity, blood pressure, BMI, and sleep disorders across respondents of varying occupations and age groups.

**File:** `report43.pbix`

## Data Cleaning

Several source issues were fixed at the model layer before any measure was built:

- **`Sleep Duration` was being imported as a whole number**, truncating values like `7.8` hours down to `7`. Fixed by re-typing the column as decimal with an explicit `en-US` culture in Power Query, so fractional hours are preserved.
- **`Blood Pressure` arrived as a single text field** (e.g. `"126/83"`) — split into two proper numeric columns, `Systolic` and `Diastolic`, so vitals can be aggregated and filtered independently.
- **`BMI Category` had a duplicate label**: "Normal" and "Normal Weight" represented the same category written two different ways — consolidated into one consistent value.
- **Duplicate respondents** (repeated `Person ID`) were removed.

## Data Model

```
Sleep_health_and_lifestyle_dataset   — fact table, one row per respondent
  Age Group                          — calculated column, banded age ranges
  Sleep Duration Category            — calculated column (e.g. Insufficient / Recommended)
  Stress Level Band                  — calculated column
  Activity Level Band                — calculated column
  Blood Pressure Category            — calculated column (Normal / Elevated / Hypertensive)
  Has Sleep Disorder                 — calculated column

_Measures                            — 35 DAX measures across 6 display folders
  01 Overview             — Total Respondents, Avg Sleep Duration, Avg Quality of Sleep,
                             % With Sleep Disorder, Distinct Occupations, Avg Age
  02 Sleep Quality         — Recommended/Insufficient Sleep %, Max/Min Sleep Duration,
                             Avg Quality (Recommended Sleepers), Sleep Duration-Quality
                             Correlation (Pearson correlation written in native DAX)
  03 Stress & Activity     — Avg Stress Level, Avg Physical Activity, High Stress %,
                             Meeting 10k Step Goal %, Avg Daily Steps (High Activity),
                             Stress-Quality Correlation
  04 Health Vitals         — Avg Heart Rate, Avg Systolic/Diastolic, Elevated or
                             Hypertensive %, Overweight or Obese %, Max Heart Rate
  05 Sleep Disorders       — Sleep Apnea/Insomnia/No Disorder Count, Avg Stress and
                             Avg Duration for the disorder group
  06 Demographics & Occupation — % Male/Female, Top Occupation, Distinct Age Groups,
                             Avg Quality of Sleep by gender
```

## Key Insights (confirmed via DAX)

- **41.4%** of respondents report some form of sleep disorder.
- **Sleep Duration × Quality of Sleep correlation: 0.88** — a strong positive relationship.
- **Stress Level × Quality of Sleep correlation: -0.90** — a strong negative relationship.
- **89%** of respondents have elevated or hypertensive blood pressure.
- Only **9.6%** meet the 10,000-steps-per-day activity goal.

## Report Pages

Six pages, each themed with a custom background and a matching 3D icon:

1. **Overview** — Total Respondents, Avg Sleep Duration, Avg Quality of Sleep, % With Sleep Disorder, Distinct Occupations, Avg Age
2. **Sleep Quality** — Sleep duration/quality distribution and the Sleep Duration-Quality correlation
3. **Stress & Activity** — Stress and physical activity levels, step-goal attainment, and the Stress-Quality correlation
4. **Health Vitals** — Heart rate, blood pressure breakdown, weight category
5. **Sleep Disorders** — Disorder type distribution and its relationship to stress and sleep duration
6. **Demographics & Occupation** — Gender split, top occupations, age group distribution
