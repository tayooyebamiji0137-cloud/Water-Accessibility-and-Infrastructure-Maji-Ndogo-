# Water-Accessibility-and-Infrastructure-Maji-Ndogo-
## Table of Content

* [Project Overveiw](#project-overveiw)
* [Data Source](#data-source)
* [Tools](#tools)
* [Data Overview](#data-overview)
* [Data Cleaning](#data-cleaning)
* [Exploratory Data Analysis](#exploratory-data-analysis)
* [Data Analysis](#data-analysis)
* [Insight](#insight)
* [Recommendation](#recommendation)
### Project Overveiw

Investigate access to water sources, queue times, contamination risks and where infrastructure improvements would benefit the most people

### Data Source
md_water_services data. The primary dataset use for this analysis is the md_water_services.sql file, containing detailed information about water service in maji-ndogo.
> [Download here](https://github.com/tayooyebamiji0137-cloud/Water-Accessibility-and-Infrastructure-Maji-Ndogo-/blob/main/md_water_services.sql)


### Tools
* SQL Server - Data analysis
* Excel - Pivot table and visualization


### Data Overview
Data dictionary (docs/data_dictionary.md)
---
Dictionary listing the main tables and important fields, e.g.:

* location — location_id, province_name, town_name, location_type (rural/urban/tap/well/shared)

* water_source — source_id, type_of_water_source, number_of_people_served, town_name, location_id

* visits — record_id, time_of_record, time_in_queue, assigned_employee_id, source_id

* well_pollution — record_id, biological, chemical, results, description

* water_quality — source_id, subjective_quality_score, visit_count

* employee — assigned_employee_id, employee_name, phone_number, email, town_name


### Data Cleaning 

In the initial data preparation phase, i performed the following tasks;
1. Data loading and inspection.
2. Data cleaning and formatting.
3. Fix well_pollution description: replace Clean Bacteria: E. coli with Bacteria: E. coli and similar for Giardia Lamblia.
4. Update results from Clean to Contaminated: Biological for rows where biological > 0.01.
5. Trim whitespace and standardize phone_number values (SQL TRIM() and formatting regexes).
6. Export a well_pollution_copy as an immutable snapshot before making destructive updates.


### Exploratory Data Analysis  
* I check for populaton survey
* How I rank sources: aggregate sum(number_of_people_served) by type_of_water_source and by source_id, then RANK() over descending population.
* Which sources to improve first: exclude tap_in_home from prioritization (internal decision) and rank communal/shared sources.
* Queue analysis: compute average time_in_queue (ignoring zero values) and analyze by dayname(time_of_record) and by hour of the day.


### Data Analysis
In the SQL scripts include some interesting code such as
>  What is the average total queue time for water?
```sql
SELECT avg(nullif(time_in_queue,0)) as avg_survey_time
FROM md_water_services.visits;
```

### Insight
The analysis result are summarized as follow:
 1. Most water sources are rural (roughly 60% of sources).
 2. 43% of our people are using shared taps. 2000 people often share one tap.
 3. 31% of our population has water infrastructure in their homes, but within that group, 45% face non-functional systems due to issues with pipes,
 pumps, and reservoirs.
 4. 18% of our people are using wells of which, but within that, only 28% are clean..
 5. Our citizens often face long wait times for water, averaging more than 120 minutes.
 6. In terms of queues:- Queues are very long on Saturdays.- Queues are longer in the mornings and evenings.- Wednesdays and Sundays have the shortest queues.
### Recommendation 
Based on the analysis,we recommend the follow actions;
 1. We want to focus our efforts on improving the water sources that affect the most people.
    - Most people will benefit if we improve the shared taps firs.
    - Wells are a good source of water, but many are contaminated.Fixing this will benefit a lot of people.
    - Fixing existing infrastructure will help many people. If they have running water again, they won't have to queue, thereby shorting queue times for
      others. So we can solve two problems at once.
    - Installing taps in homes will stretch our resources too thin, so for now, if the queue times are low, we won't improve that source.
 2. Most water sources are in rural areas. We need to ensure our teams know this as this means they will have to make these repairs/upgrades in
 rural areas where road conditions, supplies, and labour are harder challenges to overcome.
















