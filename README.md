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