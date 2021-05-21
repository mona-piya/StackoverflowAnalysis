<img src="https://stackoverflow.design/assets/img/logos/so/logo-stackoverflow.png" align="left" height="100" width="450" >
<br>
<br>
<br>
<br>
<br>
<br>
<h3 align="centre">Project-S1-2021</h3>

<h3 align="left">Student Details</h3>

| Sno  | Name                        | StudentID |
| ---- | --------------------------- | --------- |
| 1    | Piyakorn Munegan            |           |
| 2    | Sabiha Sultana              |           |
| 3    | Sanjay Kanakkot Viswanathan |           |
| 4    | Mohammed Rizwan Amanullah   | 46277404  |

# Data Cleaning

<img src="https://images.unsplash.com/photo-1556155092-490a1ba16284?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80" height=500 width=800>

As our first step, we started gathering information of all three dataset and looked into column that answers the question we have as part of our research. The below mentioned columns were choosen as keyfactors for our analysis

```python
['Age','ConvertedComp','Country','Currency','DevType','Employment',
'RaceEthnicity','Gender','SalaryType','Hobby','JobSatisfaction','JobSearchStatus',
'OperatingSystem','UndergradMajor','YearsCoding','YearsCodingProf',
'LanguageDesireNextYear','LanguageWorkedWith','FormalEducation']
```

Some of the column names were not easily understandable, for example, the column `ConvertedComp` stands for annual salary of the respondents in USD. We changed the name of column to `SalaryUSD` for easier understanding. Similarly, name of below mentioned columns were changed

| Before renaming | After renaming  |
| :-------------: | :-------------: |
|   MainBranch    |   Profession    |
|    CareerSat    | JobSatisfaction |
|     ImpSyn      | CompetenceLevel |
|     JobSat      | CurrentJobSatis |
|     JobSeek     |    JobStatus    |

#### Data refactoring

Most of the columns values were more detailed and was difficult for analysis. For instance, the values in the `EdLevel` column were as below.

```
Computer science, computer engineering, or software engineering')
Another engineering discipline (ex. civil, electrical, mechanical)'),
Information systems, information technology, or system administration'),
Mathematics or statistics'),
I never declared a major'),
A natural science (ex. biology, chemistry, physics)')
A health science (ex. nursing, pharmacy, radiology)'),
Web development or web design'),
A business discipline (ex. accounting, finance, marketing)'),
A humanities discipline (ex. literature, history, philosophy)')
A social science (ex. anthropology, psychology, political science)')
Fine arts or performing arts (ex. graphic design, music, studio art)')
```

We refactored the column values with a simple values that explains respondents education level.

```python
def refactor_ed(df):
    '''function to change Education level category to Bachelors, Masters, Professional, Associate, Doctorate, No Degree'''
    conditions_ed = [(df['EdLevel'] == 'Bachelor’s degree (BA, BS, B.Eng., etc.)'),
                     (df['EdLevel'] == 'Master’s degree (MA, MS, M.Eng., MBA, etc.)'),
                     (df['EdLevel'] == 'Professional degree (JD, MD, etc.)'),   
                     (df['EdLevel'] == 'Associate degree'),
                     (df['EdLevel'] == 'Other doctoral degree (Ph.D, Ed.D., etc.)'),
                     (df['EdLevel'] == 'Some college/university study without earning a degree') 
                     | (df['EdLevel'] == 'Secondary school (e.g. American high school, German Realschule or Gymnasium, etc.)') 
                     | (df['EdLevel'] == 'Primary/elementary school')
                     | (df['EdLevel'] == 'I never completed any formal education')]

    choices_ed = ['Bachelors', 'Masters', 'Professional', 'Associate', 'Doctorate', 'No Degree']

    df['EdLevel'] = np.select(conditions_ed, choices_ed, default = np.NaN)
    
    return df

# applying function to subsets
survey_df_2019 = refactor_ed(survey_df_2019)
survey_df_2019['EdLevel'].replace('nan', 'Bachelors', inplace=True)
```

By doing so, reduced our column values to a simplest form

```
Bachelors       36494
No Degree       18542
Masters         17305
Associate        2585
Doctorate        2032
Professional     1037
```

Similary, we followed the same for other columns such as `Gender` `Profession` `UndergradMajor` `JobStatus` `Employment`
#### Categorising the data

One of our column `Ethnicity` had 173 values and had various subcategory. Some of the values are given below for reference.

