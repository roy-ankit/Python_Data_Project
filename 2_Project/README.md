# Introduction
The aim of the project is to explore the data analyst roles and draw information on what the top-paying jobs, in-demand skills, and where high demand meets high salary in data analytics.

The Data used : [Click here](https://huggingface.co/datasets/lukebarousse/data_jobs)

# Background
The aim of the project is to enhance better understanding of the data analyst job market, with the aim of identifying top-paid and in-demand skills. By streamlining the process, it assists in better informed decision making by necessary upskilling in order to meet the criterion of optimal job opportunities more efficiently.

The questions this project wants to answer through python are:
1. What are the most demanded skills for the Top 3 popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the most optimal skills to learn for Data Analysts?

# Tools used
The following tools were used to do the necessary analysis:

- **Python:** The backbone of the analysis, allowing to analyze the data and find critical insights.The following Python libraries were used:
    - **Pandas Library:** To analyze the data. 
    - **Matplotlib Library:** To visualize the data.
    - **Seaborn Library:** To create advanced visuals. 
- **Jupyter Notebooks:** The tool used to run the Python scripts by including notes and analysis.
- **Visual Studio Code:** For execution of the project.
- **Git & GitHub:** Essential for version control and sharing the Python scripts and analysis, ensuring collaboration and project tracking.


# Analysis

Through each of the questions we are trying to unearth information of the data analyst job market and how to best position oneself in terms of skills to crack the next big job.

## 1. What are the most demanded skills for the Top 3 popular data roles?

In order to ascertain the most demanded skills, we first need to identify the three data job roles that had the most number of job postings. Once the job roles are identified, we ascertained the top five skills that were most frequently required for in those roles.

The detailed code is mentioned [here](/2_Project/Scripts/2_Skill_Demand.ipynb).

### Visualization code snippet
```python
fig, ax = plt.subplots(len(job_titles),1)
ax: list[Axes]  # Type hint for ax

sns.set_theme(style='ticks')
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head()
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r', width=0.75)
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].legend().remove()
    ax[i].set_xlim(0,100)
    for n,v in enumerate(df_plot['skill_percent']):
        ax[i].text(v+1,n,f'{v:.0f}%', va='center')
    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])
    
fig.suptitle('Likelihood of Skills Requested in US Job Postings')
fig.tight_layout(h_pad=0.5)
plt.show()
```
### Results

![Visualization of Top Skills](/2_Project/Images/Likelihood_of_Skills_Requested_in_US_Job_Postings.png)
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysts?

In order to ascertain the in-demand skills for Data Analyst, we need to first filter the data for data analyst positions in United States. On this filtered data we will group the skills required by the month of the job postings by using a pivot table. In this pivot table we will identify the top 5 skills that were trending throughout the year.

The detailed code is mentioned [here](/2_Project/Scripts/3_Skills_Trend.ipynb).

### Visualization code snippet
```python

sns.set_theme(style='ticks')
lines = sns.lineplot(data=df_plot, dashes=False)
sns.despine()
plt.title('Trending Top Skills for Data Analyst Jobs in US')
plt.xlabel('')
plt.ylabel('Likelihood in Job Postings')
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.legend().remove()
texts=[]
for i, line in enumerate(lines.get_lines()):
    if i < len(df_plot.columns):
        colour = line.get_color()
        texts.append(plt.text(11, df_plot.iloc[-1, i], df_plot.columns[i], color=colour))  
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))
plt.show()
```
### Results

![Trending Top Skills for Data Analyst jobs in US](/2_Project/Images/Trending_Top_Skills_for_Data_Analysts_in_the_US.png)
*Line graph visualizing the trending skills for data analyst jobs in US.*

### Insights
- SQL remains the most consistently demanded skill throughout the year. However, gradually it is showing a decline in demand for this skill.
- Despite the advancement in artificial intelligence, Excel still remained the second most demanded skill for data analyst throughout the year.
- Tableau is the most demanded visualisation tool for Data Analyst job roles in US in 2023.

## 3. How well do jobs and skills pay for Data Analysts?

In order to ascertain how well Data Analyst jobs pay, we filtered the data for one country United States and then compared median salary of all the job postings.

The detailed code is mentioned [here](/2_Project/Scripts/4_Salary_Analysis.ipynb).

### Visualization code snippet
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short',order=job_top6_order)
sns.set_theme(style='ticks')
sns.despine()
plt.xlabel('Median Salary (USD)')
plt.ylabel('')
plt.title('Salary Distribution of the Top Data Jobs in US')
plt.xlim(0,600000)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos :f'${int(x/1000)}K'))
plt.show()
```
### Results

![Salary distribuion of the Top Data roles in US](/2_Project/Images/Salary_Distributions_of_Data_Jobs_in_the_US.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights

- A significant variation in salary ranges across different job title were observed. Data Science and Data Engineer roles were found to be relatively higher paying than Data analyst roles, indicating the high value placed on technical capbilities.

- The earning potential in Data science roles is significantly higher with a wide range of salary distribution indicating the high value placed on advanced skills and industry experience.

- The median salaries increase with the seniority and the specilaization of the roles. Senior roles in Data science and Data engineering have higher median salaries from normal roles, reflecting higher compensation because of industry experience as well as higher responsiblities.

After determining the salary distribution of different job roles, we proceeded to identifying the highest paid skills and the most in-demanded skills for Data Analyst roles in the US.


### Visualization code snippet
```python
fig,ax=plt.subplots(2,1)
ax: list[Axes]  # Type hint for ax


