
import pandas as pd
import numpy as np


```python
file = "purchase_data.json"
itemPurch = pd.read_json(file)

normPurch = itemPurch.groupby("SN")
itemPurch.head(10)


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
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>




```python
# BASIC SUMMARY
# number of available items
items = itemPurch["Item ID"].unique()
numItems = len(items)

# average price of each purchase
avgPurch = itemPurch["Price"].mean()

# number of recorded purchases
numPurch = len(itemPurch)

#total revenue of recorded purchases
totalRev = itemPurch["Price"].sum()

#generate df of results
purchAnalysis = pd.DataFrame({"Number of Items":[numItems],
                              "Average Purchase Price":[avgPurch],
                              "Number of Purchases":[numPurch],
                              "Total Revenue":[totalRev]})
purchAnalysis = purchAnalysis.reindex(columns=["Number of Items","Average Purchase Price","Number of Purchases","Total Revenue"])

# format avg. purchase and total revenue as USD$
purchAnalysis["Average Purchase Price"]= ['${0:.2f}'.format(val) for val in purchAnalysis["Average Purchase Price"]]
purchAnalysis["Total Revenue"]= ['${0:.2f}'.format(val) for val in purchAnalysis["Total Revenue"]]

purchAnalysis
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
      <th>Number of Items</th>
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DEMOGRAPHIC ANALYSIS BY GENDER GROUP
# remove extra entries info from users who logged more than one purchase
uniqueSN = itemPurch.drop_duplicates(subset=["SN"])

# group the DF by gender to find count of users per gender
byGender = uniqueSN.groupby("Gender")
genders = pd.DataFrame(byGender["SN"].count())
genders = genders.rename(columns = {"SN":"Count of Players"})

# calculate each groups' percentage of total playerbase
totalPlyrs = genders["Count of Players"].sum()
genders["% of Players"] = genders["Count of Players"]/totalPlyrs
genders["% of Players"] = ['{0:.2f}%'.format(val*100) for val in genders["% of Players"]]

genders

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
      <th>Count of Players</th>
      <th>% of Players</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.45%</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.15%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.40%</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS BY GENDER GROUP
# group DF of purchases by gender
genBuys = itemPurch.groupby("Gender")

# calculate mean purchase price by group and format as USD$
genPurch = pd.DataFrame(genBuys["Price"].mean())
genPurch["Price"] = ['${0:.2f}'.format(val) for val in genPurch["Price"]]

# calculate total revenue by group and format as USD$
genPurch["Total Revenue"] = genBuys["Price"].sum()
genPurch["Total Revenue"] = ['${0:.2f}'.format(val) for val in genPurch["Total Revenue"]]

# calculate number of purchases by group
genPurch["Number of Purchases"] = genBuys["Price"].count()
genPurch = genPurch.rename(columns={"Price":"Average Purchase"})

# to normalize, group by Gender and SN
uniqueSNs = itemPurch.groupby(["Gender","SN"])
# normPurch finds mean purchase price for each SN, while holding the Gender of each SN
normPurch = pd.DataFrame(uniqueSNs["Price"].mean())
# unstack and find the mean of each row (Male, Female, Nonspecific)
normPurch = normPurch.unstack()
normTotals = pd.DataFrame(normPurch.mean(axis=1))

# format normalized averages to USD$ and join with summary table
genPurch = genPurch.join(normTotals)
genPurch[0] = ['${0:.2f}'.format(val) for val in genPurch[0]]
genPurch = genPurch.rename(columns={0:"Normalized Average Purchase"})

genPurch

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
      <th>Average Purchase</th>
      <th>Total Revenue</th>
      <th>Number of Purchases</th>
      <th>Normalized Average Purchase</th>
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>136</td>
      <td>$2.81</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>633</td>
      <td>$2.95</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>11</td>
      <td>$3.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DEMOGRAPHIC ANALYSIS BY AGE
# create bins to group player age by
# use uniqueSN df, no repeat entries of players
ageBins = np.arange(9,50,5)
ageBins = np.insert(ageBins,0,int(0))
ageBins

#create labels for the bins
binLabels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40-44","45-49"]

#group the df by age range
byAge = pd.cut(uniqueSN["Age"],ageBins,labels=binLabels)

