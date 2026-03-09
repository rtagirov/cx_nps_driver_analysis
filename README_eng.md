# NPS Analysis and Customer Experience Drivers

## Project Overview

The goal of this project is to analyze Net Promoter Score (NPS) based on customer survey data and identify the key factors influencing the metric.

The project includes calculation of the overall NPS, analysis of its dynamics over time, and decomposition of the metric across different data dimensions. The main objective is to identify customer segments and products that generate the largest negative contribution to the final metric.

The analysis follows a top-down driver analysis approach. Starting from the overall NPS level, the analysis explores different data dimensions, including interaction channel, product, customer demographics, and time dynamics. This approach allows the localization of the sources of customer dissatisfaction and helps identify segments that require deeper investigation.

Special attention is given to the distinction between:

- structural drivers — characteristics of the customer base that cannot be directly changed
- actionable drivers — product or service factors that can be influenced through product decisions

---

## Data

The analysis uses customer survey results represented in two forms.

### 1. Source Table

At the source level, each row contains the distribution of responses for a specific combination of attributes and score value.

Source table fields:

- `Month` — survey month  
- `age_group` — customer age group  
- `gender_id` — customer gender  
- `survey_channel` — interaction channel  
- `survey_product_name` — product  
- `NPS_score` — customer score on a 0–10 scale  
- `Num_answers` — number of responses with this score  

This table therefore contains the distribution of NPS scores within each segment.

---

### 2. Aggregated Data Mart

An aggregated dataset is built from the source table, where data is summarized at the segment level.

Aggregated data mart fields:

- `Month` — survey month  
- `survey_product_name` — product  
- `survey_channel` — interaction channel  
- `age_group` — customer age group  
- `gender_id` — customer gender  
- `total_answers` — total number of responses in the segment  
- `promoters` — number of ratings 9–10  
- `passives` — number of ratings 7–8  
- `detractors` — number of ratings 0–6  
- `promoters_share` — share of promoters in the segment  
- `detractors_share` — share of detractors in the segment  
- `NPS` — calculated NPS value for the segment  

This data mart is used for segmentation, decomposition, and contribution analysis.

---

## NPS Calculation

Two approaches are used for calculating the metric.

### 1. NPS Calculation from the Source Table

At the source data level, the metric is calculated as a weighted average score, where the weights correspond to the number of responses (`Num_answers`) for each score.

In practice, this means that the overall NPS is calculated as the average customer score, taking into account the number of responses for each score value.

This approach is used to obtain the baseline value of the metric and validate the overall NPS trend.

---

### 2. NPS Calculation from the Aggregated Data Mart

In the aggregated data mart, responses are already grouped by segment, and the following values are calculated separately:

- `promoters` — scores 9–10  
- `passives` — scores 7–8  
- `detractors` — scores 0–6  

The NPS value is then calculated using the standard formula:

NPS = (Promoters - Detractors) x 100 / Total

where:

- `Promoters` — number of ratings 9–10  
- `Detractors` — number of ratings 0–6  
- `Total` — total number of responses  

This representation is used for segment-level analysis and decomposition of metric contributions.

---

## Analytical Approach

The analysis is performed in several steps:

1. Calculation of the baseline NPS metric  
2. Analysis of monthly NPS dynamics  
3. Hypothesis testing across key dimensions:
   - interaction channel
   - product
   - age group
   - gender
   - month
4. Decomposition of segment contributions to overall NPS  
5. Identification of segments with the largest negative contribution  
6. Drill-down analysis of the key segment  
7. Conclusions and recommendations  

This step-by-step approach makes it possible to narrow the analysis space and isolate the true drivers of metric change.

---

## Key Findings

The analysis shows that the main NPS decline is associated with the VoiceBot channel, where the largest negative contribution comes from Product_2.

The decomposition revealed the following key insights:

- Product_2 is the main driver of NPS decline within the VoiceBot channel
- at the global level, gender is not a primary driver of the metric, as male and female contributions are roughly symmetric
- however, within the VoiceBot + Product_2 segment, a clear asymmetry appears: male users generate the majority of the negative contribution
- the most problematic age groups are 29–39 and 39–49
- the negative contribution persists across several months, which suggests a systemic issue rather than a one-time incident

Overall, the main root cause candidate is the interaction between VoiceBot and Product_2, especially for male users aged 29–49.

---

## Business Insight

The results indicate that the issue is not caused by VoiceBot alone, but by the combination:

VoiceBot × Product_2 × Male 29–49

which accounts for about 25% of all negative feedback and about 8 NPS points. This makes the identified segment a priority target for further analysis and product improvement.
