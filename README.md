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

#### 2024 UK Election Holdings

| Column       | Data Type       | Description                                                                      |
|-------------------|-------------|-----------------------------------------------------------------------------|
| `Constituency`      | Text     | The constituency that is up for election; used as an index.              |
| `HeldBy`      | Text     | The party that held that seat last, before the election occured.  |

## Data Cleaning and Preperation
### Process
1. For the
2. h
3. j
4. 


## Exploratory Data Analysis (EDA)

### Descriptive Statistics



#### SQL

1. What are the Top 15 most common surnames for candidates in the 2024 UK General Elections?

```sql
Select Candidatesurname, count(*) as Candidates from 'HoC-GE2024-results-by-candidate'
group by Candidatesurname order by Count(*) desc
limit 15;
```







##### Visualization: 
![image](https://github.com/user-attachments/assets/6d9141dd-6da0-4239-b7f2-818f80abe023)


The surname "Smith" seems to be the most common surname for candidates that 

2. How many sitting Members of Parliament stood in each Country and Region?
```sql
-- Find the amount of Sitting MPs standing for reelection by the country and region their constituency belongs to. 
Select Countryname, Regionname, Count(*) from 'HoC-GE2024-results-by-candidate'
where SittingMP = 'Yes'
group by Countryname, Regionname;

```

##### Visualization:
![image](https://github.com/user-attachments/assets/04efdb61-fd27-4775-994a-0e4e2f12694f)

Most of the sitting MPs who stood for reelection were from England, with Scotland being at second place, Wales at third, while Northern Ireland had the least sitting MPs standing for reelection.

3. How many votes did each country and region have cast in the 2024 UK General Elections
```sql
-- Total votes casted by each country and region for the General Election
Select Countryname, Regionname, Total(votes) as Votes from 'HoC-GE2024-results-by-candidate'
group by Countryname, Regionname;

```

##### Visualization:

![image](https://github.com/user-attachments/assets/c01b50f3-d625-4731-8df0-82e34edb94c7)

4. How many different registered parties had candidates in the General Election by Country and Region
```sql
Select Countryname, Regionname, Count(distinct Partyname) as Parties from 'HoC-GE2024-results-by-candidate'
group by Countryname, Regionname
```
##### Visualization:
![image](https://github.com/user-attachments/assets/5cfec85e-b781-4b17-a77c-495b821250bd)

5. How common is each type of constituency in each country

```sql
-- Find how common each type of constituency is by the country the constituency belongs to
with Ren as (
Select distinct Countryname as Country, Constituencyname, Constituencytype as Type
from 'HoC-GE2024-results-by-candidate')  
Select Country, Type, Count(*) as Constituencies from Ren group by Country, Type;
```




6. How many different people ran for a seat in parliament
```sql
-- Using different variables to find how many different candidates there were
Select count(distinct Candidatefirstname || candidatesurname || Regionname || Partyname || Constituencytype) + 2
as 'Distinct Candidates'
from 'HoC-GE2024-results-by-candidate' 
where Candidatesurname != 'Omilana';
```
##### Result:
![image](https://github.com/user-attachments/assets/662cb395-3f11-41e1-a725-0301ffb2a002)



This was a bit of a challenge to find the exact distinct count of candidates. Niko Omilana for example, was a candidate for 11 different constituencies, therefore he needed to be removed and added back by adding one to the final count. And there were two different Paul Kennedy's in Scotland, who were members of the Liberal Democrat party, which meant another person needed to be added to the count.



7. How many seats does each Party hold prior to the election?
```sql
-- How many seats did each party hold before the election
Select HeldBy, Count(*) from BeforeSeats
group by HeldBy
order by Count(*) desc;
```


##### Visualization: 

![image](https://github.com/user-attachments/assets/b5033918-d7b0-4066-bf30-76930011c761)



The Conservative Party has a clear majority of seats in Parliament before the 2024 General. With 372 seats, or 57.23% of the chamber in their control, they have a seemingly comfortable government to work with. Labour on the other hand, has the second highest amount of seats in the House of Commons; they hold 200 of them, or 30.77% of seats. Which is a fair amount lower than what the Conservatives hold. The next biggest three include the Scottish National Party with 46 seats (7.09%), Liberal Democrats with 10 seats (1.54%), and the Democratic Unionist Party with 8 seats (1.23%).
#### Python

## Data Manipulation
### Sorting Candidates and Ranking Their Performance

To make this data easier to analyze, these SQL queries 

```sql
-- Changing the name of the table to a simpler name
ALTER TABLE 'HoC-GE2024-results-by-candidate' RENAME TO 'Commons24';

-- Creating the 
Create Table Candidates as Select Candidatefirstname || ' ' || Candidatesurname as Candidate, Partyname as Party, Candidategender as Gender, Constituencyname as Constituency, Regionname as Region, Countryname as Country,
Case when SittingMP = 'Yes' then TRUE
when SittingMP = 'No' then FALSE else null end as Incumbent, 
Case when FormerMP = 'Yes' then TRUE
when FormerMP = 'No' then FALSE else null end as 'Former MP', Votes, Share, dense_rank() over(partition by Constituencyname order by Votes desc) as Standing from [Commons24] order by Countryname;

```


## Data Analysis



## Results

### Conclusions


### Limitations
