# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out the positions by the most popular ones, and got the the top 5 skills for those top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)


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
![Likelihood of Skills requested in Canadian Job Postings](3_Project\images\skill_demand_all_data_roles.png)

### Insights
- SQL is the most requested skill for all three roles of Data Analyst, Data Engineer, and Senior Data Engineer, with the skill being in more than half of the job postings for the top 3 roles.
- Python is a versatile skill, also appearing in the top 3 skills for each role. Although less prominent for Data Analysts at about 32% of roles, it is proven to be an important skill for Data Engineers and Senior Data Engineers appearing in 61% and 67% of job postings respectively.
- Data Engineers require technical skills such as AWS, Spark and Azure compared to Data Analysts who are expected to be procifient in more general data management and analysis tools such as Excel, Tableau, and Power BI.


## 2. How are in-demand skills trending for Data Analysts?

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

![Trending Top Skills for Data Analysts in Canada](3_Project/images/top%20skills%20trend.png)
*Line graph visualizing the trending top skills dor data analysts in Canada in 2023*

### Insights
- As expected, the line graph representing the likelihood of skills requested in data-related Canadian job postings shows consistency, with SQL, Python, and Excel being the top three skills for Data Analysts in Canada.

- SQL is clearly the most in-demand skill, maintaining above 50% likelihood in postings throughout the year. A noticeable peak in September suggests a surge in demand during the fall hiring cycle.

- Python ranks second in demand, with steady interest between 30–40%. Like SQL, it experiences minor fluctuations but no significant decline, indicating stable and potentially growing demand.

- Excel, Tableau, and Power BI are closely competing. Tableau shows notable spikes in May and September, possibly tied to project or reporting cycles. The monthly variability seen across these three tools suggests that they are more role-specific or industry-dependent in their demand.
