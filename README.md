# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from Luke Barousse's Python Course which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.


# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
- - **Pandas Library**: This was used to analyze the data.
- - **Matplotlib Library**: I visualized the data.
- - **Seaborn Library**: Helped me create more advanced visuals.
- **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code**: My go-to for executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

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

## Filter Canadian Jobs

Although this data contains much more U.S. data, I wanted to be representative of the job market I contribute to and work with Canadian data. Thus, to focus my analysis on the Canadian job market, I apply filters to the dataset, narrowing down to roles based in Canada.

```python
df_US = df[df['job_country'] == 'United States']
```







# The Analysis


Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:





## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out the positions by the most popular ones, and got the the top 5 skills for those top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project/2_Skills_Demand.ipynb)

### Visualize Data
```python
fig,ax = plt.subplots(len(job_titles),1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short']==job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue = 'skill_count', palette = 'dark:b_r')
    
plt.show()
```

### Results
![Likelihood of Skills requested in Canadian Job Postings](3_Project/images/skill_demand_all_data_roles.png)

*Bar Chart visualizing the likelihood of skills requested in the Canadian data job market*

### Insights
- SQL is the most requested skill for all three roles of Data Analyst, Data Engineer, and Senior Data Engineer, with the skill being in more than half of the job postings for the top 3 roles.
- Python is a versatile skill, also appearing in the top 3 skills for each role. Although less prominent for Data Analysts at about 32% of roles, it is proven to be an important skill for Data Engineers and Senior Data Engineers appearing in 61% and 67% of job postings respectively.
- Data Engineers require technical skills such as AWS, Spark and Azure compared to Data Analysts who are expected to be procifient in more general data management and analysis tools such as Excel, Tableau, and Power BI.





## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skill_Trends.ipynb](3_Project/3_Skill_Trends.ipynb)

