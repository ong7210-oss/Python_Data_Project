# Overview

Welcome to my deep dive into the data job market, with a special focus on data analyst roles. I created this project to better navigate and understand current job market trends. It explores the highest-paying and most requested skills to help identify the best career opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which serves as the foundation for this project. It includes detailed records on job titles, pay rates, locations, and key skill requirements. Using a series of Python scripts, I answer critical questions regarding in-demand skills, salary patterns, and where high demand meets top pay in the analytics space.

# The Questions

Below are the questions I want to answer in my project:

1. What are the most sought-after skills for the top 3 data roles?
2. How are skill demands changing over time for Data Analysts?
3. What are the typical salary ranges for Data Analyst jobs and skills?
4. Which optimal skills should data analysts prioritize learning? (High Demand AND High Pay)

# Tools I Used

To perform this deep dive into the data analyst job market, I relied on a few essential tools:

- **Python:** The core engine of my project, used to inspect the data and extract key insights. I also utilized these key Python libraries:
    - **Pandas Library:** Used for cleaning and analyzing the data.
    - **Matplotlib Library:** Used for creating data visualizations.
    - **Seaborn Library:** Used to build styled, advanced charts.
- **Jupyter Notebooks:** The environment where I ran my Python scripts and combined my code with notes and observations.
- **Visual Studio Code:** My primary code editor for writing and running Python scripts.
- **Git & GitHub:** Used for tracking code changes and publishing my project online to support easy tracking and sharing.

# Data Preparation and Cleanup

This section covers the processing steps used to refine the dataset, ensuring high data quality and accuracy before analysis.

## Import & Clean Up Data

First, I imported the required libraries and loaded the raw dataset. I then performed essential data-cleaning tasks to standardize values and handle missing entries.

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

To center the analysis on the U.S. job market, I filtered the records to isolate job postings located within the United States.

```python
df_US = df[df['job_country'] == 'United States']

```

# The Analysis

Each Jupyter notebook in this project targets a specific question about the data job market. Here is how I addressed the first question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most requested skills across the top 3 data positions, I first ranked the roles by overall posting volume. I then extracted the top 5 required skills for each of these top 3 roles. This breakdown reveals key skill requirements per role, helping identify which technical competencies to focus on based on your target career path.

View my notebook with detailed steps here: [2_Skill_Demand](/Project/2_demanded_skills.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_title), 1)

for i, title in enumerate(job_title):
    df_plot_4 = df_skill_percent[df_skill_percent['job_title_short'] == title].head(5)

    sns.barplot(data=df_plot_4, x='skill_percent', y='job_skills',ax=ax[i], hue='skill count', palette='dark:b_r',legend=False)
    
plt.show()
```

### Results

![Likelihood of Skills Requested in the US Job Postings](/Image/image_1.png)


*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights:

- SQL stands out as the most requested technical requirement for both Data Analysts and Data Scientists, featured in over 50% of openings for each role. For Data Engineers, Python takes the leading spot, appearing in roughly 68% of listings.
- Data Engineering roles lean heavily toward specialized, infrastructure-heavy tools (such as AWS, Azure, and Spark), whereas Data Analysts and Data Scientists focus more on general data manipulation and visualization tools (like Excel and Tableau).
- Python displays strong versatility across all three career tracks, though its demand peaks highest within Data Science (72%) and Data Engineering (65%) roles.

## 2. How are in-demand skills trending for Data Analysts?

To evaluate how skill requirements evolved across 2023 for Data Analysts, I isolated data analyst postings and grouped the required skills by posting month. This revealed the top 5 skills per month, tracking shifts in skill popularity across the entire year.

View my notebook with detailed steps here: [3_Skills_Trend](/Project/3_skill_trend.ipynb).

### Visualize Data

```python
sns.lineplot(data=df_plot_5, dashes=False, palette='tab10')

y_vals = [df_plot_5.iloc[-1, i] for i in range(5)]

for i in range(5):
    valign = 'center'

    # Check against other lines: if line 3 & 4 are close, push 3 down and 4 up
    if i == 3:
        valign = 'top'  # Push line 3 text slightly up
    elif i == 4:
        valign = 'bottom'  # Push line 4 text slightly down

    plt.text(11.5, df_plot_5.iloc[-1, i], df_plot_5.columns[i], va=valign)
