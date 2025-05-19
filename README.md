# Overview
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data.
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data preparation and cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.
## Import and cleanup data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter Germany jobs
To focus my analysis on the German job market, I apply filters to the dataset, narrowing down to roles based in Germany.
```python
df_DE = df[df['job_country'] == 'Germany']
```

# The Analysis
 Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?
To find the most demanded skills for the top 3 most popular data roles I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [Skills_Count.ipynb](Advanced%20Chapter/Project/2.%20Skills_Count.ipynb)

### Visualize data
```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Results
![Visualization of Top Skills for Data Nerds](Advanced%20Chapter/Project/images/Skill_demand_all_data_roles.png)

### Insights:
#### 1. SQL and Python Dominate Across All Roles
These two skills are essential for Data Analysts, Data Engineers, and Data Scientists in Germany, with particularly high demand for Python among Data Scientists (62%) and for SQL among Data Analysts (41%) and Engineers (47%).

#### 2. Role-Specific Technologies Reflect Different Focuses

- Data Analysts lean on BI tools like Tableau, Power BI, and Excel.

- Data Engineers need cloud and infrastructure skills — Azure, AWS, and Spark feature strongly.

- Data Scientists show more demand for R (27%) alongside Python and cloud tools, hinting at a statistical/machine learning focus.

#### 3. Cloud Skills Matter More for Engineers and Scientists
Azure and AWS are much more frequently requested for Data Engineers and Scientists than for Analysts, emphasizing the cloud-first direction of advanced data infrastructure and modeling roles.

## 2. How are in-demand skills trending for Data Analysts?
To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [Skills_Trend.ipynb](Advanced%20Chapter/Project/3.%20Skills_Trend.ipynb).

### Visualize data
```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_DE_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Results
![Trending Top Skills for Data Analysts in Germany](Advanced%20Chapter/Project/images/Skill_trend.png)

### Insights
#### 1. SQL Remains the Dominant Skill Throughout 2023
SQL is consistently the most in-demand skill for Data Analysts across all months.
Even during periods of decline (e.g., September), it maintains a higher likelihood than all other skills.

#### 2. Python Shows Strong Seasonality and Volatility
- Python's demand trends upward mid-year (May–July) but sees a sharp drop in September.

- It recovers by December but doesn’t reach its earlier peaks.

- This suggests fluctuating industry needs or project cycles involving Python.

#### 3. Visualization Tools (Tableau & Power BI) Decline Over Time
- Both Tableau and Power BI start the year with relatively high demand but steadily decrease throughout.

- By the end of the year, they fall below 20%, indicating a possible shift in employer preference or reporting tools being less emphasized.

## 3. What are the highest paid & most demanded skills for Data Analysts?
To identify the highest-paying roles and skills, I only got jobs in Germany and looked at their median salary. Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

View my notebook with detailed steps here: [Salary_Analysis.ipynb](Advanced%20Chapter/Project/4.%20Salary_Analysis.ipynb)

### Visualize data
```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```
### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in Germany:
![Highest paid skills and most in-demand skills for data analysts in Germany](Advanced%20Chapter/Project/images/Salary_Analysis.png)

### Insights
#### High-Paying Skills Are Less In-Demand
Skills like GitHub, Terraform, BigQuery, and Redshift offer the highest median salaries for data analysts in Germany (around $150K–$200K), but they are not among the most in-demand skills. Conversely, widely demanded skills like Python, Spark, and Pandas have lower median salaries (around $100K–$125K), indicating a trade-off between salary and demand.
#### Core Data Skills Dominate Demand
Python, Spark, Pandas, Excel, and SQL are the most in-demand skills for data analysts in Germany, suggesting that foundational data manipulation and analysis tools are prioritized by employers. These skills, however, offer moderate salaries compared to niche, specialized skills like GitHub or Terraform.

## 4. What are the most optimal skills to learn for Data Analysts?
To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [Optimal_Skills.ipynb](Advanced%20Chapter/Project/5.%20Optimal_Skills.ipynb)

### Visualize data
```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
### Results
![Optimal Skills for Data Analysts](Advanced%20Chapter/Project/images/Uncolored_skills.png)

### Insights
#### Balancing Salary and Demand
Skills like SQL, Python, and Tableau are optimal for data analysts in Germany, appearing in 30–50% of job postings with median salaries around $100K–$115K. These skills strike a balance between high demand and decent pay, making them essential for most roles.
#### High Salary, Low Demand Niche
GitHub and GCP offer the highest salaries (around $160K–$200K) but are required in fewer than 10% of data analyst jobs. These skills cater to a niche market, ideal for those prioritizing salary over job availability.
#### Low Demand, Low Salary Skills 
Skills like Go, Looker, R, and Power BI are less optimal, with both low demand (under 20% of jobs) and lower salaries (around $60K–$80K). These may be supplementary skills rather than primary ones for data analysts in Germany.

## Visualizing different technologies
Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python}).
### Visualize data
```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```
### Results
![Optimal skills colorized](Advanced%20Chapter/Project/images/Optimal_Skills.png)

### Insights
- The scatter plot shows that most of the programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

- Cloud technologies such as GCP and AWS (colored red) provide high salaries (up to $165K) but are required in fewer than 20% of jobs. This indicates a niche but lucrative market for data analysts with cloud expertise.

-  Analyst tools like Excel, Tableau, and Power BI (orange) show varied performance. Excel and Tableau are moderately demanded (20–30%) with decent pay ($100K), while Power BI and Looker have lower demand (10% and less) and salaries ($60K–$80K), making them less optimal.

# What I learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage:** Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance:** I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis:** The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights
This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation:** There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and GitHub often lead to higher salaries.
- **Market Trends:** There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills:** Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I faced
This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies:** Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth:** Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.



