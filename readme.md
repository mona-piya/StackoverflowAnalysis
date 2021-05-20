<img src="https://stackoverflow.design/assets/img/logos/so/logo-stackoverflow.png" align="left" height="100" width="450" >
<br>
<br>
<br>
<h3 align="left">Project-S1-2021</h3>

<h3 align="left">Student Details</h3>

| Sno  | Name                        | StudentID |
| ---- | --------------------------- | --------- |
| 1    | Piyakorn Munegan            |           |
| 2    | Sabiha Sultana              |           |
| 3    | Sanjay Kanakkot Viswanathan |           |
| 4    | Mohammed Rizwan Amanullah   | 46277404  |


## Data Cleaning

As our first step, we started gathering information of all three dataset and looked into column that answers the question we have as part of our research. The below mentioned columns were choosen as keyfactors for our analysis

```python
['Age','ConvertedComp','Country','Currency','DevType','Employment',
'RaceEthnicity','Gender','SalaryType','Hobby','JobSatisfaction','JobSearchStatus',
'OperatingSystem','UndergradMajor','YearsCoding','YearsCodingProf',
'LanguageDesireNextYear','LanguageWorkedWith','FormalEducation']
```

Some of the column names are not easily understandable, for example, the column `ConvertedComp` stands for annual salary of the respondents in USD. We changed the name of column to `SalaryUSD` for easier understanding.
