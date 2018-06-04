
# Analysis

*Of the 573 Players, majority are Male at 81%

*The average price of most popular game is $2.35

*The largest number of players are are between 20-24 years old


```python
# Dependencies
import os
import pandas as pd
```


```python
# Store filepath in a variable
file_json = os.path.join('purchase_data.json')
```


```python
# Read our Data file with the pandas library
json_df = pd.read_json(file_json)
json_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count


```python
#total number of rows. Can also be acheived using .shape
#.shape gives the dimensions of the dataframe can just grab the 0th element for row count
players = json_df['SN'].unique()
total_num_players = len(players)
total_players_df = pd.DataFrame({'Total Players': [total_num_players]})
total_players_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
#unique_items_df = json_df['Item Name'].value_counts()
unique_items = len(json_df['Item Name'].unique())

#'${:,.2f} means format with comma in thousands and round to two numbers
avg_purchase_price = json_df['Price'].mean()
avg_purchase_price = '${:,.2f}'.format(avg_purchase_price)

total_num_purchases = len(json_df)

total_revenue = json_df['Price'].sum()
total_revenue = '${:,.2f}'.format(total_revenue)

purchase_analysis_df = pd.DataFrame({
    'Number of Unique Items': [unique_items],
    'Average Price': [avg_purchase_price],
    'Number of Purchases': [total_num_purchases],
    'Total Revenue':[total_revenue]
}
)
purchase_analysis_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
#male_count_df = json_df.loc[json_df['Gender'] == 'Male',:]
#male_count = len(male_count_df['SN'].unique())
#above two lines can be written as line below
male_count = len(json_df.loc[json_df['Gender'] == 'Male',:]['SN'].unique())
male_percent = '{:.2f}'.format((male_count * 100) / total_num_players)

female_count = len(json_df.loc[json_df['Gender'] == 'Female',:]['SN'].unique())
female_percent = '{:.2f}'.format((female_count * 100) / total_num_players)


undisclosed_count = total_num_players - (male_count + female_count)
undisclosed_percent = '{:.2f}'.format((undisclosed_count * 100) / total_num_players)

gender_analysis_df = pd.DataFrame({
    'Gender': ['Male','Female','Other/Non-Disclosed'],
    'Total Count': [male_count,female_count,undisclosed_count],
    'Percentage of Players':[male_percent,female_percent,undisclosed_percent]
}
)
gender_analysis_df = gender_analysis_df.set_index('Gender')

gender_analysis_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
male_df = json_df.loc[json_df['Gender'] == 'Male',:]
male_purchase_count = len(male_df)
male_avg_purhcase_price = '${:.2f}'.format(male_df['Price'].mean())
male_total_purhcase_price = '${:,.2f}'.format(male_df['Price'].sum())

unique_male_users_df = male_df.groupby('SN').sum()
male_normalized_total = '${:.2f}'.format(unique_male_users_df['Price'].mean())

female_df = json_df.loc[json_df['Gender'] == 'Female',:]
female_purchase_count = len(female_df)
female_avg_purhcase_price = '${:.2f}'.format(female_df['Price'].mean())
female_total_purhcase_price = '${:,.2f}'.format(female_df['Price'].sum())

unique_female_users_df = female_df.groupby('SN').sum()
female_normalized_total = '${:.2f}'.format(unique_female_users_df['Price'].mean())

other_df = json_df.loc[(json_df['Gender'] != 'Male') & (json_df['Gender'] != 'Female'),:]
other_purchase_count = len(other_df)
other_avg_purhcase_price = '${:.2f}'.format(other_df['Price'].mean())
other_total_purhcase_price = '${:,.2f}'.format(other_df['Price'].sum())

unique_other_users_df = other_df.groupby('SN').sum()
other_normalized_total = '${:.2f}'.format(unique_other_users_df['Price'].mean())