```
'White or of European descent',
 'White or of European descent;Multiracial',
 'East Asian',
 'Black or of African descent;East Asian;Hispanic or Latino/Latina;Middle Eastern;Native American, Pacific Islander, or Indigenous Australian;South Asian;White or of European descent;Biracial;Multiracial',
 'Black or of African descent',
 'Hispanic or Latino/Latina;Multiracial',
 'Hispanic or Latino/Latina',
 'Middle Eastern',
 'South Asian',
 'Multiracial',
 'East Asian;South Asian',
 'Biracial',
 'Native American, Pacific Islander, or Indigenous Australian;White or of European descent',
 'Black or of African descent;White or of European descent;Biracial',
 'Middle Eastern;White or of European descent',
 ......
```

We categorised them into below categories.

| White or of European descent | 26848 |
| ---------------------------- | :---: |
| South Asian                  | 3783  |
| Hispanic or Latino           | 3072  |
| Middle Eastern               | 1840  |
| East Asian                   | 1661  |
| Black or of African descent  | 1633  |
| Southeast Asian              | 1371  |
| Multiracial                  |  249  |
| Biracial                     |  138  |
| Indigenous                   |  63   |

Code we wrote to categorise are given below.

```python
#combine Ethnicity by str.match(if each string starts with a match of a regular expression pattern)
df2020.loc[df['Ethnicity'].str.match('Biracial') == True, 'Ethnicity'] = 'Biracial'
df2020.loc[df['Ethnicity'].str.match('Black or of African descent') == True, 'Ethnicity'] = 'Black or of African descent'
df2020.loc[df['Ethnicity'].str.match('East Asian') == True, 'Ethnicity'] = 'East Asian'
df2020.loc[df['Ethnicity'].str.match('Hispanic or Latino') == True, 'Ethnicity'] = 'Hispanic or Latino'
df2020.loc[df['Ethnicity'].str.match('Indigenous') == True, 'Ethnicity'] = 'Indigenous'
df2020.loc[df['Ethnicity'].str.match('Middle Eastern') == True, 'Ethnicity'] = 'Middle Eastern'
df2020.loc[df['Ethnicity'].str.match('South Asian') == True, 'Ethnicity'] = 'South Asian'
df2020.loc[df['Ethnicity'].str.match('White or of European descent') == True, 'Ethnicity'] = 'White or of European descent'
df2020.loc[df['Ethnicity'].str.match('Multiracial') == True, 'Ethnicity'] = 'Multiracial'
```

The above process has been carried out for all three data frames `2018` `2019` `2020`

## Handling the null values and outliers

<img src="https://images.unsplash.com/photo-1536303100418-985cb308bb38?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80" height=500 width=800>

As with any large datasources, we had lot of null values that had to be handled. For instance, the 2020 datasource had 371,506 missing values(cells) in total.  Overview of how we handled null values for few of the columns are given below. Please check out jupyter notebook for how we handled null values in all other columns in all three dataset `2018` `2019` `2020`

#### Age 

Age column had 19,015 missing values in 2020 data frame. We filled the null values with the mean value of each gender. 

```python
#fill Age's null values with mean of each gender
means = df2020.groupby('Gender')['Age'].transform('mean')
df2020['Age'] = df2020['Age'].fillna(means)

#convert from float to int
df2020['Age'] = df2020['Age'].apply(str).str[:2]
df2020['Age'] = df2020['Age'].apply(int)
```

**outliers** ->  removed the respondents whose age are more than 60 years and less than 15 years. 

```python
#Cleaning Age's outliers from each gender)
df2020 = df2020[(df['Age'] >= 15) & (df2020['Age'] <= 60)]
```

For some of the columns, we made use of `bill()` and `fill()` method to fill the null values and made sure that the filling the values this way dosent change the order of values. Lets take an example of `DevType` column. 

| DevType                                                 | Before filling null | After bill() & fill() |
| ------------------------------------------------------- | ------------------- | --------------------- |
| full-stack                                              | 3399                | 3940                  |
| back-end                                                | 2374                | 2721                  |
| back-end;Developer, <br>front-end;Developer, full-stack | 1838                | 2146                  |
| back-end;Developer, full-stack                          | 1216                | 1411                  |
| front-end                                               | 1071                | 1229                  |
| mobile                                                  | 953                 | 1074                  |
| desktop or enterprise applications<br>full-stack        | 668                 | 779                   |
| front-end;Developer, full-stack                         | 667                 | 758                   |
| back-end;Developer, desktop or enterprise applications  | 528                 | 617                   |

