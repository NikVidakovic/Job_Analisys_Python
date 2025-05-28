# Overview
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimaljob opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://github.com/lukebarousse/data_jobs) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations and essential skills. Through a series of Python scripts. I explore key questions such as the most demanded skills, salary trends and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the most in-demand skills for the top 3 most popular data roles?

2. How are In-demand skills trending for Data Analysts?

3. How well do jobs and skills pay for Data Analysts?

4. What is the most optimal skill to learn for Data Analysts?

# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

* **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used Python libraries such as Pandas, Seaborn and Matplotlib

* **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.

* **Visual Studio Code:** My go-to for executing Python scripts.

* **Git and GitHub:** Essential for version control and sharing my Python code and analysis.

# Data Preparation and clean up
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import and Clean Up Data
I start by importing necessary libraries and loading dataset, followed by initial data cleaning tasks to ensure data accuracy.

# The Analysis

## 1. What are the most in-demand skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills. It shows which skills we should pay attention to depending on the role we are targeting.

View my notebook with detailed steps here:
[2_Skill_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)

#### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles),1)

sns.set_theme(style='ticks')

#itterating through each one using for loop
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0,78)
    #creating loop inside of a loop in order to get % labels
    for n,v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n , f'{v:.0f}%', va='center')

#keeping x axis labels on the last plot
if i != len(job_titles) - 1:
    ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)

plt.show()

```

#### Results

![Visualization of Top Skills](3_Project\images\correct.png)
*Bar charts showing likelyhood of Skills Requested in US Job Postings*
#### Insights:

- Python is a versatile skill, high demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
- From the chart it is visible that SQL is the most demanded skill for Data Analysts and Data Engineers but also a very high in demand skill for Data Scientist. 
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

## 2. How are In-demand skills trending for Data Analysts?

View my notebook with detailed steps here:
[4_Salary_Analysis.ipynb](3_Project\4_Salary_Analysis.ipynb)

#### Visualize Data

```python
import seaborn as sns

df_plot = df_DA_US_percent.iloc[:, :5]
from matplotlib.ticker import PercentFormatter

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

#### Results

![Trending Top Skills for Data Analysts in the US](3_Project\images\In_demand_skills.png)

*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

#### Insights:

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced a significat increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's trend. 

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis


![Salary Distribution for Data Jobs in the US](3_Project\images\senior_junior_roles.png)
*Box plot visualizating the salary distributions for the top 6 data job titles*

- This chart displays the yearly salary ranges for various data-related job roles, including Data Analyst, Senior Data Analyst, Data Engineer, Senior Data Engineer, Data Scientist, and Senior Data Scientist. The salaries are presented in box plots, which show the median, interquartile range, and the presence of outliers for each role.

- Overall, the chart clearly indicates that salaries tend to increase with seniority. For example, Senior Data Scientists and Senior Data Engineers have higher median salaries compared to their non-senior counterparts. Among all roles, Senior Data Scientists appear to have the highest median salary, while Data Analysts have the lowest. The spread of salaries also becomes wider in more senior roles, suggesting greater variability in earnings, likely due to factors such as experience, company, and specialization.

- There are also many outliers present, particularly in the roles of Data Scientist and Senior Data Scientist. These outliers show that some individuals in these roles can earn significantly higher salaries, sometimes exceeding $500K per year. Such extremes highlight the potential for very high compensation in certain positions within the data field.

### Highest Paid and Most demanded Skills for Data Analysts

View my notebook with detailed steps here:
[4_Salary_Analysis.ipynb](3_Project\4_Salary_Analysis.ipynb)

#### Visualize Data

``` python
fig, ax = plt.subplots(2,1)

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
sns.set_theme(style="ticks")


ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
ax[0].legend().remove()

sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')

ax[1].set_xlim(ax[0].get_xlim()) 
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD$)')
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
ax[1].legend().remove()

fig.tight_layout()

```
#### Results

![The Highest Paid & Most In-Demand Skills for Data Analysts](3_Project\images\Most_In_Demand_Skills.png)

#### Insights:
- This chart compares the top 10 highest paid skills and the top 10 most in-demand skills for data analysts based on median salary in the U.S.

