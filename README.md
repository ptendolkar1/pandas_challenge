# Below are the 3 most important trends we think that could be used to develop a top level companywide strategy for incerasing 
# revenue, profits, and sustainable long term growth.
#
# OBSERVED TREND1: The revenue and the number of purchases are not too dependent on the items offered for sale. See data below.
#    1st datafile results - Purchases 780, Revenue $2287, items 183
#    2nd datafile results - Purchases 78,  Revenue $228,  items 64
#    Conclusion - Company gets 10 times the purchases and revenue, without offering 10 times the items for sale. 
#                 So company need not invest in designing/offering new items unless there is a specific reason such as, 
#                 specific items to attract specific age-group, etc. 
#
# OBSERVED TREND2: In datafile2, many of the 'most popular' and 'most profitable' items are the same. This is the most desirable
#    combination in product offerings. Company should investigate the product characteristics AND other reasons why these items 
#    rank high in both categories. Then the company should develop the design/development guidelines for their design teams 
#    accordingly and also take actions according to other reasons, such as, the marketing maybe more appealing for those items, 
#    etc. They should also invest in modifying/tweaking other existing items accordingly as much as possible. 
# 
# OBSERVED TREND3: In both datafiles, the female players are way low in numbers. Though this observation is very obvious, we 
#    see a huge potential for long term growth and so a great opportunity! The company should launch a well designed 
#    survey among female players and also female non-players to gather their likings, expectations, behavioral patterns, etc, 
#    and then figure out how to attract more female players. The company should employ female player bloggers and be active 
#    in social media to accomplish this goal.
    

```python
import pandas as pd 
import json
import numpy as np
```


```python
t# Read data file and create dataframe. 

json_path1 = "./purchase_data2.json"
purchase_data = pd.read_json(json_path1)
purchase_df = pd.DataFrame(purchase_data)

# Count players by using unique function

player_count = len(purchase_df["SN"].unique()) 
player_count_table = pd.DataFrame({"Total Players":[player_count]})

print("\nPlayer Count\n")
player_count_table
```

    
    Player Count
    
    




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
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate asked values by using standard functions, such as, unique, mean, len, sum

number_unique_items = len(purchase_df["Item ID"].unique())   
average_purchase_price = purchase_df["Price"].mean()
total_number_of_purchases = len(purchase_df["Item ID"])
total_revenue = purchase_df["Price"].sum()

# Create table and fill it with required data. Format amounts with '$' sign & 2 decimal places

purchasing_analysis_table = pd.DataFrame({"Number of Unique Items":[number_unique_items],
                                          "Average Price":["$%.2f" % average_purchase_price],
                                          "Number of Purchases":[total_number_of_purchases],
                                          "Total Revenue ":["$%.2f" % total_revenue]})

print("\nPurchasing Analysis (Total)\n")
purchasing_analysis_table
```

    
    Purchasing Analysis (Total)
    
    




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
      <td>$2.92</td>
      <td>78</td>
      <td>64</td>
      <td>$228.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create new dataframe of all players
unique_players_df = purchase_df.drop_duplicates("SN")     

# create new dataframes of each category - unique male players, unique female players, unique other players                                         
unique_male_df = unique_players_df.loc[unique_players_df["Gender"] == "Male", :]  
unique_female_df = unique_players_df.loc[unique_players_df["Gender"] == "Female", :]
unique_other_df = unique_players_df.loc[unique_players_df["Gender"] == "Other / Non-Disclosed", :]

# calculate percentages of male, female, and other players 
percent_male = (len(unique_male_df["SN"])/len(unique_players_df["SN"]))*100
percent_female = (len(unique_female_df["SN"])/len(unique_players_df["SN"]))*100
percent_other = (len(unique_other_df["SN"])/len(unique_players_df["SN"]))*100

#create gender_demographics_table
gender_demographics_table = pd.DataFrame({"Gender":["Male", "Female", "Other / Non-Disclosed"],
                                "Percentage of Players":["%.2f" % percent_male, "%.2f" % percent_female, "%.2f" % percent_other],
                                "Total Count":[len(unique_male_df["SN"]), len(unique_female_df["SN"]), len(unique_other_df["SN"])]})
gender_demographics_table.set_index('Gender', inplace=True)
print("\nGender Demographics \n")
gender_demographics_table
```

    
    Gender Demographics 
    
    




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
      <td>81.08</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.57</td>
      <td>13</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.35</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create new dataframes of each category - male players, female players, other players                                         
male_df = purchase_df.loc[purchase_df["Gender"] == "Male", :]  
female_df = purchase_df.loc[purchase_df["Gender"] == "Female", :]
other_df = purchase_df.loc[purchase_df["Gender"] == "Other / Non-Disclosed", :]

