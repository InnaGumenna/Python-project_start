# Python-project_start
The 1st project using Python (Pandas)

import pandas as pd
df=pd.read_csv('survey_results_public.csv')
df.head()

#міняємо в назвах стовбців на маленькі літери та заміняємо пробіли
df.columns=df.columns.str.lower().str.replace(' ','_')

# 1 Скільки респондентів пройшли опитування?
count_survey_completed = df['q120'].count()
print ('#1 Скільки респондентів пройшли опитування?= ', count_survey_completed)

# 2 Скільки респондентів відповіли на всі запитання?
column_counts = df.count()
min_column_counts = column_counts.min()
print('#2 Скільки респондентів відповіли на всі запитання? = ',min_column_counts)

# 3 Які значення мір центральної тенденції для досвіду (WorkExp) респондентів?
workexp_mean = df['workexp'].mean().round(1)
workexp_median = df['workexp'].median()
print('#3 Які значення мір центральної тенденції для досвіду (WorkExp) респондентів?')
print( 'Середнє WorkExp-',workexp_mean,'  Медіана WorkExp-',workexp_median)

# 4 Скільки респондентів працює віддалено?
remotework_count = df[
    (df['remotework']=='Remote')
].remotework.count()
print('#4 Скільки респондентів працює віддалено? =',remotework_count)

# 5 Який відсоток респондентів програмує на Python?
python_contained_sum = df['languagehaveworkedwith'].str.contains('Python',case=False,na=False, regex=False).sum()
lang_worker_count = df.languagehaveworkedwith.count()
python_percent = ((python_contained_sum/lang_worker_count)*100).round(2)
print('#5 Який відсоток респондентів програмує на Python?= ',python_percent,'%' )

# 6 Скільки респондентів навчалося програмувати за допомогою онлайн курсів?
learn_by_online_courses = df['learncode'].str.contains('Online Courses', case=False, na=False, regex=False).sum()
print('#6 Скільки респондентів навчалося програмувати за допомогою онлайн курсів?= ',learn_by_online_courses)

# 7 Серед респондентів що програмують на Python в групуванні по країнам, яка середня та медіанна сума компенсації  (ConvertedCompYearly) в кожній країні?
country_grouped_python = df[
    (df['languagehaveworkedwith'].str.contains('Python',case=False,na=False, regex=False))].\
    groupby('country').\
    agg({
        'convertedcompyearly':['mean','median']
        }).round()
print('#7 Серед респондентів, що програмують на Python в групуванні по країнам,яка середня та медіанна сума компенсації (ConvertedCompYearly) в кожній країні?')
print(country_grouped_python)

# 8 Які рівні освіти мають 5 респондентів з найбільшою компенсацією?
sorted_compensation = df.sort_values(by='convertedcompyearly',ascending=False).reset_index()
highest_compensation =  sorted_compensation.loc[0:5,('edlevel') ] 
print('#8 Які рівні освіти мають 5 респондентів з найбільшою компенсацією?')
print(highest_compensation)


# Бонусні запитання:1-В кожній віковій категорії, який відсоток респондентів програмує на Python?
groupby_age_count = df.groupby('age').\
agg({
    'languagehaveworkedwith':'count'
    })
groupby_age_python = df[
    (df['languagehaveworkedwith'].str.contains('Python',case=False,na=False, regex=False))].\
    groupby('age').\
    agg({
        'languagehaveworkedwith':'count'
        })
percent_coding_python = ((groupby_age_python/groupby_age_count)*100).round()
print('В кожній віковій категорії, який відсоток респондентів програмує на Python?')
print(percent_coding_python)

# Бонусні запитання:2-Серед респондентів що знаходяться в 75 перцентилі за компенсацією середнього і працюють віддалено, які індустрії є найрозповсюдженішими?
percentile_75_compensation = df[
    (df['remotework']=='Remote')
].convertedcompyearly.quantile(0.75)
print(percentile_75_compensation)


# Бонусні запитання:2-Серед респондентів що знаходяться в 75 перцентилі за компенсацією середнього і працюють віддалено, які індустрії є найрозповсюдженішими?
df_temp = df[
    (df['remotework']=='Remote') & (df['convertedcompyearly']>142759.0) 
    ].\
    groupby('industry').count()
df_temp_1 = df_temp.sort_values(by='remotework',ascending=False)
print('Серед респондентів, що знаходяться в 75 перцентилі за компенсацією середнього і працюють віддалено,які індустрії є найрозповсюдженішими?')
print(df_temp_1['remotework'])