# ----------------------------

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in the US](/Image/image_2.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights:
- SQL holds its position as the top required skill across every month, though it displays a steady, mild downward trajectory over time.
- Excel saw a notable surge in popularity beginning around November, eventually overtaking both Python and Tableau in monthly volume by the end of the year.
- Python and Tableau maintain solid, predictable demand with minor monthly ups and downs, solidifying their status as core requirements for analysts. sas, while lower in overall volume, demonstrates a gradual upward climb toward the end of the year.

## 3. How well do jobs and skills pay for Data Analysts?

To uncover the top-paying positions and skill sets, I restricted the dataset to U.S.-based roles and evaluated their median base salaries. To establish a baseline, I first examined the overall pay distributions across major data roles—such as Data Scientist, Data Engineer, and Data Analyst—to determine how their earning potential compares. 

View my notebook with detailed steps here: [4_Salary_Analysis](/Project/4_salary_analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short',order=job_order)

ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.show()

```

#### Results

![Salary Distributions of Data Jobs in the US](/Image/image_3.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

- Pay ranges vary widely depending on specific job titles. Data Scientist positions feature the highest earning potential, reaching up to $600K, which underlines the strong market value placed on advanced expertise and deep industry experience.

- Roles like Data Engineer and Data Scientist exhibit a large cluster of high-end salary outliers, indicating that specialized skills or unique offers can yield exceptional pay. By contrast, Data Analyst salaries show greater consistency with far fewer extreme outliers.

- Median pay scales upward directly alongside seniority and specialization. Executive and senior-level roles (such as Senior Data Scientist and Senior Data Engineer) command both higher median earnings and wider overall salary spreads, reflecting broader compensation variance as scope expands.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed the focus specifically to Data Analyst roles to analyze both the top-paying technical skills and the most requested skills in the market. I built two side-by-side bar charts to highlight these comparisons clearly.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skill, x='median', y=df_DA_skill.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](/Image/image_4.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

#### Insights:

- The top chart indicates that niche, specialized tools like `dplyr, Bitbucket, and Gitlab` command the highest compensation—with some reaching near $200K—suggesting that deep technical expertise can significantly boost earning potential.

- The bottom chart demonstrates that core foundational tools like `python, tableau, and r` dominate total job postings, despite not offering the highest peak salaries. This underscores how crucial these baseline skills are for overall job availability and securing entry into the field.

- A clear separation exists between the highest-paying skills and the most requested ones. Data analysts seeking to maximize their career progression should aim for a balanced skill set that pairs broadly demanded foundational tools with high-value, specialized technical skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To pinpoint the most optimal skills (those offering both strong market demand and high compensation), I calculated the demand percentage alongside the median salary for each skill. This approach makes it simple to spot which skills deliver the best overall return on investment.

View my notebook with detailed steps here: [5_Optimal_Skills](/Project/5_optimal_skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skill_high_demand['skill_percent'], df_DA_skill_high_demand['median_salary'])
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the US](/Image/image_5.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

#### Insights:

- `Oracle` commands the high median compensation at close to $97K, despite showing up less frequently across job postings. This highlights the premium value placed on specialized enterprise database management skills in analytics.

- Foundational requirements like `Excel` and `SQL` account for a massive share of job listings but carry lower median pay compared to tools like `Tableau`, which offer higher salaries along with strong, steady job market demand.

- Skills such as `Python`and`Tableau` are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

### Visualizing Different Techonologies

To better understand these patterns, we can break down and plot these skills by technology type. We will apply color-coded category labels (such as {Programming: Python}) to distinguish between programming languages, databases, cloud tools, and visualization software.

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
sns.scatterplot(
    data=df_plot_6, 
    x='skill_percent', 
    y='median_salary', 
    hue='technology')

plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the US with Coloring by Technology](/Image/image_6.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.*

#### Insights:

- The scatter plot reveals that `programming` skills (highlighted in blue) generally cluster at higher pay scales compared to other groups, indicating that strong coding abilities yield a clear salary advantage in data analytics.

- Enterprise `database` tools (highlighted in green), such as SQL Server, rank among the top-paying software categories for data analysts. This points to strong market demand and high value placed on data storage and manipulation expertise.

- `Analytical` and BI tools (highlighted in orange), including Tableau and Power BI, appear frequently across job postings while maintaining competitive salaries. This proves that visual reporting software remains a core, versatile requirement across modern data roles.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market while strengthening my hands-on technical abilities in Python, particularly for data cleaning and visualization. Key takeaways include:

- **Advanced Python Usage**: Applying libraries such as Pandas for data manipulation, along with Seaborn and Matplotlib for charting, helped me execute complex analytical tasks efficiently.
- **Data Cleaning Importance**: I saw firsthand how vital thorough data cleanup is before running analysis, ensuring that final business insights rest on accurate, reliable data.
- **Strategic Skill Analysis**: The analysis reinforced the importance of matching technical skills with real market demand. Evaluating salary levels against job volume provides a clear framework for strategic career planning.


# Insights

This project produced several key observations regarding the current market for data analysts:

- **Skill Demand and Salary Correlation**: A strong connection exists between skill specialization and earning potential. Niche or technical tools like Python and Oracle consistently command higher base salaries.
- **Market Trends**: Technical demands fluctuate throughout the year, illustrating the dynamic nature of the data space and the need to monitor industry trends continuously.
- **Economic Value of Skills**: Pinpointing tools that sit at the intersection of high volume and top pay enables analysts to prioritize learning paths that deliver the highest economic return.


# Challenges I Faced

Navigating this project presented a few valuable technical challenges:
- **Data Inconsistencies**: Addressing missing attributes and inconsistent text formats required careful data-cleaning strategies to maintain dataset integrity.
- **Complex Data Visualization**:Translating complex, multi-variable data into clear, easy-to-read charts required careful iteration of plot types, colors, and layouts.
- **Balancing Breadth and Depth**: Determining how deep to explore specific sub-questions while keeping an organized, high-level view required continuous focus on core project goals.


# Conclusion

Exploring the data analyst market has provided clear, actionable direction on the skills, tools, and pay structures shaping the industry. These findings offer practical guidance for anyone aiming to enter or grow within data analytics. Because market needs change constantly, ongoing tracking will be vital to staying competitive. This project serves as a strong foundation for future analyses, highlighting the ongoing necessity of continuous learning in the data field.