### Visualize Data
```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_CAN_percent.iloc[:,:5]

sns.lineplot(df_plot, dashes = False, palette = 'tab10')

ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())

for i in range(5):
    plt.text(11.2,df_plot.iloc[-1,i], df_plot.columns[i])

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in Canada](3_Project/images/top_skills_trend.png)

*Line graph visualizing the trending top skills for data analysts in Canada in 2023*

### Insights
- As expected, the line graph representing the likelihood of skills requested in data-related Canadian job postings shows consistency, with SQL, Python, and Excel being the top three skills for Data Analysts in Canada.

- SQL is clearly the most in-demand skill, maintaining above 50% likelihood in postings throughout the year. A noticeable peak in September suggests a surge in demand during the fall hiring cycle.

- Python ranks second in demand, with steady interest between 30–40%. Like SQL, it experiences minor fluctuations but no significant decline, indicating stable and potentially growing demand.

- Excel, Tableau, and Power BI are closely competing. Tableau shows notable spikes in May and September, possibly tied to project or reporting cycles. The monthly variability seen across these three tools suggests that they are more role-specific or industry-dependent in their demand.


## 3. How well do jobs and skill pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in Canada and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis.ipynb](3_Project/4_Salary_Analysis.ipynb)

### Visualize Data
```python
sns.boxplot(data = df_US, x='salary_year_avg', y='job_title_short', order = job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')

plt.gca().xaxis.set_major_formatter(ticks_x)

plt.show()
```

### Results

![Salary Distribution in Canada](3_Project/images/salary_distibution_in_canada.png)


*Box plot visualizing the salary distribution for various data tech positions in Canada in 2023*

### Insights
- Median salaries tend to increase with seniority and specialization. Senior roles, such as Senior Data Engineer, not only command higher median salaries but also show greater variability in compensation, reflecting wider salary differences as responsibilities and expertise grow.
- Data Analysts have the lowest median salary among the top six roles. This aligns with expectations, as higher-paying positions typically require more specialized technical skills in areas like machine learning, cloud engineering, or advanced data modeling.
- Roles with higher outliers—such as Machine Learning Engineer and Senior Data Engineer—indicate that top performers or professionals in leading companies can earn significantly above the median. Meanwhile, Data Analysts exhibit a much lower outlier range, suggesting limited salary spikes even at senior levels.

## Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two horizontal bar charts to showcase these.
### Visualize Data
```python
fig, ax = plt.subplots(2,1)

# Top 10 Paid SKills for Data Analysts
sns.barplot(data=df_DA_toppay, x='median', y=df_DA_toppay.index, ax=ax[0], hue='median', palette= 'dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette= 'light:b')
ax[1].legend().remove()

plt.show()
```

### Results

![Salary Distribution in Canada](3_Project/images/skills_vs_pay.png)


*Horizontal Bar Charts visualizing the Top 10 Highest Paid Skills for Data Analysts in Canada in 2023*

### Insights
- Most of the highest paid skills are not the most in-demand skills for data analysts in Canada. Similarly, many high-demand skills are not among the highest paying. This suggests that niche skills—those less commonly required—may offer higher salaries due to their specialization and lower supply in the job market.

- Skills such as Snowflake and Spark are particularly valuable because they are both highly paid and highly in demand. While foundational tools like Python, SQL, and Excel may not top the salary charts, they remain essential for breaking into the field and improving job prospects.

- Cloud and Big Data tools—such as Redshift, Snowflake, BigQuery, AWS, and GCP—feature prominently among the highest-paid skills. This highlights the strong earning potential for data analysts who specialize in modern data infrastructure.





## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

### Visualize Data
```python
import matplotlib.pyplot as plt
from adjustText import adjust_text

sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
plt.show()
```

### Results

![Most Optimal Skills for Data Analysts in Canada](3_Project/images/optimal_skills.png)


*Scatter Plot visualizing the most optimal skills for data analysts categorized by technology in Canada in 2023*

### Insights
- As an entry-level Data Analyst in 2023 in Canada, programming languages such as SQL and Python are the most optimal skills to aquire because although the salaries are just under $100K they are often in high-demand appearing in 50%+ of job postings. This indicates that the skills are foundational and expected. Mid-to-Senior Analysts aiming for higher salaries should consider learning Snowflake, Spark, BigQuery or Azure which are less common but highly compensated.
- Analyst Tools such as Excel and Tableau have a decent demand being in 20-40% of job postings but have a low median salary. These tools are often used in reproting heavy or junior roles and may not command higher pay unless combined with more technical skills.
- Cloud skills are the most profitable as they tend to be required for job postings with salaries on the higher end of what is offered, however, they have a low demand. This suggests they are specialized skills that are not needed by all employers but highly values by those who require them.

# What I learned
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

This project was not without its challenges, but each obstacle provided valuable learning opportunities that strengthened both my technical and analytical skills:

- **Data Inconsistencies**: Handling missing, duplicated, or inconsistent entries required careful attention and thorough data-cleaning techniques to maintain the integrity of the analysis.

- **Complex Data Visualization**: Designing visualizations that were not only accurate but also intuitive and visually engaging proved challenging—especially when trying to represent multi-dimensional data in a clear, impactful way.

- **Balancing Breadth and Depth**: Deciding how deep to go into each analysis, while still keeping a broad overview of the job market landscape, required constant judgment to avoid losing focus or missing key insights.

- **Tool Familiarity Gaps**: As someone new to the tech and data field, I occasionally encountered roadblocks related to unfamiliar Python functions or visualization libraries, which required extra time for self-directed learning and troubleshooting.

- **Interpretation Bias**: Making sure my conclusions were based on data—not assumptions—was an important discipline, especially when patterns seemed to align with what I expected. Cross-checking insights helped maintain objectivity.

- **Time Management**: Working through a full analysis pipeline—data collection, cleaning, analysis, visualization, and interpretation—required careful planning to avoid getting stuck too long on any single stage.

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I gained not only deepened my technical expertise in Python, data manipulation, and visualization but also clarified the strategic importance of aligning technical skills with real-world market demand.

As someone new to the tech and data industry, this project gave me the confidence that I’m heading in the right direction—especially by choosing to focus on Python, a highly demanded and versatile language, instead of less-requested tools like R. It also helped me understand the growing relevance of cloud-based technologies such as Snowflake, BigQuery, and Azure, which offer strong salary potential and are becoming increasingly valuable in modern data environments.

Looking forward, once I’ve gained more hands-on experience as a data analyst, I intend to tailor my learning toward both tools required by my future employer and high-impact, specialized skills like those used in big data and cloud platforms.

This project serves as a solid foundation for future explorations and underscores the importance of continuous learning, skill adaptability, and proactive market analysis for long-term success in the data field.