sns.barplot(data=df_DA_top_pay, x='median', y='job_skills', ax=ax[0], hue='median', palette='dark:b_r', width=0.75)
ax[0].set_title('Highest Paid Skills for Data Analysts in the US')
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].legend().remove()
ax[0].set_xlim(0,200000)
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,pos:f'${int(x/1000)}K'))

sns.barplot(data=df_DA_top_demand, x='median', y='job_skills', ax=ax[1], hue='median', palette='light:b', width=0.75)
ax[1].set_title('Most In-Demand Skills for Data Analysts in the US')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_ylabel('')
ax[1].legend().remove()
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,pos:f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
sns.despine()
fig.tight_layout()
plt.show()
```

### Results

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](/2_Project/Images/Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_the_US.png)
*Bar graphs showing the highest paid skills and most in-demand skills for Data Analyst job postings in US.*

### Insights

- The first graph is indicative of the fact that niche technical skills like dplyr, Bitbucket, and Gitlab are high paying skills with compensations offered being close to $200K. Thus advanced technical skills despite having less opportunities can significantly increase earning potential.

- The second graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

In order to ascertain the most optimal skills to learn, we need to identify the skills that are highly paid as well as most in demand.

The detailed code is mentioned [here](/2_Project/Scripts/5_Optimal_Skills.ipynb).

### Visualization code snippet
```python

sns.scatterplot(data=df_DA_skills_high_demand, x=df_DA_skills_high_demand['skill_percent'], y=df_DA_skills_high_demand['median_salary'])
sns.set_theme(style='ticks')

texts=[]
for i,x in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i],df_DA_skills_high_demand['median_salary'].iloc[i],x))
sns.despine()
adjust_text(texts,force_text=(1,1), arrowprops=dict(arrowstyle='->', color='gray'))
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.gca().xaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.xlabel('Percent of Job Postings')
plt.ylabel('Median Yearly Salary (USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.tight_layout()
plt.show()

```

### Results

![Most Optimal Skills for Data Analysts in the US](/2_Project/Images/Most_Optimal_Skills_for_Data_Analysts_in_the_US.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

### Insights

- Oracle has the highest median salary of more than $97K. However, Oracle has a very less demand of less than 10% job postings. This implies that high value is placed on specialized database skills within the data analyst profession.

- Basic skills like Excel and SQL is the requirement in more than 40% of job listings but they attract lower median salaries compared to specialized skills like Python and Tableau.

- Skills like Python,and Tableau, offered a higher median salary of more than $92K and they were also prevelant in more than 30% job posting. SQL offered $90K median salary and were present in almost 60% of job postings. This indicated that these three tools can lead to high paying job opportunities in data analytics.


## Visualizing Different Techonologies
Different skills can be further categorised differently as analysis, database, programming skills. We thus visualize the different skills as per the technology classification.

### Visualization code snippet
```python
sns.set_theme(style='ticks')
ax= sns.scatterplot(data=df_plot, x=df_plot['skill_percent'], y=df_plot['median_salary'], hue=df_plot['technology'])
sns.despine()
texts=[]
for i, txt in enumerate(df_plot.index):
    texts.append(plt.text(df_plot['skill_percent'].iloc[i], df_plot['median_salary'].iloc[i] , txt))

adjust_text(texts,expand_points=(3,3),force_text=(1,1), arrowprops = dict(arrowstyle = '->', color='gray'))
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.gca().xaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.xlabel('Percent of Job Postings')
plt.ylabel('Median Yearly Salary (USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.legend(title='Technology')
plt.tight_layout()
plt.show()
```

### Results

![Most Optimal Skills for Data Analysts in the US in terms of technology](/2_Project/Images/Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Technology.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.*

### Insights
- The scatter plot shows that most of the programming skills tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

- The database skills, such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

- Analyst tools, including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# Learnings

Key learnigns in terms of technical ability:

1. Data preparation - Understanding the data and preparing it for analysis through cleaning and manipulation is very important in order to ensure the quality of the insights drawn from the data.
2. Python libraries - Understanding the capabilities of different libraries and how each can be leveraged like pandas for data amanipulation, seaborn and matplotlib for data visualization and other libraries like adjustText to create scatter free labels.

# Inference

The following inference an be drawn about the job market in 2023 from the given dataset:

1. Skill and Salary correlation : There is a clear correlation between the demand for specific skills and the salaries offered for these . 
2. Experience level : Like in any industry, experience is valued and relevant industry experience offers higher salary.
3. Niche skills : Very niche skills might not have a lot of opportunities but when they do they are very highly compensated in the region of $150K and above.
4. Economic value : Understanding the skills that are both in-demand and well-compensated can help data analyst to upskill and seek better economic returns.

# Conclusion

This project provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics.

# Reference
The idea of this project is based on the work provided by Mr. Luke Barousse. The work that has been referenced is provided [here](https://www.lukebarousse.com/python).