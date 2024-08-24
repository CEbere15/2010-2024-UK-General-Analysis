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
- [Exploratory Data Analysis](#exploratory-data-analysis-eda)
- [Data Analysis (DA)](#data-analysis-da)
- [Results](#results)
  - [Conclusions](#conclusions)
  - [Limitations](#limitations)
- [License](#license)

## Overview

### Summary
This data project was made with the goal of analyzing and finding insights for the 2024 United Kingdom General Election. It focuses mainly on how each party's nominee did in every constituency 
### Data Source



### Tools


### Data Dictionaries

#### 2024 UK Election Relevant Columns

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
### Sorting Candidates and Ranking Their Performance

To make this 

```sql
ALTER TABLE table_name RENAME TO new_table_name; 
```

## Exploratory Data Analysis



## Data Analysis



## Results

### Conclusions


### Limitations