As we can see above, order of values never changed while filling the null values with bfill and ffill method. 

For `EdLevel` we had only 993 null values. So we assigned it to bachelors degree as the number of respondents who have bachelors degree were 20,290.

All the null values were handled for all three data sets and ensured the dataset is as below.

| Columns(2020)          | Before handling null | After handling null |
| ---------------------- | :------------------: | :-----------------: |
| Age                    |        19015         |          0          |
| Gender                 |        13904         |          0          |
| SalaryUSD              |        29705         |          0          |
| Country                |         389          |          0          |
| DevType                |        15091         |          0          |
| Hobbyist               |          45          |          0          |
| EdLevel                |         7030         |          0          |
| Employment             |         607          |          0          |
| Ethnicity              |        18513         |          0          |
| CurrentJobSatis        |        19267         |          0          |
| JobStatus              |        12734         |          0          |
| LanguageDesireNextYear |        10348         |          0          |
| LanguageWorkedWith     |         7083         |          0          |
| Profession             |         299          |          0          |
| UndergradMajor         |        13466         |          0          |
| YearsCodePro           |        18112         |          0          |
| JobSeek                |         2153         |          0          |

## Data analysis

After cleaning and handling outliers in all three datasets, we started looking for valuable insights that we can draw from it.

<img src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80" height=500 width=800>

### Distribution of respondents based on country

We made use of `plotly` to create a geoplot showing where the respondents are from and how its been distributed around the world. We found that most of the respondents are from America. The second highest number of respondents are from India

<ADD IMAGE>

### Impact of participation rate due to different ethnicity

Consistent with data in all three years, We found that `white or european descent` has highest participation rate overall. 

```python
fig, ax = plt.subplots(figsize=(15, 5))
sns.barplot(x = count, y = participation_rate, palette = 'Set1')
plt.xlabel('Ethnicity', size = 16)
for i, v in enumerate(count):
    ax.text(  v+3,
              i-.15,
              f'{count[i]*100/sum(count):.2f}%',
              style = 'italic',
              fontsize=14,
              )
```

<ADD IMAGE>

### Most popular programming language in three years

We found javascript to be the most popular languages in all three years. Followed by HTML/CSS, SQL, and python. 

<ADD IMAGE>

### Distribution of developers based on their developer role

Most of the respondends  were either back-end or full stack developers. 

<ADD IMAGE>

### Salary distribution of top ten countries

Overall, the contry which has highest mean annual salary is United States of America(240,000) Dollars. The second highest country which provides highest mean salary is Australia(164,926) Dollars. Though India has higher number of respondents, it has lowest mean salary of $25,213 which shows that mean salary of developed country is much higher than that of developing countries.

```python
fig = plt.figure(figsize = (20, 10))

countries = cleaned_df_2019['Country'].value_counts().sort_values(ascending = False)[:10].index.tolist()

for i, country in enumerate(countries):
    plt.subplot(4, 3, i + 1)
    temp_salaries = cleaned_df_2019.loc[cleaned_df_2019['Country'] == country, 'SalaryUSD']

    ax = temp_salaries.plot(kind = 'kde')
    ax.axvline(temp_salaries.mean(), linestyle = '-', color = 'red')
    ax.text((temp_salaries.mean() + 1500), (float(ax.get_ylim()[1]) * 0.55), 'mean = $ ' + str(round(temp_salaries.mean(),2)), fontsize = 12)
    ax.set_xlabel('Annual Salary in USD')
    ax.set_xlim(-temp_salaries.mean(), temp_salaries.mean() + 2 * temp_salaries.std())
    ax.set_title('Annual Salary Distribution in {}'.format(country))

plt.tight_layout()
plt.show()
```

<ADD IMAGE>

### Analysis of impact of education on salary

The respondents who have done Doctorate has the highest mean salary among all other education level. Secondly, the respondents who has done Bachelors degree has more salary than that of Masters degree holders. This may be due to years of professional coding experience and due to the higher number of respondents in that category than that of Masters degree(No of respondends in Bachelor degree is 35659 and number of respondents in masters degree is 16940)

What is interesting is that the respondents who dont have any degree has a mean salary of $90k. This shows the improvement in online learning and advancement of technology that is shifting the company from relying on University degrees.

<ADD IMAGE>