byAge = pd.DataFrame(byAge.value_counts())
byAge = byAge.reindex(binLabels)
byAge = byAge.rename(columns={"Age":"Number of Players"})

# calculate percentage of total players in each age range, format as %
byAge["% of Total"] = byAge["Number of Players"] / (len(uniqueSN["SN"])) 
byAge["% of Total"] = ['{0:.2f}%'.format(val*100) for val in byAge["% of Total"]]
byAge
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
      <th>Number of Players</th>
      <th>% of Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>19</td>
      <td>3.32%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>23</td>
      <td>4.01%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>100</td>
      <td>17.45%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>259</td>
      <td>45.20%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>87</td>
      <td>15.18%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>47</td>
      <td>8.20%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>27</td>
      <td>4.71%</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>10</td>
      <td>1.75%</td>
    </tr>
    <tr>
      <th>45-49</th>
      <td>1</td>
      <td>0.17%</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASE ANALYSIS BY AGE GROUP
# establish bins to analyze purchase data by age group
ageBins = np.arange(9,50,5)
ageBins = np.insert(ageBins,0,int(0))
binLabels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40-44","45-49"]

# create new column in df by binning age into ranges
itemPurch["Age Group"] = pd.cut(itemPurch["Age"],ageBins,labels=binLabels)
buyAge = itemPurch.groupby("Age Group")

# create 3 dataframes that hold purchase info for each age group
purchCount = pd.DataFrame(buyAge["SN"].count()).rename(columns={"SN":"Number of Purchases"})
avgPrice = pd.DataFrame(buyAge["Price"].mean()).rename(columns={"Price":"Average Purchase Value"})
ageTotal = pd.DataFrame(buyAge["Price"].sum()).rename(columns={"Price":"Total Revenue"})

# join the dataframes on their index, "Age Group" (default)
ageMerge = purchCount.join(avgPrice,how='outer')
ageMerge = ageMerge.join(ageTotal,how='outer')

# to normalize the average purchases and remove redundancy, group by age group and screenname
uniqueAges = itemPurch.groupby(["Age Group","SN"])
# make a dataframe of the average purchase per SN
normAge = pd.DataFrame(uniqueAges["Price"].mean())

# unstack and take the sum of each row to find gender group averages, normalized
normAge = normAge.unstack()
normAge = pd.DataFrame(normAge.mean(axis=1))

# merge with ageMerge, format as USD$ and relabel column
ageMerge = ageMerge.join(normAge)
ageMerge[0] = ['${0:.2f}'.format(val) for val in ageMerge[0]]
ageMerge = ageMerge.rename(columns={0:"Normalized Average Purchase"})

# format average spending and total revenue to USD$
ageMerge["Average Purchase Value"] = ['${0:.2f}'.format(val) for val in ageMerge["Average Purchase Value"]]
ageMerge["Total Revenue"] = ['${0:.2f}'.format(val) for val in ageMerge["Total Revenue"]]
ageMerge.fillna("0",inplace=True)

ageMerge

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
      <th>Number of Purchases</th>
      <th>Average Purchase Value</th>
      <th>Total Revenue</th>
      <th>Normalized Average Purchase</th>
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
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$3.00</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$2.75</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$2.89</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$3.01</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$3.16</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$2.80</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>16</td>
      <td>$3.19</td>
      <td>$51.03</td>
      <td>$3.22</td>
    </tr>
    <tr>
      <th>45-49</th>
      <td>1</td>
      <td>$2.72</td>
      <td>$2.72</td>
      <td>$2.72</td>
    </tr>
  </tbody>
</table>
</div>




