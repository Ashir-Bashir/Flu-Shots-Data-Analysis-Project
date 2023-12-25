# Flu-Shots-Data-Analysis-Project

1.[Project Overview](Project-Overview)

2.[Objectives](Objective)

3.[Data Sources](Data-Sources)

4.[Analytical Tools](Analytical-Tools)

5.[Data Aggregation and Preparation](Data-Aggregation-and-Preparation)

6.[Exploratory Data Analysis](Exploratory-Data-Analysis)

7.[Results/Findings](Results/Findings)

8.[Recommendations](Recommendations)

10.[Limitations](Limitations)

11.[References](References)

### Project Overview
This comprehensive study aims to explore the intricacies of 2022 flu vaccination trends. In addition to the analytical insights, the study's objective extends to creating a user-friendly dashboard that encapsulates critical metrics and patterns.

![Flu shots dashboard](https://github.com/Ashir-Bashir/Flu-Shots-Data-Analysis-Project/assets/152665079/6d1034e3-a747-4585-96f5-62241e086449)


### Objective
Develop a flu shots dashboard that provides:

Percentage breakdown of patients receiving flu shots by:
Age
Race
County (geographical representation)
Overall percentage for 2022
Cumulative count of flu shots administered throughout 2022.
Total number of flu shots administered in 2022.
List of patients indicating their flu shot status.
Requirements:
Patients considered must be 'Active' at our 'Hospital'.

### Data Sources
The study relies on synthetic data generated via Synthea, ensuring privacy compliance while offering comprehensive data points.

### Analytical Tools
1. SQL: For data extraction, manipulation, and preliminary analysis.

2. Tableau: To create an intuitive and interactive dashboard.


### Data Cleaning/Aggregation/Preperation
In the intial data preperation stage, i performed the following tasks:

Segmentation of 'Active' patients.

Categorization based on vaccination status.

Data merging for comprehensive insights.

Data loading and Inspection

Handling missing values

Data cleaning and formating

Visual Checking for Data Quality

### Exploratory Data Analysis (EDA)
EDA involved exploring the Flu shots data to answer key questions, such as

1.	How does the distribution of flu vaccination rates vary across different age groups, races, and geographical locations (counties)?
   
2. What are the temporal trends in flu vaccination rates throughout 2022? Are there specific months or periods when vaccination rates peak or decline?
   
3.	Within the 'Active' patients at our hospital, which specific age range exhibits the highest proportion of individuals who have received flu shots? Are there discernible patterns that highlight a particular age group as more vaccinated than others?

### Data Analaysis
Includes interesting code/features used:
```sql
with active_patients as
(
	select distinct patient
	from encounters as e
	join patients as pat
	  on e.patient = pat.id
	where start between '2020-01-01 00:00' and '2022-12-31 23:59'
	  and pat.deathdate is null
	  and extract(month from age('2022-12-31',pat.birthdate)) >= 6
),

flu_shot_2022 as
(
select patient, min(date) as earliest_flu_shot_2022 
from immunizations
where code = '5302'
  and date between '2022-01-01 00:00' and '2022-12-31 23:59'
group by patient
)

select pat.birthdate
      ,pat.race
	  ,pat.county
	  ,pat.id
	  ,pat.first
	  ,pat.last
	  ,extract(YEAR FROM age('12-31-2022', birthdate)) as age
	  ,flu.earliest_flu_shot_2022
	  ,flu.patient
	  ,case when flu.patient is not null then 1 
	   else 0
	   end as flu_shot_2022
from patients as pat
left join flu_shot_2022 as flu
  on pat.id = flu.patient
where 1=1
  and pat.id in (select patient from active_patients)
```
### Results/Findings
The analysis and results are summarised as follows:

Distribution Across Demographics and Geographies:
Age-wise: Dukes County is notably lagging, with only a 25% vaccination rate among the 18-34 age group. In contrast, Nantucket County has no vaccinations for the 0-17 and 18-34 age brackets but a commendable 100% rate for the 35-49 and 50-64 age groups, with the 65+ group at 66.7%.
Race-wise: Dukes County reveals stark disparities, with a 100% flu shot rate among Asians but only 80% among the White community. Meanwhile, Nantucket displays a 100% rate for Black individuals and 85.7% for Whites.
Geographically: Leading the charge, Nantucket County boasts an 87% vaccination rate, closely followed by Norfolk County at 85.9%. Barnstable and Dukes Counties trail at 80% and 81%, respectively.

Temporal Trends in 2022:
Vaccination activities consistently peak on average across the counties in July, November, and December. These months consistently indicate heightened flu shot administrations, hinting at a pronounced seasonal pattern.

Age Dynamics within 'Active' Patients:
Within the 'Active' patient demographic, the 50-64 age bracket emerges as the most vaccinated group, particularly pronounced in counties like Nantucket. This underscores the potential efficacy of targeting this age cohort in vaccination initiatives. Conversely, a spotlight on the 18-34 age group in Dukes County is essential, given its notably lower vaccination rates.

Overall in essence, while some demographics and regions showcase commendable flu vaccination rates, there's a discernible need to address specific areas and age groups to elevate overall vaccination efficacy.
In this comprehensive analysis of flu vaccination trends across various counties, specific patterns and disparities emerge, shedding light on areas that require focused attention. By doing so, we can bridge the existing gaps, ensuring more comprehensive flu vaccination coverage across all demographics and regions.

### Recommedations 
Targeted Outreach: It's crucial to tailor our efforts towards communities or groups that have historically been underrepresented or have lower vaccination rates. By understanding their specific concerns, cultural nuances, and barriers to vaccination, we can design campaigns that resonate more effectively, ensuring these groups have equitable access and information.

Local Initiatives: Instead of adopting a one-size-fits-all approach, it's essential to identify regions or localities where vaccination rates are suboptimal. By focusing on these areas, we can devise specific interventions that address local challenges, be they logistical, informational, or cultural. This might involve organising local vaccination drives, collaborating with community leaders, or adjusting messaging to tackle specific concerns prevalent in these regions.

Continuous Monitoring: The healthcare landscape, particularly regarding vaccinations, is ever-changing. Therefore, it's vital to establish robust feedback mechanisms that facilitate ongoing evaluation and adaptation of our strategies. By consistently monitoring vaccination rates, gathering feedback from communities, and assessing the effectiveness of our interventions, we can adjust and refine our approaches as necessary, ensuring our efforts remain pertinent and effective.

### Limitations
This project provides valuable insights based on data from the year 2022. However, it's crucial to recognise its temporal limitations; the data represents a particular moment and may not reflect evolving patterns or changes in subsequent years. While synthetic data serves its purpose for modelling scenarios, it does introduce a level of detachment from genuine real-world dynamics, potentially impacting the project's relevance and conclusions. Additionally, external factors, such as public health initiatives or broader societal behaviours, could significantly shape vaccination trends. Understanding and acknowledging these external dynamics is essential for interpreting findings within a wider context and ensuring the project's insights retain their relevance and applicability.
Time-bound to 2022; subsequent years might exhibit different trends.
Data Authenticity: Considerations regarding synthetic data origins.
External Dynamics: Influence of external factors on vaccination trends.

### References
Synthea's Official Documentation.
CDC's Insights on Flu Vaccinations.
SQL and Tableau Mastery Resources.
