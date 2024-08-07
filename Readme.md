<h1 align="center" style="display: block; font-size: 2.5em; font-weight: bold; margin-block-start: 1em; margin-block-end: 1em;">
	<img
		width="300"
		alt="Wait logo"
		src="figures/static/wait-logo.jpg">
<br /><br />
  <strong>Data Mining Advanced HR Analytics for WAIT </strong><br /><br />
</h1>

<div align="center">
  <img src="https://img.shields.io/badge/Python-v3.11%2B-yellow" alt="Python Badge">
  <img src="https://img.shields.io/badge/Contributors-4-green" alt="Contributors Badge">
</div>

# 👨‍💻 Team and Tools
The project is carried out by a three-person team of Master's students of Computer Science in Business as part of the Data Mining course:

`Adriana Kuczaj` - https://github.com/ada-kuczaj<br>
`Wojciech Maziarz` - https://github.com/Wojtini<br>
`Oleksii Karbovskyi` - https://github.com/Aleksey9777

In the project, we use the **Python** programming language and tools such as pandas and matplotlib libraries for cluster modeling.

# ⚙️ How to run
```console
# In virtual env
pip install -r requirements_{linux|windows}.txt
# To run whole pipeline and generate files
kedro run
# To start vizualization service
kedro viz run --autoreload
```

# 📝 Business context