purchase_analysis_df = pd.DataFrame({
    'Gender': ['Female','Male','Other/Non-Disclosed'],
    'Purchase Count': [female_purchase_count,male_purchase_count,other_purchase_count],
    'Average Purchase Price':[female_avg_purhcase_price,male_avg_purhcase_price,other_avg_purhcase_price],
    'Total Purchase Price':[female_total_purhcase_price,male_total_purhcase_price,other_total_purhcase_price],
    'Normalized Total': [male_normalized_total,female_normalized_total,other_normalized_total]
}
)
purchase_analysis_df = purchase_analysis_df.set_index('Gender')

purchase_analysis_df[['Purchase Count','Average Purchase Price','Total Purchase Price','Normalized Total']]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Price</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
age_bins = [0,9,14,19,24,29,34,39,200]
age_labels = ['<10','10-14','15-19','20-24','25-29','30-34','34-39','40+']

#copy df into new one
temp_df = json_df
temp_df['Age Range'] = pd.cut(json_df['Age'],age_bins,labels=age_labels)
temp_df = temp_df.drop_duplicates(['SN'],keep='last')

range_series = temp_df['Age Range'].value_counts()
#convert series to range
temp_df = pd.DataFrame({'Range':range_series.index, 'Total Count':range_series.values})

temp_df['Percentage of Players'] = ''
temp_df = temp_df.set_index('Range')
temp_df['Percentage of Players'] = (temp_df['Total Count'] / total_num_players) * 100

def two_digits(number):
    rounded = round(number, 2)
    return rounded
temp_df['Percentage of Players'] = temp_df['Percentage of Players'].map(two_digits)

temp_df.sort_index(inplace=True)
temp_df[['Percentage of Players','Total Count']]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>34-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
#print(json_df.head())
age_df = json_df.groupby('Age Range').Price.agg(['count','mean','sum'])
age_df['mean'] = age_df['mean'].map('${:,.2f}'.format)
age_df['sum'] = age_df['sum'].map('${:,.2f}'.format)

age_df = age_df.rename(columns={
    'count': 'Purchase Count',
    'mean': 'Average Purchase Price',
    'sum': 'Total Purchase Value'
})

unique_male_users_df = male_df.groupby('SN').sum()
male_normalized_total = '${:.2f}'.format(unique_male_users_df['Price'].mean())

age_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>34-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
spenders_df = json_df.groupby('SN').Price.agg(['count','mean','sum'])

spenders_df['mean'] = spenders_df['mean'].map('${:,.2f}'.format)
spenders_df['sum'] = spenders_df['sum'].map('${:,.2f}'.format)

spenders_df = spenders_df.rename(columns={
    'count': 'Purchase Count',
    'mean': 'Average Purchase Price',
    'sum': 'Total Purchase Value'
})

spenders_df.nlargest(5, 'Purchase Count')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Hailaphos89</th>
      <td>4</td>
      <td>$1.47</td>
      <td>$5.87</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popluar Items


```python
items_df = json_df.groupby(['Item ID','Item Name']).Price.agg(['count','mean','sum'])

items_df['mean'] = items_df['mean'].map('${:,.2f}'.format)
items_df['sum'] = items_df['sum'].map('${:,.2f}'.format)

items_df = items_df.rename(columns={
    'count': 'Purchase Count',
    'mean': 'Average Purchase Price',
    'sum': 'Total Purchase Value'
})

items_df.nlargest(5, 'Purchase Count')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Item


```python
#inital DF is the same as Most Popluar Item
#Most Profitable just pulls the ones that has bigges purchase value
items_df = json_df.groupby(['Item ID','Item Name']).Price.agg(['count','mean','sum'])

items_df = items_df.rename(columns={
    'count': 'Purchase Count',
    'mean': 'Average Purchase Price',
    'sum': 'Total Purchase Value'
})

summary_df = items_df.nlargest(5, 'Total Purchase Value')
summary_df['Average Purchase Price'] = items_df['Average Purchase Price'].map('${:,.2f}'.format)
summary_df['Total Purchase Value'] = items_df['Total Purchase Value'].map('${:,.2f}'.format)
summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


