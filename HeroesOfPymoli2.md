

```python
# Observed Trend 1 
# Age 20-25 has higher purchse count and total purchase value.

# Observed Trend 2
# Male gender has the highest total purchase value, purchase count, 
# and percentage of players.

# Observed Trend 3
# Item ID 39 and 84 has the higher number of purchase count at 11.
```


```python
# Dependencies
import pandas as pd
import numpy as np
```


```python
# Read CSV
purchase_data = pd.read_json("HeroesOfPymoli/purchase_data.json")
purchase_data1 = pd.read_json("HeroesOfPymoli/purchase_data2.json")
```


```python
# Players count
players_count = len(purchase_data["SN"].unique())
players_count_df = pd.DataFrame({"Total Players": players_count}, index=[0])
players_count_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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




```python
# Purchsing Analysis (Total)
unique_items = len(purchase_data["Item Name"].unique())
average_price = np.round(purchase_data["Price"].mean(),2)
purchase_number = purchase_data["Price"].count()
total_revenue = np.round(purchase_data["Price"].sum(),2)
purchase_data_df = pd.DataFrame({
    "Number of Unique Items": unique_items,
    "Average Price": average_price,
    "Number of Purchases": purchase_number,
    "Total Revenue": total_revenue
}, index=[0])
purchase_data_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>2.93</td>
      <td>780</td>
      <td>179</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
gender = purchase_data.groupby(["Gender"])["SN"].nunique()
gender_total = gender.sum()
gender_df = pd.DataFrame({
    "Total Players": gender,
    "Percentage of Players": round((gender/gender_total)*100,2)
})
gender_df = gender_df.sort_values(by=["Total Players"], ascending=False)
gender_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Players</th>
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
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
purchase_gender = purchase_data.groupby("Gender")
purchase_count = purchase_gender["Gender"].count()
purchase_total = np.round(purchase_gender["Price"].sum(),2)
purchase_price = np.round(purchase_gender["Price"].mean(),2)
normalized_total = np.round(purchase_total / gender,2)
purchase_df = pd.DataFrame({
    "Purchase Count": purchase_count,
    "Average Purchase Price": purchase_price,
    "Total Purchase Value": purchase_total,
    "Normalized Totals": normalized_total
})
purchase_df = purchase_df.sort_values(by=["Purchase Count"], ascending=False)
purchase_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <th>Male</th>
      <td>2.95</td>
      <td>4.02</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>2.82</td>
      <td>3.83</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>3.25</td>
      <td>4.47</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
bins = [0, 10, 15, 20, 25, 30, 35, 40, 45, 50]
group_names = ["< 10","10-15", "15-20", "20-25","25-30","30-35","35-40","40-45","45-50"]
pd.cut(purchase_data["Age"],bins, labels=group_names)
purchase_data["Age Group"]=pd.cut(purchase_data["Age"],bins, labels=group_names)
purchase_age_group = purchase_data.groupby("Age Group")
purchase_age = purchase_data["Age Group"].value_counts()
purchase_age_count = purchase_age_group["Price"].count()
purchase_age_total = np.round(purchase_age_group["Price"].sum(),2)
purchase_age_price = np.round(purchase_age_group["Price"].mean(),2)
normalized_age_total = np.round(purchase_age_total / purchase_age,2)
purchase_age_df = pd.DataFrame({
     "Purchase Count": purchase_age_count,
     "Average Purchase Price": purchase_age_price,
     "Total Purchase Value": purchase_age_total,
     "Normalized Totals": normalized_age_total
})
purchase_age_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-15</th>
      <td>2.87</td>
      <td>2.87</td>
      <td>78</td>
      <td>224.15</td>
    </tr>
    <tr>
      <th>15-20</th>
      <td>2.87</td>
      <td>2.87</td>
      <td>184</td>
      <td>528.74</td>
    </tr>
    <tr>
      <th>20-25</th>
      <td>2.96</td>
      <td>2.96</td>
      <td>305</td>
      <td>902.61</td>
    </tr>
    <tr>
      <th>25-30</th>
      <td>2.89</td>
      <td>2.89</td>
      <td>76</td>
      <td>219.82</td>
    </tr>
    <tr>
      <th>30-35</th>
      <td>3.07</td>
      <td>3.07</td>
      <td>58</td>
      <td>178.26</td>
    </tr>
    <tr>
      <th>35-40</th>
      <td>2.90</td>
      <td>2.90</td>
      <td>44</td>
      <td>127.49</td>
    </tr>
    <tr>
      <th>40-45</th>
      <td>2.88</td>
      <td>2.88</td>
      <td>3</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>45-50</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>&lt; 10</th>
      <td>3.02</td>
      <td>3.02</td>
      <td>32</td>
      <td>96.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
spenders = purchase_data.groupby(["SN"])
spenders_count = spenders["SN"].count()
spenders_price = np.round(spenders["Price"].mean(),2)
spenders_total = np.round(spenders["Price"].sum(),2)

spenders_data_df = pd.DataFrame({
    "Average Purchase Price": spenders_price,
    "Purchases Count": spenders_count,
    "Total Purchase Value": spenders_total
})
spenders_data_df = spenders_data_df.sort_values(by=["Purchases Count"], ascending=False)
spenders_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchases Count</th>
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
      <td>3.41</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.18</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>2.49</td>
      <td>4</td>
      <td>9.97</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.39</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Sondastan54</th>
      <td>2.56</td>
      <td>4</td>
      <td>10.24</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
popular = purchase_data.groupby(["Item ID","Item Name"])
popular_count = popular["Item ID"].count()
popular_price = popular["Price"].mean()
popular_total = popular["Price"].sum()
popular_data_df = pd.DataFrame({
    "Purchases Count": popular_count,
    "Item Price": popular_price,
    "Total Purchase Value": popular_total
})
popular_data_df
popular_data_df = popular_data_df.sort_values(by=["Purchases Count"], ascending=False)
popular_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchases Count</th>
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
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>1.24</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
profitable = purchase_data.groupby(["Item ID","Item Name"])
profitable_count = popular["Item ID"].count()
profitable_price = popular["Price"].mean()
profitable_total = popular["Price"].sum()
profitable_data_df = pd.DataFrame({
    "Purchases Count": profitable_count,
    "Item Price": profitable_price,
    "Total Purchase Value": profitable_total
})
profitable_data_df
profitable_data_df = profitable_data_df.sort_values(by=["Total Purchase Value"], ascending=False)
profitable_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchases Count</th>
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
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>4.25</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>4.95</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>4.87</td>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>3.61</td>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