The project focuses on optimizing human resources management in the [WAIT](https://github.com/wait-wro) team through advanced data analysis. WAIT, for which the project is being implemented, is an open non-profit organization bringing together individuals interested in AI, Data Science and BI issues. Currently, the organization has data on the declared competences of people ready to work in projects, but there is no knowledge about the actual level and development of these skills.

# 🎯A problem to solve

The project addresses the problem of ineffective allocation of resources in project management. The goal is to provide data-driven insights that will help improve project team composition through identifying the right people by dividing users into distinguishable groups (clusters).

The aim of the project is to understand the actual state and purpose of the skills of individual WAIT members - monitoring how they develop and whether they are being properly utilized in projects. By identifying characteristics that make certain members more suitable for specific projects, we aim to achieve the following benefits.

# 💡Benefits 

- [x] faster identification of suitable candidates for project roles,
- [x] better ability to quickly find replacement people when needed,
- [x] identification of weak points in WAIT, such as the lack of specialists in specific areas,
- [x] determination of possible directions for team training development,
- [x] reducing delays and improving project success rates,
- [x] identification of skill gaps within the team,
- [x] streamlining the recruitment process.


# 🤖 Machine Learning Method: Clustering 

To achieve the project goal, we will apply a machine learning technique known as **clustering**. It is a method used in exploratory data analysis that allows you to discover hidden structures in data without prior knowledge of labels or categories.

Based on user characteristics and their suitability for specific roles, we aim to achieve a clustered dataset in such a way that items in the same group (cluster) are more similar to each other than to items in other groups.

This method will allow us to identify patterns and relationships and segment people into distinct groups, which will facilitate better assignment to projects. 

# 🧩 Solution

## Data Acquisition


The data used in this project was collected through a survey conducted among members of the WAIT organization. The survey aimed to gather information about participation preferences in projects, the participants' skills in various fields, and their engagement with the community. Members of WAIT could choose the form of participation, including involvement in data projects, organizing community life, or consuming content. Additionally, questions covered technical skills, business skills, and areas of industry interest.

##  Kedro

For this project, we decided to use Kedro, an open-source platform for data workflow management that integrates with Python and facilitates building scalable data pipelines. By utilizing Kedro, we gained better control over data quality and improved our ability to manage complex data analysis processes. This tool allowed us to organize and manage code in a modular and easily scalable manner. Thanks to Kedro, we were able to effectively model the team members' competencies, conduct advanced analyses, and create precise user clusters, which directly contributed to better human resource allocation and optimization of project management within the WAIT organization.

## Data preprocessing
In the initial data preprocessing stage, several operations were carried out to improve the quality and usability of the collected information.

### 1. Handling missing values
Several survey responses contained missing values and were removed from further analysis to ensure data consistency. We excluded individuals who are only interested in attending meetups but do not wish to participate in projects. Consequently, they did not indicate their technical skills, and thus were not considered in our analysis.

### 2. Additional data processing
To enhance data analysis, three boolean columns were added to the data sheet, representing forms of participation: involvement in data projects, organizing community life, and consuming content. These columns take values of 1 (true) or 0 (false) based on the respondents' answers. Subsequently, filtering was conducted to retain only individuals interested in participating in data projects, discarding other records. The resulting table is presented below.


| Value                                                                       | Consumer | Organizer | Participant |
| --------------------------------------------------------------------------- | -------- | --------- | ----------- |
| I want to organize the "life" Community                                     | 0        | 1         | 0           |
| I want to participate in the data project                                   | 0        | 0         | 1           |
| I want to participate in the data project and organize the "life" Community | 0        | 1         | 1           |
| For now, I want to draw, observe, "consume content"                         | 1        | 0         | 0           |


### 3. Removal of "business" columns
Eight columns related to "Industry Knowledge" were removed from the analysis because respondents misinterpreted these questions. Additionally, it was agreed that these columns would not be included in future surveys. Therefore, they were permanently removed from the data set.

### 4. Data transformation
Modifications were also made in the classification of respondents. Those who were uninterested and unaware were swapped places, allowing for more consistent groups in the dendrogram, thereby improving clustering results. Skill level schema is presented below. By converting these values, we are now able to treat these qualitative data as quantitative, which facilitates more advanced statistical analyses and modeling.

| Level | Description                                          |
| ----- | ---------------------------------------------------- |
| 0     | Unaware - I have not heard of this                   |
| 1     | Uninterested - I don't know, I prefer something else |
| 2     | Interested - I don't know, but I want to learn       |
| 3     | Competent - I know/am interested                     |
| 4     | Mentor - I know and want to help others              |

### 5. Skill scale adjustment
After consulting with the client, merging mentors with the "3" skill level group was rejected. This decision was based on recognizing the value of the declaration of willingness to teach others as a valuable distinction that deserves separate consideration. Therefore, distance scale adjustments were made to better reflect differences between skill level groups. This change aimed to provide more precise differentiation between groups with varying levels of expertise.

#### Scale Mapping

| Original Value | Converted Value |
|----------------|-----------------|
| 0              | 0               |
| 1              | 2               |
| 2              | 3               |
| 3              | 5               |
| 4              | 6               |


## Analysis

The data analysis process illustrates the pathway from the initial stage of generating a correlation matrix visualization, through deriving insights, to creating specific visualizations tailored to different stakeholder groups. Below is a description of the process, which encompasses a series of steps leading to valuable insights.

### Generating a correlation matrix visualization
The chart presents a correlation matrix that shows the relationships between various variables. 

<p align="center">
<img
    width="450"
		alt="Combined correlation matrix visualization"
		src="figures/corrMatrix_combined.png">

### Overall skill level distribution
The chart below highlights the distribution of skill levels among individuals for various technical and management skills. It shows areas of high interest and competence, such as SQL and Python, as well as areas where awareness and interest need to be increased, such as Computer Vision and Time Series Analysis. This data can be valuable for planning training programs and identifying skills that require more focus for development.
<br /><br />

<p align="center">
<img
    width="500"
		alt="Overall stacked bar visualization"
		src="figures/overall_stacked_bar_visual.png">
</p>

<details>
		<summary>Distributios details</summary>

#### Top by Count of Interested
This chart displays the count of people who are interested in each skill:
- Skills: MS SQL, AWS, Azure, Tableau, GPC, Power BI, Python, Computer Vision.
- Observation: Most skills show a significant proportion of green (Interested), with MS SQL and AWS having high interest levels. There's also a notable amount of red (Competent) and a small amount of purple (Mentor) in Python and Computer Vision.
- Summary: In skills where we observe a deficiency of competent individuals and mentors, there is a strategic opportunity to develop a community that nurtures and supports those with a significant interest in these areas. This approach will leverage existing interest to cultivate proficiency and expertise.
<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Top interested stacked bar visualization"
			src="figures/top_interested_stacked_bar_visual.png">
	</p>

#### Top by Count of Not Interested
This chart highlights the count of people not interested in each skill:
- Skills: Areas Time Series, Computer Vision, Bash, Docker, Areas NLP, Areas Classical ML, CLI, GPC.
- Observation: Blue (Not Interested) and orange (Unaware) dominate most bars, showing a lack of interest in these skills. There is still a substantial amount of green (Interested) and red (Competent) in skills like Computer Vision and Docker.
- Summary: The analysis reveals a notable lack of interest in certain skills. It is essential to assess the strategic importance of these skills to determine whether it is necessary to increase engagement or if the current level of interest is sufficient for organizational needs.
<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Top not interested stacked bar visualization"
			src="figures/top_not_interested_stacked_bar_visual.png">
	</p>

#### Top by Count of Unaware
This chart shows the count of people who are unaware of each skill:
- Skills: Project & Graf, Promo Social Media, Wsp. z adm. LEW, Ux/UI, Podst. z Finans, Frontend, Project Management, Matem. Rel. i Anuk.
- Observation: Blue (Not Interested) and orange Unaware are dominant, indicating high levels of unawareness. There's a significant amount of red (Competent) and green (Interested) in Project Management.
- Summary: The data indicates a considerable number of individuals are unaware of these skills. Leveraging the expertise of existing mentors and competent individuals could bridge this knowledge gap, enhancing awareness and interest through targeted educational initiatives.
<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Top Unaware stacked bar visualization"
			src="figures/top_unaware_stacked_bar_visual.png">
	</p>

</details>

## Clustering

The final result of the conducted analysis are the dendrograms presented below, which visualizes hierarchical clustering of the data. This involves iteratively combining the closest data points, thereby creating a hierarchy of clusters. The dendrograms illustrates this hierarchy in the form of a tree, where:

- the horizontal axis represents the IDs of WAIT organization members.
- the vertical axis represents the distance or diversity between clusters (the higher the level, the greater the diversity).
<br /><br />

### Hard skills hierarchical clustering visualization

<p align="center">
<img
    width="400"
		alt="Hard skills hierarchical clustering visualization"
		src="figures/hard_skills_grouping/hierarchical_clustering_visual.png">
</p>

### Soft skills hierarchical clustering visualization

<p align="center">
<img
    width="400"
		alt="Soft skills hierarchical clustering visualization"
		src="figures/soft_skills_grouping/hierarchical_clustering_visual.png">
</p>

Clusters are marked with different colors, which makes it easier to identify the groups. The height on the vertical axis at which two points or clusters are joined indicates the level of similarity (a lower level means greater similarity).

# 🗝️ Findings
 The analysis conducted for the WAIT organization's HR data yielded several significant insights, detailed below:

Despite our efforts, clustering individuals proved to be extremely challenging due to the fact that each person possesses unique skills and competencies. Apart from a few obvious groups, the differences between participants are significant, making it difficult to distinctly separate the groups. This difficulty is likely caused by the large number of skills, which complicates the clear distinction of groups. Additionally, this issue is related to the so-called "curse of dimensionality," where the increasing number of data dimensions complicates the analysis and interpretation of results.

We divided the sample and conducted two separate clusterings: one for soft skills and the other for hard skills. Soft skills were analyzed exclusively for organizers, while hard skills were examined only among participants. 

Below are the identified groups along with a brief description.

## Hard skills (Participants)

<details>
	<summary>Cluster 0 - All Rounders</summary>
	Individuals who possess a broad and deep set of competencies, with a significant portion eager to mentor and share their knowledge with others.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Hard skills group 0"
			src="figures/static/hard_skill_group/Group_0.png">
	</p>
</details>
<details>
	<summary>Cluster 1 - Newcomers</summary>
	Individuals who have developed competence in several areas but remain unfamiliar with others. Some have a clear vision of what they wish to learn next.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Hard skills group 1"
			src="figures/static/hard_skill_group/Group_1.png">
	</p>
	</details>
<details>
	<summary>Cluster 2 - All Rounders Wannabe</summary>
	Individuals who demonstrate proficiency in their current skills and exhibit a strong desire to expand their knowledge across all areas.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Hard skills group 2"
			src="figures/static/hard_skill_group/Group_2.png">
	</p>
	</details>
<details>
	<summary>Cluster 3 - Focused Experts</summary>
	Individuals who are proficient in their areas of expertise, have a clear understanding of their preferences, and are committed to refining specific skills.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Hard skills group 3"
			src="figures/static/hard_skill_group/Group_3.png">
	</p>
	</details>
<details>
	<summary>Cluster 4 - Focused Newcomers</summary>
	Individuals who have attained competence in their skills, recognize their preferences, but are still exploring the best paths for their future development.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Hard skills group 4"
			src="figures/static/hard_skill_group/Group_4.png">
	</p>
</details>

## Soft skills (Organizers)

<details>
	<summary>Cluster 0 - Project Menagers</summary>
	Individuals who demonstrate a strong, singular interest in the field of project management, consistently aiming to assume the role of Project Manager.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Soft skills group 0"
			src="figures/static/soft_skill_group/Soft_Skills_Group_0.png">
	</p>
</details>
<details>
	<summary>Cluster 1 - Networking Contact</summary>
	Individuals who prioritize and excel in building and nurturing professional relationships, emphasizing the importance of networking in their career growth.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Soft skills group 1"
			src="figures/static/soft_skill_group/Soft_Skills_Group_1.png">
	</p>
	</details>
<details>
	<summary>Cluster 2 - Growth Seekers</summary>
	Organizers at the beginning of their career journey, eager to learn and grow in their roles, showing potential for future leadership and organizational capabilities.	
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Soft skills group 2"
			src="figures/static/soft_skill_group/Soft_Skills_Group_2.png">
	</p>
	</details>
<details>
	<summary>Cluster 3 - Relationships Experts</summary>
	Individuals who possess an extensive network of valuable contacts, leveraging these relationships to create opportunities and drive mutual success.
	<br /><br />
	<p align="center">
	<img
		width="500"
			alt="Soft skills group 3"
			src="figures/static/soft_skill_group/Soft_Skills_Group_3.png">
	</p>
	</details>
<br />

# 📖 Additional sources of knowledge

- Morzy, T. (2013). Data mining. Warsaw: PWN Scientific Publishing House. ISBN 978-83-01-17175-9. Accessed: 19/05/2024. Available in Ibuk Libra: [link](https://libra.ibuk.pl/reader/exploracja-anych-tadeusz-morzy-63810)

- Walesiak, M. (2016). Generalized GDM distance measure in statistical multivariate analysis using the R program. Publishing House of the Wrocław University of Economics. ISBN: 978-83-7695-581-0. Available in Ibuk Libra: [link](https://www.ibuk.pl/fiszka/170477/uogolniona-miara-odleglosci-gdm-w-statystyczna-alizacja-wielowymiarowej-z-powiedzniem-programu-r.html)