Purchase_Count_male = len(male_df["Item ID"])   
Purchase_Count_female = len(female_df["Item ID"])
Purchase_Count_other = len(other_df["Item ID"])

Average_Purchase_Price_male = male_df["Price"].sum()/Purchase_Count_male
Average_Purchase_Price_female = female_df["Price"].sum()/Purchase_Count_female
Average_Purchase_Price_other = other_df["Price"].sum()/Purchase_Count_other

Total_Purchase_Value_male = male_df["Price"].sum()
Total_Purchase_Value_female = female_df["Price"].sum()
Total_Purchase_Value_other = other_df["Price"].sum()

Normalized_Totals_male = male_df["Price"].mean()
Normalized_Totals_female = female_df["Price"].mean()
Normalized_Totals_other = other_df["Price"].mean()

gender_purchase_table = pd.DataFrame({" ":["Gender", "Female", "Male", "Other / Non-Disclosed"],
                                "Purchase Count":["", Purchase_Count_female, Purchase_Count_male, Purchase_Count_other],
                                "Average Purchase Price":["", "$%.2f" % Average_Purchase_Price_female, "$%.2f" % Average_Purchase_Price_male, "$%.2f" % Average_Purchase_Price_other],
                                "Total Purchase Value":["" , "$%.2f" % Total_Purchase_Value_female, "$%.2f" % Total_Purchase_Value_male, "$%.2f" % Total_Purchase_Value_other],      
                                "Normalized Totals":["" , "$%.2f" % Normalized_Totals_female, "$%.2f" % Normalized_Totals_male, "$%.2f" % Normalized_Totals_other]})
gender_purchase_table.set_index(" ", inplace=True)

print("\nPurchasing Analysis (Gender)\n")
gender_purchase_table
```

    
    Purchasing Analysis (Gender)
    
    




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
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gender</th>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>Female</th>
      <td>$3.18</td>
      <td>$3.18</td>
      <td>13</td>
      <td>$41.38</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.88</td>
      <td>$2.88</td>
      <td>64</td>
      <td>$184.60</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$2.12</td>
      <td>$2.12</td>
      <td>1</td>
      <td>$2.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create bins as requested
age_bins = [0,9,14,19,24,29,34,39,150] 

# Create labels for these bins
bin_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#Create a reduced_df with relevant data /columns
reduced_df = purchase_df[["Age", "Price"]]

# Slice the data and place it into bins
# Place the data series into a new column inside of the DataFrame

reduced_df["Age Group"] = pd.cut(reduced_df["Age"],age_bins,labels=bin_labels)
Age_Group = reduced_df.groupby("Age Group")

Purchase_Count = Age_Group.size().to_frame(name= "Purchase_Count")          
Average_Purchase_Price = Age_Group["Price"].mean().to_frame(name= "Average_Purchase_Price")   
Total_Purchase_Value = Age_Group["Price"].sum().to_frame(name= "Total_Purchase_Value")     
Normalized_Totals = Age_Group["Price"].sum().to_frame(name= "Normalized_Totals")

Age_Demo_df = pd.concat([Purchase_Count, Average_Purchase_Price, Total_Purchase_Value, Normalized_Totals], axis=1)
Age_Demo_df['Average_Purchase_Price'] = Age_Demo_df['Average_Purchase_Price'].map("${:.2f}".format)
Age_Demo_df['Total_Purchase_Value'] = Age_Demo_df['Total_Purchase_Value'].map("${:.2f}".format)
Age_Demo_df['Normalized_Totals'] = Age_Demo_df['Normalized_Totals'].map("${:.2f}".format)

print("\nPurchasing Analysis (Age)\n")
Age_Demo_df

```

    
    Purchasing Analysis (Age)
    
    

    C:\Users\Prakash\Anaconda\lib\site-packages\ipykernel_launcher.py:13: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      del sys.path[0]
    




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
      <th>Purchase_Count</th>
      <th>Average_Purchase_Price</th>
      <th>Total_Purchase_Value</th>
      <th>Normalized_Totals</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>$2.76</td>
      <td>$13.82</td>
      <td>$13.82</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>$2.99</td>
      <td>$8.96</td>
      <td>$8.96</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>$2.76</td>
      <td>$30.41</td>
      <td>$30.41</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>$3.02</td>
      <td>$108.89</td>
      <td>$108.89</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>$2.90</td>
      <td>$26.11</td>
      <td>$26.11</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>$1.98</td>
      <td>$13.89</td>
      <td>$13.89</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>$3.56</td>
      <td>$21.37</td>
      <td>$21.37</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>$4.65</td>
      <td>$4.65</td>
      <td>$4.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