- In the top panel, highly technical and specialized skills like Golang, Redis, and Elasticsearch offer the highest median salaries, exceeding $120K in some cases. These skills are more niche and less common among typical data analysts but command a premium due to their technical depth and relevance in high-performance systems.

- In contrast, the bottom panel shows that the most in-demand skills are more mainstream tools such as AWS, Python, Tableau, SQL, and Power BI, with median salaries generally between $70K and $90K. While widely required in job postings, these skills do not lead to the highest compensation levels, possibly due to their broader availability in the workforce.


## 4. What is the most optimal skill to learn for Data Analysts?


View my notebook with detailed steps here:
[5_Optimal_Skills.ipynb](3_Project\5_Optimal_Skills.ipynb)

```python
from adjustText import adjust_text

import seaborn as sns
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
sns.despine()
sns.set_theme(style='ticks')
plt.title('Salary vs Count of Job Postings for Top 10 Skills')
plt.xlabel('Count of Job postings')
plt.ylabel('Median Yearly Salary ($USD)')
plt.tight_layout()

texts = [] 

for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_plot['median_salary'].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle="->", color='grey', lw=1))

from matplotlib.ticker import PercentFormatter

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.title('Most Optimal Skills for Data Analysts in the US')
plt.xlabel('Percent of Data Analyst Job')
plt.ylabel('Median Yearly Salary ($USD)')
plt.tight_layout()

plt.show()
```

#### Results
![Most Optimal Skills for Data Analysts in the US](3_Project\images\optimal_skill.png)
*A scatterplot visualizing the most optimal skills (high paying & high demand) for data analysts in the US*

#### Insights:
- Skills like SQL, Excel, Python, and Tableau appear in a high percentage of job postings, indicating they are essential and widely used, though their salaries range from moderate to high. SQL stands out as both highly demanded and decently paid.

- On the other hand, niche skills like Oracle, Flow, SQL Server, and Go offer higher salaries but appear in a smaller share of job listings, suggesting they are more specialized and valued when needed.

- Cloud skills like Azure and AWS are less in demand for data analysts but also command lower to moderate salaries. Analyst tools like PowerPoint and Looker have low demand and relatively lower salaries.

# What I learned 


* **Advanced Python Usage:**
Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization

* **Data Cleaning Importance:**
Learned that clean data is critical for reliable insights and avoiding misleading conclusions. I also learned how to identify and handle missing data, duplicates, and inconsistent data to improve dataset quality

* **Strategic Skill Analysis:**
This project emphasized the importance the importance of aligning one's skill with market demand. Understanding the relationship between skill demand, salary and job avilability allows for more strategic career planning in tech industry.

# Challanges I Faced

This project was not without its challanges, but it provided good learning opportunities:

* **Complex Data Transformations:**
Multi-step transformations (like grouping, aggregating, merging) can quickly become hard to read and debug, particularly when chaining multiple operations.

* **Data Quality Issues:**
Dealing with missing values, inconsistent formats, or duplicate records often requires careful cleaning and preprocessing with pandas, which can be time-consuming and prone to oversight.

* **Balancing Technical Precision with Interpretability:**
One key challenge in data analysis is striking the right balance between technical precision and interpretability. While complex transformations and advanced visualizations can reveal deep insights, they can also make the analysis harder to follow for non-technical stakeholders. Ensuring that findings are both accurate and easily understandable is essential for creating value from data.

# Insights

* SQL, Python, and Excel are among the most consistently demanded and essential skills for data analysts, with SQL being the most prevalent.

* Trends in skill demand shift over time—Excel saw a notable increase in demand, surpassing other tools like Python and Tableau by year-end.

* Salary and skill value do not always align—niche skills like Golang or Redis command high pay but appear in fewer job listings.

* The optimal skills for data analysts are those that strike a balance between demand and compensation, with SQL and Python standing out.

* Seniority directly influences salary, with senior roles showing broader salary ranges and higher earning potential.

# Conclusion

This project offered a deep, data-driven view into the evolving data analyst job market. By combining advanced Python techniques with robust data cleaning and strategic skill analysis, I was able to uncover key trends in skill demand, salary expectations, and role specialization. Beyond the technical insights, this project also emphasized the importance of aligning one's learning path with market needs and highlighted the value of clear, interpretable data storytelling. It’s a strong step toward more informed and intentional career planning in the data field.