```python
# BIGGEST SPENDERS IN PYMOLI
# group purchase df by screenname (SN) to see who bought the most
highRollers = itemPurch.groupby("SN")

# create df holding the 5 largest spenders
bigMoney = pd.DataFrame(highRollers["Price"].sum().sort_values(ascending=False).head(5))
bigMoney = bigMoney.rename(columns={"Price":"Total Spending"})

# create df holding most frequent buyers
freqBuyers = pd.DataFrame(highRollers["Price"].count().sort_values(ascending=False))
freqBuyers = freqBuyers.rename(columns={"Price":"Number of Purchases"})

# merge the two dataframes and add a column for average purchase cost
bigMoney = bigMoney.join(freqBuyers)
bigMoney["Average Purchase Value"] = bigMoney["Total Spending"]/bigMoney["Number of Purchases"]

# format to USD$ and set column index
bigMoney["Total Spending"] = ['${0:.2f}'.format(val) for val in bigMoney["Total Spending"]]
bigMoney["Average Purchase Value"] = ['${0:.2f}'.format(val) for val in bigMoney["Average Purchase Value"]]
bigMoney = bigMoney.reindex(columns = [["Number of Purchases","Average Purchase Value","Total Spending"]])

bigMoney

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
      <th>Number of Purchases</th>
      <th>Average Purchase Value</th>
      <th>Total Spending</th>
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
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#MOST POPULAR ITEMS
# group by Item ID - ("Item Name" can have multiple item IDs and prices assigned to it, so it's no good to use)
itemPerformance = itemPurch.groupby("Item ID")

# create a dataframe storing the item IDs that appear most frequently
armory = pd.DataFrame(itemPerformance["Item ID"].count().sort_values(ascending=False).head(5))
armory = armory.rename(columns={"Item ID":"Number of Purchases"})

# create a second dataframe that holds total revenue for each item ID
armoryRev = pd.DataFrame(itemPerformance["Price"].sum().sort_values(ascending=False))
armoryRev = armoryRev.rename(columns={"Price":"Total Revenue"})

# join the two dfs, add a column for unit price, format to USD$
armoryFull = armory.join(armoryRev)
armoryFull["Unit Price"] = armoryFull["Total Revenue"]/armoryFull["Number of Purchases"]
armoryFull["Total Revenue"] = ['${0:.2f}'.format(val) for val in armoryFull["Total Revenue"]]
armoryFull["Unit Price"] = ['${0:.2f}'.format(val) for val in armoryFull["Unit Price"]]

uniqueItems = itemPurch.drop_duplicates(subset=["Item ID"])
uniqueItems = uniqueItems[["Item ID","Item Name"]]
uniqueItems.set_index("Item ID",inplace=True)

armoryFull = armoryFull.join(uniqueItems)
armoryFull = armoryFull[["Item Name","Unit Price","Number of Purchases","Total Revenue"]]

armoryFull = armoryFull.set_index([armoryFull.index,"Item Name"])
armoryFull


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
      <th>Unit Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
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
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST PROFITABLE ITEMS
# group df by item ID and sum the price column to receive total revenue per item ID
# sort in order of total revenue, descending
itemPerformance = itemPurch.groupby("Item ID")
armoryRev = pd.DataFrame(itemPerformance["Price"].sum().sort_values(ascending=False)).head()
armoryRev = armoryRev.rename(columns={"Price":"Total Revenue"})

# create dataframe for the count of purchases per item ID
armoryCount = pd.DataFrame(itemPerformance["Item ID"].count())
armoryCount = armoryCount.rename(columns={"Item ID":"Purchase Count"})

# join into output table
armoryMaxRev = armoryRev.join(armoryCount)
armoryMaxRev = armoryMaxRev.join(uniqueItems)

# calculate unit price by dividing revenue/purchase count
armoryMaxRev["Unit Price"] = armoryMaxRev["Total Revenue"]/armoryMaxRev["Purchase Count"]

# add row index of Item Name and set column index
armoryMaxRev = armoryMaxRev.set_index([armoryMaxRev.index,"Item Name"])
armoryMaxRev = armoryMaxRev[["Total Revenue","Unit Price","Purchase Count"]]
# format unit price and total revenue to USD$
armoryMaxRev["Unit Price"] = ['${0:.2f}'.format(val) for val in armoryMaxRev["Unit Price"]]
armoryMaxRev["Total Revenue"] = ['${0:.2f}'.format(val) for val in armoryMaxRev["Total Revenue"]]

armoryMaxRev

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
      <th>Total Revenue</th>
      <th>Unit Price</th>
      <th>Purchase Count</th>
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
      <td>$37.26</td>
      <td>$4.14</td>
      <td>9</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>$29.75</td>
      <td>$4.25</td>
      <td>7</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>$29.70</td>
      <td>$4.95</td>
      <td>6</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$29.22</td>
      <td>$4.87</td>
      <td>6</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>$28.88</td>
      <td>$3.61</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>


