# 2024 United Kingdom General Election Analysis


### Table of Contents
- [Overview](#overview)
  - [Summary](#summary)
  - [Data Source](#data-source)
  - [Tools](#tools)
  - [Data Dictionary](#data-dictionary)
  - [Objectives](#objectives)
  - [References](#references)
- [Data Cleaning and Preperation](#data-cleaning-and-preperation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis (DA)](#data-analysis)
- [Results](#results)
  - [Conclusions](#conclusions)
  - [Limitations](#limitations)
- [License](#license)

## Overview

### Summary
This data project was made with the goal of analyzing and finding insights for the 2024 United Kingdom General Election. It focuses mainly on how each party's nominee did in every constituency 
### Data Source
2024 UK General Election Results: The primary dataset used for the analysis is in the 'HoC-GE2024-results-by-candidate.csv' file, which has shows the performances of , whether they are the incumbent, the race that they participated in, their vote share, along with information on which party they are affiliated with, what party the seat their contesting belongs to, and more.

Incumbent Data: A secondary dataset that for the analysis is in 'incumbent_data.csv' file, which shows who the most recent seat holder is for each race, when their term started, their decision in the upcoming election, and the partisan ratings for their contest.


### Tools

  -  Excel - Data Cleaning / Preperation
      -  Used to compile datasets from multiple sources and organize them into worksheets, for more thorough analysis.
  -  DB Browser (SQLite) - Data Analysis and Manipulation
      -  Used for querying and manipulating the data to gain insights into it.
  -  Python - Data Analysis and Visualization
     -   Employed to create charts and visualizations that would be challenging to produce in Tableau, along with performing statistical analyses to understand how variables might have affected a party's performance.
  -  Tableau - Data Visualization
     -   Utilized for visualizing the data.


### Data Dictionaries

#### 2024 / 2019 UK Election Relevant Columns

| Column       | Data Type       | Description                                                                      |
|-------------------|-------------|-----------------------------------------------------------------------------|
| `ONS ID`      | Text     | Index of the values.                        |
| `ONS region ID`    | Text | Index for the different regions.     |
| `Constituency name`      | Text     | Name of the constituency being contested.                |
| `County name`      | Text     | Name of the counties that the constituency resides in.                   |
| `Region name`      | Text     | Name of the region that the constituency resides in.  |
| `Country name`   | Text   | Name of the country that the constituency resides in.|
| `Constituency type` | Text     | The type of constituency, such as a county or borough.           |
| `Party name`      | Text     | The name of the party that the candidate is affiliated with.|
| `Party abbreviation`      | Text     | Abbreviation for the party that the candidate belongs to.|
| `Candidate First Name`      | Text     | First name of the candidate. |
| `Candidate surname`      | Text     | Surname of the candidate.  |
| `Candidate gender`      | Text     | Gender of the candidate.       |
| `Sitting MP`      | Text     | Whether the candidate is a sitting member of parliament.  |
| `Votes`      | Integer     | Whether the candidate was a member of parliament at some point. |
| `Share`      | Float     | Share of the  total votes that the candidate won.   |
| `Change`      | Text     | A measure on how well the Party did in comparison to the prior general election   |

#### 2024/2019 UK Election Holdings

| Column       | Data Type       | Description                                                                      |
|-------------------|-------------|-----------------------------------------------------------------------------|
| `Constituency`      | Text     | The constituency that is up for election; used as an index.              |
| `HeldBy`      | Text     | The party that held that seat last, before the election occured.  |

## Data Cleaning and Preperation
### Process
1. Manipulating the data to take into account only the parts that are useful for analyzing it.
2. Finding how the new constituency lines would have gone in the prior election.
3. Making separate tables for how many seats were held before the 2024 and 2019 election.
4. Making a table for the counties of the 2024 UK General in order to analyze how big an impact they had on votes. 


## Exploratory Data Analysis (EDA)

### SQL

1. What are the Top 15 most common surnames for candidates in the 2024 UK General Elections?

```sql
Select Candidatesurname, count(*) as Candidates from 'HoC-GE2024-results-by-candidate'
group by Candidatesurname order by Count(*) desc
limit 15;
```






</br></br>


![image](https://github.com/user-attachments/assets/6d9141dd-6da0-4239-b7f2-818f80abe023)


The surname "Smith" seems to be the most common surname for candidates that 

2. How many sitting Members of Parliament stood in each Country and Region?
```sql
-- Find the amount of Sitting MPs standing for reelection by the country and region their constituency belongs to. 
Select Countryname, Regionname, Count(*) from 'HoC-GE2024-results-by-candidate'
where SittingMP = 'Yes'
group by Countryname, Regionname;

```
</br></br>


![image](https://github.com/user-attachments/assets/04efdb61-fd27-4775-994a-0e4e2f12694f)

</br>
Most of the sitting MPs who stood for reelection were from England, with Scotland being at second place, Wales at third, and Northern Ireland at the bottom when it came to sitting MPs standing for reelection. 
</br>

For the regions in England, South East, North West and London had the most MPs standing for reelection. While the North East, East Midlands and East of England had the least MPs standing. 

3. How many votes did each country and region have cast in the 2024 UK General Elections
```sql
-- Total votes casted by each country and region for the General Election
Select Countryname, Regionname, Total(votes) as Votes from 'HoC-GE2024-results-by-candidate'
group by Countryname, Regionname;

```
The South East of England had the most votes casted in it, 
</br>
</br>

![image](https://github.com/user-attachments/assets/c01b50f3-d625-4731-8df0-82e34edb94c7)

4. How many different registered parties had candidates in the General Election by Country and Region
```sql
Select Countryname, Regionname, Count(distinct Partyname) as Parties from 'HoC-GE2024-results-by-candidate'
group by Countryname, Regionname
```
</br>
</br>

![image](https://github.com/user-attachments/assets/5cfec85e-b781-4b17-a77c-495b821250bd)





5. How many different people ran for a seat in parliament
```sql
-- Using different variables to find how many different candidates there were
Select count(distinct Candidatefirstname || candidatesurname || Regionname || Partyname || Constituencytype) + 2
as 'Distinct Candidates'
from 'HoC-GE2024-results-by-candidate' 
where Candidatesurname != 'Omilana';
```
</br>
</br>

![image](https://github.com/user-attachments/assets/662cb395-3f11-41e1-a725-0301ffb2a002)


</br>

This was a bit of a challenge to find the exact distinct count of candidates. Niko Omilana for example, was a candidate for 11 different constituencies, therefore he needed to be removed and added back by adding one to the final count. And there were two different Paul Kennedy's in Scotland, who were members of the Liberal Democrat party, which meant another person needed to be added to the count.



</br>
6. How many seats does each Party hold prior to the election?
```sql
-- How many seats did each party hold before the election
Select HeldBy, Count(*) from BeforeSeats
group by HeldBy
order by Count(*) desc;
```

</br>


![image](https://github.com/user-attachments/assets/b5033918-d7b0-4066-bf30-76930011c761)

</br>


The Conservative Party has a clear majority of seats in Parliament before the 2024 General. With 372 seats, or 57.23% of the chamber in their control, they have a seemingly comfortable government to work with. Labour on the other hand, has the second highest amount of seats in the House of Commons; they hold 200 of them, or 30.77% of seats. Which is a fair amount lower than what the Conservatives hold. The next biggest three include the Scottish National Party with 46 seats (7.09%), Liberal Democrats with 10 seats (1.54%), and the Democratic Unionist Party with 8 seats (1.23%).
### Python

#### Descriptive Statistics

#### Distributions
To better understand our numerical data, it is best that we visualize the numerical values, in order to get a better understanding of the spread that they have.

##### Votes

```py
```
</br>

![Untitled-1](https://github.com/user-attachments/assets/90f004c3-95e6-4d0e-8c9f-212e045cc101)



```py
```



##### Share and Change
</br>

![Untitled](https://github.com/user-attachments/assets/98aedfb3-e1bb-4c7c-a88c-fb0f88d25c52)
</br>
Now we should look at how share of votes that each candidate received, varied by the Party they were affiliated with.

```py
data.boxplot(column='Share', by='Party name', grid=False, vert=False, figsize=(50,100))

# Customize plot
plt.title('Boxplots by Category')
plt.suptitle('')  # Suppress the automatic 'Boxplot grouped by Category' title
plt.xlabel('Category')
plt.ylabel('Value')

```

</br>

![Untitled](https://github.com/user-attachments/assets/f37767c5-7aaf-4980-b68e-85d18abe0f7e)

</br>

The result shows that a handful of parties that fielded candidates, had those candidates get far larger shares than others. Some of these parties include Reform UK, Labour and its affiliates, the Conservative Party, Sinn Fein, the Scottish National Party, just to name a few. The fact that they do not have many outliers on the top, this seems to show that these are the parties that performed well throughout. 

</br>


#### Univariate Analysis

#### Comparitive Analysis 



## Data Manipulation
### Sorting Candidates and Ranking Their Performance

To make this data easier to analyze, these SQL queries 

```sql
-- Changing the name of the table to a simpler name
ALTER TABLE 'HoC-GE2024-results-by-candidate' RENAME TO 'Commons24';

-- Creating the 
Create Table Candidates as
Select Candidatefirstname || ' ' || Candidatesurname as Candidate,
  CandidateFirstName as Name,
  CandidateSurname as Surname,
  Partyname as Party,
  Candidategender as Gender,
  Constituencyname as Constituency,
  Regionname as Region,
  Countryname as Country,
  Case when SittingMP = 'Yes' then TRUE
  when SittingMP = 'No' then FALSE else null end as Incumbent, 
  Case when FormerMP = 'Yes' then TRUE
  when FormerMP = 'No' then FALSE else null end as 'Former MP',
  Votes, Share,
  dense_rank() over(partition by Constituencyname order by Votes desc) as Standing
from [Commons24] order by Countryname;

```


## Data Analysis



## Results

### Conclusions


### Limitations