SN_Group = purchase_df.groupby("SN")

Purchase_Count = SN_Group.size().to_frame(name= "Purchase_Count")           
Average_Purchase_Price = SN_Group["Price"].mean().to_frame(name= "Average_Purchase_Price")   
Total_Purchase_Value = SN_Group["Price"].sum().to_frame(name= "Total_Purchase_Value")     

SN_df = pd.concat([Purchase_Count, Average_Purchase_Price, Total_Purchase_Value], axis=1)
SN_df['Average_Purchase_Price'] = SN_df['Average_Purchase_Price'].map("${:.2f}".format)
SN_df['Total_Purchase_Value'] = SN_df['Total_Purchase_Value'].map("${:.2f}".format)

SN1_df = pd.DataFrame(SN_df)

# Sort the DataFrame by the values in the "Total_Purchase_Value" column
SN1_df = SN1_df.sort_values("Total_Purchase_Value", ascending=False)

print("\nTop Spenders\n")
SN1_df.head()
```

    
    Top Spenders
    
    




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
      <th>Purchase_Count</th>
      <th>Average_Purchase_Price</th>
      <th>Total_Purchase_Value</th>
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
      <th>Sundaky74</th>
      <td>2</td>
      <td>$3.71</td>
      <td>$7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>$2.56</td>
      <td>$5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>$4.81</td>
      <td>$4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>$4.78</td>
      <td>$4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>$4.71</td>
      <td>$4.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_group = purchase_df.groupby("Item ID")

Purchase_Count = item_group["Item ID"].size().to_frame(name= "Purchase Count")  
Item_Price = item_group["Price"].mean().to_frame(name= "Item_Price")  
Item_Name = item_group["Item Name"].unique().to_frame(name= "Item Name") 
Total_Purchase_Value = item_group["Price"].sum().to_frame(name= "Total_Purchase_Value")

item_group_df = pd.concat([Item_Name, Purchase_Count, Item_Price, Total_Purchase_Value], axis=1)

item_group_df['Total_Purchase_Value'] = item_group_df['Total_Purchase_Value'].map("${:.2f}".format)
item_group_df['Item_Price'] = item_group_df['Item_Price'].map("${:.2f}".format)

item_group1_df = pd.DataFrame(item_group_df)

# Sort the DataFrame by the values in the "Purchase Count" column
item_group1_df = item_group1_df.sort_values("Purchase Count", ascending=False)

print("\nMost Popular Items\n")
item_group1_df.head()     # 5 most popular items by Purchase Count 

```

    
    Most Popular Items
    
    




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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item_Price</th>
      <th>Total_Purchase_Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>94</th>
      <td>[Mourning Blade]</td>
      <td>3</td>
      <td>$3.64</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>90</th>
      <td>[Betrayer]</td>
      <td>2</td>
      <td>$4.12</td>
      <td>$8.24</td>
    </tr>
    <tr>
      <th>111</th>
      <td>[Misery's End]</td>
      <td>2</td>
      <td>$1.79</td>
      <td>$3.58</td>
    </tr>
    <tr>
      <th>64</th>
      <td>[Fusion Pummel]</td>
      <td>2</td>
      <td>$2.42</td>
      <td>$4.84</td>
    </tr>
    <tr>
      <th>154</th>
      <td>[Feral Katana]</td>
      <td>2</td>
      <td>$4.11</td>
      <td>$8.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Sort the DataFrame by the values in the "Total_Purchase_Value" column
item_group1_df = item_group1_df.sort_values("Total_Purchase_Value", ascending=False)

print("\nMost Profitable Items\n")
item_group1_df.head()     # 5 most profitable items by Total_Purchase_Value
```

    
    Most Profitable Items
    
    




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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item_Price</th>
      <th>Total_Purchase_Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117</th>
      <td>[Heartstriker, Legacy of the Light]</td>
      <td>2</td>
      <td>$4.71</td>
      <td>$9.42</td>
    </tr>
    <tr>
      <th>93</th>
      <td>[Apocalyptic Battlescythe]</td>
      <td>2</td>
      <td>$4.49</td>
      <td>$8.98</td>
    </tr>
    <tr>
      <th>90</th>
      <td>[Betrayer]</td>
      <td>2</td>
      <td>$4.12</td>
      <td>$8.24</td>
    </tr>
    <tr>
      <th>154</th>
      <td>[Feral Katana]</td>
      <td>2</td>
      <td>$4.11</td>
      <td>$8.22</td>
    </tr>
    <tr>
      <th>180</th>
      <td>[Stormcaller]</td>
      <td>2</td>
      <td>$2.77</td>
      <td>$5.54</td>
    </tr>
  </tbody>
</table>
</div>


