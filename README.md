# 📊 Data Analyst Job Market Analysis

Welcome to my project exploring the **data job market**, with a specific focus on **Data Analyst roles**. This analysis was driven by a personal goal: to better understand the landscape of analytics jobs — what skills are most valuable, what pays best, and where the opportunities are.

Using real-world job data from [Luke Barousse's Python Job Market Course](https://github.com/lukebarousse/data-jobs), this project provides insights into **top-paying** and **in-demand skills**, helping both aspiring and experienced data professionals make smarter career decisions.

---

## Key Questions

This project aims to answer the following:

- **What are the most in-demand skills** for the top 3 most popular data roles?
- **How are in-demand skills trending** over time for Data Analysts?
- **How well do jobs and skills pay** in the Data Analyst market?
- **What are the optimal skills** for a Data Analyst to learn?  
  (i.e., skills that are both **high in demand** and **high paying**)

---

## Tools & Technologies Used

To analyze and visualize this job market data, I used the following tools:

### Python
The backbone of the entire analysis — powering data cleaning, transformation, and insights.

### Key Python Libraries
- **Pandas**: For data exploration, transformation, and filtering.
- **Matplotlib**: For creating basic charts and figures.
- **Seaborn**: For cleaner, more advanced data visualizations.

### 🧪 Development Environment
- **Jupyter Notebooks**: Ideal for iterative analysis and embedding commentary alongside code.
- **Visual Studio Code**: My main editor for writing and testing scripts.

### 🌐 Version Control & Collaboration
- **Git & GitHub**: Used for tracking changes, sharing work, and organizing project files.

---

# Data Preparation & Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean!
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

## Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.

```python
df_US = df[df['job_country'] == 'United States']

```
# The Analysis

## 1. What are the most demanded skills for the Top 3 most popular data roles?

To find the most demanded skills for the Top 3 most popular data roles, I filtered out those positions by which ones were the most popular, and got the Top 5 skills for these Top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project\2_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Visualization of Top Skills for Data Professionals](3_Project\images\skill_demand_all_data_roles.png)

### Insights

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysis?

### Data Visualization

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results
![Trending Top Skills for Data Analysts in the US](3_Project\images\Trending_Top_Skills_for_Data_Analysts_in_the_US.png)
*Bar graph visualizing the trending top skills for Data Analysts in the US in 2023.*


### Insights

#### SQL
- Most in-demand skill in both regions.
- Dominates the market, consistently present in **over 55–65%** of job postings.
- Remains foundational and non-negotiable globally.

#### Excel
- Steady usage across both regions (~43–45%) with a mid-year **spike (June–August)** and a notable **dip in Q4**.
- Rebounds in December — possibly tied to **financial cycles or reporting periods**.

#### Python
- Stable demand (~30–35%) in both regions.
- Slightly more prevalent in the **US**, but overall globally consistent.
- Increasingly recognized as a **core skill alongside SQL**.

#### Tableau
- Slightly more favored than Power BI in both regions.
- Demand tapers slightly in Q4 but remains a **reliable business intelligence tool** requirement.

#### Power BI
- Steady but lowest in demand (~20–23%).
- Shows some volatility in the US but trails Tableau globally.
- Indicates **growing adoption**, particularly where Microsoft ecosystems are dominant.



## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Professions


#### Data Visualization

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results
![Salary Distribution of Data Jobs in the US](3_Project\images\Salary_Distributions_of_Data_Jobs_in_the_US.png)
*Box plot visualizing the salary distributions for the Top 6 Data Job Titles*


#### Insights

##### Key Observations
- Senior roles consistently demonstrate higher median salaries
- Significant salary variability exists within each job category
- Potential for high earnings, especially in senior positions

##### Salary Ranges
| Role | Median Salary Range | Notable Characteristics |
|------|---------------------|------------------------|
| Senior Data Scientist | $100k - $600k | Highest overall median salary |
| Senior Data Engineer | $90k - $550k | Competitive compensation |
| Data Scientist | $70k - $500k | Wide salary distribution |
| Data Engineer | $65k - $450k | Significant earning potential |
| Senior Data Analyst | $80k - $250k | More compressed salary range |
| Data Analyst | $50k - $200k | Entry-level opportunities |

##### Salary Progression
1. Clear salary increase from junior to senior roles
2. Significant outliers indicate exceptional compensation opportunities
3. Variability influenced by:
   - Experience level
   - Geographic location
   - Company size
   - Industry specialization



### Highest Paid & Most Demanded Skills for Data Analysts

#### Data Visualization

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:r')

plt.show()
```
#### Results

Here's the breakdown of the highest-paid & most in-demand skills for Data Analysts in the US:
![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\images\Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_the_US.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for Data Analysts in the US,*


#### Insights

- The top graph shows specialized technical skills like 'dplyr', 'Bitbucket', and 'Gitlab' are associated with higher salaries, some reaching up to $200k, suggestig that advanced technical proficiencies can increase earning potential.
- The bottom visual highlights that foundational skills like 'Excel', 'PowerPoint, and 'SQL' are some of the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for hirability in Data Analysis roles.
- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data Analysts aiming to maximize their career potential should consider developing a diverse skills set that includes both high-paying specialized skills and widely demanded foundational skills.



## 4. What is the most optimal skill to learn for Data Analysts?

### Data Visualization

```python
from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])

```

#### Results

![Most Optimal Skills for Data Analysts in The US](3_Project\images\Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Coloring_by_Technology.png)

*A scatter plot visualising the most optimal skills (high paying & demand) for data analysts in the US.*

#### Insights
- The scatter plot shows that most of the 'programming' skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the Data Analytical field.

- Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries - showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

- Database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among Data Analyst tools. This indicates a significant demand & valuation for data management and manipulation expertise in the industry.


# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.