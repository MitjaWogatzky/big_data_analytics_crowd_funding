```python
from plotly.offline import init_notebook_mode, iplot

init_notebook_mode(connected=True)
```


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-latest.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly_express as px
```

# Dateien importieren


```python
# Datei importieren

df_final = pd.read_csv("df_final.csv", sep=",", index_col=0)
df_dropna_funding = pd.read_csv("df_dropna_funding", sep=",", index_col=0)
```


```python
df_final.head()
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
      <th>funded_amount</th>
      <th>loan_amount</th>
      <th>success_ratio</th>
      <th>success_classes</th>
      <th>activity</th>
      <th>continent</th>
      <th>sector</th>
      <th>country_code</th>
      <th>country</th>
      <th>currency</th>
      <th>...</th>
      <th>lender_count</th>
      <th>team_count</th>
      <th>female</th>
      <th>sex_majority</th>
      <th>male</th>
      <th>repayment_interval</th>
      <th>success_class</th>
      <th>team_category</th>
      <th>ISO_ALPHA</th>
      <th>ISO_ALPHA3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>300</td>
      <td>300</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Fruits &amp; Vegetables</td>
      <td>Asia</td>
      <td>Food</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>...</td>
      <td>12</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
      <td>PAK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>575</td>
      <td>575</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Rickshaw</td>
      <td>Asia</td>
      <td>Transportation</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>...</td>
      <td>14</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>2 to 5 persons</td>
      <td>PK</td>
      <td>PAK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>150</td>
      <td>150</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Transportation</td>
      <td>Asia</td>
      <td>Transportation</td>
      <td>IN</td>
      <td>India</td>
      <td>INR</td>
      <td>...</td>
      <td>6</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>bullet</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>IN</td>
      <td>IND</td>
    </tr>
    <tr>
      <th>3</th>
      <td>200</td>
      <td>200</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Embroidery</td>
      <td>Asia</td>
      <td>Arts</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>...</td>
      <td>8</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
      <td>PAK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>400</td>
      <td>400</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Milk Sales</td>
      <td>Asia</td>
      <td>Food</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>...</td>
      <td>16</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>monthly</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
      <td>PAK</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
df_dropna_funding.head()
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
      <th>funded_amount</th>
      <th>loan_amount</th>
      <th>success_ratio</th>
      <th>success_classes</th>
      <th>activity</th>
      <th>continent</th>
      <th>sector</th>
      <th>country_code</th>
      <th>country</th>
      <th>currency</th>
      <th>term_in_months</th>
      <th>lender_count</th>
      <th>team_count</th>
      <th>female</th>
      <th>sex_majority</th>
      <th>male</th>
      <th>repayment_interval</th>
      <th>success_class</th>
      <th>team_category</th>
      <th>ISO_ALPHA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>300</td>
      <td>300</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Fruits &amp; Vegetables</td>
      <td>Asia</td>
      <td>Food</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>12</td>
      <td>12</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>575</td>
      <td>575</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Rickshaw</td>
      <td>Asia</td>
      <td>Transportation</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>11</td>
      <td>14</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>2 to 5 persons</td>
      <td>PK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>150</td>
      <td>150</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Transportation</td>
      <td>Asia</td>
      <td>Transportation</td>
      <td>IN</td>
      <td>India</td>
      <td>INR</td>
      <td>43</td>
      <td>6</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>bullet</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>IN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>200</td>
      <td>200</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Embroidery</td>
      <td>Asia</td>
      <td>Arts</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>11</td>
      <td>8</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>irregular</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>400</td>
      <td>400</td>
      <td>100.0</td>
      <td>Gleich100</td>
      <td>Milk Sales</td>
      <td>Asia</td>
      <td>Food</td>
      <td>PK</td>
      <td>Pakistan</td>
      <td>PKR</td>
      <td>14</td>
      <td>16</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>female</td>
      <td>0.0</td>
      <td>monthly</td>
      <td>Gleich100</td>
      <td>1 person</td>
      <td>PK</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_final.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 690885 entries, 0 to 690884
    Data columns (total 21 columns):
     #   Column              Non-Null Count   Dtype  
    ---  ------              --------------   -----  
     0   funded_amount       690885 non-null  int64  
     1   loan_amount         690885 non-null  int64  
     2   success_ratio       690885 non-null  float64
     3   success_classes     687419 non-null  object 
     4   activity            690885 non-null  object 
     5   continent           690885 non-null  object 
     6   sector              690885 non-null  object 
     7   country_code        690885 non-null  object 
     8   country             690885 non-null  object 
     9   currency            690885 non-null  object 
     10  term_in_months      690885 non-null  int64  
     11  lender_count        690885 non-null  int64  
     12  team_count          686588 non-null  float64
     13  female              686588 non-null  float64
     14  sex_majority        686588 non-null  object 
     15  male                686588 non-null  float64
     16  repayment_interval  690885 non-null  object 
     17  success_class       687419 non-null  object 
     18  team_category       686588 non-null  object 
     19  ISO_ALPHA           670786 non-null  object 
     20  ISO_ALPHA3          670786 non-null  object 
    dtypes: float64(4), int64(4), object(13)
    memory usage: 116.0+ MB
    


```python
df_dropna_funding.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 643860 entries, 0 to 671200
    Data columns (total 20 columns):
     #   Column              Non-Null Count   Dtype  
    ---  ------              --------------   -----  
     0   funded_amount       643860 non-null  int64  
     1   loan_amount         643860 non-null  int64  
     2   success_ratio       643860 non-null  float64
     3   success_classes     643860 non-null  object 
     4   activity            643860 non-null  object 
     5   continent           643860 non-null  object 
     6   sector              643860 non-null  object 
     7   country_code        643860 non-null  object 
     8   country             643860 non-null  object 
     9   currency            643860 non-null  object 
     10  term_in_months      643860 non-null  int64  
     11  lender_count        643860 non-null  int64  
     12  team_count          643860 non-null  float64
     13  female              643860 non-null  float64
     14  sex_majority        643860 non-null  object 
     15  male                643860 non-null  float64
     16  repayment_interval  643860 non-null  object 
     17  success_class       643860 non-null  object 
     18  team_category       643860 non-null  object 
     19  ISO_ALPHA           643860 non-null  object 
    dtypes: float64(4), int64(4), object(12)
    memory usage: 103.2+ MB
    

# Data Preprocessing 


```python
# Datensatz zu groß -> Categorien umwandeln

df_final["activity"] = df_final["activity"].astype("category")
df_final["sector"] = df_final["sector"].astype("category")
df_final["continent"] = df_final["continent"].astype("category")
df_final["country_code"] = df_final["country_code"].astype("category")
df_final["country"] = df_final["country"].astype("category")
df_final["currency"] = df_final["currency"].astype("category")
df_final["sex_majority"] = df_final["sex_majority"].astype("category")
df_final["repayment_interval"] = df_final["repayment_interval"].astype("category")
df_final["team_category"] = df_final["team_category"].astype("category")

df_dropna_funding["activity"] = df_final["activity"].astype("category")
df_dropna_funding["sector"] = df_final["sector"].astype("category")
df_dropna_funding["continent"] = df_final["continent"].astype("category")
df_dropna_funding["country_code"] = df_final["country_code"].astype("category")
df_dropna_funding["country"] = df_final["country"].astype("category")
df_dropna_funding["currency"] = df_final["currency"].astype("category")
df_dropna_funding["sex_majority"] = df_final["sex_majority"].astype("category")
df_dropna_funding["repayment_interval"] = df_final["repayment_interval"].astype("category")
df_dropna_funding["team_category"] = df_final["team_category"].astype("category")

# "Downcasting" von int Datensätzen

df_final[["funded_amount", "loan_amount", "term_in_months", "lender_count"]] = df_final[["funded_amount", "loan_amount", "term_in_months", "lender_count"]].apply(pd.to_numeric, downcast="unsigned")
df_dropna_funding[["funded_amount", "loan_amount", "term_in_months", "lender_count"]] = df_final[["funded_amount", "loan_amount", "term_in_months", "lender_count"]].apply(pd.to_numeric, downcast="unsigned")
```


```python
df_final.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 690885 entries, 0 to 690884
    Data columns (total 21 columns):
     #   Column              Non-Null Count   Dtype   
    ---  ------              --------------   -----   
     0   funded_amount       690885 non-null  uint16  
     1   loan_amount         690885 non-null  uint16  
     2   success_ratio       690885 non-null  float64 
     3   success_classes     687419 non-null  object  
     4   activity            690885 non-null  category
     5   continent           690885 non-null  category
     6   sector              690885 non-null  category
     7   country_code        690885 non-null  category
     8   country             690885 non-null  category
     9   currency            690885 non-null  category
     10  term_in_months      690885 non-null  uint8   
     11  lender_count        690885 non-null  uint16  
     12  team_count          686588 non-null  float64 
     13  female              686588 non-null  float64 
     14  sex_majority        686588 non-null  category
     15  male                686588 non-null  float64 
     16  repayment_interval  690885 non-null  category
     17  success_class       687419 non-null  object  
     18  team_category       686588 non-null  category
     19  ISO_ALPHA           670786 non-null  object  
     20  ISO_ALPHA3          670786 non-null  object  
    dtypes: category(9), float64(4), object(4), uint16(3), uint8(1)
    memory usage: 74.8+ MB
    


```python
df_dropna_funding.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 643860 entries, 0 to 671200
    Data columns (total 20 columns):
     #   Column              Non-Null Count   Dtype   
    ---  ------              --------------   -----   
     0   funded_amount       643860 non-null  uint16  
     1   loan_amount         643860 non-null  uint16  
     2   success_ratio       643860 non-null  float64 
     3   success_classes     643860 non-null  object  
     4   activity            643860 non-null  category
     5   continent           643860 non-null  category
     6   sector              643860 non-null  category
     7   country_code        643860 non-null  category
     8   country             643860 non-null  category
     9   currency            643860 non-null  category
     10  term_in_months      643860 non-null  uint8   
     11  lender_count        643860 non-null  uint16  
     12  team_count          643860 non-null  float64 
     13  female              643860 non-null  float64 
     14  sex_majority        639726 non-null  category
     15  male                643860 non-null  float64 
     16  repayment_interval  643860 non-null  category
     17  success_class       643860 non-null  object  
     18  team_category       639726 non-null  category
     19  ISO_ALPHA           643860 non-null  object  
    dtypes: category(9), float64(4), object(3), uint16(3), uint8(1)
    memory usage: 49.8+ MB
    

# Pairplot metrischen Variablen


```python
sns.pairplot(data=df_dropna_funding, corner=True) 
```




    <seaborn.axisgrid.PairGrid at 0x18860c01580>




    
![png](output_13_1.png)
    


## Erkenntnisse Pairplot

Grundlegegend ist zu beachten, dass aufgrund der hohen Menge an Finanzierungsprojekten (671.204 Projekte) und somit ebenso großen Anzahl an Punkten je Graphik, keine Aussagen bzgl. Mengenverteilungen getroffen werden können. Dafür ist die Puntkewolke zu dicht. 

+ __funded_amount__:Der Großteil der finanzierten Projekte hat ein Volumen bis zu 10.000 US-Dollar. Es gab keine Finanzierung, die höher lag, als der gewünschte Betrag; Augenscheinlich gibt es einen Zusammenhang - je höhe die Finzierungssumme, desto höher die Anzahl der Darlehendsgeber
+ __success_ratio__: Der Löwenanteil der Projekte hat 100% der gewünschten Summe als Funding erhalten.
+ __term_in_month__: Der Großteil der Projekte hat eine Finanzierungsdauer unter 50 Monaten
+ __team_count__: Der Großteil der Projekte hat eine sehr kleine Teamgröße (ein bis zwei Personen); je größer die Teams, desto höher die Wahrscheinlichkeit, dass mehr Männer ein einem Team sind, als Frauen

# Sektoren

## Anzahl Projekte


```python
# nach Anzahl Projekte

df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.sum})
df_sector_rename = df_sector.rename(columns={"sector": "amount_projects"})
df_sector = df_sector_rename.reset_index()
df_sector_sorted = df_sector.sort_values(by=["amount_projects"])
#df_sector_sorted_fund = df_sector.sort_values(by=["funded_amount"])

sns.set(rc={'figure.figsize':(12,9)})
sns.set_theme(style="whitegrid")
ax = sns.barplot(x="sector", y="amount_projects", data=df_sector_sorted, palette="Blues", order=df_sector_sorted["sector"])
for item in ax.get_xticklabels(): item.set_rotation(45)
for i, v in enumerate(df_sector_sorted["amount_projects"].iteritems()):        
    ax.text(i ,v[1], "{:,}".format(v[1]), color='darkred', va ='bottom', rotation=45)
plt.tight_layout()
plt.show()
```


    
![png](output_17_0.png)
    


## Fundingdurchschnitt und Projektanzahl


```python
# Spalten erstellen
df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.sum})
funded_sum = df_sector["funded_amount"].tolist()
df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.mean})
funded_mean = df_sector["funded_amount"].tolist()
df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.median})
funded_median = df_sector["funded_amount"].tolist()

# DataFrame erstellen und Spalten einfügen
df_sector = df_final.groupby("sector").agg({"sector":np.size})
df_sector = df_sector.rename(columns={"sector": "amount_projects"})
df_sector_fund = df_sector.reset_index()
df_sector_fund.insert(2,'funded_sum',funded_sum)
df_sector_fund.insert(3,'funded_mean',funded_mean)
df_sector_fund.insert(4,'funded_median',funded_median)
df_sector_fund

# Plotten
fig = px.scatter(df_sector_fund, x="funded_mean", y="amount_projects", color="sector", size="funded_sum", 
           hover_name="sector", size_max=60)
fig.show()
```


<div>                            <div id="568f6b65-23b8-4bfc-b7b5-d542abd8cc88" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("568f6b65-23b8-4bfc-b7b5-d542abd8cc88")) {                    Plotly.newPlot(                        "568f6b65-23b8-4bfc-b7b5-d542abd8cc88",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Agriculture<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Agriculture"], "legendgroup": "Agriculture", "marker": {"color": "#636efa", "size": [140447560.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Agriculture", "orientation": "v", "showlegend": true, "type": "scatter", "x": [762.572539310225], "xaxis": "x", "y": [184176], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Arts<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Arts"], "legendgroup": "Arts", "marker": {"color": "#EF553B", "size": [12793980.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Arts", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1025.7339854084823], "xaxis": "x", "y": [12473], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Clothing<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Clothing"], "legendgroup": "Clothing", "marker": {"color": "#00cc96", "size": [37161720.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Clothing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1105.8061060524906], "xaxis": "x", "y": [33606], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Construction<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Construction"], "legendgroup": "Construction", "marker": {"color": "#ab63fa", "size": [6718340.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Construction", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1029.7884733292458], "xaxis": "x", "y": [6524], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Education<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Education"], "legendgroup": "Education", "marker": {"color": "#FFA15A", "size": [31831020.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Education", "orientation": "v", "showlegend": true, "type": "scatter", "x": [970.5171047015062], "xaxis": "x", "y": [32798], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Entertainment<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Entertainment"], "legendgroup": "Entertainment", "marker": {"color": "#19d3f3", "size": [1084920.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Entertainment", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1264.4755244755245], "xaxis": "x", "y": [858], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Food<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Food"], "legendgroup": "Food", "marker": {"color": "#FF6692", "size": [123531360.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Food", "orientation": "v", "showlegend": true, "type": "scatter", "x": [878.0144142607361], "xaxis": "x", "y": [140694], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Health<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Health"], "legendgroup": "Health", "marker": {"color": "#B6E880", "size": [9888850.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Health", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1033.5336538461538], "xaxis": "x", "y": [9568], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Housing<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Housing"], "legendgroup": "Housing", "marker": {"color": "#FF97FF", "size": [24671850.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Housing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [651.8150114924308], "xaxis": "x", "y": [37851], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Manufacturing<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Manufacturing"], "legendgroup": "Manufacturing", "marker": {"color": "#FECB52", "size": [5932650.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Manufacturing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [903.8162705667276], "xaxis": "x", "y": [6564], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Personal Use<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Personal Use"], "legendgroup": "Personal Use", "marker": {"color": "#636efa", "size": [14350975.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Personal Use", "orientation": "v", "showlegend": true, "type": "scatter", "x": [392.65027770937644], "xaxis": "x", "y": [36549], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Retail<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Retail"], "legendgroup": "Retail", "marker": {"color": "#EF553B", "size": [94783955.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Retail", "orientation": "v", "showlegend": true, "type": "scatter", "x": [750.6985925978727], "xaxis": "x", "y": [126261], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Services<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Services"], "legendgroup": "Services", "marker": {"color": "#00cc96", "size": [45173910.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Services", "orientation": "v", "showlegend": true, "type": "scatter", "x": [971.9626912236159], "xaxis": "x", "y": [46477], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Transportation<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Transportation"], "legendgroup": "Transportation", "marker": {"color": "#ab63fa", "size": [10196125.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Transportation", "orientation": "v", "showlegend": true, "type": "scatter", "x": [643.4916377406122], "xaxis": "x", "y": [15845], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Wholesale<br>funded_mean=%{x}<br>amount_projects=%{y}<br>funded_sum=%{marker.size}<extra></extra>", "hovertext": ["Wholesale"], "legendgroup": "Wholesale", "marker": {"color": "#FFA15A", "size": [932950.0], "sizemode": "area", "sizeref": 39013.21111111111, "symbol": "circle"}, "mode": "markers", "name": "Wholesale", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1455.4602184087364], "xaxis": "x", "y": [641], "yaxis": "y"}],                        {"legend": {"itemsizing": "constant", "title": {"text": "sector"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "funded_mean"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('568f6b65-23b8-4bfc-b7b5-d542abd8cc88');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Funding-Höhe und Funding Erfolg


```python
# nach Anzahl Projekten

# Nach Sektor und Mittelwerten gruppieren
df_group_sector_mean = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.mean, "loan_amount":np.mean, "success_ratio":np.mean, "team_count":np.mean, "term_in_months":np.mean, "lender_count":np.mean})
df_group_sector_rename = df_group_sector_mean.rename(columns={"sector": "amount_projects"})
df_group_sector_mean = df_group_sector_rename.reset_index()
# Plotten
fig = px.scatter(df_group_sector_mean, x="loan_amount", y="funded_amount", size="amount_projects", color="sector",
           hover_name="sector", size_max=60)
fig.show()
```


<div>                            <div id="37ec2226-4c9c-40d7-816c-f7805be4acaf" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("37ec2226-4c9c-40d7-816c-f7805be4acaf")) {                    Plotly.newPlot(                        "37ec2226-4c9c-40d7-816c-f7805be4acaf",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Agriculture<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Agriculture"], "legendgroup": "Agriculture", "marker": {"color": "#636efa", "size": [184176], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Agriculture", "orientation": "v", "showlegend": true, "type": "scatter", "x": [816.2579543480149], "xaxis": "x", "y": [762.572539310225], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Arts<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Arts"], "legendgroup": "Arts", "marker": {"color": "#EF553B", "size": [12473], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Arts", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1059.348192094925], "xaxis": "x", "y": [1025.7339854084823], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Clothing<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Clothing"], "legendgroup": "Clothing", "marker": {"color": "#00cc96", "size": [33606], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Clothing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1186.7850978991846], "xaxis": "x", "y": [1105.8061060524906], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Construction<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Construction"], "legendgroup": "Construction", "marker": {"color": "#ab63fa", "size": [6524], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Construction", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1090.0636112814225], "xaxis": "x", "y": [1029.7884733292458], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Education<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Education"], "legendgroup": "Education", "marker": {"color": "#FFA15A", "size": [32798], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Education", "orientation": "v", "showlegend": true, "type": "scatter", "x": [998.8932251966584], "xaxis": "x", "y": [970.5171047015062], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Entertainment<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Entertainment"], "legendgroup": "Entertainment", "marker": {"color": "#19d3f3", "size": [858], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Entertainment", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1691.1130536130536], "xaxis": "x", "y": [1264.4755244755245], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Food<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Food"], "legendgroup": "Food", "marker": {"color": "#FF6692", "size": [140694], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Food", "orientation": "v", "showlegend": true, "type": "scatter", "x": [929.185324178714], "xaxis": "x", "y": [878.0144142607361], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Health<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Health"], "legendgroup": "Health", "marker": {"color": "#B6E880", "size": [9568], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Health", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1118.979933110368], "xaxis": "x", "y": [1033.5336538461538], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Housing<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Housing"], "legendgroup": "Housing", "marker": {"color": "#FF97FF", "size": [37851], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Housing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [721.7517370743177], "xaxis": "x", "y": [651.8150114924308], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Manufacturing<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Manufacturing"], "legendgroup": "Manufacturing", "marker": {"color": "#FECB52", "size": [6564], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Manufacturing", "orientation": "v", "showlegend": true, "type": "scatter", "x": [919.6488421694089], "xaxis": "x", "y": [903.8162705667276], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Personal Use<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Personal Use"], "legendgroup": "Personal Use", "marker": {"color": "#636efa", "size": [36549], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Personal Use", "orientation": "v", "showlegend": true, "type": "scatter", "x": [414.1194013516102], "xaxis": "x", "y": [392.65027770937644], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Retail<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Retail"], "legendgroup": "Retail", "marker": {"color": "#EF553B", "size": [126261], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Retail", "orientation": "v", "showlegend": true, "type": "scatter", "x": [811.3869286636412], "xaxis": "x", "y": [750.6985925978727], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Services<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Services"], "legendgroup": "Services", "marker": {"color": "#00cc96", "size": [46477], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Services", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1087.0118553262905], "xaxis": "x", "y": [971.9626912236159], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Transportation<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Transportation"], "legendgroup": "Transportation", "marker": {"color": "#ab63fa", "size": [15845], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Transportation", "orientation": "v", "showlegend": true, "type": "scatter", "x": [725.3581571473651], "xaxis": "x", "y": [643.4916377406122], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>sector=Wholesale<br>loan_amount=%{x}<br>funded_amount=%{y}<br>amount_projects=%{marker.size}<extra></extra>", "hovertext": ["Wholesale"], "legendgroup": "Wholesale", "marker": {"color": "#FFA15A", "size": [641], "sizemode": "area", "sizeref": 51.16, "symbol": "circle"}, "mode": "markers", "name": "Wholesale", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1574.4929797191887], "xaxis": "x", "y": [1455.4602184087364], "yaxis": "y"}],                        {"legend": {"itemsizing": "constant", "title": {"text": "sector"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "loan_amount"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "funded_amount"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('37ec2226-4c9c-40d7-816c-f7805be4acaf');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


**ERKENNTNISSE**

**Projektanzahl:**
Die **meisten Projete** mit 180.301 Projekten hat der Agrarbereich, gefolgt von "Nahrung" mit 136.657 und Einzelhandel mit 124.494 Projekten.
Die **wenigsten Projekte** gibt es im Bereich "Großhandel" mit lediglich 634 Projekten, gefolgt von "Entertainment" mit 830 Projekten und Manufactoring mit bereits 6.208 Projekten.

**Fundingvolumen:**
Der  Funding-Niveau ist ingesamt sehr niedirg. Der Mittelwert aller erhaltenen Fundingvolumen reicht von 411 US-Dollar im Sektor "Personal Use" bis 1570 bei "Wholesale". Der Großteil befindet sich zwischen 600 und 800 US Dollar.

**Funding-Erfolg**
Der lineare Verlauf zeigt, dass die Höhe der ausgezahlten Fundingsumme in allen Sektoren nahezu der gewünschten Höhe entspricht (Funding-Erflolg: 94% "Housing bis 99% "Manufacturing"). Einzig Entertainment liegt augenscheinlich unter diesem Bild (Funding-Erfolg: 89%). 

**Legende:**
+ loan_amount: Mittelwerte gewünschte Fundingsumme in US-Dollar
+ funded_amount: Mittelwerte erhaltene Fundingsumme in US-Dollar
+ Projektanzahl (sector_size): Anzahl Projekte je Sektor 
+ Funding-Erfolg: Das Verhältnis von gewünschter Funding-Summe und erhaltener Fundingsumme

## Aktivitäten je Sektor


```python
df_activity = df_final.groupby(["sector", "activity"]).agg({"activity":np.size, "funded_amount":np.mean})
df_activity = df_activity.dropna()
funded_mean = df_activity["funded_amount"].tolist()
df_activity = df_final.groupby(["sector", "activity"]).agg({"activity":np.size, "funded_amount":np.median})
df_activity = df_activity.dropna()
funded_median = df_activity["funded_amount"].tolist()

df_activity = df_final.groupby(["sector", "activity"]).agg({"activity":np.size, "funded_amount":np.sum})
df_activ = df_activity.rename(columns={"activity": "amount_projects"})
df_activity = df_activ.reset_index()
df_activity = df_activity.loc[df_activity["funded_amount"]!=0, :]
df_activity.insert(4,'funded_mean',funded_mean)
df_activity.insert(5,'funded_median',funded_median)
df_activity = df_activity.sort_values(by=["funded_mean"])
df_activity = df_activity.loc[df_activity["funded_mean"]< 1900, :]
#df_activity = df_activity.loc[df_activity["activity"]!="Renewable Energy Products", :]
#df_activity = df_activity.loc[df_activity["activity"]!="Landscaping / Gardening", :]

fig = px.treemap(df_activity, path=[px.Constant('Overall fundings'), 'sector', 'activity'], values='amount_projects',
                  color='funded_mean', hover_name="activity")
fig.show()
```


<div>                            <div id="f5daeeb2-ceb0-469d-afc3-9b012f4a7522" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("f5daeeb2-ceb0-469d-afc3-9b012f4a7522")) {                    Plotly.newPlot(                        "f5daeeb2-ceb0-469d-afc3-9b012f4a7522",                        [{"branchvalues": "total", "customdata": [[null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1687.5], [null], [null], [954.4448674716269], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1141.6666666666667], [null], [null], [1083.001498608435], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [752.5179856115108], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1011.8112014453478], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [979.8340548340549], [null], [null], [null], [null], [null], [null], [null], [null], [1174.1878898128898], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [397.95081967213116], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [827.3148148148148], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [995.628151883714], [null], [null], [1558.173076923077], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1036.7723285486443], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [773.6196319018405], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1070.1754385964912], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [864.2857142857143], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1131.9444444444443], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1533.1275720164608], [null], [null], [null], [null], [null], [null], [916.4923954372623], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1019.106186069167], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1008.094262295082], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [817.8571428571429], [null], [null], [null], [null], [null], [989.2765685019206], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1076.6618497109826], [null], [null], [null], [null], [null], [null], [null], [null], [976.2854717438759], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [280.0], [null], [null], [null], [null], [null], [null], [null], [1460.248447204969], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [622.420959818617], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [555.8265582655827], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1220.7925636007828], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [780.7299096528768], [null], [null], [null], [null], [null], [1356.6231234158706], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1055.7037828875605], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [762.0901639344262], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [954.5454545454545], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1056.5051020408164], [null], [null], [null], [null], [null], [1018.8496172895092], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1134.242209631728], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [797.8295819935691], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1172.0496191064442], [null], [null], [null], [null], [1212.1630280018985], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [661.3155333900985], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1253.683574879227], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1184.7643097643097], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1263.9026017344897], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1250.6278538812785], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1026.233552631579], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [861.9854721549636], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [993.9260563380282], [null], [null], [null], [null], [670.7600502512563], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [700.5263157894736], [null], [null], [null], [null], [null], [null], [null], [1470.5711206896551], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1435.0], [null], [null], [734.4645293315143], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [666.8857414577814], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [584.3840552789954], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [475.15784361340457], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1801.4925373134329], [null], [null], [null], [832.0497630331754], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1004.7988285000961], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1058.6730477895608], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [890.4406975522639], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1090.7404556650247], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [826.7433977835416], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [617.4652241112829], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [835.3658536585366], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [969.8138901497957], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [945.2674897119341], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [561.2704096466664], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1670.4301075268818], [null], [null], [null], [null], [null], [null], [956.3001356326293], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1102.019427402863], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1512.5744047619048], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [970.24331919929], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [266.52298142765653], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [564.0982691233947], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1012.5031557687453], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [943.6379928315412], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [809.3166175024583], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1034.914101646385], [null], [null], [null], [null], [1595.1515151515152], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [770.6093189964158], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1034.4166666666667], [null], [null], [null], [null], [null], [null], [null], [null], [995.617088607595], [null], [null], [null], [null], [null], [null], [null], [null], [1066.408864430728], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [881.7251461988304], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [887.406015037594], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [836.3071660064818], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1209.3373493975903], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [973.1191222570533], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [763.4986595174263], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1070.7174231332358], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [727.6620370370371], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [864.9150743099788], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [459.88036653656883], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1039.3092105263158], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1220.2777777777778], [null], [null], [null], [null], [1326.0416666666667], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1144.867549668874], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1380.6346381969158], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1067.1333333333334], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1029.4463087248323], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1186.304347826087], [null], [null], [null], [null], [687.5], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1711.4644970414201], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [559.375], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [418.9950980392157], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [641.1592041162626], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [806.1438080224208], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [554.9627632254751], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1281.3279002876318], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [885.5130784708249], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1084.959349593496], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [627.978515625], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1099.2592592592594], [null], [null], [489.95862624487404], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [867.8387650085763], [null], [null], [null], [876.912100456621], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [805.659844299347], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1103.4934497816594], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [948.3434881949734], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [978.9226519337017], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [831.9117647058823], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [846.2130177514792], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [816.6666666666666], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [751.7123287671233], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1341.8925554900804], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [985.9258669427081], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [494.15392633773456], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [997.0430107526881], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1129.6971502333433], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1069.0041916846042], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1028.8272471910113], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [964.8351648351648], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [962.3833592534992], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [812.3456790123457], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [646.2714619526568], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [997.3281756027347], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1502.629151291513], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [725.4464285714286], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1484.5052083333333], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [766.5424912689174], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [706.9536423841059], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [983.6], [null], [null], [null], [null], [954.8915310805173], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1871.9568062827225], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1230.7692307692307], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [954.7619047619048], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [994.0944881889764], [null], [null], [806.875], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [836.084142394822], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1110.796460176991], [null], [null], [null], [782.3309492847854], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [800.0], [null], [null], [null], [null], [null], [null], [null], [1309.3023255813953], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [1289.779005524862], [762.572539310225], [1024.621187800963], [1105.8061060524906], [1029.7884733292458], [970.5171047015062], [1264.4755244755245], [878.0144142607361], [1033.5336538461538], [651.8150114924308], [903.8162705667276], [392.65027770937644], [739.8809637480506], [952.173120977419], [643.4916377406122], [1455.4602184087364], [806.4195854388852]], "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "hovertemplate": "<b>%{hovertext}</b><br><br>labels=%{label}<br>amount_projects=%{value}<br>parent=%{parent}<br>id=%{id}<br>funded_mean=%{color}<extra></extra>", "hovertext": [null, null, null, null, null, null, null, null, null, null, null, null, "Adult Care", null, null, "Agriculture", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Air Conditioning", null, null, "Animal Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Aquaculture", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Arts", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Auto Repair", null, null, null, null, null, null, null, null, "Bakery", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Balut-Making", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Barber Shop", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Beauty Salon", null, null, "Beekeeping", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Beverages", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Bicycle Repair", null, null, null, null, null, null, null, null, null, null, null, null, null, "Bicycle Sales", null, null, null, null, null, null, null, null, null, null, null, null, "Blacksmith", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Bookbinding", null, null, null, null, null, null, null, null, null, null, null, null, null, "Bookstore", null, null, null, null, null, null, "Bricks", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Butcher Shop", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Cafe", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Call Center", null, null, null, null, null, "Carpentry", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Catering", null, null, null, null, null, null, null, null, "Cattle", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Celebrations", null, null, null, null, null, null, null, "Cement", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Cereals", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Charcoal Sales", null, null, null, null, null, null, null, null, null, "Cheese Making", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Cloth & Dressmaking Supplies", null, null, null, null, null, "Clothing", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Clothing Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Cobbler", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Computer", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Computers", null, null, null, null, null, "Construction", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Construction Supplies", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Consumer Goods", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Cosmetics Sales", null, null, null, null, "Crafts", null, null, null, null, null, null, null, null, null, null, null, null, null, "Dairy", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Decorations Sales", null, null, null, null, null, null, null, null, null, null, "Dental", null, null, null, null, null, null, null, null, null, null, null, "Education provider", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Electrical Goods", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Electrician", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Electronics Repair", null, null, null, null, null, null, null, null, null, null, null, null, null, "Electronics Sales", null, null, null, null, "Embroidery", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Energy", null, null, null, null, null, null, null, "Entertainment", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Event Planning", null, null, "Farm Supplies", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Farming", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Fish Selling", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Fishing", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Florist", null, null, null, "Flowers", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Food", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Food Market", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Food Production/Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Food Stall", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Fruits & Vegetables", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Fuel/Firewood", null, null, null, null, null, null, null, null, null, null, null, null, null, "Funerals", null, null, null, null, null, null, null, null, null, null, null, null, null, "Furniture Making", null, null, null, null, null, null, null, null, null, null, "Games", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "General Store", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Goods Distribution", null, null, null, null, null, null, "Grocery Store", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Hardware", null, null, null, null, null, null, null, null, null, null, "Health", null, null, null, null, null, null, null, null, null, null, null, "Higher education costs", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Home Appliances", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Home Energy", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Home Products Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Hotel", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Internet Cafe", null, null, null, null, null, null, null, null, null, null, null, null, null, "Jewelry", null, null, null, null, "Knitting", null, null, null, null, null, null, null, null, null, null, null, null, null, "Land Rental", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Laundry", null, null, null, null, null, null, null, null, "Liquor Store / Off-License", null, null, null, null, null, null, null, null, "Livestock", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Machine Shop", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Machinery Rental", null, null, null, null, null, null, null, null, null, null, null, "Manufacturing", null, null, null, null, null, null, null, null, null, null, null, null, "Medical Clinic", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Metal Shop", null, null, null, null, null, null, null, null, null, null, null, "Milk Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Mobile Phones", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Mobile Transactions", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Motorcycle Repair", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Motorcycle Transport", null, null, null, null, null, null, null, null, null, null, null, null, "Movie Tapes & DVDs", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Music Discs & Tapes", null, null, null, null, "Musical Instruments", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Musical Performance", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Natural Medicines", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Office Supplies", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Paper Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Party Supplies", null, null, null, null, "Patchwork", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Perfumes", null, null, null, null, null, null, null, null, null, null, null, null, "Personal Care Products", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Personal Expenses", null, null, null, null, null, null, null, null, null, null, null, null, "Personal Housing Expenses", null, null, null, null, null, null, null, null, null, null, null, null, null, "Personal Medical Expenses", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Personal Products Sales", null, null, null, null, null, null, null, null, null, null, "Pharmacy", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Phone Accessories", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Phone Repair", null, null, null, null, null, null, null, null, null, null, null, null, null, "Phone Use Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Photography", null, null, "Pigs", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Plastics Sales", null, null, null, "Poultry", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Primary/secondary school costs", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Printing", null, null, null, null, null, null, null, null, null, null, "Property", null, null, null, null, null, null, null, null, null, null, null, null, "Pub", null, null, null, null, null, null, null, null, null, null, null, "Quarrying", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Recycled Materials", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Recycling", null, null, null, null, null, null, null, null, null, null, null, null, null, "Religious Articles", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Restaurant", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Retail", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Rickshaw", null, null, null, null, null, null, null, null, null, null, null, null, null, "Secretarial Services", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Services", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Sewing", null, null, null, null, null, null, null, null, null, null, null, null, null, "Shoe Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Souvenir Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Spare Parts", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Sporting Good Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Tailoring", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Taxi", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Textiles", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Timber Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Tourism", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Transportation", null, null, null, null, null, null, null, null, null, null, null, null, "Traveling Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Upholstery", null, null, null, null, "Used Clothing", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Used Shoes", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Utilities", null, null, null, null, null, null, null, null, null, null, null, null, "Vehicle", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Vehicle Repairs", null, null, "Veterinary Sales", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Waste Management", null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Water Distribution", null, null, null, "Weaving", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Wedding Expenses", null, null, null, null, null, null, null, "Well digging", null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, "Wholesale", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)", "(?)"], "ids": ["Overall fundings/Agriculture/Adult Care", "Overall fundings/Arts/Adult Care", "Overall fundings/Clothing/Adult Care", "Overall fundings/Construction/Adult Care", "Overall fundings/Education/Adult Care", "Overall fundings/Entertainment/Adult Care", "Overall fundings/Food/Adult Care", "Overall fundings/Health/Adult Care", "Overall fundings/Housing/Adult Care", "Overall fundings/Manufacturing/Adult Care", "Overall fundings/Personal Use/Adult Care", "Overall fundings/Retail/Adult Care", "Overall fundings/Services/Adult Care", "Overall fundings/Transportation/Adult Care", "Overall fundings/Wholesale/Adult Care", "Overall fundings/Agriculture/Agriculture", "Overall fundings/Arts/Agriculture", "Overall fundings/Clothing/Agriculture", "Overall fundings/Construction/Agriculture", "Overall fundings/Education/Agriculture", "Overall fundings/Entertainment/Agriculture", "Overall fundings/Food/Agriculture", "Overall fundings/Health/Agriculture", "Overall fundings/Housing/Agriculture", "Overall fundings/Manufacturing/Agriculture", "Overall fundings/Personal Use/Agriculture", "Overall fundings/Retail/Agriculture", "Overall fundings/Services/Agriculture", "Overall fundings/Transportation/Agriculture", "Overall fundings/Wholesale/Agriculture", "Overall fundings/Agriculture/Air Conditioning", "Overall fundings/Arts/Air Conditioning", "Overall fundings/Clothing/Air Conditioning", "Overall fundings/Construction/Air Conditioning", "Overall fundings/Education/Air Conditioning", "Overall fundings/Entertainment/Air Conditioning", "Overall fundings/Food/Air Conditioning", "Overall fundings/Health/Air Conditioning", "Overall fundings/Housing/Air Conditioning", "Overall fundings/Manufacturing/Air Conditioning", "Overall fundings/Personal Use/Air Conditioning", "Overall fundings/Retail/Air Conditioning", "Overall fundings/Services/Air Conditioning", "Overall fundings/Transportation/Air Conditioning", "Overall fundings/Wholesale/Air Conditioning", "Overall fundings/Agriculture/Animal Sales", "Overall fundings/Arts/Animal Sales", "Overall fundings/Clothing/Animal Sales", "Overall fundings/Construction/Animal Sales", "Overall fundings/Education/Animal Sales", "Overall fundings/Entertainment/Animal Sales", "Overall fundings/Food/Animal Sales", "Overall fundings/Health/Animal Sales", "Overall fundings/Housing/Animal Sales", "Overall fundings/Manufacturing/Animal Sales", "Overall fundings/Personal Use/Animal Sales", "Overall fundings/Retail/Animal Sales", "Overall fundings/Services/Animal Sales", "Overall fundings/Transportation/Animal Sales", "Overall fundings/Wholesale/Animal Sales", "Overall fundings/Agriculture/Aquaculture", "Overall fundings/Arts/Aquaculture", "Overall fundings/Clothing/Aquaculture", "Overall fundings/Construction/Aquaculture", "Overall fundings/Education/Aquaculture", "Overall fundings/Entertainment/Aquaculture", "Overall fundings/Food/Aquaculture", "Overall fundings/Health/Aquaculture", "Overall fundings/Housing/Aquaculture", "Overall fundings/Manufacturing/Aquaculture", "Overall fundings/Personal Use/Aquaculture", "Overall fundings/Retail/Aquaculture", "Overall fundings/Services/Aquaculture", "Overall fundings/Transportation/Aquaculture", "Overall fundings/Wholesale/Aquaculture", "Overall fundings/Agriculture/Arts", "Overall fundings/Arts/Arts", "Overall fundings/Clothing/Arts", "Overall fundings/Construction/Arts", "Overall fundings/Education/Arts", "Overall fundings/Entertainment/Arts", "Overall fundings/Food/Arts", "Overall fundings/Health/Arts", "Overall fundings/Housing/Arts", "Overall fundings/Manufacturing/Arts", "Overall fundings/Personal Use/Arts", "Overall fundings/Retail/Arts", "Overall fundings/Services/Arts", "Overall fundings/Transportation/Arts", "Overall fundings/Wholesale/Arts", "Overall fundings/Agriculture/Auto Repair", "Overall fundings/Arts/Auto Repair", "Overall fundings/Clothing/Auto Repair", "Overall fundings/Construction/Auto Repair", "Overall fundings/Education/Auto Repair", "Overall fundings/Entertainment/Auto Repair", "Overall fundings/Food/Auto Repair", "Overall fundings/Health/Auto Repair", "Overall fundings/Housing/Auto Repair", "Overall fundings/Manufacturing/Auto Repair", "Overall fundings/Personal Use/Auto Repair", "Overall fundings/Retail/Auto Repair", "Overall fundings/Services/Auto Repair", "Overall fundings/Transportation/Auto Repair", "Overall fundings/Wholesale/Auto Repair", "Overall fundings/Agriculture/Bakery", "Overall fundings/Arts/Bakery", "Overall fundings/Clothing/Bakery", "Overall fundings/Construction/Bakery", "Overall fundings/Education/Bakery", "Overall fundings/Entertainment/Bakery", "Overall fundings/Food/Bakery", "Overall fundings/Health/Bakery", "Overall fundings/Housing/Bakery", "Overall fundings/Manufacturing/Bakery", "Overall fundings/Personal Use/Bakery", "Overall fundings/Retail/Bakery", "Overall fundings/Services/Bakery", "Overall fundings/Transportation/Bakery", "Overall fundings/Wholesale/Bakery", "Overall fundings/Agriculture/Balut-Making", "Overall fundings/Arts/Balut-Making", "Overall fundings/Clothing/Balut-Making", "Overall fundings/Construction/Balut-Making", "Overall fundings/Education/Balut-Making", "Overall fundings/Entertainment/Balut-Making", "Overall fundings/Food/Balut-Making", "Overall fundings/Health/Balut-Making", "Overall fundings/Housing/Balut-Making", "Overall fundings/Manufacturing/Balut-Making", "Overall fundings/Personal Use/Balut-Making", "Overall fundings/Retail/Balut-Making", "Overall fundings/Services/Balut-Making", "Overall fundings/Transportation/Balut-Making", "Overall fundings/Wholesale/Balut-Making", "Overall fundings/Agriculture/Barber Shop", "Overall fundings/Arts/Barber Shop", "Overall fundings/Clothing/Barber Shop", "Overall fundings/Construction/Barber Shop", "Overall fundings/Education/Barber Shop", "Overall fundings/Entertainment/Barber Shop", "Overall fundings/Food/Barber Shop", "Overall fundings/Health/Barber Shop", "Overall fundings/Housing/Barber Shop", "Overall fundings/Manufacturing/Barber Shop", "Overall fundings/Personal Use/Barber Shop", "Overall fundings/Retail/Barber Shop", "Overall fundings/Services/Barber Shop", "Overall fundings/Transportation/Barber Shop", "Overall fundings/Wholesale/Barber Shop", "Overall fundings/Agriculture/Beauty Salon", "Overall fundings/Arts/Beauty Salon", "Overall fundings/Clothing/Beauty Salon", "Overall fundings/Construction/Beauty Salon", "Overall fundings/Education/Beauty Salon", "Overall fundings/Entertainment/Beauty Salon", "Overall fundings/Food/Beauty Salon", "Overall fundings/Health/Beauty Salon", "Overall fundings/Housing/Beauty Salon", "Overall fundings/Manufacturing/Beauty Salon", "Overall fundings/Personal Use/Beauty Salon", "Overall fundings/Retail/Beauty Salon", "Overall fundings/Services/Beauty Salon", "Overall fundings/Transportation/Beauty Salon", "Overall fundings/Wholesale/Beauty Salon", "Overall fundings/Agriculture/Beekeeping", "Overall fundings/Arts/Beekeeping", "Overall fundings/Clothing/Beekeeping", "Overall fundings/Construction/Beekeeping", "Overall fundings/Education/Beekeeping", "Overall fundings/Entertainment/Beekeeping", "Overall fundings/Food/Beekeeping", "Overall fundings/Health/Beekeeping", "Overall fundings/Housing/Beekeeping", "Overall fundings/Manufacturing/Beekeeping", "Overall fundings/Personal Use/Beekeeping", "Overall fundings/Retail/Beekeeping", "Overall fundings/Services/Beekeeping", "Overall fundings/Transportation/Beekeeping", "Overall fundings/Wholesale/Beekeeping", "Overall fundings/Agriculture/Beverages", "Overall fundings/Arts/Beverages", "Overall fundings/Clothing/Beverages", "Overall fundings/Construction/Beverages", "Overall fundings/Education/Beverages", "Overall fundings/Entertainment/Beverages", "Overall fundings/Food/Beverages", "Overall fundings/Health/Beverages", "Overall fundings/Housing/Beverages", "Overall fundings/Manufacturing/Beverages", "Overall fundings/Personal Use/Beverages", "Overall fundings/Retail/Beverages", "Overall fundings/Services/Beverages", "Overall fundings/Transportation/Beverages", "Overall fundings/Wholesale/Beverages", "Overall fundings/Agriculture/Bicycle Repair", "Overall fundings/Arts/Bicycle Repair", "Overall fundings/Clothing/Bicycle Repair", "Overall fundings/Construction/Bicycle Repair", "Overall fundings/Education/Bicycle Repair", "Overall fundings/Entertainment/Bicycle Repair", "Overall fundings/Food/Bicycle Repair", "Overall fundings/Health/Bicycle Repair", "Overall fundings/Housing/Bicycle Repair", "Overall fundings/Manufacturing/Bicycle Repair", "Overall fundings/Personal Use/Bicycle Repair", "Overall fundings/Retail/Bicycle Repair", "Overall fundings/Services/Bicycle Repair", "Overall fundings/Transportation/Bicycle Repair", "Overall fundings/Wholesale/Bicycle Repair", "Overall fundings/Agriculture/Bicycle Sales", "Overall fundings/Arts/Bicycle Sales", "Overall fundings/Clothing/Bicycle Sales", "Overall fundings/Construction/Bicycle Sales", "Overall fundings/Education/Bicycle Sales", "Overall fundings/Entertainment/Bicycle Sales", "Overall fundings/Food/Bicycle Sales", "Overall fundings/Health/Bicycle Sales", "Overall fundings/Housing/Bicycle Sales", "Overall fundings/Manufacturing/Bicycle Sales", "Overall fundings/Personal Use/Bicycle Sales", "Overall fundings/Retail/Bicycle Sales", "Overall fundings/Services/Bicycle Sales", "Overall fundings/Transportation/Bicycle Sales", "Overall fundings/Wholesale/Bicycle Sales", "Overall fundings/Agriculture/Blacksmith", "Overall fundings/Arts/Blacksmith", "Overall fundings/Clothing/Blacksmith", "Overall fundings/Construction/Blacksmith", "Overall fundings/Education/Blacksmith", "Overall fundings/Entertainment/Blacksmith", "Overall fundings/Food/Blacksmith", "Overall fundings/Health/Blacksmith", "Overall fundings/Housing/Blacksmith", "Overall fundings/Manufacturing/Blacksmith", "Overall fundings/Personal Use/Blacksmith", "Overall fundings/Retail/Blacksmith", "Overall fundings/Services/Blacksmith", "Overall fundings/Transportation/Blacksmith", "Overall fundings/Wholesale/Blacksmith", "Overall fundings/Agriculture/Bookbinding", "Overall fundings/Arts/Bookbinding", "Overall fundings/Clothing/Bookbinding", "Overall fundings/Construction/Bookbinding", "Overall fundings/Education/Bookbinding", "Overall fundings/Entertainment/Bookbinding", "Overall fundings/Food/Bookbinding", "Overall fundings/Health/Bookbinding", "Overall fundings/Housing/Bookbinding", "Overall fundings/Manufacturing/Bookbinding", "Overall fundings/Personal Use/Bookbinding", "Overall fundings/Retail/Bookbinding", "Overall fundings/Services/Bookbinding", "Overall fundings/Transportation/Bookbinding", "Overall fundings/Wholesale/Bookbinding", "Overall fundings/Agriculture/Bookstore", "Overall fundings/Arts/Bookstore", "Overall fundings/Clothing/Bookstore", "Overall fundings/Construction/Bookstore", "Overall fundings/Education/Bookstore", "Overall fundings/Entertainment/Bookstore", "Overall fundings/Food/Bookstore", "Overall fundings/Health/Bookstore", "Overall fundings/Housing/Bookstore", "Overall fundings/Manufacturing/Bookstore", "Overall fundings/Personal Use/Bookstore", "Overall fundings/Retail/Bookstore", "Overall fundings/Services/Bookstore", "Overall fundings/Transportation/Bookstore", "Overall fundings/Wholesale/Bookstore", "Overall fundings/Agriculture/Bricks", "Overall fundings/Arts/Bricks", "Overall fundings/Clothing/Bricks", "Overall fundings/Construction/Bricks", "Overall fundings/Education/Bricks", "Overall fundings/Entertainment/Bricks", "Overall fundings/Food/Bricks", "Overall fundings/Health/Bricks", "Overall fundings/Housing/Bricks", "Overall fundings/Manufacturing/Bricks", "Overall fundings/Personal Use/Bricks", "Overall fundings/Retail/Bricks", "Overall fundings/Services/Bricks", "Overall fundings/Transportation/Bricks", "Overall fundings/Wholesale/Bricks", "Overall fundings/Agriculture/Butcher Shop", "Overall fundings/Arts/Butcher Shop", "Overall fundings/Clothing/Butcher Shop", "Overall fundings/Construction/Butcher Shop", "Overall fundings/Education/Butcher Shop", "Overall fundings/Entertainment/Butcher Shop", "Overall fundings/Food/Butcher Shop", "Overall fundings/Health/Butcher Shop", "Overall fundings/Housing/Butcher Shop", "Overall fundings/Manufacturing/Butcher Shop", "Overall fundings/Personal Use/Butcher Shop", "Overall fundings/Retail/Butcher Shop", "Overall fundings/Services/Butcher Shop", "Overall fundings/Transportation/Butcher Shop", "Overall fundings/Wholesale/Butcher Shop", "Overall fundings/Agriculture/Cafe", "Overall fundings/Arts/Cafe", "Overall fundings/Clothing/Cafe", "Overall fundings/Construction/Cafe", "Overall fundings/Education/Cafe", "Overall fundings/Entertainment/Cafe", "Overall fundings/Food/Cafe", "Overall fundings/Health/Cafe", "Overall fundings/Housing/Cafe", "Overall fundings/Manufacturing/Cafe", "Overall fundings/Personal Use/Cafe", "Overall fundings/Retail/Cafe", "Overall fundings/Services/Cafe", "Overall fundings/Transportation/Cafe", "Overall fundings/Wholesale/Cafe", "Overall fundings/Agriculture/Call Center", "Overall fundings/Arts/Call Center", "Overall fundings/Clothing/Call Center", "Overall fundings/Construction/Call Center", "Overall fundings/Education/Call Center", "Overall fundings/Entertainment/Call Center", "Overall fundings/Food/Call Center", "Overall fundings/Health/Call Center", "Overall fundings/Housing/Call Center", "Overall fundings/Manufacturing/Call Center", "Overall fundings/Personal Use/Call Center", "Overall fundings/Retail/Call Center", "Overall fundings/Services/Call Center", "Overall fundings/Transportation/Call Center", "Overall fundings/Wholesale/Call Center", "Overall fundings/Agriculture/Carpentry", "Overall fundings/Arts/Carpentry", "Overall fundings/Clothing/Carpentry", "Overall fundings/Construction/Carpentry", "Overall fundings/Education/Carpentry", "Overall fundings/Entertainment/Carpentry", "Overall fundings/Food/Carpentry", "Overall fundings/Health/Carpentry", "Overall fundings/Housing/Carpentry", "Overall fundings/Manufacturing/Carpentry", "Overall fundings/Personal Use/Carpentry", "Overall fundings/Retail/Carpentry", "Overall fundings/Services/Carpentry", "Overall fundings/Transportation/Carpentry", "Overall fundings/Wholesale/Carpentry", "Overall fundings/Agriculture/Catering", "Overall fundings/Arts/Catering", "Overall fundings/Clothing/Catering", "Overall fundings/Construction/Catering", "Overall fundings/Education/Catering", "Overall fundings/Entertainment/Catering", "Overall fundings/Food/Catering", "Overall fundings/Health/Catering", "Overall fundings/Housing/Catering", "Overall fundings/Manufacturing/Catering", "Overall fundings/Personal Use/Catering", "Overall fundings/Retail/Catering", "Overall fundings/Services/Catering", "Overall fundings/Transportation/Catering", "Overall fundings/Wholesale/Catering", "Overall fundings/Agriculture/Cattle", "Overall fundings/Arts/Cattle", "Overall fundings/Clothing/Cattle", "Overall fundings/Construction/Cattle", "Overall fundings/Education/Cattle", "Overall fundings/Entertainment/Cattle", "Overall fundings/Food/Cattle", "Overall fundings/Health/Cattle", "Overall fundings/Housing/Cattle", "Overall fundings/Manufacturing/Cattle", "Overall fundings/Personal Use/Cattle", "Overall fundings/Retail/Cattle", "Overall fundings/Services/Cattle", "Overall fundings/Transportation/Cattle", "Overall fundings/Wholesale/Cattle", "Overall fundings/Agriculture/Celebrations", "Overall fundings/Arts/Celebrations", "Overall fundings/Clothing/Celebrations", "Overall fundings/Construction/Celebrations", "Overall fundings/Education/Celebrations", "Overall fundings/Entertainment/Celebrations", "Overall fundings/Food/Celebrations", "Overall fundings/Health/Celebrations", "Overall fundings/Housing/Celebrations", "Overall fundings/Manufacturing/Celebrations", "Overall fundings/Personal Use/Celebrations", "Overall fundings/Retail/Celebrations", "Overall fundings/Services/Celebrations", "Overall fundings/Transportation/Celebrations", "Overall fundings/Wholesale/Celebrations", "Overall fundings/Agriculture/Cement", "Overall fundings/Arts/Cement", "Overall fundings/Clothing/Cement", "Overall fundings/Construction/Cement", "Overall fundings/Education/Cement", "Overall fundings/Entertainment/Cement", "Overall fundings/Food/Cement", "Overall fundings/Health/Cement", "Overall fundings/Housing/Cement", "Overall fundings/Manufacturing/Cement", "Overall fundings/Personal Use/Cement", "Overall fundings/Retail/Cement", "Overall fundings/Services/Cement", "Overall fundings/Transportation/Cement", "Overall fundings/Wholesale/Cement", "Overall fundings/Agriculture/Cereals", "Overall fundings/Arts/Cereals", "Overall fundings/Clothing/Cereals", "Overall fundings/Construction/Cereals", "Overall fundings/Education/Cereals", "Overall fundings/Entertainment/Cereals", "Overall fundings/Food/Cereals", "Overall fundings/Health/Cereals", "Overall fundings/Housing/Cereals", "Overall fundings/Manufacturing/Cereals", "Overall fundings/Personal Use/Cereals", "Overall fundings/Retail/Cereals", "Overall fundings/Services/Cereals", "Overall fundings/Transportation/Cereals", "Overall fundings/Wholesale/Cereals", "Overall fundings/Agriculture/Charcoal Sales", "Overall fundings/Arts/Charcoal Sales", "Overall fundings/Clothing/Charcoal Sales", "Overall fundings/Construction/Charcoal Sales", "Overall fundings/Education/Charcoal Sales", "Overall fundings/Entertainment/Charcoal Sales", "Overall fundings/Food/Charcoal Sales", "Overall fundings/Health/Charcoal Sales", "Overall fundings/Housing/Charcoal Sales", "Overall fundings/Manufacturing/Charcoal Sales", "Overall fundings/Personal Use/Charcoal Sales", "Overall fundings/Retail/Charcoal Sales", "Overall fundings/Services/Charcoal Sales", "Overall fundings/Transportation/Charcoal Sales", "Overall fundings/Wholesale/Charcoal Sales", "Overall fundings/Agriculture/Cheese Making", "Overall fundings/Arts/Cheese Making", "Overall fundings/Clothing/Cheese Making", "Overall fundings/Construction/Cheese Making", "Overall fundings/Education/Cheese Making", "Overall fundings/Entertainment/Cheese Making", "Overall fundings/Food/Cheese Making", "Overall fundings/Health/Cheese Making", "Overall fundings/Housing/Cheese Making", "Overall fundings/Manufacturing/Cheese Making", "Overall fundings/Personal Use/Cheese Making", "Overall fundings/Retail/Cheese Making", "Overall fundings/Services/Cheese Making", "Overall fundings/Transportation/Cheese Making", "Overall fundings/Wholesale/Cheese Making", "Overall fundings/Agriculture/Child Care", "Overall fundings/Arts/Child Care", "Overall fundings/Clothing/Child Care", "Overall fundings/Construction/Child Care", "Overall fundings/Education/Child Care", "Overall fundings/Entertainment/Child Care", "Overall fundings/Food/Child Care", "Overall fundings/Health/Child Care", "Overall fundings/Housing/Child Care", "Overall fundings/Manufacturing/Child Care", "Overall fundings/Personal Use/Child Care", "Overall fundings/Retail/Child Care", "Overall fundings/Services/Child Care", "Overall fundings/Transportation/Child Care", "Overall fundings/Wholesale/Child Care", "Overall fundings/Agriculture/Cleaning Services", "Overall fundings/Arts/Cleaning Services", "Overall fundings/Clothing/Cleaning Services", "Overall fundings/Construction/Cleaning Services", "Overall fundings/Education/Cleaning Services", "Overall fundings/Entertainment/Cleaning Services", "Overall fundings/Food/Cleaning Services", "Overall fundings/Health/Cleaning Services", "Overall fundings/Housing/Cleaning Services", "Overall fundings/Manufacturing/Cleaning Services", "Overall fundings/Personal Use/Cleaning Services", "Overall fundings/Retail/Cleaning Services", "Overall fundings/Services/Cleaning Services", "Overall fundings/Transportation/Cleaning Services", "Overall fundings/Wholesale/Cleaning Services", "Overall fundings/Agriculture/Cloth & Dressmaking Supplies", "Overall fundings/Arts/Cloth & Dressmaking Supplies", "Overall fundings/Clothing/Cloth & Dressmaking Supplies", "Overall fundings/Construction/Cloth & Dressmaking Supplies", "Overall fundings/Education/Cloth & Dressmaking Supplies", "Overall fundings/Entertainment/Cloth & Dressmaking Supplies", "Overall fundings/Food/Cloth & Dressmaking Supplies", "Overall fundings/Health/Cloth & Dressmaking Supplies", "Overall fundings/Housing/Cloth & Dressmaking Supplies", "Overall fundings/Manufacturing/Cloth & Dressmaking Supplies", "Overall fundings/Personal Use/Cloth & Dressmaking Supplies", "Overall fundings/Retail/Cloth & Dressmaking Supplies", "Overall fundings/Services/Cloth & Dressmaking Supplies", "Overall fundings/Transportation/Cloth & Dressmaking Supplies", "Overall fundings/Wholesale/Cloth & Dressmaking Supplies", "Overall fundings/Agriculture/Clothing", "Overall fundings/Arts/Clothing", "Overall fundings/Clothing/Clothing", "Overall fundings/Construction/Clothing", "Overall fundings/Education/Clothing", "Overall fundings/Entertainment/Clothing", "Overall fundings/Food/Clothing", "Overall fundings/Health/Clothing", "Overall fundings/Housing/Clothing", "Overall fundings/Manufacturing/Clothing", "Overall fundings/Personal Use/Clothing", "Overall fundings/Retail/Clothing", "Overall fundings/Services/Clothing", "Overall fundings/Transportation/Clothing", "Overall fundings/Wholesale/Clothing", "Overall fundings/Agriculture/Clothing Sales", "Overall fundings/Arts/Clothing Sales", "Overall fundings/Clothing/Clothing Sales", "Overall fundings/Construction/Clothing Sales", "Overall fundings/Education/Clothing Sales", "Overall fundings/Entertainment/Clothing Sales", "Overall fundings/Food/Clothing Sales", "Overall fundings/Health/Clothing Sales", "Overall fundings/Housing/Clothing Sales", "Overall fundings/Manufacturing/Clothing Sales", "Overall fundings/Personal Use/Clothing Sales", "Overall fundings/Retail/Clothing Sales", "Overall fundings/Services/Clothing Sales", "Overall fundings/Transportation/Clothing Sales", "Overall fundings/Wholesale/Clothing Sales", "Overall fundings/Agriculture/Cobbler", "Overall fundings/Arts/Cobbler", "Overall fundings/Clothing/Cobbler", "Overall fundings/Construction/Cobbler", "Overall fundings/Education/Cobbler", "Overall fundings/Entertainment/Cobbler", "Overall fundings/Food/Cobbler", "Overall fundings/Health/Cobbler", "Overall fundings/Housing/Cobbler", "Overall fundings/Manufacturing/Cobbler", "Overall fundings/Personal Use/Cobbler", "Overall fundings/Retail/Cobbler", "Overall fundings/Services/Cobbler", "Overall fundings/Transportation/Cobbler", "Overall fundings/Wholesale/Cobbler", "Overall fundings/Agriculture/Communications", "Overall fundings/Arts/Communications", "Overall fundings/Clothing/Communications", "Overall fundings/Construction/Communications", "Overall fundings/Education/Communications", "Overall fundings/Entertainment/Communications", "Overall fundings/Food/Communications", "Overall fundings/Health/Communications", "Overall fundings/Housing/Communications", "Overall fundings/Manufacturing/Communications", "Overall fundings/Personal Use/Communications", "Overall fundings/Retail/Communications", "Overall fundings/Services/Communications", "Overall fundings/Transportation/Communications", "Overall fundings/Wholesale/Communications", "Overall fundings/Agriculture/Computer", "Overall fundings/Arts/Computer", "Overall fundings/Clothing/Computer", "Overall fundings/Construction/Computer", "Overall fundings/Education/Computer", "Overall fundings/Entertainment/Computer", "Overall fundings/Food/Computer", "Overall fundings/Health/Computer", "Overall fundings/Housing/Computer", "Overall fundings/Manufacturing/Computer", "Overall fundings/Personal Use/Computer", "Overall fundings/Retail/Computer", "Overall fundings/Services/Computer", "Overall fundings/Transportation/Computer", "Overall fundings/Wholesale/Computer", "Overall fundings/Agriculture/Computers", "Overall fundings/Arts/Computers", "Overall fundings/Clothing/Computers", "Overall fundings/Construction/Computers", "Overall fundings/Education/Computers", "Overall fundings/Entertainment/Computers", "Overall fundings/Food/Computers", "Overall fundings/Health/Computers", "Overall fundings/Housing/Computers", "Overall fundings/Manufacturing/Computers", "Overall fundings/Personal Use/Computers", "Overall fundings/Retail/Computers", "Overall fundings/Services/Computers", "Overall fundings/Transportation/Computers", "Overall fundings/Wholesale/Computers", "Overall fundings/Agriculture/Construction", "Overall fundings/Arts/Construction", "Overall fundings/Clothing/Construction", "Overall fundings/Construction/Construction", "Overall fundings/Education/Construction", "Overall fundings/Entertainment/Construction", "Overall fundings/Food/Construction", "Overall fundings/Health/Construction", "Overall fundings/Housing/Construction", "Overall fundings/Manufacturing/Construction", "Overall fundings/Personal Use/Construction", "Overall fundings/Retail/Construction", "Overall fundings/Services/Construction", "Overall fundings/Transportation/Construction", "Overall fundings/Wholesale/Construction", "Overall fundings/Agriculture/Construction Supplies", "Overall fundings/Arts/Construction Supplies", "Overall fundings/Clothing/Construction Supplies", "Overall fundings/Construction/Construction Supplies", "Overall fundings/Education/Construction Supplies", "Overall fundings/Entertainment/Construction Supplies", "Overall fundings/Food/Construction Supplies", "Overall fundings/Health/Construction Supplies", "Overall fundings/Housing/Construction Supplies", "Overall fundings/Manufacturing/Construction Supplies", "Overall fundings/Personal Use/Construction Supplies", "Overall fundings/Retail/Construction Supplies", "Overall fundings/Services/Construction Supplies", "Overall fundings/Transportation/Construction Supplies", "Overall fundings/Wholesale/Construction Supplies", "Overall fundings/Agriculture/Consumer Goods", "Overall fundings/Arts/Consumer Goods", "Overall fundings/Clothing/Consumer Goods", "Overall fundings/Construction/Consumer Goods", "Overall fundings/Education/Consumer Goods", "Overall fundings/Entertainment/Consumer Goods", "Overall fundings/Food/Consumer Goods", "Overall fundings/Health/Consumer Goods", "Overall fundings/Housing/Consumer Goods", "Overall fundings/Manufacturing/Consumer Goods", "Overall fundings/Personal Use/Consumer Goods", "Overall fundings/Retail/Consumer Goods", "Overall fundings/Services/Consumer Goods", "Overall fundings/Transportation/Consumer Goods", "Overall fundings/Wholesale/Consumer Goods", "Overall fundings/Agriculture/Cosmetics Sales", "Overall fundings/Arts/Cosmetics Sales", "Overall fundings/Clothing/Cosmetics Sales", "Overall fundings/Construction/Cosmetics Sales", "Overall fundings/Education/Cosmetics Sales", "Overall fundings/Entertainment/Cosmetics Sales", "Overall fundings/Food/Cosmetics Sales", "Overall fundings/Health/Cosmetics Sales", "Overall fundings/Housing/Cosmetics Sales", "Overall fundings/Manufacturing/Cosmetics Sales", "Overall fundings/Personal Use/Cosmetics Sales", "Overall fundings/Retail/Cosmetics Sales", "Overall fundings/Services/Cosmetics Sales", "Overall fundings/Transportation/Cosmetics Sales", "Overall fundings/Wholesale/Cosmetics Sales", "Overall fundings/Agriculture/Crafts", "Overall fundings/Arts/Crafts", "Overall fundings/Clothing/Crafts", "Overall fundings/Construction/Crafts", "Overall fundings/Education/Crafts", "Overall fundings/Entertainment/Crafts", "Overall fundings/Food/Crafts", "Overall fundings/Health/Crafts", "Overall fundings/Housing/Crafts", "Overall fundings/Manufacturing/Crafts", "Overall fundings/Personal Use/Crafts", "Overall fundings/Retail/Crafts", "Overall fundings/Services/Crafts", "Overall fundings/Transportation/Crafts", "Overall fundings/Wholesale/Crafts", "Overall fundings/Agriculture/Dairy", "Overall fundings/Arts/Dairy", "Overall fundings/Clothing/Dairy", "Overall fundings/Construction/Dairy", "Overall fundings/Education/Dairy", "Overall fundings/Entertainment/Dairy", "Overall fundings/Food/Dairy", "Overall fundings/Health/Dairy", "Overall fundings/Housing/Dairy", "Overall fundings/Manufacturing/Dairy", "Overall fundings/Personal Use/Dairy", "Overall fundings/Retail/Dairy", "Overall fundings/Services/Dairy", "Overall fundings/Transportation/Dairy", "Overall fundings/Wholesale/Dairy", "Overall fundings/Agriculture/Decorations Sales", "Overall fundings/Arts/Decorations Sales", "Overall fundings/Clothing/Decorations Sales", "Overall fundings/Construction/Decorations Sales", "Overall fundings/Education/Decorations Sales", "Overall fundings/Entertainment/Decorations Sales", "Overall fundings/Food/Decorations Sales", "Overall fundings/Health/Decorations Sales", "Overall fundings/Housing/Decorations Sales", "Overall fundings/Manufacturing/Decorations Sales", "Overall fundings/Personal Use/Decorations Sales", "Overall fundings/Retail/Decorations Sales", "Overall fundings/Services/Decorations Sales", "Overall fundings/Transportation/Decorations Sales", "Overall fundings/Wholesale/Decorations Sales", "Overall fundings/Agriculture/Dental", "Overall fundings/Arts/Dental", "Overall fundings/Clothing/Dental", "Overall fundings/Construction/Dental", "Overall fundings/Education/Dental", "Overall fundings/Entertainment/Dental", "Overall fundings/Food/Dental", "Overall fundings/Health/Dental", "Overall fundings/Housing/Dental", "Overall fundings/Manufacturing/Dental", "Overall fundings/Personal Use/Dental", "Overall fundings/Retail/Dental", "Overall fundings/Services/Dental", "Overall fundings/Transportation/Dental", "Overall fundings/Wholesale/Dental", "Overall fundings/Agriculture/Education provider", "Overall fundings/Arts/Education provider", "Overall fundings/Clothing/Education provider", "Overall fundings/Construction/Education provider", "Overall fundings/Education/Education provider", "Overall fundings/Entertainment/Education provider", "Overall fundings/Food/Education provider", "Overall fundings/Health/Education provider", "Overall fundings/Housing/Education provider", "Overall fundings/Manufacturing/Education provider", "Overall fundings/Personal Use/Education provider", "Overall fundings/Retail/Education provider", "Overall fundings/Services/Education provider", "Overall fundings/Transportation/Education provider", "Overall fundings/Wholesale/Education provider", "Overall fundings/Agriculture/Electrical Goods", "Overall fundings/Arts/Electrical Goods", "Overall fundings/Clothing/Electrical Goods", "Overall fundings/Construction/Electrical Goods", "Overall fundings/Education/Electrical Goods", "Overall fundings/Entertainment/Electrical Goods", "Overall fundings/Food/Electrical Goods", "Overall fundings/Health/Electrical Goods", "Overall fundings/Housing/Electrical Goods", "Overall fundings/Manufacturing/Electrical Goods", "Overall fundings/Personal Use/Electrical Goods", "Overall fundings/Retail/Electrical Goods", "Overall fundings/Services/Electrical Goods", "Overall fundings/Transportation/Electrical Goods", "Overall fundings/Wholesale/Electrical Goods", "Overall fundings/Agriculture/Electrician", "Overall fundings/Arts/Electrician", "Overall fundings/Clothing/Electrician", "Overall fundings/Construction/Electrician", "Overall fundings/Education/Electrician", "Overall fundings/Entertainment/Electrician", "Overall fundings/Food/Electrician", "Overall fundings/Health/Electrician", "Overall fundings/Housing/Electrician", "Overall fundings/Manufacturing/Electrician", "Overall fundings/Personal Use/Electrician", "Overall fundings/Retail/Electrician", "Overall fundings/Services/Electrician", "Overall fundings/Transportation/Electrician", "Overall fundings/Wholesale/Electrician", "Overall fundings/Agriculture/Electronics Repair", "Overall fundings/Arts/Electronics Repair", "Overall fundings/Clothing/Electronics Repair", "Overall fundings/Construction/Electronics Repair", "Overall fundings/Education/Electronics Repair", "Overall fundings/Entertainment/Electronics Repair", "Overall fundings/Food/Electronics Repair", "Overall fundings/Health/Electronics Repair", "Overall fundings/Housing/Electronics Repair", "Overall fundings/Manufacturing/Electronics Repair", "Overall fundings/Personal Use/Electronics Repair", "Overall fundings/Retail/Electronics Repair", "Overall fundings/Services/Electronics Repair", "Overall fundings/Transportation/Electronics Repair", "Overall fundings/Wholesale/Electronics Repair", "Overall fundings/Agriculture/Electronics Sales", "Overall fundings/Arts/Electronics Sales", "Overall fundings/Clothing/Electronics Sales", "Overall fundings/Construction/Electronics Sales", "Overall fundings/Education/Electronics Sales", "Overall fundings/Entertainment/Electronics Sales", "Overall fundings/Food/Electronics Sales", "Overall fundings/Health/Electronics Sales", "Overall fundings/Housing/Electronics Sales", "Overall fundings/Manufacturing/Electronics Sales", "Overall fundings/Personal Use/Electronics Sales", "Overall fundings/Retail/Electronics Sales", "Overall fundings/Services/Electronics Sales", "Overall fundings/Transportation/Electronics Sales", "Overall fundings/Wholesale/Electronics Sales", "Overall fundings/Agriculture/Embroidery", "Overall fundings/Arts/Embroidery", "Overall fundings/Clothing/Embroidery", "Overall fundings/Construction/Embroidery", "Overall fundings/Education/Embroidery", "Overall fundings/Entertainment/Embroidery", "Overall fundings/Food/Embroidery", "Overall fundings/Health/Embroidery", "Overall fundings/Housing/Embroidery", "Overall fundings/Manufacturing/Embroidery", "Overall fundings/Personal Use/Embroidery", "Overall fundings/Retail/Embroidery", "Overall fundings/Services/Embroidery", "Overall fundings/Transportation/Embroidery", "Overall fundings/Wholesale/Embroidery", "Overall fundings/Agriculture/Energy", "Overall fundings/Arts/Energy", "Overall fundings/Clothing/Energy", "Overall fundings/Construction/Energy", "Overall fundings/Education/Energy", "Overall fundings/Entertainment/Energy", "Overall fundings/Food/Energy", "Overall fundings/Health/Energy", "Overall fundings/Housing/Energy", "Overall fundings/Manufacturing/Energy", "Overall fundings/Personal Use/Energy", "Overall fundings/Retail/Energy", "Overall fundings/Services/Energy", "Overall fundings/Transportation/Energy", "Overall fundings/Wholesale/Energy", "Overall fundings/Agriculture/Entertainment", "Overall fundings/Arts/Entertainment", "Overall fundings/Clothing/Entertainment", "Overall fundings/Construction/Entertainment", "Overall fundings/Education/Entertainment", "Overall fundings/Entertainment/Entertainment", "Overall fundings/Food/Entertainment", "Overall fundings/Health/Entertainment", "Overall fundings/Housing/Entertainment", "Overall fundings/Manufacturing/Entertainment", "Overall fundings/Personal Use/Entertainment", "Overall fundings/Retail/Entertainment", "Overall fundings/Services/Entertainment", "Overall fundings/Transportation/Entertainment", "Overall fundings/Wholesale/Entertainment", "Overall fundings/Agriculture/Event Planning", "Overall fundings/Arts/Event Planning", "Overall fundings/Clothing/Event Planning", "Overall fundings/Construction/Event Planning", "Overall fundings/Education/Event Planning", "Overall fundings/Entertainment/Event Planning", "Overall fundings/Food/Event Planning", "Overall fundings/Health/Event Planning", "Overall fundings/Housing/Event Planning", "Overall fundings/Manufacturing/Event Planning", "Overall fundings/Personal Use/Event Planning", "Overall fundings/Retail/Event Planning", "Overall fundings/Services/Event Planning", "Overall fundings/Transportation/Event Planning", "Overall fundings/Wholesale/Event Planning", "Overall fundings/Agriculture/Farm Supplies", "Overall fundings/Arts/Farm Supplies", "Overall fundings/Clothing/Farm Supplies", "Overall fundings/Construction/Farm Supplies", "Overall fundings/Education/Farm Supplies", "Overall fundings/Entertainment/Farm Supplies", "Overall fundings/Food/Farm Supplies", "Overall fundings/Health/Farm Supplies", "Overall fundings/Housing/Farm Supplies", "Overall fundings/Manufacturing/Farm Supplies", "Overall fundings/Personal Use/Farm Supplies", "Overall fundings/Retail/Farm Supplies", "Overall fundings/Services/Farm Supplies", "Overall fundings/Transportation/Farm Supplies", "Overall fundings/Wholesale/Farm Supplies", "Overall fundings/Agriculture/Farming", "Overall fundings/Arts/Farming", "Overall fundings/Clothing/Farming", "Overall fundings/Construction/Farming", "Overall fundings/Education/Farming", "Overall fundings/Entertainment/Farming", "Overall fundings/Food/Farming", "Overall fundings/Health/Farming", "Overall fundings/Housing/Farming", "Overall fundings/Manufacturing/Farming", "Overall fundings/Personal Use/Farming", "Overall fundings/Retail/Farming", "Overall fundings/Services/Farming", "Overall fundings/Transportation/Farming", "Overall fundings/Wholesale/Farming", "Overall fundings/Agriculture/Film", "Overall fundings/Arts/Film", "Overall fundings/Clothing/Film", "Overall fundings/Construction/Film", "Overall fundings/Education/Film", "Overall fundings/Entertainment/Film", "Overall fundings/Food/Film", "Overall fundings/Health/Film", "Overall fundings/Housing/Film", "Overall fundings/Manufacturing/Film", "Overall fundings/Personal Use/Film", "Overall fundings/Retail/Film", "Overall fundings/Services/Film", "Overall fundings/Transportation/Film", "Overall fundings/Wholesale/Film", "Overall fundings/Agriculture/Fish Selling", "Overall fundings/Arts/Fish Selling", "Overall fundings/Clothing/Fish Selling", "Overall fundings/Construction/Fish Selling", "Overall fundings/Education/Fish Selling", "Overall fundings/Entertainment/Fish Selling", "Overall fundings/Food/Fish Selling", "Overall fundings/Health/Fish Selling", "Overall fundings/Housing/Fish Selling", "Overall fundings/Manufacturing/Fish Selling", "Overall fundings/Personal Use/Fish Selling", "Overall fundings/Retail/Fish Selling", "Overall fundings/Services/Fish Selling", "Overall fundings/Transportation/Fish Selling", "Overall fundings/Wholesale/Fish Selling", "Overall fundings/Agriculture/Fishing", "Overall fundings/Arts/Fishing", "Overall fundings/Clothing/Fishing", "Overall fundings/Construction/Fishing", "Overall fundings/Education/Fishing", "Overall fundings/Entertainment/Fishing", "Overall fundings/Food/Fishing", "Overall fundings/Health/Fishing", "Overall fundings/Housing/Fishing", "Overall fundings/Manufacturing/Fishing", "Overall fundings/Personal Use/Fishing", "Overall fundings/Retail/Fishing", "Overall fundings/Services/Fishing", "Overall fundings/Transportation/Fishing", "Overall fundings/Wholesale/Fishing", "Overall fundings/Agriculture/Florist", "Overall fundings/Arts/Florist", "Overall fundings/Clothing/Florist", "Overall fundings/Construction/Florist", "Overall fundings/Education/Florist", "Overall fundings/Entertainment/Florist", "Overall fundings/Food/Florist", "Overall fundings/Health/Florist", "Overall fundings/Housing/Florist", "Overall fundings/Manufacturing/Florist", "Overall fundings/Personal Use/Florist", "Overall fundings/Retail/Florist", "Overall fundings/Services/Florist", "Overall fundings/Transportation/Florist", "Overall fundings/Wholesale/Florist", "Overall fundings/Agriculture/Flowers", "Overall fundings/Arts/Flowers", "Overall fundings/Clothing/Flowers", "Overall fundings/Construction/Flowers", "Overall fundings/Education/Flowers", "Overall fundings/Entertainment/Flowers", "Overall fundings/Food/Flowers", "Overall fundings/Health/Flowers", "Overall fundings/Housing/Flowers", "Overall fundings/Manufacturing/Flowers", "Overall fundings/Personal Use/Flowers", "Overall fundings/Retail/Flowers", "Overall fundings/Services/Flowers", "Overall fundings/Transportation/Flowers", "Overall fundings/Wholesale/Flowers", "Overall fundings/Agriculture/Food", "Overall fundings/Arts/Food", "Overall fundings/Clothing/Food", "Overall fundings/Construction/Food", "Overall fundings/Education/Food", "Overall fundings/Entertainment/Food", "Overall fundings/Food/Food", "Overall fundings/Health/Food", "Overall fundings/Housing/Food", "Overall fundings/Manufacturing/Food", "Overall fundings/Personal Use/Food", "Overall fundings/Retail/Food", "Overall fundings/Services/Food", "Overall fundings/Transportation/Food", "Overall fundings/Wholesale/Food", "Overall fundings/Agriculture/Food Market", "Overall fundings/Arts/Food Market", "Overall fundings/Clothing/Food Market", "Overall fundings/Construction/Food Market", "Overall fundings/Education/Food Market", "Overall fundings/Entertainment/Food Market", "Overall fundings/Food/Food Market", "Overall fundings/Health/Food Market", "Overall fundings/Housing/Food Market", "Overall fundings/Manufacturing/Food Market", "Overall fundings/Personal Use/Food Market", "Overall fundings/Retail/Food Market", "Overall fundings/Services/Food Market", "Overall fundings/Transportation/Food Market", "Overall fundings/Wholesale/Food Market", "Overall fundings/Agriculture/Food Production/Sales", "Overall fundings/Arts/Food Production/Sales", "Overall fundings/Clothing/Food Production/Sales", "Overall fundings/Construction/Food Production/Sales", "Overall fundings/Education/Food Production/Sales", "Overall fundings/Entertainment/Food Production/Sales", "Overall fundings/Food/Food Production/Sales", "Overall fundings/Health/Food Production/Sales", "Overall fundings/Housing/Food Production/Sales", "Overall fundings/Manufacturing/Food Production/Sales", "Overall fundings/Personal Use/Food Production/Sales", "Overall fundings/Retail/Food Production/Sales", "Overall fundings/Services/Food Production/Sales", "Overall fundings/Transportation/Food Production/Sales", "Overall fundings/Wholesale/Food Production/Sales", "Overall fundings/Agriculture/Food Stall", "Overall fundings/Arts/Food Stall", "Overall fundings/Clothing/Food Stall", "Overall fundings/Construction/Food Stall", "Overall fundings/Education/Food Stall", "Overall fundings/Entertainment/Food Stall", "Overall fundings/Food/Food Stall", "Overall fundings/Health/Food Stall", "Overall fundings/Housing/Food Stall", "Overall fundings/Manufacturing/Food Stall", "Overall fundings/Personal Use/Food Stall", "Overall fundings/Retail/Food Stall", "Overall fundings/Services/Food Stall", "Overall fundings/Transportation/Food Stall", "Overall fundings/Wholesale/Food Stall", "Overall fundings/Agriculture/Fruits & Vegetables", "Overall fundings/Arts/Fruits & Vegetables", "Overall fundings/Clothing/Fruits & Vegetables", "Overall fundings/Construction/Fruits & Vegetables", "Overall fundings/Education/Fruits & Vegetables", "Overall fundings/Entertainment/Fruits & Vegetables", "Overall fundings/Food/Fruits & Vegetables", "Overall fundings/Health/Fruits & Vegetables", "Overall fundings/Housing/Fruits & Vegetables", "Overall fundings/Manufacturing/Fruits & Vegetables", "Overall fundings/Personal Use/Fruits & Vegetables", "Overall fundings/Retail/Fruits & Vegetables", "Overall fundings/Services/Fruits & Vegetables", "Overall fundings/Transportation/Fruits & Vegetables", "Overall fundings/Wholesale/Fruits & Vegetables", "Overall fundings/Agriculture/Fuel/Firewood", "Overall fundings/Arts/Fuel/Firewood", "Overall fundings/Clothing/Fuel/Firewood", "Overall fundings/Construction/Fuel/Firewood", "Overall fundings/Education/Fuel/Firewood", "Overall fundings/Entertainment/Fuel/Firewood", "Overall fundings/Food/Fuel/Firewood", "Overall fundings/Health/Fuel/Firewood", "Overall fundings/Housing/Fuel/Firewood", "Overall fundings/Manufacturing/Fuel/Firewood", "Overall fundings/Personal Use/Fuel/Firewood", "Overall fundings/Retail/Fuel/Firewood", "Overall fundings/Services/Fuel/Firewood", "Overall fundings/Transportation/Fuel/Firewood", "Overall fundings/Wholesale/Fuel/Firewood", "Overall fundings/Agriculture/Funerals", "Overall fundings/Arts/Funerals", "Overall fundings/Clothing/Funerals", "Overall fundings/Construction/Funerals", "Overall fundings/Education/Funerals", "Overall fundings/Entertainment/Funerals", "Overall fundings/Food/Funerals", "Overall fundings/Health/Funerals", "Overall fundings/Housing/Funerals", "Overall fundings/Manufacturing/Funerals", "Overall fundings/Personal Use/Funerals", "Overall fundings/Retail/Funerals", "Overall fundings/Services/Funerals", "Overall fundings/Transportation/Funerals", "Overall fundings/Wholesale/Funerals", "Overall fundings/Agriculture/Furniture Making", "Overall fundings/Arts/Furniture Making", "Overall fundings/Clothing/Furniture Making", "Overall fundings/Construction/Furniture Making", "Overall fundings/Education/Furniture Making", "Overall fundings/Entertainment/Furniture Making", "Overall fundings/Food/Furniture Making", "Overall fundings/Health/Furniture Making", "Overall fundings/Housing/Furniture Making", "Overall fundings/Manufacturing/Furniture Making", "Overall fundings/Personal Use/Furniture Making", "Overall fundings/Retail/Furniture Making", "Overall fundings/Services/Furniture Making", "Overall fundings/Transportation/Furniture Making", "Overall fundings/Wholesale/Furniture Making", "Overall fundings/Agriculture/Games", "Overall fundings/Arts/Games", "Overall fundings/Clothing/Games", "Overall fundings/Construction/Games", "Overall fundings/Education/Games", "Overall fundings/Entertainment/Games", "Overall fundings/Food/Games", "Overall fundings/Health/Games", "Overall fundings/Housing/Games", "Overall fundings/Manufacturing/Games", "Overall fundings/Personal Use/Games", "Overall fundings/Retail/Games", "Overall fundings/Services/Games", "Overall fundings/Transportation/Games", "Overall fundings/Wholesale/Games", "Overall fundings/Agriculture/General Store", "Overall fundings/Arts/General Store", "Overall fundings/Clothing/General Store", "Overall fundings/Construction/General Store", "Overall fundings/Education/General Store", "Overall fundings/Entertainment/General Store", "Overall fundings/Food/General Store", "Overall fundings/Health/General Store", "Overall fundings/Housing/General Store", "Overall fundings/Manufacturing/General Store", "Overall fundings/Personal Use/General Store", "Overall fundings/Retail/General Store", "Overall fundings/Services/General Store", "Overall fundings/Transportation/General Store", "Overall fundings/Wholesale/General Store", "Overall fundings/Agriculture/Goods Distribution", "Overall fundings/Arts/Goods Distribution", "Overall fundings/Clothing/Goods Distribution", "Overall fundings/Construction/Goods Distribution", "Overall fundings/Education/Goods Distribution", "Overall fundings/Entertainment/Goods Distribution", "Overall fundings/Food/Goods Distribution", "Overall fundings/Health/Goods Distribution", "Overall fundings/Housing/Goods Distribution", "Overall fundings/Manufacturing/Goods Distribution", "Overall fundings/Personal Use/Goods Distribution", "Overall fundings/Retail/Goods Distribution", "Overall fundings/Services/Goods Distribution", "Overall fundings/Transportation/Goods Distribution", "Overall fundings/Wholesale/Goods Distribution", "Overall fundings/Agriculture/Grocery Store", "Overall fundings/Arts/Grocery Store", "Overall fundings/Clothing/Grocery Store", "Overall fundings/Construction/Grocery Store", "Overall fundings/Education/Grocery Store", "Overall fundings/Entertainment/Grocery Store", "Overall fundings/Food/Grocery Store", "Overall fundings/Health/Grocery Store", "Overall fundings/Housing/Grocery Store", "Overall fundings/Manufacturing/Grocery Store", "Overall fundings/Personal Use/Grocery Store", "Overall fundings/Retail/Grocery Store", "Overall fundings/Services/Grocery Store", "Overall fundings/Transportation/Grocery Store", "Overall fundings/Wholesale/Grocery Store", "Overall fundings/Agriculture/Hardware", "Overall fundings/Arts/Hardware", "Overall fundings/Clothing/Hardware", "Overall fundings/Construction/Hardware", "Overall fundings/Education/Hardware", "Overall fundings/Entertainment/Hardware", "Overall fundings/Food/Hardware", "Overall fundings/Health/Hardware", "Overall fundings/Housing/Hardware", "Overall fundings/Manufacturing/Hardware", "Overall fundings/Personal Use/Hardware", "Overall fundings/Retail/Hardware", "Overall fundings/Services/Hardware", "Overall fundings/Transportation/Hardware", "Overall fundings/Wholesale/Hardware", "Overall fundings/Agriculture/Health", "Overall fundings/Arts/Health", "Overall fundings/Clothing/Health", "Overall fundings/Construction/Health", "Overall fundings/Education/Health", "Overall fundings/Entertainment/Health", "Overall fundings/Food/Health", "Overall fundings/Health/Health", "Overall fundings/Housing/Health", "Overall fundings/Manufacturing/Health", "Overall fundings/Personal Use/Health", "Overall fundings/Retail/Health", "Overall fundings/Services/Health", "Overall fundings/Transportation/Health", "Overall fundings/Wholesale/Health", "Overall fundings/Agriculture/Higher education costs", "Overall fundings/Arts/Higher education costs", "Overall fundings/Clothing/Higher education costs", "Overall fundings/Construction/Higher education costs", "Overall fundings/Education/Higher education costs", "Overall fundings/Entertainment/Higher education costs", "Overall fundings/Food/Higher education costs", "Overall fundings/Health/Higher education costs", "Overall fundings/Housing/Higher education costs", "Overall fundings/Manufacturing/Higher education costs", "Overall fundings/Personal Use/Higher education costs", "Overall fundings/Retail/Higher education costs", "Overall fundings/Services/Higher education costs", "Overall fundings/Transportation/Higher education costs", "Overall fundings/Wholesale/Higher education costs", "Overall fundings/Agriculture/Home Appliances", "Overall fundings/Arts/Home Appliances", "Overall fundings/Clothing/Home Appliances", "Overall fundings/Construction/Home Appliances", "Overall fundings/Education/Home Appliances", "Overall fundings/Entertainment/Home Appliances", "Overall fundings/Food/Home Appliances", "Overall fundings/Health/Home Appliances", "Overall fundings/Housing/Home Appliances", "Overall fundings/Manufacturing/Home Appliances", "Overall fundings/Personal Use/Home Appliances", "Overall fundings/Retail/Home Appliances", "Overall fundings/Services/Home Appliances", "Overall fundings/Transportation/Home Appliances", "Overall fundings/Wholesale/Home Appliances", "Overall fundings/Agriculture/Home Energy", "Overall fundings/Arts/Home Energy", "Overall fundings/Clothing/Home Energy", "Overall fundings/Construction/Home Energy", "Overall fundings/Education/Home Energy", "Overall fundings/Entertainment/Home Energy", "Overall fundings/Food/Home Energy", "Overall fundings/Health/Home Energy", "Overall fundings/Housing/Home Energy", "Overall fundings/Manufacturing/Home Energy", "Overall fundings/Personal Use/Home Energy", "Overall fundings/Retail/Home Energy", "Overall fundings/Services/Home Energy", "Overall fundings/Transportation/Home Energy", "Overall fundings/Wholesale/Home Energy", "Overall fundings/Agriculture/Home Products Sales", "Overall fundings/Arts/Home Products Sales", "Overall fundings/Clothing/Home Products Sales", "Overall fundings/Construction/Home Products Sales", "Overall fundings/Education/Home Products Sales", "Overall fundings/Entertainment/Home Products Sales", "Overall fundings/Food/Home Products Sales", "Overall fundings/Health/Home Products Sales", "Overall fundings/Housing/Home Products Sales", "Overall fundings/Manufacturing/Home Products Sales", "Overall fundings/Personal Use/Home Products Sales", "Overall fundings/Retail/Home Products Sales", "Overall fundings/Services/Home Products Sales", "Overall fundings/Transportation/Home Products Sales", "Overall fundings/Wholesale/Home Products Sales", "Overall fundings/Agriculture/Hotel", "Overall fundings/Arts/Hotel", "Overall fundings/Clothing/Hotel", "Overall fundings/Construction/Hotel", "Overall fundings/Education/Hotel", "Overall fundings/Entertainment/Hotel", "Overall fundings/Food/Hotel", "Overall fundings/Health/Hotel", "Overall fundings/Housing/Hotel", "Overall fundings/Manufacturing/Hotel", "Overall fundings/Personal Use/Hotel", "Overall fundings/Retail/Hotel", "Overall fundings/Services/Hotel", "Overall fundings/Transportation/Hotel", "Overall fundings/Wholesale/Hotel", "Overall fundings/Agriculture/Internet Cafe", "Overall fundings/Arts/Internet Cafe", "Overall fundings/Clothing/Internet Cafe", "Overall fundings/Construction/Internet Cafe", "Overall fundings/Education/Internet Cafe", "Overall fundings/Entertainment/Internet Cafe", "Overall fundings/Food/Internet Cafe", "Overall fundings/Health/Internet Cafe", "Overall fundings/Housing/Internet Cafe", "Overall fundings/Manufacturing/Internet Cafe", "Overall fundings/Personal Use/Internet Cafe", "Overall fundings/Retail/Internet Cafe", "Overall fundings/Services/Internet Cafe", "Overall fundings/Transportation/Internet Cafe", "Overall fundings/Wholesale/Internet Cafe", "Overall fundings/Agriculture/Jewelry", "Overall fundings/Arts/Jewelry", "Overall fundings/Clothing/Jewelry", "Overall fundings/Construction/Jewelry", "Overall fundings/Education/Jewelry", "Overall fundings/Entertainment/Jewelry", "Overall fundings/Food/Jewelry", "Overall fundings/Health/Jewelry", "Overall fundings/Housing/Jewelry", "Overall fundings/Manufacturing/Jewelry", "Overall fundings/Personal Use/Jewelry", "Overall fundings/Retail/Jewelry", "Overall fundings/Services/Jewelry", "Overall fundings/Transportation/Jewelry", "Overall fundings/Wholesale/Jewelry", "Overall fundings/Agriculture/Knitting", "Overall fundings/Arts/Knitting", "Overall fundings/Clothing/Knitting", "Overall fundings/Construction/Knitting", "Overall fundings/Education/Knitting", "Overall fundings/Entertainment/Knitting", "Overall fundings/Food/Knitting", "Overall fundings/Health/Knitting", "Overall fundings/Housing/Knitting", "Overall fundings/Manufacturing/Knitting", "Overall fundings/Personal Use/Knitting", "Overall fundings/Retail/Knitting", "Overall fundings/Services/Knitting", "Overall fundings/Transportation/Knitting", "Overall fundings/Wholesale/Knitting", "Overall fundings/Agriculture/Land Rental", "Overall fundings/Arts/Land Rental", "Overall fundings/Clothing/Land Rental", "Overall fundings/Construction/Land Rental", "Overall fundings/Education/Land Rental", "Overall fundings/Entertainment/Land Rental", "Overall fundings/Food/Land Rental", "Overall fundings/Health/Land Rental", "Overall fundings/Housing/Land Rental", "Overall fundings/Manufacturing/Land Rental", "Overall fundings/Personal Use/Land Rental", "Overall fundings/Retail/Land Rental", "Overall fundings/Services/Land Rental", "Overall fundings/Transportation/Land Rental", "Overall fundings/Wholesale/Land Rental", "Overall fundings/Agriculture/Landscaping / Gardening", "Overall fundings/Arts/Landscaping / Gardening", "Overall fundings/Clothing/Landscaping / Gardening", "Overall fundings/Construction/Landscaping / Gardening", "Overall fundings/Education/Landscaping / Gardening", "Overall fundings/Entertainment/Landscaping / Gardening", "Overall fundings/Food/Landscaping / Gardening", "Overall fundings/Health/Landscaping / Gardening", "Overall fundings/Housing/Landscaping / Gardening", "Overall fundings/Manufacturing/Landscaping / Gardening", "Overall fundings/Personal Use/Landscaping / Gardening", "Overall fundings/Retail/Landscaping / Gardening", "Overall fundings/Services/Landscaping / Gardening", "Overall fundings/Transportation/Landscaping / Gardening", "Overall fundings/Wholesale/Landscaping / Gardening", "Overall fundings/Agriculture/Laundry", "Overall fundings/Arts/Laundry", "Overall fundings/Clothing/Laundry", "Overall fundings/Construction/Laundry", "Overall fundings/Education/Laundry", "Overall fundings/Entertainment/Laundry", "Overall fundings/Food/Laundry", "Overall fundings/Health/Laundry", "Overall fundings/Housing/Laundry", "Overall fundings/Manufacturing/Laundry", "Overall fundings/Personal Use/Laundry", "Overall fundings/Retail/Laundry", "Overall fundings/Services/Laundry", "Overall fundings/Transportation/Laundry", "Overall fundings/Wholesale/Laundry", "Overall fundings/Agriculture/Liquor Store / Off-License", "Overall fundings/Arts/Liquor Store / Off-License", "Overall fundings/Clothing/Liquor Store / Off-License", "Overall fundings/Construction/Liquor Store / Off-License", "Overall fundings/Education/Liquor Store / Off-License", "Overall fundings/Entertainment/Liquor Store / Off-License", "Overall fundings/Food/Liquor Store / Off-License", "Overall fundings/Health/Liquor Store / Off-License", "Overall fundings/Housing/Liquor Store / Off-License", "Overall fundings/Manufacturing/Liquor Store / Off-License", "Overall fundings/Personal Use/Liquor Store / Off-License", "Overall fundings/Retail/Liquor Store / Off-License", "Overall fundings/Services/Liquor Store / Off-License", "Overall fundings/Transportation/Liquor Store / Off-License", "Overall fundings/Wholesale/Liquor Store / Off-License", "Overall fundings/Agriculture/Livestock", "Overall fundings/Arts/Livestock", "Overall fundings/Clothing/Livestock", "Overall fundings/Construction/Livestock", "Overall fundings/Education/Livestock", "Overall fundings/Entertainment/Livestock", "Overall fundings/Food/Livestock", "Overall fundings/Health/Livestock", "Overall fundings/Housing/Livestock", "Overall fundings/Manufacturing/Livestock", "Overall fundings/Personal Use/Livestock", "Overall fundings/Retail/Livestock", "Overall fundings/Services/Livestock", "Overall fundings/Transportation/Livestock", "Overall fundings/Wholesale/Livestock", "Overall fundings/Agriculture/Machine Shop", "Overall fundings/Arts/Machine Shop", "Overall fundings/Clothing/Machine Shop", "Overall fundings/Construction/Machine Shop", "Overall fundings/Education/Machine Shop", "Overall fundings/Entertainment/Machine Shop", "Overall fundings/Food/Machine Shop", "Overall fundings/Health/Machine Shop", "Overall fundings/Housing/Machine Shop", "Overall fundings/Manufacturing/Machine Shop", "Overall fundings/Personal Use/Machine Shop", "Overall fundings/Retail/Machine Shop", "Overall fundings/Services/Machine Shop", "Overall fundings/Transportation/Machine Shop", "Overall fundings/Wholesale/Machine Shop", "Overall fundings/Agriculture/Machinery Rental", "Overall fundings/Arts/Machinery Rental", "Overall fundings/Clothing/Machinery Rental", "Overall fundings/Construction/Machinery Rental", "Overall fundings/Education/Machinery Rental", "Overall fundings/Entertainment/Machinery Rental", "Overall fundings/Food/Machinery Rental", "Overall fundings/Health/Machinery Rental", "Overall fundings/Housing/Machinery Rental", "Overall fundings/Manufacturing/Machinery Rental", "Overall fundings/Personal Use/Machinery Rental", "Overall fundings/Retail/Machinery Rental", "Overall fundings/Services/Machinery Rental", "Overall fundings/Transportation/Machinery Rental", "Overall fundings/Wholesale/Machinery Rental", "Overall fundings/Agriculture/Manufacturing", "Overall fundings/Arts/Manufacturing", "Overall fundings/Clothing/Manufacturing", "Overall fundings/Construction/Manufacturing", "Overall fundings/Education/Manufacturing", "Overall fundings/Entertainment/Manufacturing", "Overall fundings/Food/Manufacturing", "Overall fundings/Health/Manufacturing", "Overall fundings/Housing/Manufacturing", "Overall fundings/Manufacturing/Manufacturing", "Overall fundings/Personal Use/Manufacturing", "Overall fundings/Retail/Manufacturing", "Overall fundings/Services/Manufacturing", "Overall fundings/Transportation/Manufacturing", "Overall fundings/Wholesale/Manufacturing", "Overall fundings/Agriculture/Medical Clinic", "Overall fundings/Arts/Medical Clinic", "Overall fundings/Clothing/Medical Clinic", "Overall fundings/Construction/Medical Clinic", "Overall fundings/Education/Medical Clinic", "Overall fundings/Entertainment/Medical Clinic", "Overall fundings/Food/Medical Clinic", "Overall fundings/Health/Medical Clinic", "Overall fundings/Housing/Medical Clinic", "Overall fundings/Manufacturing/Medical Clinic", "Overall fundings/Personal Use/Medical Clinic", "Overall fundings/Retail/Medical Clinic", "Overall fundings/Services/Medical Clinic", "Overall fundings/Transportation/Medical Clinic", "Overall fundings/Wholesale/Medical Clinic", "Overall fundings/Agriculture/Metal Shop", "Overall fundings/Arts/Metal Shop", "Overall fundings/Clothing/Metal Shop", "Overall fundings/Construction/Metal Shop", "Overall fundings/Education/Metal Shop", "Overall fundings/Entertainment/Metal Shop", "Overall fundings/Food/Metal Shop", "Overall fundings/Health/Metal Shop", "Overall fundings/Housing/Metal Shop", "Overall fundings/Manufacturing/Metal Shop", "Overall fundings/Personal Use/Metal Shop", "Overall fundings/Retail/Metal Shop", "Overall fundings/Services/Metal Shop", "Overall fundings/Transportation/Metal Shop", "Overall fundings/Wholesale/Metal Shop", "Overall fundings/Agriculture/Milk Sales", "Overall fundings/Arts/Milk Sales", "Overall fundings/Clothing/Milk Sales", "Overall fundings/Construction/Milk Sales", "Overall fundings/Education/Milk Sales", "Overall fundings/Entertainment/Milk Sales", "Overall fundings/Food/Milk Sales", "Overall fundings/Health/Milk Sales", "Overall fundings/Housing/Milk Sales", "Overall fundings/Manufacturing/Milk Sales", "Overall fundings/Personal Use/Milk Sales", "Overall fundings/Retail/Milk Sales", "Overall fundings/Services/Milk Sales", "Overall fundings/Transportation/Milk Sales", "Overall fundings/Wholesale/Milk Sales", "Overall fundings/Agriculture/Mobile Phones", "Overall fundings/Arts/Mobile Phones", "Overall fundings/Clothing/Mobile Phones", "Overall fundings/Construction/Mobile Phones", "Overall fundings/Education/Mobile Phones", "Overall fundings/Entertainment/Mobile Phones", "Overall fundings/Food/Mobile Phones", "Overall fundings/Health/Mobile Phones", "Overall fundings/Housing/Mobile Phones", "Overall fundings/Manufacturing/Mobile Phones", "Overall fundings/Personal Use/Mobile Phones", "Overall fundings/Retail/Mobile Phones", "Overall fundings/Services/Mobile Phones", "Overall fundings/Transportation/Mobile Phones", "Overall fundings/Wholesale/Mobile Phones", "Overall fundings/Agriculture/Mobile Transactions", "Overall fundings/Arts/Mobile Transactions", "Overall fundings/Clothing/Mobile Transactions", "Overall fundings/Construction/Mobile Transactions", "Overall fundings/Education/Mobile Transactions", "Overall fundings/Entertainment/Mobile Transactions", "Overall fundings/Food/Mobile Transactions", "Overall fundings/Health/Mobile Transactions", "Overall fundings/Housing/Mobile Transactions", "Overall fundings/Manufacturing/Mobile Transactions", "Overall fundings/Personal Use/Mobile Transactions", "Overall fundings/Retail/Mobile Transactions", "Overall fundings/Services/Mobile Transactions", "Overall fundings/Transportation/Mobile Transactions", "Overall fundings/Wholesale/Mobile Transactions", "Overall fundings/Agriculture/Motorcycle Repair", "Overall fundings/Arts/Motorcycle Repair", "Overall fundings/Clothing/Motorcycle Repair", "Overall fundings/Construction/Motorcycle Repair", "Overall fundings/Education/Motorcycle Repair", "Overall fundings/Entertainment/Motorcycle Repair", "Overall fundings/Food/Motorcycle Repair", "Overall fundings/Health/Motorcycle Repair", "Overall fundings/Housing/Motorcycle Repair", "Overall fundings/Manufacturing/Motorcycle Repair", "Overall fundings/Personal Use/Motorcycle Repair", "Overall fundings/Retail/Motorcycle Repair", "Overall fundings/Services/Motorcycle Repair", "Overall fundings/Transportation/Motorcycle Repair", "Overall fundings/Wholesale/Motorcycle Repair", "Overall fundings/Agriculture/Motorcycle Transport", "Overall fundings/Arts/Motorcycle Transport", "Overall fundings/Clothing/Motorcycle Transport", "Overall fundings/Construction/Motorcycle Transport", "Overall fundings/Education/Motorcycle Transport", "Overall fundings/Entertainment/Motorcycle Transport", "Overall fundings/Food/Motorcycle Transport", "Overall fundings/Health/Motorcycle Transport", "Overall fundings/Housing/Motorcycle Transport", "Overall fundings/Manufacturing/Motorcycle Transport", "Overall fundings/Personal Use/Motorcycle Transport", "Overall fundings/Retail/Motorcycle Transport", "Overall fundings/Services/Motorcycle Transport", "Overall fundings/Transportation/Motorcycle Transport", "Overall fundings/Wholesale/Motorcycle Transport", "Overall fundings/Agriculture/Movie Tapes & DVDs", "Overall fundings/Arts/Movie Tapes & DVDs", "Overall fundings/Clothing/Movie Tapes & DVDs", "Overall fundings/Construction/Movie Tapes & DVDs", "Overall fundings/Education/Movie Tapes & DVDs", "Overall fundings/Entertainment/Movie Tapes & DVDs", "Overall fundings/Food/Movie Tapes & DVDs", "Overall fundings/Health/Movie Tapes & DVDs", "Overall fundings/Housing/Movie Tapes & DVDs", "Overall fundings/Manufacturing/Movie Tapes & DVDs", "Overall fundings/Personal Use/Movie Tapes & DVDs", "Overall fundings/Retail/Movie Tapes & DVDs", "Overall fundings/Services/Movie Tapes & DVDs", "Overall fundings/Transportation/Movie Tapes & DVDs", "Overall fundings/Wholesale/Movie Tapes & DVDs", "Overall fundings/Agriculture/Music Discs & Tapes", "Overall fundings/Arts/Music Discs & Tapes", "Overall fundings/Clothing/Music Discs & Tapes", "Overall fundings/Construction/Music Discs & Tapes", "Overall fundings/Education/Music Discs & Tapes", "Overall fundings/Entertainment/Music Discs & Tapes", "Overall fundings/Food/Music Discs & Tapes", "Overall fundings/Health/Music Discs & Tapes", "Overall fundings/Housing/Music Discs & Tapes", "Overall fundings/Manufacturing/Music Discs & Tapes", "Overall fundings/Personal Use/Music Discs & Tapes", "Overall fundings/Retail/Music Discs & Tapes", "Overall fundings/Services/Music Discs & Tapes", "Overall fundings/Transportation/Music Discs & Tapes", "Overall fundings/Wholesale/Music Discs & Tapes", "Overall fundings/Agriculture/Musical Instruments", "Overall fundings/Arts/Musical Instruments", "Overall fundings/Clothing/Musical Instruments", "Overall fundings/Construction/Musical Instruments", "Overall fundings/Education/Musical Instruments", "Overall fundings/Entertainment/Musical Instruments", "Overall fundings/Food/Musical Instruments", "Overall fundings/Health/Musical Instruments", "Overall fundings/Housing/Musical Instruments", "Overall fundings/Manufacturing/Musical Instruments", "Overall fundings/Personal Use/Musical Instruments", "Overall fundings/Retail/Musical Instruments", "Overall fundings/Services/Musical Instruments", "Overall fundings/Transportation/Musical Instruments", "Overall fundings/Wholesale/Musical Instruments", "Overall fundings/Agriculture/Musical Performance", "Overall fundings/Arts/Musical Performance", "Overall fundings/Clothing/Musical Performance", "Overall fundings/Construction/Musical Performance", "Overall fundings/Education/Musical Performance", "Overall fundings/Entertainment/Musical Performance", "Overall fundings/Food/Musical Performance", "Overall fundings/Health/Musical Performance", "Overall fundings/Housing/Musical Performance", "Overall fundings/Manufacturing/Musical Performance", "Overall fundings/Personal Use/Musical Performance", "Overall fundings/Retail/Musical Performance", "Overall fundings/Services/Musical Performance", "Overall fundings/Transportation/Musical Performance", "Overall fundings/Wholesale/Musical Performance", "Overall fundings/Agriculture/Natural Medicines", "Overall fundings/Arts/Natural Medicines", "Overall fundings/Clothing/Natural Medicines", "Overall fundings/Construction/Natural Medicines", "Overall fundings/Education/Natural Medicines", "Overall fundings/Entertainment/Natural Medicines", "Overall fundings/Food/Natural Medicines", "Overall fundings/Health/Natural Medicines", "Overall fundings/Housing/Natural Medicines", "Overall fundings/Manufacturing/Natural Medicines", "Overall fundings/Personal Use/Natural Medicines", "Overall fundings/Retail/Natural Medicines", "Overall fundings/Services/Natural Medicines", "Overall fundings/Transportation/Natural Medicines", "Overall fundings/Wholesale/Natural Medicines", "Overall fundings/Agriculture/Office Supplies", "Overall fundings/Arts/Office Supplies", "Overall fundings/Clothing/Office Supplies", "Overall fundings/Construction/Office Supplies", "Overall fundings/Education/Office Supplies", "Overall fundings/Entertainment/Office Supplies", "Overall fundings/Food/Office Supplies", "Overall fundings/Health/Office Supplies", "Overall fundings/Housing/Office Supplies", "Overall fundings/Manufacturing/Office Supplies", "Overall fundings/Personal Use/Office Supplies", "Overall fundings/Retail/Office Supplies", "Overall fundings/Services/Office Supplies", "Overall fundings/Transportation/Office Supplies", "Overall fundings/Wholesale/Office Supplies", "Overall fundings/Agriculture/Paper Sales", "Overall fundings/Arts/Paper Sales", "Overall fundings/Clothing/Paper Sales", "Overall fundings/Construction/Paper Sales", "Overall fundings/Education/Paper Sales", "Overall fundings/Entertainment/Paper Sales", "Overall fundings/Food/Paper Sales", "Overall fundings/Health/Paper Sales", "Overall fundings/Housing/Paper Sales", "Overall fundings/Manufacturing/Paper Sales", "Overall fundings/Personal Use/Paper Sales", "Overall fundings/Retail/Paper Sales", "Overall fundings/Services/Paper Sales", "Overall fundings/Transportation/Paper Sales", "Overall fundings/Wholesale/Paper Sales", "Overall fundings/Agriculture/Party Supplies", "Overall fundings/Arts/Party Supplies", "Overall fundings/Clothing/Party Supplies", "Overall fundings/Construction/Party Supplies", "Overall fundings/Education/Party Supplies", "Overall fundings/Entertainment/Party Supplies", "Overall fundings/Food/Party Supplies", "Overall fundings/Health/Party Supplies", "Overall fundings/Housing/Party Supplies", "Overall fundings/Manufacturing/Party Supplies", "Overall fundings/Personal Use/Party Supplies", "Overall fundings/Retail/Party Supplies", "Overall fundings/Services/Party Supplies", "Overall fundings/Transportation/Party Supplies", "Overall fundings/Wholesale/Party Supplies", "Overall fundings/Agriculture/Patchwork", "Overall fundings/Arts/Patchwork", "Overall fundings/Clothing/Patchwork", "Overall fundings/Construction/Patchwork", "Overall fundings/Education/Patchwork", "Overall fundings/Entertainment/Patchwork", "Overall fundings/Food/Patchwork", "Overall fundings/Health/Patchwork", "Overall fundings/Housing/Patchwork", "Overall fundings/Manufacturing/Patchwork", "Overall fundings/Personal Use/Patchwork", "Overall fundings/Retail/Patchwork", "Overall fundings/Services/Patchwork", "Overall fundings/Transportation/Patchwork", "Overall fundings/Wholesale/Patchwork", "Overall fundings/Agriculture/Perfumes", "Overall fundings/Arts/Perfumes", "Overall fundings/Clothing/Perfumes", "Overall fundings/Construction/Perfumes", "Overall fundings/Education/Perfumes", "Overall fundings/Entertainment/Perfumes", "Overall fundings/Food/Perfumes", "Overall fundings/Health/Perfumes", "Overall fundings/Housing/Perfumes", "Overall fundings/Manufacturing/Perfumes", "Overall fundings/Personal Use/Perfumes", "Overall fundings/Retail/Perfumes", "Overall fundings/Services/Perfumes", "Overall fundings/Transportation/Perfumes", "Overall fundings/Wholesale/Perfumes", "Overall fundings/Agriculture/Personal Care Products", "Overall fundings/Arts/Personal Care Products", "Overall fundings/Clothing/Personal Care Products", "Overall fundings/Construction/Personal Care Products", "Overall fundings/Education/Personal Care Products", "Overall fundings/Entertainment/Personal Care Products", "Overall fundings/Food/Personal Care Products", "Overall fundings/Health/Personal Care Products", "Overall fundings/Housing/Personal Care Products", "Overall fundings/Manufacturing/Personal Care Products", "Overall fundings/Personal Use/Personal Care Products", "Overall fundings/Retail/Personal Care Products", "Overall fundings/Services/Personal Care Products", "Overall fundings/Transportation/Personal Care Products", "Overall fundings/Wholesale/Personal Care Products", "Overall fundings/Agriculture/Personal Expenses", "Overall fundings/Arts/Personal Expenses", "Overall fundings/Clothing/Personal Expenses", "Overall fundings/Construction/Personal Expenses", "Overall fundings/Education/Personal Expenses", "Overall fundings/Entertainment/Personal Expenses", "Overall fundings/Food/Personal Expenses", "Overall fundings/Health/Personal Expenses", "Overall fundings/Housing/Personal Expenses", "Overall fundings/Manufacturing/Personal Expenses", "Overall fundings/Personal Use/Personal Expenses", "Overall fundings/Retail/Personal Expenses", "Overall fundings/Services/Personal Expenses", "Overall fundings/Transportation/Personal Expenses", "Overall fundings/Wholesale/Personal Expenses", "Overall fundings/Agriculture/Personal Housing Expenses", "Overall fundings/Arts/Personal Housing Expenses", "Overall fundings/Clothing/Personal Housing Expenses", "Overall fundings/Construction/Personal Housing Expenses", "Overall fundings/Education/Personal Housing Expenses", "Overall fundings/Entertainment/Personal Housing Expenses", "Overall fundings/Food/Personal Housing Expenses", "Overall fundings/Health/Personal Housing Expenses", "Overall fundings/Housing/Personal Housing Expenses", "Overall fundings/Manufacturing/Personal Housing Expenses", "Overall fundings/Personal Use/Personal Housing Expenses", "Overall fundings/Retail/Personal Housing Expenses", "Overall fundings/Services/Personal Housing Expenses", "Overall fundings/Transportation/Personal Housing Expenses", "Overall fundings/Wholesale/Personal Housing Expenses", "Overall fundings/Agriculture/Personal Medical Expenses", "Overall fundings/Arts/Personal Medical Expenses", "Overall fundings/Clothing/Personal Medical Expenses", "Overall fundings/Construction/Personal Medical Expenses", "Overall fundings/Education/Personal Medical Expenses", "Overall fundings/Entertainment/Personal Medical Expenses", "Overall fundings/Food/Personal Medical Expenses", "Overall fundings/Health/Personal Medical Expenses", "Overall fundings/Housing/Personal Medical Expenses", "Overall fundings/Manufacturing/Personal Medical Expenses", "Overall fundings/Personal Use/Personal Medical Expenses", "Overall fundings/Retail/Personal Medical Expenses", "Overall fundings/Services/Personal Medical Expenses", "Overall fundings/Transportation/Personal Medical Expenses", "Overall fundings/Wholesale/Personal Medical Expenses", "Overall fundings/Agriculture/Personal Products Sales", "Overall fundings/Arts/Personal Products Sales", "Overall fundings/Clothing/Personal Products Sales", "Overall fundings/Construction/Personal Products Sales", "Overall fundings/Education/Personal Products Sales", "Overall fundings/Entertainment/Personal Products Sales", "Overall fundings/Food/Personal Products Sales", "Overall fundings/Health/Personal Products Sales", "Overall fundings/Housing/Personal Products Sales", "Overall fundings/Manufacturing/Personal Products Sales", "Overall fundings/Personal Use/Personal Products Sales", "Overall fundings/Retail/Personal Products Sales", "Overall fundings/Services/Personal Products Sales", "Overall fundings/Transportation/Personal Products Sales", "Overall fundings/Wholesale/Personal Products Sales", "Overall fundings/Agriculture/Pharmacy", "Overall fundings/Arts/Pharmacy", "Overall fundings/Clothing/Pharmacy", "Overall fundings/Construction/Pharmacy", "Overall fundings/Education/Pharmacy", "Overall fundings/Entertainment/Pharmacy", "Overall fundings/Food/Pharmacy", "Overall fundings/Health/Pharmacy", "Overall fundings/Housing/Pharmacy", "Overall fundings/Manufacturing/Pharmacy", "Overall fundings/Personal Use/Pharmacy", "Overall fundings/Retail/Pharmacy", "Overall fundings/Services/Pharmacy", "Overall fundings/Transportation/Pharmacy", "Overall fundings/Wholesale/Pharmacy", "Overall fundings/Agriculture/Phone Accessories", "Overall fundings/Arts/Phone Accessories", "Overall fundings/Clothing/Phone Accessories", "Overall fundings/Construction/Phone Accessories", "Overall fundings/Education/Phone Accessories", "Overall fundings/Entertainment/Phone Accessories", "Overall fundings/Food/Phone Accessories", "Overall fundings/Health/Phone Accessories", "Overall fundings/Housing/Phone Accessories", "Overall fundings/Manufacturing/Phone Accessories", "Overall fundings/Personal Use/Phone Accessories", "Overall fundings/Retail/Phone Accessories", "Overall fundings/Services/Phone Accessories", "Overall fundings/Transportation/Phone Accessories", "Overall fundings/Wholesale/Phone Accessories", "Overall fundings/Agriculture/Phone Repair", "Overall fundings/Arts/Phone Repair", "Overall fundings/Clothing/Phone Repair", "Overall fundings/Construction/Phone Repair", "Overall fundings/Education/Phone Repair", "Overall fundings/Entertainment/Phone Repair", "Overall fundings/Food/Phone Repair", "Overall fundings/Health/Phone Repair", "Overall fundings/Housing/Phone Repair", "Overall fundings/Manufacturing/Phone Repair", "Overall fundings/Personal Use/Phone Repair", "Overall fundings/Retail/Phone Repair", "Overall fundings/Services/Phone Repair", "Overall fundings/Transportation/Phone Repair", "Overall fundings/Wholesale/Phone Repair", "Overall fundings/Agriculture/Phone Use Sales", "Overall fundings/Arts/Phone Use Sales", "Overall fundings/Clothing/Phone Use Sales", "Overall fundings/Construction/Phone Use Sales", "Overall fundings/Education/Phone Use Sales", "Overall fundings/Entertainment/Phone Use Sales", "Overall fundings/Food/Phone Use Sales", "Overall fundings/Health/Phone Use Sales", "Overall fundings/Housing/Phone Use Sales", "Overall fundings/Manufacturing/Phone Use Sales", "Overall fundings/Personal Use/Phone Use Sales", "Overall fundings/Retail/Phone Use Sales", "Overall fundings/Services/Phone Use Sales", "Overall fundings/Transportation/Phone Use Sales", "Overall fundings/Wholesale/Phone Use Sales", "Overall fundings/Agriculture/Photography", "Overall fundings/Arts/Photography", "Overall fundings/Clothing/Photography", "Overall fundings/Construction/Photography", "Overall fundings/Education/Photography", "Overall fundings/Entertainment/Photography", "Overall fundings/Food/Photography", "Overall fundings/Health/Photography", "Overall fundings/Housing/Photography", "Overall fundings/Manufacturing/Photography", "Overall fundings/Personal Use/Photography", "Overall fundings/Retail/Photography", "Overall fundings/Services/Photography", "Overall fundings/Transportation/Photography", "Overall fundings/Wholesale/Photography", "Overall fundings/Agriculture/Pigs", "Overall fundings/Arts/Pigs", "Overall fundings/Clothing/Pigs", "Overall fundings/Construction/Pigs", "Overall fundings/Education/Pigs", "Overall fundings/Entertainment/Pigs", "Overall fundings/Food/Pigs", "Overall fundings/Health/Pigs", "Overall fundings/Housing/Pigs", "Overall fundings/Manufacturing/Pigs", "Overall fundings/Personal Use/Pigs", "Overall fundings/Retail/Pigs", "Overall fundings/Services/Pigs", "Overall fundings/Transportation/Pigs", "Overall fundings/Wholesale/Pigs", "Overall fundings/Agriculture/Plastics Sales", "Overall fundings/Arts/Plastics Sales", "Overall fundings/Clothing/Plastics Sales", "Overall fundings/Construction/Plastics Sales", "Overall fundings/Education/Plastics Sales", "Overall fundings/Entertainment/Plastics Sales", "Overall fundings/Food/Plastics Sales", "Overall fundings/Health/Plastics Sales", "Overall fundings/Housing/Plastics Sales", "Overall fundings/Manufacturing/Plastics Sales", "Overall fundings/Personal Use/Plastics Sales", "Overall fundings/Retail/Plastics Sales", "Overall fundings/Services/Plastics Sales", "Overall fundings/Transportation/Plastics Sales", "Overall fundings/Wholesale/Plastics Sales", "Overall fundings/Agriculture/Poultry", "Overall fundings/Arts/Poultry", "Overall fundings/Clothing/Poultry", "Overall fundings/Construction/Poultry", "Overall fundings/Education/Poultry", "Overall fundings/Entertainment/Poultry", "Overall fundings/Food/Poultry", "Overall fundings/Health/Poultry", "Overall fundings/Housing/Poultry", "Overall fundings/Manufacturing/Poultry", "Overall fundings/Personal Use/Poultry", "Overall fundings/Retail/Poultry", "Overall fundings/Services/Poultry", "Overall fundings/Transportation/Poultry", "Overall fundings/Wholesale/Poultry", "Overall fundings/Agriculture/Primary/secondary school costs", "Overall fundings/Arts/Primary/secondary school costs", "Overall fundings/Clothing/Primary/secondary school costs", "Overall fundings/Construction/Primary/secondary school costs", "Overall fundings/Education/Primary/secondary school costs", "Overall fundings/Entertainment/Primary/secondary school costs", "Overall fundings/Food/Primary/secondary school costs", "Overall fundings/Health/Primary/secondary school costs", "Overall fundings/Housing/Primary/secondary school costs", "Overall fundings/Manufacturing/Primary/secondary school costs", "Overall fundings/Personal Use/Primary/secondary school costs", "Overall fundings/Retail/Primary/secondary school costs", "Overall fundings/Services/Primary/secondary school costs", "Overall fundings/Transportation/Primary/secondary school costs", "Overall fundings/Wholesale/Primary/secondary school costs", "Overall fundings/Agriculture/Printing", "Overall fundings/Arts/Printing", "Overall fundings/Clothing/Printing", "Overall fundings/Construction/Printing", "Overall fundings/Education/Printing", "Overall fundings/Entertainment/Printing", "Overall fundings/Food/Printing", "Overall fundings/Health/Printing", "Overall fundings/Housing/Printing", "Overall fundings/Manufacturing/Printing", "Overall fundings/Personal Use/Printing", "Overall fundings/Retail/Printing", "Overall fundings/Services/Printing", "Overall fundings/Transportation/Printing", "Overall fundings/Wholesale/Printing", "Overall fundings/Agriculture/Property", "Overall fundings/Arts/Property", "Overall fundings/Clothing/Property", "Overall fundings/Construction/Property", "Overall fundings/Education/Property", "Overall fundings/Entertainment/Property", "Overall fundings/Food/Property", "Overall fundings/Health/Property", "Overall fundings/Housing/Property", "Overall fundings/Manufacturing/Property", "Overall fundings/Personal Use/Property", "Overall fundings/Retail/Property", "Overall fundings/Services/Property", "Overall fundings/Transportation/Property", "Overall fundings/Wholesale/Property", "Overall fundings/Agriculture/Pub", "Overall fundings/Arts/Pub", "Overall fundings/Clothing/Pub", "Overall fundings/Construction/Pub", "Overall fundings/Education/Pub", "Overall fundings/Entertainment/Pub", "Overall fundings/Food/Pub", "Overall fundings/Health/Pub", "Overall fundings/Housing/Pub", "Overall fundings/Manufacturing/Pub", "Overall fundings/Personal Use/Pub", "Overall fundings/Retail/Pub", "Overall fundings/Services/Pub", "Overall fundings/Transportation/Pub", "Overall fundings/Wholesale/Pub", "Overall fundings/Agriculture/Quarrying", "Overall fundings/Arts/Quarrying", "Overall fundings/Clothing/Quarrying", "Overall fundings/Construction/Quarrying", "Overall fundings/Education/Quarrying", "Overall fundings/Entertainment/Quarrying", "Overall fundings/Food/Quarrying", "Overall fundings/Health/Quarrying", "Overall fundings/Housing/Quarrying", "Overall fundings/Manufacturing/Quarrying", "Overall fundings/Personal Use/Quarrying", "Overall fundings/Retail/Quarrying", "Overall fundings/Services/Quarrying", "Overall fundings/Transportation/Quarrying", "Overall fundings/Wholesale/Quarrying", "Overall fundings/Agriculture/Recycled Materials", "Overall fundings/Arts/Recycled Materials", "Overall fundings/Clothing/Recycled Materials", "Overall fundings/Construction/Recycled Materials", "Overall fundings/Education/Recycled Materials", "Overall fundings/Entertainment/Recycled Materials", "Overall fundings/Food/Recycled Materials", "Overall fundings/Health/Recycled Materials", "Overall fundings/Housing/Recycled Materials", "Overall fundings/Manufacturing/Recycled Materials", "Overall fundings/Personal Use/Recycled Materials", "Overall fundings/Retail/Recycled Materials", "Overall fundings/Services/Recycled Materials", "Overall fundings/Transportation/Recycled Materials", "Overall fundings/Wholesale/Recycled Materials", "Overall fundings/Agriculture/Recycling", "Overall fundings/Arts/Recycling", "Overall fundings/Clothing/Recycling", "Overall fundings/Construction/Recycling", "Overall fundings/Education/Recycling", "Overall fundings/Entertainment/Recycling", "Overall fundings/Food/Recycling", "Overall fundings/Health/Recycling", "Overall fundings/Housing/Recycling", "Overall fundings/Manufacturing/Recycling", "Overall fundings/Personal Use/Recycling", "Overall fundings/Retail/Recycling", "Overall fundings/Services/Recycling", "Overall fundings/Transportation/Recycling", "Overall fundings/Wholesale/Recycling", "Overall fundings/Agriculture/Religious Articles", "Overall fundings/Arts/Religious Articles", "Overall fundings/Clothing/Religious Articles", "Overall fundings/Construction/Religious Articles", "Overall fundings/Education/Religious Articles", "Overall fundings/Entertainment/Religious Articles", "Overall fundings/Food/Religious Articles", "Overall fundings/Health/Religious Articles", "Overall fundings/Housing/Religious Articles", "Overall fundings/Manufacturing/Religious Articles", "Overall fundings/Personal Use/Religious Articles", "Overall fundings/Retail/Religious Articles", "Overall fundings/Services/Religious Articles", "Overall fundings/Transportation/Religious Articles", "Overall fundings/Wholesale/Religious Articles", "Overall fundings/Agriculture/Renewable Energy Products", "Overall fundings/Arts/Renewable Energy Products", "Overall fundings/Clothing/Renewable Energy Products", "Overall fundings/Construction/Renewable Energy Products", "Overall fundings/Education/Renewable Energy Products", "Overall fundings/Entertainment/Renewable Energy Products", "Overall fundings/Food/Renewable Energy Products", "Overall fundings/Health/Renewable Energy Products", "Overall fundings/Housing/Renewable Energy Products", "Overall fundings/Manufacturing/Renewable Energy Products", "Overall fundings/Personal Use/Renewable Energy Products", "Overall fundings/Retail/Renewable Energy Products", "Overall fundings/Services/Renewable Energy Products", "Overall fundings/Transportation/Renewable Energy Products", "Overall fundings/Wholesale/Renewable Energy Products", "Overall fundings/Agriculture/Restaurant", "Overall fundings/Arts/Restaurant", "Overall fundings/Clothing/Restaurant", "Overall fundings/Construction/Restaurant", "Overall fundings/Education/Restaurant", "Overall fundings/Entertainment/Restaurant", "Overall fundings/Food/Restaurant", "Overall fundings/Health/Restaurant", "Overall fundings/Housing/Restaurant", "Overall fundings/Manufacturing/Restaurant", "Overall fundings/Personal Use/Restaurant", "Overall fundings/Retail/Restaurant", "Overall fundings/Services/Restaurant", "Overall fundings/Transportation/Restaurant", "Overall fundings/Wholesale/Restaurant", "Overall fundings/Agriculture/Retail", "Overall fundings/Arts/Retail", "Overall fundings/Clothing/Retail", "Overall fundings/Construction/Retail", "Overall fundings/Education/Retail", "Overall fundings/Entertainment/Retail", "Overall fundings/Food/Retail", "Overall fundings/Health/Retail", "Overall fundings/Housing/Retail", "Overall fundings/Manufacturing/Retail", "Overall fundings/Personal Use/Retail", "Overall fundings/Retail/Retail", "Overall fundings/Services/Retail", "Overall fundings/Transportation/Retail", "Overall fundings/Wholesale/Retail", "Overall fundings/Agriculture/Rickshaw", "Overall fundings/Arts/Rickshaw", "Overall fundings/Clothing/Rickshaw", "Overall fundings/Construction/Rickshaw", "Overall fundings/Education/Rickshaw", "Overall fundings/Entertainment/Rickshaw", "Overall fundings/Food/Rickshaw", "Overall fundings/Health/Rickshaw", "Overall fundings/Housing/Rickshaw", "Overall fundings/Manufacturing/Rickshaw", "Overall fundings/Personal Use/Rickshaw", "Overall fundings/Retail/Rickshaw", "Overall fundings/Services/Rickshaw", "Overall fundings/Transportation/Rickshaw", "Overall fundings/Wholesale/Rickshaw", "Overall fundings/Agriculture/Secretarial Services", "Overall fundings/Arts/Secretarial Services", "Overall fundings/Clothing/Secretarial Services", "Overall fundings/Construction/Secretarial Services", "Overall fundings/Education/Secretarial Services", "Overall fundings/Entertainment/Secretarial Services", "Overall fundings/Food/Secretarial Services", "Overall fundings/Health/Secretarial Services", "Overall fundings/Housing/Secretarial Services", "Overall fundings/Manufacturing/Secretarial Services", "Overall fundings/Personal Use/Secretarial Services", "Overall fundings/Retail/Secretarial Services", "Overall fundings/Services/Secretarial Services", "Overall fundings/Transportation/Secretarial Services", "Overall fundings/Wholesale/Secretarial Services", "Overall fundings/Agriculture/Services", "Overall fundings/Arts/Services", "Overall fundings/Clothing/Services", "Overall fundings/Construction/Services", "Overall fundings/Education/Services", "Overall fundings/Entertainment/Services", "Overall fundings/Food/Services", "Overall fundings/Health/Services", "Overall fundings/Housing/Services", "Overall fundings/Manufacturing/Services", "Overall fundings/Personal Use/Services", "Overall fundings/Retail/Services", "Overall fundings/Services/Services", "Overall fundings/Transportation/Services", "Overall fundings/Wholesale/Services", "Overall fundings/Agriculture/Sewing", "Overall fundings/Arts/Sewing", "Overall fundings/Clothing/Sewing", "Overall fundings/Construction/Sewing", "Overall fundings/Education/Sewing", "Overall fundings/Entertainment/Sewing", "Overall fundings/Food/Sewing", "Overall fundings/Health/Sewing", "Overall fundings/Housing/Sewing", "Overall fundings/Manufacturing/Sewing", "Overall fundings/Personal Use/Sewing", "Overall fundings/Retail/Sewing", "Overall fundings/Services/Sewing", "Overall fundings/Transportation/Sewing", "Overall fundings/Wholesale/Sewing", "Overall fundings/Agriculture/Shoe Sales", "Overall fundings/Arts/Shoe Sales", "Overall fundings/Clothing/Shoe Sales", "Overall fundings/Construction/Shoe Sales", "Overall fundings/Education/Shoe Sales", "Overall fundings/Entertainment/Shoe Sales", "Overall fundings/Food/Shoe Sales", "Overall fundings/Health/Shoe Sales", "Overall fundings/Housing/Shoe Sales", "Overall fundings/Manufacturing/Shoe Sales", "Overall fundings/Personal Use/Shoe Sales", "Overall fundings/Retail/Shoe Sales", "Overall fundings/Services/Shoe Sales", "Overall fundings/Transportation/Shoe Sales", "Overall fundings/Wholesale/Shoe Sales", "Overall fundings/Agriculture/Souvenir Sales", "Overall fundings/Arts/Souvenir Sales", "Overall fundings/Clothing/Souvenir Sales", "Overall fundings/Construction/Souvenir Sales", "Overall fundings/Education/Souvenir Sales", "Overall fundings/Entertainment/Souvenir Sales", "Overall fundings/Food/Souvenir Sales", "Overall fundings/Health/Souvenir Sales", "Overall fundings/Housing/Souvenir Sales", "Overall fundings/Manufacturing/Souvenir Sales", "Overall fundings/Personal Use/Souvenir Sales", "Overall fundings/Retail/Souvenir Sales", "Overall fundings/Services/Souvenir Sales", "Overall fundings/Transportation/Souvenir Sales", "Overall fundings/Wholesale/Souvenir Sales", "Overall fundings/Agriculture/Spare Parts", "Overall fundings/Arts/Spare Parts", "Overall fundings/Clothing/Spare Parts", "Overall fundings/Construction/Spare Parts", "Overall fundings/Education/Spare Parts", "Overall fundings/Entertainment/Spare Parts", "Overall fundings/Food/Spare Parts", "Overall fundings/Health/Spare Parts", "Overall fundings/Housing/Spare Parts", "Overall fundings/Manufacturing/Spare Parts", "Overall fundings/Personal Use/Spare Parts", "Overall fundings/Retail/Spare Parts", "Overall fundings/Services/Spare Parts", "Overall fundings/Transportation/Spare Parts", "Overall fundings/Wholesale/Spare Parts", "Overall fundings/Agriculture/Sporting Good Sales", "Overall fundings/Arts/Sporting Good Sales", "Overall fundings/Clothing/Sporting Good Sales", "Overall fundings/Construction/Sporting Good Sales", "Overall fundings/Education/Sporting Good Sales", "Overall fundings/Entertainment/Sporting Good Sales", "Overall fundings/Food/Sporting Good Sales", "Overall fundings/Health/Sporting Good Sales", "Overall fundings/Housing/Sporting Good Sales", "Overall fundings/Manufacturing/Sporting Good Sales", "Overall fundings/Personal Use/Sporting Good Sales", "Overall fundings/Retail/Sporting Good Sales", "Overall fundings/Services/Sporting Good Sales", "Overall fundings/Transportation/Sporting Good Sales", "Overall fundings/Wholesale/Sporting Good Sales", "Overall fundings/Agriculture/Tailoring", "Overall fundings/Arts/Tailoring", "Overall fundings/Clothing/Tailoring", "Overall fundings/Construction/Tailoring", "Overall fundings/Education/Tailoring", "Overall fundings/Entertainment/Tailoring", "Overall fundings/Food/Tailoring", "Overall fundings/Health/Tailoring", "Overall fundings/Housing/Tailoring", "Overall fundings/Manufacturing/Tailoring", "Overall fundings/Personal Use/Tailoring", "Overall fundings/Retail/Tailoring", "Overall fundings/Services/Tailoring", "Overall fundings/Transportation/Tailoring", "Overall fundings/Wholesale/Tailoring", "Overall fundings/Agriculture/Taxi", "Overall fundings/Arts/Taxi", "Overall fundings/Clothing/Taxi", "Overall fundings/Construction/Taxi", "Overall fundings/Education/Taxi", "Overall fundings/Entertainment/Taxi", "Overall fundings/Food/Taxi", "Overall fundings/Health/Taxi", "Overall fundings/Housing/Taxi", "Overall fundings/Manufacturing/Taxi", "Overall fundings/Personal Use/Taxi", "Overall fundings/Retail/Taxi", "Overall fundings/Services/Taxi", "Overall fundings/Transportation/Taxi", "Overall fundings/Wholesale/Taxi", "Overall fundings/Agriculture/Technology", "Overall fundings/Arts/Technology", "Overall fundings/Clothing/Technology", "Overall fundings/Construction/Technology", "Overall fundings/Education/Technology", "Overall fundings/Entertainment/Technology", "Overall fundings/Food/Technology", "Overall fundings/Health/Technology", "Overall fundings/Housing/Technology", "Overall fundings/Manufacturing/Technology", "Overall fundings/Personal Use/Technology", "Overall fundings/Retail/Technology", "Overall fundings/Services/Technology", "Overall fundings/Transportation/Technology", "Overall fundings/Wholesale/Technology", "Overall fundings/Agriculture/Textiles", "Overall fundings/Arts/Textiles", "Overall fundings/Clothing/Textiles", "Overall fundings/Construction/Textiles", "Overall fundings/Education/Textiles", "Overall fundings/Entertainment/Textiles", "Overall fundings/Food/Textiles", "Overall fundings/Health/Textiles", "Overall fundings/Housing/Textiles", "Overall fundings/Manufacturing/Textiles", "Overall fundings/Personal Use/Textiles", "Overall fundings/Retail/Textiles", "Overall fundings/Services/Textiles", "Overall fundings/Transportation/Textiles", "Overall fundings/Wholesale/Textiles", "Overall fundings/Agriculture/Timber Sales", "Overall fundings/Arts/Timber Sales", "Overall fundings/Clothing/Timber Sales", "Overall fundings/Construction/Timber Sales", "Overall fundings/Education/Timber Sales", "Overall fundings/Entertainment/Timber Sales", "Overall fundings/Food/Timber Sales", "Overall fundings/Health/Timber Sales", "Overall fundings/Housing/Timber Sales", "Overall fundings/Manufacturing/Timber Sales", "Overall fundings/Personal Use/Timber Sales", "Overall fundings/Retail/Timber Sales", "Overall fundings/Services/Timber Sales", "Overall fundings/Transportation/Timber Sales", "Overall fundings/Wholesale/Timber Sales", "Overall fundings/Agriculture/Tourism", "Overall fundings/Arts/Tourism", "Overall fundings/Clothing/Tourism", "Overall fundings/Construction/Tourism", "Overall fundings/Education/Tourism", "Overall fundings/Entertainment/Tourism", "Overall fundings/Food/Tourism", "Overall fundings/Health/Tourism", "Overall fundings/Housing/Tourism", "Overall fundings/Manufacturing/Tourism", "Overall fundings/Personal Use/Tourism", "Overall fundings/Retail/Tourism", "Overall fundings/Services/Tourism", "Overall fundings/Transportation/Tourism", "Overall fundings/Wholesale/Tourism", "Overall fundings/Agriculture/Transportation", "Overall fundings/Arts/Transportation", "Overall fundings/Clothing/Transportation", "Overall fundings/Construction/Transportation", "Overall fundings/Education/Transportation", "Overall fundings/Entertainment/Transportation", "Overall fundings/Food/Transportation", "Overall fundings/Health/Transportation", "Overall fundings/Housing/Transportation", "Overall fundings/Manufacturing/Transportation", "Overall fundings/Personal Use/Transportation", "Overall fundings/Retail/Transportation", "Overall fundings/Services/Transportation", "Overall fundings/Transportation/Transportation", "Overall fundings/Wholesale/Transportation", "Overall fundings/Agriculture/Traveling Sales", "Overall fundings/Arts/Traveling Sales", "Overall fundings/Clothing/Traveling Sales", "Overall fundings/Construction/Traveling Sales", "Overall fundings/Education/Traveling Sales", "Overall fundings/Entertainment/Traveling Sales", "Overall fundings/Food/Traveling Sales", "Overall fundings/Health/Traveling Sales", "Overall fundings/Housing/Traveling Sales", "Overall fundings/Manufacturing/Traveling Sales", "Overall fundings/Personal Use/Traveling Sales", "Overall fundings/Retail/Traveling Sales", "Overall fundings/Services/Traveling Sales", "Overall fundings/Transportation/Traveling Sales", "Overall fundings/Wholesale/Traveling Sales", "Overall fundings/Agriculture/Upholstery", "Overall fundings/Arts/Upholstery", "Overall fundings/Clothing/Upholstery", "Overall fundings/Construction/Upholstery", "Overall fundings/Education/Upholstery", "Overall fundings/Entertainment/Upholstery", "Overall fundings/Food/Upholstery", "Overall fundings/Health/Upholstery", "Overall fundings/Housing/Upholstery", "Overall fundings/Manufacturing/Upholstery", "Overall fundings/Personal Use/Upholstery", "Overall fundings/Retail/Upholstery", "Overall fundings/Services/Upholstery", "Overall fundings/Transportation/Upholstery", "Overall fundings/Wholesale/Upholstery", "Overall fundings/Agriculture/Used Clothing", "Overall fundings/Arts/Used Clothing", "Overall fundings/Clothing/Used Clothing", "Overall fundings/Construction/Used Clothing", "Overall fundings/Education/Used Clothing", "Overall fundings/Entertainment/Used Clothing", "Overall fundings/Food/Used Clothing", "Overall fundings/Health/Used Clothing", "Overall fundings/Housing/Used Clothing", "Overall fundings/Manufacturing/Used Clothing", "Overall fundings/Personal Use/Used Clothing", "Overall fundings/Retail/Used Clothing", "Overall fundings/Services/Used Clothing", "Overall fundings/Transportation/Used Clothing", "Overall fundings/Wholesale/Used Clothing", "Overall fundings/Agriculture/Used Shoes", "Overall fundings/Arts/Used Shoes", "Overall fundings/Clothing/Used Shoes", "Overall fundings/Construction/Used Shoes", "Overall fundings/Education/Used Shoes", "Overall fundings/Entertainment/Used Shoes", "Overall fundings/Food/Used Shoes", "Overall fundings/Health/Used Shoes", "Overall fundings/Housing/Used Shoes", "Overall fundings/Manufacturing/Used Shoes", "Overall fundings/Personal Use/Used Shoes", "Overall fundings/Retail/Used Shoes", "Overall fundings/Services/Used Shoes", "Overall fundings/Transportation/Used Shoes", "Overall fundings/Wholesale/Used Shoes", "Overall fundings/Agriculture/Utilities", "Overall fundings/Arts/Utilities", "Overall fundings/Clothing/Utilities", "Overall fundings/Construction/Utilities", "Overall fundings/Education/Utilities", "Overall fundings/Entertainment/Utilities", "Overall fundings/Food/Utilities", "Overall fundings/Health/Utilities", "Overall fundings/Housing/Utilities", "Overall fundings/Manufacturing/Utilities", "Overall fundings/Personal Use/Utilities", "Overall fundings/Retail/Utilities", "Overall fundings/Services/Utilities", "Overall fundings/Transportation/Utilities", "Overall fundings/Wholesale/Utilities", "Overall fundings/Agriculture/Vehicle", "Overall fundings/Arts/Vehicle", "Overall fundings/Clothing/Vehicle", "Overall fundings/Construction/Vehicle", "Overall fundings/Education/Vehicle", "Overall fundings/Entertainment/Vehicle", "Overall fundings/Food/Vehicle", "Overall fundings/Health/Vehicle", "Overall fundings/Housing/Vehicle", "Overall fundings/Manufacturing/Vehicle", "Overall fundings/Personal Use/Vehicle", "Overall fundings/Retail/Vehicle", "Overall fundings/Services/Vehicle", "Overall fundings/Transportation/Vehicle", "Overall fundings/Wholesale/Vehicle", "Overall fundings/Agriculture/Vehicle Repairs", "Overall fundings/Arts/Vehicle Repairs", "Overall fundings/Clothing/Vehicle Repairs", "Overall fundings/Construction/Vehicle Repairs", "Overall fundings/Education/Vehicle Repairs", "Overall fundings/Entertainment/Vehicle Repairs", "Overall fundings/Food/Vehicle Repairs", "Overall fundings/Health/Vehicle Repairs", "Overall fundings/Housing/Vehicle Repairs", "Overall fundings/Manufacturing/Vehicle Repairs", "Overall fundings/Personal Use/Vehicle Repairs", "Overall fundings/Retail/Vehicle Repairs", "Overall fundings/Services/Vehicle Repairs", "Overall fundings/Transportation/Vehicle Repairs", "Overall fundings/Wholesale/Vehicle Repairs", "Overall fundings/Agriculture/Veterinary Sales", "Overall fundings/Arts/Veterinary Sales", "Overall fundings/Clothing/Veterinary Sales", "Overall fundings/Construction/Veterinary Sales", "Overall fundings/Education/Veterinary Sales", "Overall fundings/Entertainment/Veterinary Sales", "Overall fundings/Food/Veterinary Sales", "Overall fundings/Health/Veterinary Sales", "Overall fundings/Housing/Veterinary Sales", "Overall fundings/Manufacturing/Veterinary Sales", "Overall fundings/Personal Use/Veterinary Sales", "Overall fundings/Retail/Veterinary Sales", "Overall fundings/Services/Veterinary Sales", "Overall fundings/Transportation/Veterinary Sales", "Overall fundings/Wholesale/Veterinary Sales", "Overall fundings/Agriculture/Waste Management", "Overall fundings/Arts/Waste Management", "Overall fundings/Clothing/Waste Management", "Overall fundings/Construction/Waste Management", "Overall fundings/Education/Waste Management", "Overall fundings/Entertainment/Waste Management", "Overall fundings/Food/Waste Management", "Overall fundings/Health/Waste Management", "Overall fundings/Housing/Waste Management", "Overall fundings/Manufacturing/Waste Management", "Overall fundings/Personal Use/Waste Management", "Overall fundings/Retail/Waste Management", "Overall fundings/Services/Waste Management", "Overall fundings/Transportation/Waste Management", "Overall fundings/Wholesale/Waste Management", "Overall fundings/Agriculture/Water Distribution", "Overall fundings/Arts/Water Distribution", "Overall fundings/Clothing/Water Distribution", "Overall fundings/Construction/Water Distribution", "Overall fundings/Education/Water Distribution", "Overall fundings/Entertainment/Water Distribution", "Overall fundings/Food/Water Distribution", "Overall fundings/Health/Water Distribution", "Overall fundings/Housing/Water Distribution", "Overall fundings/Manufacturing/Water Distribution", "Overall fundings/Personal Use/Water Distribution", "Overall fundings/Retail/Water Distribution", "Overall fundings/Services/Water Distribution", "Overall fundings/Transportation/Water Distribution", "Overall fundings/Wholesale/Water Distribution", "Overall fundings/Agriculture/Weaving", "Overall fundings/Arts/Weaving", "Overall fundings/Clothing/Weaving", "Overall fundings/Construction/Weaving", "Overall fundings/Education/Weaving", "Overall fundings/Entertainment/Weaving", "Overall fundings/Food/Weaving", "Overall fundings/Health/Weaving", "Overall fundings/Housing/Weaving", "Overall fundings/Manufacturing/Weaving", "Overall fundings/Personal Use/Weaving", "Overall fundings/Retail/Weaving", "Overall fundings/Services/Weaving", "Overall fundings/Transportation/Weaving", "Overall fundings/Wholesale/Weaving", "Overall fundings/Agriculture/Wedding Expenses", "Overall fundings/Arts/Wedding Expenses", "Overall fundings/Clothing/Wedding Expenses", "Overall fundings/Construction/Wedding Expenses", "Overall fundings/Education/Wedding Expenses", "Overall fundings/Entertainment/Wedding Expenses", "Overall fundings/Food/Wedding Expenses", "Overall fundings/Health/Wedding Expenses", "Overall fundings/Housing/Wedding Expenses", "Overall fundings/Manufacturing/Wedding Expenses", "Overall fundings/Personal Use/Wedding Expenses", "Overall fundings/Retail/Wedding Expenses", "Overall fundings/Services/Wedding Expenses", "Overall fundings/Transportation/Wedding Expenses", "Overall fundings/Wholesale/Wedding Expenses", "Overall fundings/Agriculture/Well digging", "Overall fundings/Arts/Well digging", "Overall fundings/Clothing/Well digging", "Overall fundings/Construction/Well digging", "Overall fundings/Education/Well digging", "Overall fundings/Entertainment/Well digging", "Overall fundings/Food/Well digging", "Overall fundings/Health/Well digging", "Overall fundings/Housing/Well digging", "Overall fundings/Manufacturing/Well digging", "Overall fundings/Personal Use/Well digging", "Overall fundings/Retail/Well digging", "Overall fundings/Services/Well digging", "Overall fundings/Transportation/Well digging", "Overall fundings/Wholesale/Well digging", "Overall fundings/Agriculture/Wholesale", "Overall fundings/Arts/Wholesale", "Overall fundings/Clothing/Wholesale", "Overall fundings/Construction/Wholesale", "Overall fundings/Education/Wholesale", "Overall fundings/Entertainment/Wholesale", "Overall fundings/Food/Wholesale", "Overall fundings/Health/Wholesale", "Overall fundings/Housing/Wholesale", "Overall fundings/Manufacturing/Wholesale", "Overall fundings/Personal Use/Wholesale", "Overall fundings/Retail/Wholesale", "Overall fundings/Services/Wholesale", "Overall fundings/Transportation/Wholesale", "Overall fundings/Wholesale/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings"], "labels": ["Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Adult Care", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Agriculture", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Air Conditioning", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Animal Sales", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Aquaculture", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Arts", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Auto Repair", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Bakery", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Balut-Making", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Barber Shop", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beauty Salon", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beekeeping", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Beverages", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Repair", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Bicycle Sales", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Blacksmith", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookbinding", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bookstore", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Bricks", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Butcher Shop", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Cafe", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Call Center", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Carpentry", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Catering", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Cattle", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Celebrations", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cement", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Cereals", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Charcoal Sales", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Cheese Making", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Child Care", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cleaning Services", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Cloth & Dressmaking Supplies", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Clothing Sales", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Cobbler", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Communications", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computer", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Computers", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Construction Supplies", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Consumer Goods", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Cosmetics Sales", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Crafts", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Dairy", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Decorations Sales", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Dental", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Education provider", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrical Goods", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electrician", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Repair", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Electronics Sales", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Embroidery", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Energy", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Entertainment", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Event Planning", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farm Supplies", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Farming", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Film", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fish Selling", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Fishing", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Florist", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Flowers", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Market", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Production/Sales", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Food Stall", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fruits & Vegetables", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Fuel/Firewood", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Funerals", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Furniture Making", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "Games", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "General Store", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Goods Distribution", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Grocery Store", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Hardware", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Health", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Higher education costs", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Appliances", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Energy", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Home Products Sales", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Hotel", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Internet Cafe", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Jewelry", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Knitting", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Land Rental", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Landscaping / Gardening", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Laundry", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Liquor Store / Off-License", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Livestock", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machine Shop", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Machinery Rental", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Manufacturing", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Medical Clinic", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Metal Shop", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Milk Sales", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Phones", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Mobile Transactions", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Repair", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Motorcycle Transport", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Movie Tapes & DVDs", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Music Discs & Tapes", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Instruments", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Musical Performance", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Natural Medicines", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Office Supplies", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Paper Sales", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Party Supplies", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Patchwork", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Perfumes", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Care Products", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Housing Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Medical Expenses", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Personal Products Sales", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Pharmacy", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Accessories", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Repair", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Phone Use Sales", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Photography", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Pigs", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Plastics Sales", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Poultry", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Primary/secondary school costs", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Printing", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Property", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Pub", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Quarrying", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycled Materials", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Recycling", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Religious Articles", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Renewable Energy Products", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Restaurant", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Retail", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Rickshaw", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Secretarial Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Services", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Sewing", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Shoe Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Souvenir Sales", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Spare Parts", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Sporting Good Sales", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Tailoring", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Taxi", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Technology", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Textiles", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Timber Sales", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Tourism", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Transportation", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Traveling Sales", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Upholstery", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Clothing", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Used Shoes", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Utilities", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Vehicle Repairs", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Veterinary Sales", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Waste Management", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Water Distribution", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Weaving", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Wedding Expenses", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Well digging", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Wholesale", "Agriculture", "Arts", "Clothing", "Construction", "Education", "Entertainment", "Food", "Health", "Housing", "Manufacturing", "Personal Use", "Retail", "Services", "Transportation", "Wholesale", "Overall fundings"], "marker": {"coloraxis": "coloraxis", "colors": [null, null, null, null, null, null, null, null, null, null, null, null, 1687.5, null, null, 954.4448674716269, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1141.6666666666667, null, null, 1083.001498608435, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 752.5179856115108, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1011.8112014453478, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 979.8340548340549, null, null, null, null, null, null, null, null, 1174.1878898128898, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 397.95081967213116, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 827.3148148148148, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 995.628151883714, null, null, 1558.173076923077, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1036.7723285486443, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 773.6196319018405, null, null, null, null, null, null, null, null, null, null, null, null, null, 1070.1754385964912, null, null, null, null, null, null, null, null, null, null, null, null, 864.2857142857143, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1131.9444444444443, null, null, null, null, null, null, null, null, null, null, null, null, null, 1533.1275720164608, null, null, null, null, null, null, 916.4923954372623, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1019.106186069167, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1008.094262295082, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 817.8571428571429, null, null, null, null, null, 989.2765685019206, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1076.6618497109826, null, null, null, null, null, null, null, null, 976.2854717438759, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 280.0, null, null, null, null, null, null, null, 1460.248447204969, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 622.420959818617, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 555.8265582655827, null, null, null, null, null, null, null, null, null, 1220.7925636007828, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 780.7299096528768, null, null, null, null, null, 1356.6231234158706, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1055.7037828875605, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 762.0901639344262, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 954.5454545454545, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1056.5051020408164, null, null, null, null, null, 1018.8496172895092, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1134.242209631728, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 797.8295819935691, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1172.0496191064442, null, null, null, null, 1212.1630280018985, null, null, null, null, null, null, null, null, null, null, null, null, null, 661.3155333900985, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1253.683574879227, null, null, null, null, null, null, null, null, null, null, 1184.7643097643097, null, null, null, null, null, null, null, null, null, null, null, 1263.9026017344897, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1250.6278538812785, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1026.233552631579, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 861.9854721549636, null, null, null, null, null, null, null, null, null, null, null, null, null, 993.9260563380282, null, null, null, null, 670.7600502512563, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 700.5263157894736, null, null, null, null, null, null, null, 1470.5711206896551, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1435.0, null, null, 734.4645293315143, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 666.8857414577814, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 584.3840552789954, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 475.15784361340457, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1801.4925373134329, null, null, null, 832.0497630331754, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1004.7988285000961, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1058.6730477895608, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 890.4406975522639, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1090.7404556650247, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 826.7433977835416, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 617.4652241112829, null, null, null, null, null, null, null, null, null, null, null, null, null, 835.3658536585366, null, null, null, null, null, null, null, null, null, null, null, null, null, 969.8138901497957, null, null, null, null, null, null, null, null, null, null, 945.2674897119341, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 561.2704096466664, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1670.4301075268818, null, null, null, null, null, null, 956.3001356326293, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1102.019427402863, null, null, null, null, null, null, null, null, null, null, 1512.5744047619048, null, null, null, null, null, null, null, null, null, null, null, 970.24331919929, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 266.52298142765653, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 564.0982691233947, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1012.5031557687453, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 943.6379928315412, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 809.3166175024583, null, null, null, null, null, null, null, null, null, null, null, null, null, 1034.914101646385, null, null, null, null, 1595.1515151515152, null, null, null, null, null, null, null, null, null, null, null, null, null, 770.6093189964158, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1034.4166666666667, null, null, null, null, null, null, null, null, 995.617088607595, null, null, null, null, null, null, null, null, 1066.408864430728, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 881.7251461988304, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 887.406015037594, null, null, null, null, null, null, null, null, null, null, null, 836.3071660064818, null, null, null, null, null, null, null, null, null, null, null, null, 1209.3373493975903, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 973.1191222570533, null, null, null, null, null, null, null, null, null, null, null, 763.4986595174263, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1070.7174231332358, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 727.6620370370371, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 864.9150743099788, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 459.88036653656883, null, null, null, null, null, null, null, null, null, null, null, null, 1039.3092105263158, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1220.2777777777778, null, null, null, null, 1326.0416666666667, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1144.867549668874, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1380.6346381969158, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1067.1333333333334, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1029.4463087248323, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1186.304347826087, null, null, null, null, 687.5, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1711.4644970414201, null, null, null, null, null, null, null, null, null, null, null, null, 559.375, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 418.9950980392157, null, null, null, null, null, null, null, null, null, null, null, null, 641.1592041162626, null, null, null, null, null, null, null, null, null, null, null, null, null, 806.1438080224208, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 554.9627632254751, null, null, null, null, null, null, null, null, null, null, 1281.3279002876318, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 885.5130784708249, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1084.959349593496, null, null, null, null, null, null, null, null, null, null, null, null, null, 627.978515625, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1099.2592592592594, null, null, 489.95862624487404, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 867.8387650085763, null, null, null, 876.912100456621, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 805.659844299347, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1103.4934497816594, null, null, null, null, null, null, null, null, null, null, 948.3434881949734, null, null, null, null, null, null, null, null, null, null, null, null, 978.9226519337017, null, null, null, null, null, null, null, null, null, null, null, 831.9117647058823, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 846.2130177514792, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 816.6666666666666, null, null, null, null, null, null, null, null, null, null, null, null, null, 751.7123287671233, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1341.8925554900804, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 985.9258669427081, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 494.15392633773456, null, null, null, null, null, null, null, null, null, null, null, null, null, 997.0430107526881, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1129.6971502333433, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1069.0041916846042, null, null, null, null, null, null, null, null, null, null, null, null, null, 1028.8272471910113, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 964.8351648351648, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 962.3833592534992, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 812.3456790123457, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 646.2714619526568, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 997.3281756027347, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1502.629151291513, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 725.4464285714286, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1484.5052083333333, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 766.5424912689174, null, null, null, null, null, null, null, null, null, null, null, null, 706.9536423841059, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 983.6, null, null, null, null, 954.8915310805173, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1871.9568062827225, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1230.7692307692307, null, null, null, null, null, null, null, null, null, null, null, null, 954.7619047619048, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 994.0944881889764, null, null, 806.875, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 836.084142394822, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1110.796460176991, null, null, null, 782.3309492847854, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 800.0, null, null, null, null, null, null, null, 1309.3023255813953, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1289.779005524862, 762.572539310225, 1024.621187800963, 1105.8061060524906, 1029.7884733292458, 970.5171047015062, 1264.4755244755245, 878.0144142607361, 1033.5336538461538, 651.8150114924308, 903.8162705667276, 392.65027770937644, 739.8809637480506, 952.173120977419, 643.4916377406122, 1455.4602184087364, 806.4195854388852]}, "name": "", "parents": ["Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings/Agriculture", "Overall fundings/Arts", "Overall fundings/Clothing", "Overall fundings/Construction", "Overall fundings/Education", "Overall fundings/Entertainment", "Overall fundings/Food", "Overall fundings/Health", "Overall fundings/Housing", "Overall fundings/Manufacturing", "Overall fundings/Personal Use", "Overall fundings/Retail", "Overall fundings/Services", "Overall fundings/Transportation", "Overall fundings/Wholesale", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", "Overall fundings", ""], "type": "treemap", "values": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2.0, 0.0, 0.0, 27579.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 36.0, 0.0, 0.0, 9342.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 139.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1107.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1386.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 3848.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 61.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 972.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 6742.0, 0.0, 0.0, 52.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2508.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 163.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 57.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 448.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 36.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 486.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 526.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2053.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1464.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 77.0, 0.0, 0.0, 0.0, 0.0, 0.0, 781.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 692.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 8246.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 161.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7939.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4428.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 511.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2103.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5129.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 22919.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 488.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 55.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 392.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2221.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2118.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 622.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4857.0, 0.0, 0.0, 0.0, 0.0, 4214.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 8221.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 414.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 297.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4497.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 438.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 304.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 413.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 568.0, 0.0, 0.0, 0.0, 0.0, 2388.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 190.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 464.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5.0, 0.0, 0.0, 4398.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 73605.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 13459.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 10295.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 67.0, 0.0, 0.0, 0.0, 844.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 10414.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7261.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 28557.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 9744.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 16964.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1941.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 41.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2203.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 243.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 65349.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 279.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 15483.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 978.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1344.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 20282.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 20299.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7164.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 3961.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 279.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1017.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1397.0, 0.0, 0.0, 0.0, 0.0, 495.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 279.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 300.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1580.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 13447.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 171.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 133.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2777.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 332.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 957.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1865.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 683.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 216.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 471.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5893.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 152.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 90.0, 0.0, 0.0, 0.0, 0.0, 72.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 151.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 843.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 375.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 298.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 115.0, 0.0, 0.0, 0.0, 0.0, 24.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 338.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 8.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 6732.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 36538.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5709.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 3894.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1043.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 497.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 123.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1024.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 405.0, 0.0, 0.0, 27312.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 583.0, 0.0, 0.0, 0.0, 10512.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 7964.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 458.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1313.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 905.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 170.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 845.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 372.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 73.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 5091.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 25117.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2878.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 93.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 10071.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 8827.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2848.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 182.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1286.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 81.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 9843.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 2779.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1084.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 504.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 192.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 4295.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 151.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 125.0, 0.0, 0.0, 0.0, 0.0, 4794.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 764.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 195.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1281.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 635.0, 0.0, 0.0, 200.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 309.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 565.0, 0.0, 0.0, 0.0, 3076.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 405.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 43.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 362.0, 184176.0, 12460.0, 33606.0, 6524.0, 32798.0, 858.0, 140694.0, 9568.0, 37851.0, 6564.0, 36549.0, 125676.0, 45835.0, 15845.0, 641.0, 689645.0]}],                        {"coloraxis": {"colorbar": {"title": {"text": "funded_mean"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('f5daeeb2-ceb0-469d-afc3-9b012f4a7522');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
df_activity = df_activity.sort_values(by=["funded_mean"])
df_activity.head(10)
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
      <th>sector</th>
      <th>activity</th>
      <th>amount_projects</th>
      <th>funded_amount</th>
      <th>funded_mean</th>
      <th>funded_median</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1708</th>
      <td>Personal Use</td>
      <td>Home Appliances</td>
      <td>20299.0</td>
      <td>5410150.0</td>
      <td>266.522981</td>
      <td>175.0</td>
    </tr>
    <tr>
      <th>1655</th>
      <td>Personal Use</td>
      <td>Celebrations</td>
      <td>5.0</td>
      <td>1400.0</td>
      <td>280.000000</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>986</th>
      <td>Food</td>
      <td>Balut-Making</td>
      <td>61.0</td>
      <td>24275.0</td>
      <td>397.950820</td>
      <td>300.0</td>
    </tr>
    <tr>
      <th>1741</th>
      <td>Personal Use</td>
      <td>Personal Expenses</td>
      <td>6732.0</td>
      <td>2820675.0</td>
      <td>418.995098</td>
      <td>225.0</td>
    </tr>
    <tr>
      <th>2218</th>
      <td>Transportation</td>
      <td>Motorcycle Transport</td>
      <td>5893.0</td>
      <td>2710075.0</td>
      <td>459.880367</td>
      <td>350.0</td>
    </tr>
    <tr>
      <th>1038</th>
      <td>Food</td>
      <td>Fishing</td>
      <td>10295.0</td>
      <td>4891750.0</td>
      <td>475.157844</td>
      <td>350.0</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Agriculture</td>
      <td>Pigs</td>
      <td>27312.0</td>
      <td>13381750.0</td>
      <td>489.958626</td>
      <td>300.0</td>
    </tr>
    <tr>
      <th>2253</th>
      <td>Transportation</td>
      <td>Rickshaw</td>
      <td>2878.0</td>
      <td>1422175.0</td>
      <td>494.153926</td>
      <td>400.0</td>
    </tr>
    <tr>
      <th>1907</th>
      <td>Retail</td>
      <td>Personal Products Sales</td>
      <td>3894.0</td>
      <td>2161025.0</td>
      <td>554.962763</td>
      <td>350.0</td>
    </tr>
    <tr>
      <th>1821</th>
      <td>Retail</td>
      <td>Charcoal Sales</td>
      <td>4428.0</td>
      <td>2461200.0</td>
      <td>555.826558</td>
      <td>325.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_activity = df_activity.sort_values(by=["funded_mean"])
df_activity.tail(10)
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
      <th>sector</th>
      <th>activity</th>
      <th>amount_projects</th>
      <th>funded_amount</th>
      <th>funded_mean</th>
      <th>funded_median</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>308</th>
      <td>Arts</td>
      <td>Textiles</td>
      <td>1084.0</td>
      <td>1628850.0</td>
      <td>1502.629151</td>
      <td>775.0</td>
    </tr>
    <tr>
      <th>1217</th>
      <td>Health</td>
      <td>Health</td>
      <td>1344.0</td>
      <td>2032900.0</td>
      <td>1512.574405</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1810</th>
      <td>Retail</td>
      <td>Bookstore</td>
      <td>486.0</td>
      <td>745100.0</td>
      <td>1533.127572</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Agriculture</td>
      <td>Beekeeping</td>
      <td>52.0</td>
      <td>81025.0</td>
      <td>1558.173077</td>
      <td>1450.0</td>
    </tr>
    <tr>
      <th>247</th>
      <td>Arts</td>
      <td>Knitting</td>
      <td>495.0</td>
      <td>789600.0</td>
      <td>1595.151515</td>
      <td>875.0</td>
    </tr>
    <tr>
      <th>2355</th>
      <td>Wholesale</td>
      <td>Goods Distribution</td>
      <td>279.0</td>
      <td>466050.0</td>
      <td>1670.430108</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Services</td>
      <td>Adult Care</td>
      <td>2.0</td>
      <td>3375.0</td>
      <td>1687.500000</td>
      <td>1687.5</td>
    </tr>
    <tr>
      <th>1902</th>
      <td>Retail</td>
      <td>Perfumes</td>
      <td>338.0</td>
      <td>578475.0</td>
      <td>1711.464497</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1854</th>
      <td>Retail</td>
      <td>Florist</td>
      <td>67.0</td>
      <td>120700.0</td>
      <td>1801.492537</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>478</th>
      <td>Clothing</td>
      <td>Used Shoes</td>
      <td>764.0</td>
      <td>1430175.0</td>
      <td>1871.956806</td>
      <td>650.0</td>
    </tr>
  </tbody>
</table>
</div>





# Kontinente und Länder

## Anzahl Projekte nach Kontinenten


```python
# Daten vorbereiten
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.sum})
df_cont_count_rename = df_cont_count.rename(columns={"continent": "amount_projects"})
df_cont_count = df_cont_count_rename.reset_index()
df_cont_count_sorted = df_cont_count.sort_values(by=["amount_projects"])
df_cont_count_sorted_fund = df_cont_count.sort_values(by=["funded_amount"])

# Plotten
fig = px.bar(df_cont_count_sorted_fund, x="continent", y="amount_projects", hover_name="country")
fig.show()
```


<div>                            <div id="fb21d975-77da-4db3-b339-c35356b5d47f" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("fb21d975-77da-4db3-b339-c35356b5d47f")) {                    Plotly.newPlot(                        "fb21d975-77da-4db3-b339-c35356b5d47f",                        [{"alignmentgroup": "True", "hovertemplate": "<b>%{hovertext}</b><br><br>continent=%{x}<br>amount_projects=%{y}<extra></extra>", "hovertext": ["Afghanistan", "Belize", "Azerbaijan", "Armenia", "Albania", "Afghanistan", "Zimbabwe", "Zambia", "Yemen", "Virgin Islands", "Vietnam", "Vanuatu", "Ukraine", "Uganda", "Turkey", "Togo", "Timor-Leste", "The Democratic Republic of the Congo", "Thailand", "Tanzania", "Tajikistan", "Suriname", "South Sudan", "South Africa", "Somalia", "Solomon Islands", "Sierra Leone", "Senegal", "Benin", "Samoa", "Bhutan", "Brazil", "Kyrgyzstan", "Kosovo", "Kenya", "Jordan", "Israel", "Iraq", "Indonesia", "India", "Honduras", "Haiti", "Guatemala", "Ghana", "Georgia", "El Salvador", "Egypt", "Ecuador", "Dominican Republic", "Cote D'Ivoire", "Costa Rica", "Congo", "Colombia", "China", "Chile", "Cameroon", "Cambodia", "Burundi", "Burkina Faso", "Bolivia", "Lao People's Democratic Republic", "Rwanda", "Peru", "Cote D'Ivoire", "Congo", "Colombia", "China", "Chile", "Cameroon", "Cambodia", "Burundi", "Burkina Faso", "Brazil", "Bolivia", "Bhutan", "Benin", "Azerbaijan", "Armenia", "Albania", "Afghanistan", "Zambia", "Zambia", "Yemen", "Virgin Islands", "Vietnam", "Vanuatu", "United States", "Uganda", "Togo", "Timor-Leste", "Ecuador", "Philippines", "Egypt", "Ghana", "Paraguay", "Palestine", "Pakistan", "Nigeria", "Nepal", "Namibia", "Myanmar (Burma)", "Mozambique", "Mongolia", "Moldova", "Mauritania", "Mali", "Malawi", "Madagascar", "Liberia", "Lesotho", "Lebanon", "Lao People's Democratic Republic", "Kyrgyzstan", "Kosovo", "Kenya", "Jordan", "Israel", "Iraq", "Indonesia", "India", "Guam", "Georgia", "The Democratic Republic of the Congo", "Lebanon", "Liberia", "Namibia", "Myanmar (Burma)", "Mozambique", "Mongolia", "Moldova", "Mexico", "Mauritania", "Mali", "Malawi", "Madagascar", "Liberia", "Lesotho", "Lebanon", "Lao People's Democratic Republic", "Kyrgyzstan", "Kosovo", "Kenya", "Jordan", "Israel", "Iraq", "Indonesia", "India", "Honduras", "Haiti", "Guatemala", "Guam", "Ghana", "Nepal", "Georgia", "Nicaragua", "Pakistan", "Yemen", "Virgin Islands", "Vietnam", "Vanuatu", "United States", "Ukraine", "Uganda", "Turkey", "Togo", "Timor-Leste", "The Democratic Republic of the Congo", "Thailand", "Tanzania", "Tajikistan", "South Sudan", "South Africa", "Somalia", "Solomon Islands", "Sierra Leone", "Senegal", "Samoa", "Saint Vincent and the Grenadines", "Rwanda", "Puerto Rico", "Philippines", "Panama", "Palestine", "Nigeria", "Lesotho", "El Salvador", "Dominican Republic", "South Sudan", "South Africa", "Somalia", "Sierra Leone", "Senegal", "Saint Vincent and the Grenadines", "Rwanda", "Puerto Rico", "Philippines", "Peru", "Paraguay", "Panama", "Palestine", "Pakistan", "Nigeria", "Nicaragua", "Nepal", "Namibia", "Myanmar (Burma)", "Mozambique", "Mongolia", "Moldova", "Mexico", "Mauritania", "Mali", "Malawi", "Madagascar", "Suriname", "Egypt", "Tajikistan", "Thailand", "Cote D'Ivoire", "Costa Rica", "Congo", "China", "Cameroon", "Cambodia", "Burundi", "Burkina Faso", "Bhutan", "Benin", "Belize", "Azerbaijan", "Armenia", "Albania", "Afghanistan", "Zimbabwe", "Zambia", "Yemen", "Virgin Islands", "Vietnam", "United States", "Ukraine", "Uganda", "Turkey", "Togo", "Timor-Leste", "The Democratic Republic of the Congo", "Tanzania", "Thailand", "Zimbabwe", "Tajikistan", "Guam", "Ghana", "El Salvador", "Egypt", "Ecuador", "Dominican Republic", "Cote D'Ivoire", "Costa Rica", "Congo", "Colombia", "Guatemala", "Chile", "Burundi", "Burkina Faso", "Brazil", "Bolivia", "Benin", "Belize", "Albania", "Yemen", "Virgin Islands", "Vietnam", "Cameroon", "Haiti", "Honduras", "Kenya", "Sierra Leone", "Senegal", "Samoa", "Saint Vincent and the Grenadines", "Rwanda", "Puerto Rico", "Peru", "Paraguay", "Panama", "Nigeria", "Nicaragua", "Namibia", "Mozambique", "Moldova", "Mexico", "Mauritania", "Mali", "Malawi", "Madagascar", "Liberia", "Lesotho", "Tanzania", "Kosovo", "Vanuatu", "Solomon Islands", "United States", "Turkey", "Indonesia", "India", "Honduras", "Haiti", "Guatemala", "Guam", "Georgia", "El Salvador", "Ecuador", "Dominican Republic", "Iraq", "Costa Rica", "China", "Chile", "Cambodia", "Brazil", "Bolivia", "Bhutan", "Belize", "Azerbaijan", "Armenia", "Albania", "Colombia", "Israel", "Jordan", "Kosovo", "Timor-Leste", "Thailand", "Tajikistan", "Suriname", "Solomon Islands", "Samoa", "Saint Vincent and the Grenadines", "Puerto Rico", "Philippines", "Peru", "Paraguay", "Panama", "Palestine", "Pakistan", "Nicaragua", "Nepal", "Myanmar (Burma)", "Mongolia", "Moldova", "Mexico", "Lebanon", "Lao People's Democratic Republic", "Kyrgyzstan", "Ukraine", "Somalia", "Zimbabwe", "South Sudan", "Malawi", "Madagascar", "Liberia", "Lesotho", "Lebanon", "Lao People's Democratic Republic", "Kyrgyzstan", "Kenya", "Jordan", "Israel", "South Africa", "Indonesia", "India", "Honduras", "Haiti", "Guatemala", "Guam", "Ghana", "Georgia", "El Salvador", "Egypt", "Ecuador", "Dominican Republic", "Mali", "Cote D'Ivoire", "Mauritania", "Mongolia", "Suriname", "South Sudan", "South Africa", "Somalia", "Solomon Islands", "Sierra Leone", "Senegal", "Samoa", "Saint Vincent and the Grenadines", "Rwanda", "Puerto Rico", "Philippines", "Peru", "Paraguay", "Panama", "Palestine", "Pakistan", "Nigeria", "Nicaragua", "Nepal", "Namibia", "Myanmar (Burma)", "Mozambique", "Mexico", "Costa Rica", "Iraq", "Bolivia", "Burkina Faso", "Brazil", "Virgin Islands", "Togo", "Armenia", "Turkey", "Uganda", "Ukraine", "Congo", "Azerbaijan", "United States", "Bhutan", "Benin", "Vanuatu", "Belize", "Burundi", "Zimbabwe", "Zambia", "Cameroon", "Tanzania", "The Democratic Republic of the Congo", "Chile", "Afghanistan", "Cambodia", "Colombia", "China", "Suriname", "Guam", "Vanuatu", "Afghanistan", "Mauritania", "Bhutan", "Namibia", "Cote D'Ivoire", "Chile", "Belize", "South Sudan", "Saint Vincent and the Grenadines", "Somalia", "Panama", "Puerto Rico", "Nepal", "Lesotho", "China", "Thailand", "Solomon Islands", "Benin", "Suriname", "South Africa", "Brazil", "Moldova", "Israel", "Turkey", "Congo", "Cameroon", "Egypt", "Zambia", "Lao People's Democratic Republic", "Liberia", "Madagascar", "Togo", "Ukraine", "Mongolia", "Malawi", "Kosovo", "Yemen", "Nigeria", "Mozambique", "Costa Rica", "Dominican Republic", "Timor-Leste", "Albania", "Haiti", "Burundi", "Iraq", "Azerbaijan", "Burkina Faso", "Myanmar (Burma)", "Georgia", "Zimbabwe", "Sierra Leone", "Jordan", "Indonesia", "Ghana", "Samoa", "Honduras", "India", "Tanzania", "Kyrgyzstan", "Senegal", "Mali", "Mexico", "Nicaragua", "Guatemala", "The Democratic Republic of the Congo", "Armenia", "Lebanon", "Palestine", "Pakistan", "Colombia", "Tajikistan", "Uganda", "Ecuador", "Rwanda", "Cambodia", "United States", "El Salvador", "Vietnam", "Paraguay", "Peru", "Kenya", "Bolivia", "Philippines"], "legendgroup": "", "marker": {"color": "#636efa"}, "name": "", "offsetgroup": "", "orientation": "v", "showlegend": false, "textposition": "auto", "type": "bar", "x": ["Africa", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "Oceania", "N_America", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "S_America", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "N_America", "Europe", "Oceania", "Oceania", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "Oceania", "S_America", "S_America", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "S_America", "Oceania", "Oceania", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "S_America", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Oceania", "Europe", "Europe", "Europe", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Africa", "Africa", "Africa", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Asia", "Europe", "Asia", "Africa", "Asia", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Africa", "Asia", "S_America", "Asia", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Asia", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Europe", "Asia", "Asia", "Europe", "Asia", "Asia", "Asia", "Europe", "Europe", "Asia", "Europe", "Europe", "Asia", "Europe", "Europe", "Asia", "Asia", "Europe", "Asia", "Asia", "Europe", "Europe", "Europe", "Europe", "Europe", "Asia", "Oceania", "Oceania", "Asia", "Africa", "Asia", "Africa", "Africa", "S_America", "N_America", "Africa", "N_America", "Africa", "N_America", "N_America", "Asia", "Africa", "Asia", "Asia", "Oceania", "Africa", "S_America", "Africa", "S_America", "Europe", "Asia", "Europe", "Africa", "Africa", "Africa", "Africa", "Asia", "Africa", "Africa", "Africa", "Europe", "Asia", "Africa", "Europe", "Asia", "Africa", "Africa", "N_America", "N_America", "Asia", "Europe", "N_America", "Africa", "Asia", "Asia", "Africa", "Asia", "Asia", "Africa", "Africa", "Asia", "Asia", "Africa", "Oceania", "N_America", "Asia", "Africa", "Asia", "Africa", "Africa", "N_America", "N_America", "N_America", "Africa", "Asia", "Asia", "Asia", "Asia", "S_America", "Asia", "Africa", "S_America", "Africa", "Asia", "N_America", "N_America", "Asia", "S_America", "S_America", "Africa", "S_America", "Asia"], "xaxis": "x", "y": [null, null, null, null, null, null, null, null, null, 2.0, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, 1.0, 4.0, 2.0, 1.0, 2.0, 8.0, 1.0, 10.0, 125.0, 160.0, 48.0, 75.0, 193.0, 68.0, 717.0, 422.0, 134.0, 180.0, 554.0, 497.0, 223.0, 378.0, 284.0, 348.0, 190.0, 1703.0, 128.0, 2230.0, 1639.0, 784.0, 1486.0, 3682.0, 3821.0, 5749.0, 963.0, 964.0, 1320.0, 1419.0, 2313.0, 10136.0, 3483.0, 1561.0, 496.0, 2690.0, 1934.0, 3617.0, 880.0, 993.0, 1945.0, 2460.0, 1870.0, 2409.0, 4034.0, 5415.0, 4167.0, 6214.0, 4374.0, 7396.0, 6557.0, 11237.0, 5219.0, 5774.0, 3269.0, 6639.0, 5741.0, 11781.0, 7310.0, 3073.0, 8631.0, 8792.0, 8167.0, 26857.0, 21995.0, 19580.0, 20601.0, 13521.0, 6735.0, 34836.0, 6093.0, 39875.0, 21686.0, 11903.0, 22233.0, 75825.0, 17612.0, 160441.0], "yaxis": "y"}],                        {"barmode": "relative", "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "continent"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('fb21d975-77da-4db3-b339-c35356b5d47f');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Fundingdurchschnitt und Projektanzahl

### Alle Länder


```python
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.sum})
funded_amount = df_cont_count["funded_amount"].tolist()
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.mean})
country_mean = df_cont_count["funded_amount"].tolist()
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.median})
country_median = df_cont_count["funded_amount"].tolist()

df_cont_count = df_final.groupby(["continent", "country"]).agg({"country":np.size})
df_cont_count_rename = df_cont_count.rename(columns={"country": "amount_projects"})
df_cont_count = df_cont_count_rename.reset_index()
df_cont_count.insert(3,'funded_amount',funded_amount)
df_cont_count.insert(4,'country_mean',country_mean)
df_cont_count.insert(5,'country_median',country_median)
df_cont_count = df_cont_count.sort_values(by=["country_mean"])
df_cont_count = df_cont_count.loc[df_cont_count["funded_amount"]!=0, :]
#df_cont_count = df_cont_count.loc[df_cont_count["country_mean"]<50000, :]
df_cont_count
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
      <th>continent</th>
      <th>country</th>
      <th>amount_projects</th>
      <th>funded_amount</th>
      <th>country_mean</th>
      <th>country_median</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>53</th>
      <td>Africa</td>
      <td>Nigeria</td>
      <td>10136.0</td>
      <td>1905325.0</td>
      <td>187.976026</td>
      <td>125.0</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Africa</td>
      <td>Togo</td>
      <td>5749.0</td>
      <td>1486800.0</td>
      <td>258.618890</td>
      <td>175.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Africa</td>
      <td>Liberia</td>
      <td>3682.0</td>
      <td>1168225.0</td>
      <td>317.280011</td>
      <td>275.0</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Africa</td>
      <td>Madagascar</td>
      <td>3821.0</td>
      <td>1223575.0</td>
      <td>320.223763</td>
      <td>250.0</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Asia</td>
      <td>Philippines</td>
      <td>160441.0</td>
      <td>54476375.0</td>
      <td>339.541483</td>
      <td>275.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Asia</td>
      <td>Afghanistan</td>
      <td>2.0</td>
      <td>14000.0</td>
      <td>7000.000000</td>
      <td>7000.0</td>
    </tr>
    <tr>
      <th>448</th>
      <td>S_America</td>
      <td>Chile</td>
      <td>10.0</td>
      <td>76250.0</td>
      <td>7625.000000</td>
      <td>4725.0</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Asia</td>
      <td>Bhutan</td>
      <td>2.0</td>
      <td>15625.0</td>
      <td>7812.500000</td>
      <td>7812.5</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Africa</td>
      <td>Mauritania</td>
      <td>1.0</td>
      <td>15000.0</td>
      <td>15000.000000</td>
      <td>15000.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Africa</td>
      <td>Cote D'Ivoire</td>
      <td>1.0</td>
      <td>50000.0</td>
      <td>50000.000000</td>
      <td>50000.0</td>
    </tr>
  </tbody>
</table>
<p>86 rows × 6 columns</p>
</div>




```python
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.sum})
funded_amount = df_cont_count["funded_amount"].tolist()
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.mean})
country_mean = df_cont_count["funded_amount"].tolist()
df_cont_count = df_final.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.median})
country_median = df_cont_count["funded_amount"].tolist()

df_cont_count = df_final.groupby(["continent", "country"]).agg({"country":np.size})
df_cont_count_rename = df_cont_count.rename(columns={"country": "amount_projects"})
df_cont_count = df_cont_count_rename.reset_index()
df_cont_count.insert(3,'funded_amount',funded_amount)
df_cont_count.insert(4,'country_mean',country_mean)
df_cont_count.insert(5,'country_median',country_median)
df_cont_count = df_cont_count.sort_values(by=["funded_amount"])
df_cont_count = df_cont_count.loc[df_cont_count["funded_amount"]!=0, :]
df_cont_count = df_cont_count.loc[df_cont_count["country_mean"]<15000, :]
df_cont_count

fig = px.scatter(df_cont_count, x="country_mean", y="amount_projects", color="continent", size="funded_amount", 
           hover_name="country", log_x=True, size_max=60)
fig.show()
```


<div>                            <div id="44716aff-a40d-40ca-bc08-736a636a8e19" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("44716aff-a40d-40ca-bc08-736a636a8e19")) {                    Plotly.newPlot(                        "44716aff-a40d-40ca-bc08-736a636a8e19",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Oceania<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Guam", "Vanuatu", "Solomon Islands", "Samoa"], "legendgroup": "Oceania", "marker": {"color": "#636efa", "size": [395.0, 9250.0, 493875.0, 5641825.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "Oceania", "orientation": "v", "showlegend": true, "type": "scatter", "x": [395.0, 2312.5, 891.471119133574, 762.8211195240671], "xaxis": "x", "y": [1.0, 4.0, 554.0, 7396.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Asia<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Afghanistan", "Bhutan", "Nepal", "China", "Thailand", "Israel", "Lao People's Democratic Republic", "Mongolia", "Yemen", "Timor-Leste", "Iraq", "Azerbaijan", "Myanmar (Burma)", "Georgia", "Jordan", "Indonesia", "India", "Kyrgyzstan", "Armenia", "Lebanon", "Palestine", "Pakistan", "Tajikistan", "Cambodia", "Vietnam", "Philippines"], "legendgroup": "Asia", "marker": {"color": "#EF553B", "size": [14000.0, 15625.0, 307625.0, 373475.0, 423350.0, 719450.0, 1160425.0, 1591400.0, 1784075.0, 2326900.0, 2611225.0, 2699575.0, 3035850.0, 3369500.0, 4425525.0, 4547025.0, 6466850.0, 6728550.0, 11186675.0, 11556350.0, 12032025.0, 12467100.0, 13781775.0, 18817100.0, 27323400.0, 54476375.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "Asia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [7000.0, 7812.5, 429.044630404463, 2787.126865671642, 2351.9444444444443, 3786.5789473684213, 780.9051144010767, 1650.8298755186722, 771.3251188932123, 865.0185873605948, 2629.6324269889224, 1387.956298200514, 1623.4491978609626, 1398.7131589871317, 1062.0410367170625, 731.7388155777277, 575.496128860016, 1165.3186698995496, 1296.1041594253272, 1314.4165150136487, 1473.2490510591404, 464.20300107979295, 703.8700204290092, 540.1624755999541, 1259.9557318085401, 339.5414825387526], "xaxis": "x", "y": [2.0, 2.0, 717.0, 134.0, 180.0, 190.0, 1486.0, 964.0, 2313.0, 2690.0, 993.0, 1945.0, 1870.0, 2409.0, 4167.0, 6214.0, 11237.0, 5774.0, 8631.0, 8792.0, 8167.0, 26857.0, 19580.0, 34836.0, 21686.0, 160441.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Africa<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Namibia", "South Sudan", "Somalia", "Lesotho", "Benin", "South Africa", "Congo", "Cameroon", "Egypt", "Zambia", "Liberia", "Madagascar", "Togo", "Malawi", "Nigeria", "Mozambique", "Burundi", "Burkina Faso", "Zimbabwe", "Sierra Leone", "Ghana", "Tanzania", "Senegal", "Mali", "The Democratic Republic of the Congo", "Uganda", "Rwanda", "Kenya"], "legendgroup": "Africa", "marker": {"color": "#00cc96", "size": [32375.0, 120900.0, 225875.0, 359525.0, 516825.0, 574025.0, 786250.0, 875500.0, 1084925.0, 1147950.0, 1168225.0, 1223575.0, 1486800.0, 1612500.0, 1905325.0, 1918825.0, 2558550.0, 2909975.0, 3372725.0, 3961050.0, 4792475.0, 6518075.0, 6819675.0, 8340850.0, 11020275.0, 14142675.0, 15505600.0, 32248405.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "Africa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [4046.875, 755.625, 3011.6666666666665, 851.9549763033175, 1039.8893360160966, 1518.584656084656, 6142.578125, 392.60089686098655, 661.9432580841977, 1464.2219387755101, 317.28001086366106, 320.22376341271917, 258.6188902417812, 1221.590909090909, 187.97602604577742, 550.9115704852139, 2907.443181818182, 1182.9166666666667, 836.074615765989, 731.4958448753463, 1095.6732967535436, 1248.9126269400267, 2086.165494034873, 1256.3413164633228, 3586.1617312072894, 686.5042959079657, 2302.242019302153, 425.3004286185295], "xaxis": "x", "y": [8.0, 160.0, 75.0, 422.0, 497.0, 378.0, 128.0, 2230.0, 1639.0, 784.0, 3682.0, 3821.0, 5749.0, 1320.0, 10136.0, 3483.0, 880.0, 2460.0, 4034.0, 5415.0, 4374.0, 5219.0, 3269.0, 6639.0, 3073.0, 20601.0, 6735.0, 75825.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=S_America<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Chile", "Suriname", "Brazil", "Colombia", "Ecuador", "Paraguay", "Peru", "Bolivia"], "legendgroup": "S_America", "marker": {"color": "#ab63fa", "size": [76250.0, 540475.0, 661025.0, 12473775.0, 14598900.0, 29412700.0, 30394850.0, 36552400.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "S_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [7625.0, 2423.654708520179, 2327.5528169014083, 567.1186633325756, 1079.720434879077, 2471.032512811896, 1367.1052039760716, 2075.4258460140813], "xaxis": "x", "y": [10.0, 223.0, 284.0, 21995.0, 13521.0, 11903.0, 22233.0, 17612.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=N_America<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Belize", "Saint Vincent and the Grenadines", "Panama", "Puerto Rico", "Costa Rica", "Dominican Republic", "Haiti", "Honduras", "Mexico", "Nicaragua", "Guatemala", "United States", "El Salvador"], "legendgroup": "N_America", "marker": {"color": "#FFA15A", "size": [114025.0, 147675.0, 273275.0, 299825.0, 2015525.0, 2083500.0, 2549475.0, 5667925.0, 9391225.0, 9854375.0, 10934975.0, 23158540.0, 23357725.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "N_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [912.2, 3076.5625, 1415.9326424870467, 4409.191176470588, 1291.175528507367, 4200.604838709677, 704.8589991705834, 864.4082659752936, 1635.8169308482843, 836.4633732280791, 1495.8926128590972, 3800.8435910060725, 585.7736677115987], "xaxis": "x", "y": [125.0, 48.0, 193.0, 68.0, 1561.0, 496.0, 3617.0, 6557.0, 5741.0, 11781.0, 7310.0, 6093.0, 39875.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Europe<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Moldova", "Turkey", "Ukraine", "Kosovo", "Albania"], "legendgroup": "Europe", "marker": {"color": "#19d3f3", "size": [686850.0, 743625.0, 1561350.0, 1778600.0, 2490000.0], "sizemode": "area", "sizeref": 15132.326388888889, "symbol": "circle"}, "mode": "markers", "name": "Europe", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1973.7068965517242, 436.6559013505578, 1621.3395638629283, 1253.417899929528, 1287.4870734229576], "xaxis": "x", "y": [348.0, 1703.0, 963.0, 1419.0, 1934.0], "yaxis": "y"}],                        {"legend": {"itemsizing": "constant", "title": {"text": "continent"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "country_mean"}, "type": "log"}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('44716aff-a40d-40ca-bc08-736a636a8e19');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### Nach Kontinenten


```python
# Logarithmierte Darstellung

fig = px.scatter(df_cont_count, x="country_mean", y="amount_projects",
           color="continent", hover_name="country", log_x=True, facet_col="continent", size="funded_amount")
fig.show()
```


<div>                            <div id="b228d483-6721-453f-bd09-765700f94867" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("b228d483-6721-453f-bd09-765700f94867")) {                    Plotly.newPlot(                        "b228d483-6721-453f-bd09-765700f94867",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Oceania<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Guam", "Vanuatu", "Solomon Islands", "Samoa"], "legendgroup": "Oceania", "marker": {"color": "#636efa", "size": [395.0, 9250.0, 493875.0, 5641825.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "Oceania", "orientation": "v", "showlegend": true, "type": "scatter", "x": [395.0, 2312.5, 891.471119133574, 762.8211195240671], "xaxis": "x", "y": [1.0, 4.0, 554.0, 7396.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Asia<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Afghanistan", "Bhutan", "Nepal", "China", "Thailand", "Israel", "Lao People's Democratic Republic", "Mongolia", "Yemen", "Timor-Leste", "Iraq", "Azerbaijan", "Myanmar (Burma)", "Georgia", "Jordan", "Indonesia", "India", "Kyrgyzstan", "Armenia", "Lebanon", "Palestine", "Pakistan", "Tajikistan", "Cambodia", "Vietnam", "Philippines"], "legendgroup": "Asia", "marker": {"color": "#EF553B", "size": [14000.0, 15625.0, 307625.0, 373475.0, 423350.0, 719450.0, 1160425.0, 1591400.0, 1784075.0, 2326900.0, 2611225.0, 2699575.0, 3035850.0, 3369500.0, 4425525.0, 4547025.0, 6466850.0, 6728550.0, 11186675.0, 11556350.0, 12032025.0, 12467100.0, 13781775.0, 18817100.0, 27323400.0, 54476375.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "Asia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [7000.0, 7812.5, 429.044630404463, 2787.126865671642, 2351.9444444444443, 3786.5789473684213, 780.9051144010767, 1650.8298755186722, 771.3251188932123, 865.0185873605948, 2629.6324269889224, 1387.956298200514, 1623.4491978609626, 1398.7131589871317, 1062.0410367170625, 731.7388155777277, 575.496128860016, 1165.3186698995496, 1296.1041594253272, 1314.4165150136487, 1473.2490510591404, 464.20300107979295, 703.8700204290092, 540.1624755999541, 1259.9557318085401, 339.5414825387526], "xaxis": "x2", "y": [2.0, 2.0, 717.0, 134.0, 180.0, 190.0, 1486.0, 964.0, 2313.0, 2690.0, 993.0, 1945.0, 1870.0, 2409.0, 4167.0, 6214.0, 11237.0, 5774.0, 8631.0, 8792.0, 8167.0, 26857.0, 19580.0, 34836.0, 21686.0, 160441.0], "yaxis": "y2"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Africa<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Namibia", "South Sudan", "Somalia", "Lesotho", "Benin", "South Africa", "Congo", "Cameroon", "Egypt", "Zambia", "Liberia", "Madagascar", "Togo", "Malawi", "Nigeria", "Mozambique", "Burundi", "Burkina Faso", "Zimbabwe", "Sierra Leone", "Ghana", "Tanzania", "Senegal", "Mali", "The Democratic Republic of the Congo", "Uganda", "Rwanda", "Kenya"], "legendgroup": "Africa", "marker": {"color": "#00cc96", "size": [32375.0, 120900.0, 225875.0, 359525.0, 516825.0, 574025.0, 786250.0, 875500.0, 1084925.0, 1147950.0, 1168225.0, 1223575.0, 1486800.0, 1612500.0, 1905325.0, 1918825.0, 2558550.0, 2909975.0, 3372725.0, 3961050.0, 4792475.0, 6518075.0, 6819675.0, 8340850.0, 11020275.0, 14142675.0, 15505600.0, 32248405.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "Africa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [4046.875, 755.625, 3011.6666666666665, 851.9549763033175, 1039.8893360160966, 1518.584656084656, 6142.578125, 392.60089686098655, 661.9432580841977, 1464.2219387755101, 317.28001086366106, 320.22376341271917, 258.6188902417812, 1221.590909090909, 187.97602604577742, 550.9115704852139, 2907.443181818182, 1182.9166666666667, 836.074615765989, 731.4958448753463, 1095.6732967535436, 1248.9126269400267, 2086.165494034873, 1256.3413164633228, 3586.1617312072894, 686.5042959079657, 2302.242019302153, 425.3004286185295], "xaxis": "x3", "y": [8.0, 160.0, 75.0, 422.0, 497.0, 378.0, 128.0, 2230.0, 1639.0, 784.0, 3682.0, 3821.0, 5749.0, 1320.0, 10136.0, 3483.0, 880.0, 2460.0, 4034.0, 5415.0, 4374.0, 5219.0, 3269.0, 6639.0, 3073.0, 20601.0, 6735.0, 75825.0], "yaxis": "y3"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=S_America<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Chile", "Suriname", "Brazil", "Colombia", "Ecuador", "Paraguay", "Peru", "Bolivia"], "legendgroup": "S_America", "marker": {"color": "#ab63fa", "size": [76250.0, 540475.0, 661025.0, 12473775.0, 14598900.0, 29412700.0, 30394850.0, 36552400.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "S_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [7625.0, 2423.654708520179, 2327.5528169014083, 567.1186633325756, 1079.720434879077, 2471.032512811896, 1367.1052039760716, 2075.4258460140813], "xaxis": "x4", "y": [10.0, 223.0, 284.0, 21995.0, 13521.0, 11903.0, 22233.0, 17612.0], "yaxis": "y4"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=N_America<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Belize", "Saint Vincent and the Grenadines", "Panama", "Puerto Rico", "Costa Rica", "Dominican Republic", "Haiti", "Honduras", "Mexico", "Nicaragua", "Guatemala", "United States", "El Salvador"], "legendgroup": "N_America", "marker": {"color": "#FFA15A", "size": [114025.0, 147675.0, 273275.0, 299825.0, 2015525.0, 2083500.0, 2549475.0, 5667925.0, 9391225.0, 9854375.0, 10934975.0, 23158540.0, 23357725.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "N_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [912.2, 3076.5625, 1415.9326424870467, 4409.191176470588, 1291.175528507367, 4200.604838709677, 704.8589991705834, 864.4082659752936, 1635.8169308482843, 836.4633732280791, 1495.8926128590972, 3800.8435910060725, 585.7736677115987], "xaxis": "x5", "y": [125.0, 48.0, 193.0, 68.0, 1561.0, 496.0, 3617.0, 6557.0, 5741.0, 11781.0, 7310.0, 6093.0, 39875.0], "yaxis": "y5"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Europe<br>country_mean=%{x}<br>amount_projects=%{y}<br>funded_amount=%{marker.size}<extra></extra>", "hovertext": ["Moldova", "Turkey", "Ukraine", "Kosovo", "Albania"], "legendgroup": "Europe", "marker": {"color": "#19d3f3", "size": [686850.0, 743625.0, 1561350.0, 1778600.0, 2490000.0], "sizemode": "area", "sizeref": 136190.9375, "symbol": "circle"}, "mode": "markers", "name": "Europe", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1973.7068965517242, 436.6559013505578, 1621.3395638629283, 1253.417899929528, 1287.4870734229576], "xaxis": "x6", "y": [348.0, 1703.0, 963.0, 1419.0, 1934.0], "yaxis": "y6"}],                        {"annotations": [{"font": {}, "showarrow": false, "text": "continent=Oceania", "x": 0.075, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Asia", "x": 0.24499999999999997, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Africa", "x": 0.415, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=S_America", "x": 0.585, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=N_America", "x": 0.7549999999999999, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Europe", "x": 0.925, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}], "legend": {"itemsizing": "constant", "title": {"text": "continent"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 0.15], "title": {"text": "country_mean"}, "type": "log"}, "xaxis2": {"anchor": "y2", "domain": [0.16999999999999998, 0.31999999999999995], "matches": "x", "title": {"text": "country_mean"}, "type": "log"}, "xaxis3": {"anchor": "y3", "domain": [0.33999999999999997, 0.49], "matches": "x", "title": {"text": "country_mean"}, "type": "log"}, "xaxis4": {"anchor": "y4", "domain": [0.51, 0.66], "matches": "x", "title": {"text": "country_mean"}, "type": "log"}, "xaxis5": {"anchor": "y5", "domain": [0.6799999999999999, 0.83], "matches": "x", "title": {"text": "country_mean"}, "type": "log"}, "xaxis6": {"anchor": "y6", "domain": [0.85, 1.0], "matches": "x", "title": {"text": "country_mean"}, "type": "log"}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}}, "yaxis2": {"anchor": "x2", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis3": {"anchor": "x3", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis4": {"anchor": "x4", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis5": {"anchor": "x5", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis6": {"anchor": "x6", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('b228d483-6721-453f-bd09-765700f94867');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Anzahl Projekt vs Fundingsumme je Land und Kontinent


```python
fig = px.scatter(df_cont_count, x="amount_projects", y="funded_amount", color="continent",
           hover_name="country", log_x=True, size_max=60)
fig.show()
```


<div>                            <div id="767337e4-2ca0-4e23-a127-e870101de8c9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("767337e4-2ca0-4e23-a127-e870101de8c9")) {                    Plotly.newPlot(                        "767337e4-2ca0-4e23-a127-e870101de8c9",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Oceania<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Guam", "Vanuatu", "Solomon Islands", "Samoa"], "legendgroup": "Oceania", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Oceania", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1.0, 4.0, 554.0, 7396.0], "xaxis": "x", "y": [395.0, 9250.0, 493875.0, 5641825.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Asia<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Afghanistan", "Bhutan", "Nepal", "China", "Thailand", "Israel", "Lao People's Democratic Republic", "Mongolia", "Yemen", "Timor-Leste", "Iraq", "Azerbaijan", "Myanmar (Burma)", "Georgia", "Jordan", "Indonesia", "India", "Kyrgyzstan", "Armenia", "Lebanon", "Palestine", "Pakistan", "Tajikistan", "Cambodia", "Vietnam", "Philippines"], "legendgroup": "Asia", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Asia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2.0, 2.0, 717.0, 134.0, 180.0, 190.0, 1486.0, 964.0, 2313.0, 2690.0, 993.0, 1945.0, 1870.0, 2409.0, 4167.0, 6214.0, 11237.0, 5774.0, 8631.0, 8792.0, 8167.0, 26857.0, 19580.0, 34836.0, 21686.0, 160441.0], "xaxis": "x", "y": [14000.0, 15625.0, 307625.0, 373475.0, 423350.0, 719450.0, 1160425.0, 1591400.0, 1784075.0, 2326900.0, 2611225.0, 2699575.0, 3035850.0, 3369500.0, 4425525.0, 4547025.0, 6466850.0, 6728550.0, 11186675.0, 11556350.0, 12032025.0, 12467100.0, 13781775.0, 18817100.0, 27323400.0, 54476375.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Africa<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Namibia", "South Sudan", "Somalia", "Lesotho", "Benin", "South Africa", "Congo", "Cameroon", "Egypt", "Zambia", "Liberia", "Madagascar", "Togo", "Malawi", "Nigeria", "Mozambique", "Burundi", "Burkina Faso", "Zimbabwe", "Sierra Leone", "Ghana", "Tanzania", "Senegal", "Mali", "The Democratic Republic of the Congo", "Uganda", "Rwanda", "Kenya"], "legendgroup": "Africa", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Africa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [8.0, 160.0, 75.0, 422.0, 497.0, 378.0, 128.0, 2230.0, 1639.0, 784.0, 3682.0, 3821.0, 5749.0, 1320.0, 10136.0, 3483.0, 880.0, 2460.0, 4034.0, 5415.0, 4374.0, 5219.0, 3269.0, 6639.0, 3073.0, 20601.0, 6735.0, 75825.0], "xaxis": "x", "y": [32375.0, 120900.0, 225875.0, 359525.0, 516825.0, 574025.0, 786250.0, 875500.0, 1084925.0, 1147950.0, 1168225.0, 1223575.0, 1486800.0, 1612500.0, 1905325.0, 1918825.0, 2558550.0, 2909975.0, 3372725.0, 3961050.0, 4792475.0, 6518075.0, 6819675.0, 8340850.0, 11020275.0, 14142675.0, 15505600.0, 32248405.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=S_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Chile", "Suriname", "Brazil", "Colombia", "Ecuador", "Paraguay", "Peru", "Bolivia"], "legendgroup": "S_America", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "S_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [10.0, 223.0, 284.0, 21995.0, 13521.0, 11903.0, 22233.0, 17612.0], "xaxis": "x", "y": [76250.0, 540475.0, 661025.0, 12473775.0, 14598900.0, 29412700.0, 30394850.0, 36552400.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=N_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Belize", "Saint Vincent and the Grenadines", "Panama", "Puerto Rico", "Costa Rica", "Dominican Republic", "Haiti", "Honduras", "Mexico", "Nicaragua", "Guatemala", "United States", "El Salvador"], "legendgroup": "N_America", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "N_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [125.0, 48.0, 193.0, 68.0, 1561.0, 496.0, 3617.0, 6557.0, 5741.0, 11781.0, 7310.0, 6093.0, 39875.0], "xaxis": "x", "y": [114025.0, 147675.0, 273275.0, 299825.0, 2015525.0, 2083500.0, 2549475.0, 5667925.0, 9391225.0, 9854375.0, 10934975.0, 23158540.0, 23357725.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Europe<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Moldova", "Turkey", "Ukraine", "Kosovo", "Albania"], "legendgroup": "Europe", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Europe", "orientation": "v", "showlegend": true, "type": "scatter", "x": [348.0, 1703.0, 963.0, 1419.0, 1934.0], "xaxis": "x", "y": [686850.0, 743625.0, 1561350.0, 1778600.0, 2490000.0], "yaxis": "y"}],                        {"legend": {"title": {"text": "continent"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}, "type": "log"}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "funded_amount"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('767337e4-2ca0-4e23-a127-e870101de8c9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# Logarithmierte Darstellung

fig = px.scatter(df_cont_count, x="amount_projects", y="funded_amount",
           color="continent", hover_name="country", log_x=True, facet_col="continent")
fig.show()
```


<div>                            <div id="ed81b912-aae4-43f2-b403-c2759cc1ba38" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("ed81b912-aae4-43f2-b403-c2759cc1ba38")) {                    Plotly.newPlot(                        "ed81b912-aae4-43f2-b403-c2759cc1ba38",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Oceania<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Guam", "Vanuatu", "Solomon Islands", "Samoa"], "legendgroup": "Oceania", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Oceania", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1.0, 4.0, 554.0, 7396.0], "xaxis": "x", "y": [395.0, 9250.0, 493875.0, 5641825.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Asia<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Afghanistan", "Bhutan", "Nepal", "China", "Thailand", "Israel", "Lao People's Democratic Republic", "Mongolia", "Yemen", "Timor-Leste", "Iraq", "Azerbaijan", "Myanmar (Burma)", "Georgia", "Jordan", "Indonesia", "India", "Kyrgyzstan", "Armenia", "Lebanon", "Palestine", "Pakistan", "Tajikistan", "Cambodia", "Vietnam", "Philippines"], "legendgroup": "Asia", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Asia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2.0, 2.0, 717.0, 134.0, 180.0, 190.0, 1486.0, 964.0, 2313.0, 2690.0, 993.0, 1945.0, 1870.0, 2409.0, 4167.0, 6214.0, 11237.0, 5774.0, 8631.0, 8792.0, 8167.0, 26857.0, 19580.0, 34836.0, 21686.0, 160441.0], "xaxis": "x2", "y": [14000.0, 15625.0, 307625.0, 373475.0, 423350.0, 719450.0, 1160425.0, 1591400.0, 1784075.0, 2326900.0, 2611225.0, 2699575.0, 3035850.0, 3369500.0, 4425525.0, 4547025.0, 6466850.0, 6728550.0, 11186675.0, 11556350.0, 12032025.0, 12467100.0, 13781775.0, 18817100.0, 27323400.0, 54476375.0], "yaxis": "y2"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Africa<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Namibia", "South Sudan", "Somalia", "Lesotho", "Benin", "South Africa", "Congo", "Cameroon", "Egypt", "Zambia", "Liberia", "Madagascar", "Togo", "Malawi", "Nigeria", "Mozambique", "Burundi", "Burkina Faso", "Zimbabwe", "Sierra Leone", "Ghana", "Tanzania", "Senegal", "Mali", "The Democratic Republic of the Congo", "Uganda", "Rwanda", "Kenya"], "legendgroup": "Africa", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Africa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [8.0, 160.0, 75.0, 422.0, 497.0, 378.0, 128.0, 2230.0, 1639.0, 784.0, 3682.0, 3821.0, 5749.0, 1320.0, 10136.0, 3483.0, 880.0, 2460.0, 4034.0, 5415.0, 4374.0, 5219.0, 3269.0, 6639.0, 3073.0, 20601.0, 6735.0, 75825.0], "xaxis": "x3", "y": [32375.0, 120900.0, 225875.0, 359525.0, 516825.0, 574025.0, 786250.0, 875500.0, 1084925.0, 1147950.0, 1168225.0, 1223575.0, 1486800.0, 1612500.0, 1905325.0, 1918825.0, 2558550.0, 2909975.0, 3372725.0, 3961050.0, 4792475.0, 6518075.0, 6819675.0, 8340850.0, 11020275.0, 14142675.0, 15505600.0, 32248405.0], "yaxis": "y3"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=S_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Chile", "Suriname", "Brazil", "Colombia", "Ecuador", "Paraguay", "Peru", "Bolivia"], "legendgroup": "S_America", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "S_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [10.0, 223.0, 284.0, 21995.0, 13521.0, 11903.0, 22233.0, 17612.0], "xaxis": "x4", "y": [76250.0, 540475.0, 661025.0, 12473775.0, 14598900.0, 29412700.0, 30394850.0, 36552400.0], "yaxis": "y4"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=N_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Belize", "Saint Vincent and the Grenadines", "Panama", "Puerto Rico", "Costa Rica", "Dominican Republic", "Haiti", "Honduras", "Mexico", "Nicaragua", "Guatemala", "United States", "El Salvador"], "legendgroup": "N_America", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "N_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [125.0, 48.0, 193.0, 68.0, 1561.0, 496.0, 3617.0, 6557.0, 5741.0, 11781.0, 7310.0, 6093.0, 39875.0], "xaxis": "x5", "y": [114025.0, 147675.0, 273275.0, 299825.0, 2015525.0, 2083500.0, 2549475.0, 5667925.0, 9391225.0, 9854375.0, 10934975.0, 23158540.0, 23357725.0], "yaxis": "y5"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Europe<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Moldova", "Turkey", "Ukraine", "Kosovo", "Albania"], "legendgroup": "Europe", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Europe", "orientation": "v", "showlegend": true, "type": "scatter", "x": [348.0, 1703.0, 963.0, 1419.0, 1934.0], "xaxis": "x6", "y": [686850.0, 743625.0, 1561350.0, 1778600.0, 2490000.0], "yaxis": "y6"}],                        {"annotations": [{"font": {}, "showarrow": false, "text": "continent=Oceania", "x": 0.075, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Asia", "x": 0.24499999999999997, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Africa", "x": 0.415, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=S_America", "x": 0.585, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=N_America", "x": 0.7549999999999999, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}, {"font": {}, "showarrow": false, "text": "continent=Europe", "x": 0.925, "xanchor": "center", "xref": "paper", "y": 1.0, "yanchor": "bottom", "yref": "paper"}], "legend": {"title": {"text": "continent"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 0.15], "title": {"text": "amount_projects"}, "type": "log"}, "xaxis2": {"anchor": "y2", "domain": [0.16999999999999998, 0.31999999999999995], "matches": "x", "title": {"text": "amount_projects"}, "type": "log"}, "xaxis3": {"anchor": "y3", "domain": [0.33999999999999997, 0.49], "matches": "x", "title": {"text": "amount_projects"}, "type": "log"}, "xaxis4": {"anchor": "y4", "domain": [0.51, 0.66], "matches": "x", "title": {"text": "amount_projects"}, "type": "log"}, "xaxis5": {"anchor": "y5", "domain": [0.6799999999999999, 0.83], "matches": "x", "title": {"text": "amount_projects"}, "type": "log"}, "xaxis6": {"anchor": "y6", "domain": [0.85, 1.0], "matches": "x", "title": {"text": "amount_projects"}, "type": "log"}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "funded_amount"}}, "yaxis2": {"anchor": "x2", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis3": {"anchor": "x3", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis4": {"anchor": "x4", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis5": {"anchor": "x5", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}, "yaxis6": {"anchor": "x6", "domain": [0.0, 1.0], "matches": "y", "showticklabels": false}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('ed81b912-aae4-43f2-b403-c2759cc1ba38');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


# Länder und Sektoren im Verhältnis


```python
df_activity
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
      <th>sector</th>
      <th>activity</th>
      <th>amount_projects</th>
      <th>funded_amount</th>
      <th>funded_mean</th>
      <th>funded_median</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1708</th>
      <td>Personal Use</td>
      <td>Home Appliances</td>
      <td>20299.0</td>
      <td>5410150.0</td>
      <td>266.522981</td>
      <td>175.0</td>
    </tr>
    <tr>
      <th>1655</th>
      <td>Personal Use</td>
      <td>Celebrations</td>
      <td>5.0</td>
      <td>1400.0</td>
      <td>280.000000</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>986</th>
      <td>Food</td>
      <td>Balut-Making</td>
      <td>61.0</td>
      <td>24275.0</td>
      <td>397.950820</td>
      <td>300.0</td>
    </tr>
    <tr>
      <th>1741</th>
      <td>Personal Use</td>
      <td>Personal Expenses</td>
      <td>6732.0</td>
      <td>2820675.0</td>
      <td>418.995098</td>
      <td>225.0</td>
    </tr>
    <tr>
      <th>2218</th>
      <td>Transportation</td>
      <td>Motorcycle Transport</td>
      <td>5893.0</td>
      <td>2710075.0</td>
      <td>459.880367</td>
      <td>350.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2355</th>
      <td>Wholesale</td>
      <td>Goods Distribution</td>
      <td>279.0</td>
      <td>466050.0</td>
      <td>1670.430108</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Services</td>
      <td>Adult Care</td>
      <td>2.0</td>
      <td>3375.0</td>
      <td>1687.500000</td>
      <td>1687.5</td>
    </tr>
    <tr>
      <th>1902</th>
      <td>Retail</td>
      <td>Perfumes</td>
      <td>338.0</td>
      <td>578475.0</td>
      <td>1711.464497</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1854</th>
      <td>Retail</td>
      <td>Florist</td>
      <td>67.0</td>
      <td>120700.0</td>
      <td>1801.492537</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>478</th>
      <td>Clothing</td>
      <td>Used Shoes</td>
      <td>764.0</td>
      <td>1430175.0</td>
      <td>1871.956806</td>
      <td>650.0</td>
    </tr>
  </tbody>
</table>
<p>156 rows × 6 columns</p>
</div>




```python
df_count_sect = df_final.groupby(["continent", "country", "sector", "activity"]).agg({"funded_amount":np.sum, "activity":np.size})
df_activ = df_count_sect.rename(columns={"activity": "amount_projects"})
df_count_sect = df_activ.reset_index()
```


```python
df_count_sect = df_count_sect.dropna()
df_count_sect = df_count_sect.sort_values(by=["funded_amount"])
df_count_sect = df_count_sect.loc[df_count_sect["funded_amount"]< 6000000, :]
df_count_sect.tail(10)
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
      <th>continent</th>
      <th>country</th>
      <th>sector</th>
      <th>activity</th>
      <th>funded_amount</th>
      <th>amount_projects</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>693351</th>
      <td>N_America</td>
      <td>El Salvador</td>
      <td>Housing</td>
      <td>Personal Housing Expenses</td>
      <td>2939750.0</td>
      <td>5925.0</td>
    </tr>
    <tr>
      <th>358007</th>
      <td>Asia</td>
      <td>Philippines</td>
      <td>Food</td>
      <td>Fish Selling</td>
      <td>2969850.0</td>
      <td>9060.0</td>
    </tr>
    <tr>
      <th>1081734</th>
      <td>S_America</td>
      <td>Bolivia</td>
      <td>Food</td>
      <td>Food Stall</td>
      <td>2994550.0</td>
      <td>1092.0</td>
    </tr>
    <tr>
      <th>358008</th>
      <td>Asia</td>
      <td>Philippines</td>
      <td>Food</td>
      <td>Fishing</td>
      <td>3129675.0</td>
      <td>8682.0</td>
    </tr>
    <tr>
      <th>217662</th>
      <td>Asia</td>
      <td>Armenia</td>
      <td>Agriculture</td>
      <td>Farming</td>
      <td>3463950.0</td>
      <td>2451.0</td>
    </tr>
    <tr>
      <th>834788</th>
      <td>N_America</td>
      <td>United States</td>
      <td>Food</td>
      <td>Food Production/Sales</td>
      <td>3628190.0</td>
      <td>709.0</td>
    </tr>
    <tr>
      <th>300824</th>
      <td>Asia</td>
      <td>Kyrgyzstan</td>
      <td>Agriculture</td>
      <td>Livestock</td>
      <td>3707650.0</td>
      <td>3222.0</td>
    </tr>
    <tr>
      <th>357027</th>
      <td>Asia</td>
      <td>Philippines</td>
      <td>Agriculture</td>
      <td>Farming</td>
      <td>4063150.0</td>
      <td>11924.0</td>
    </tr>
    <tr>
      <th>835837</th>
      <td>N_America</td>
      <td>United States</td>
      <td>Services</td>
      <td>Services</td>
      <td>4215145.0</td>
      <td>1323.0</td>
    </tr>
    <tr>
      <th>414621</th>
      <td>Asia</td>
      <td>Vietnam</td>
      <td>Housing</td>
      <td>Personal Housing Expenses</td>
      <td>5690050.0</td>
      <td>7358.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = px.treemap(df_count_sect, path=[px.Constant('world'), 'continent', 'country','sector'], values='amount_projects',
                  color='funded_amount')
fig.show()
```



var gd = document.getElementById('081b945d-6396-467e-a77a-9807301f6b25');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
df_iso = df_final.groupby(["ISO_ALPHA3", "sector", "activity"]).agg({"funded_amount":np.sum, "activity":np.size})
df_activ = df_iso.rename(columns={"activity": "amount_projects"})
df_iso = df_activ.reset_index()
```


```python
df_iso = df_iso.dropna()
df_iso.loc[df_iso["funded_amount"]==0, :]
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
      <th>ISO_ALPHA3</th>
      <th>sector</th>
      <th>activity</th>
      <th>funded_amount</th>
      <th>amount_projects</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>70447</th>
      <td>JOR</td>
      <td>Services</td>
      <td>Cleaning Services</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>77875</th>
      <td>KHM</td>
      <td>Services</td>
      <td>Printing</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>92354</th>
      <td>MEX</td>
      <td>Retail</td>
      <td>Mobile Phones</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>124259</th>
      <td>PRI</td>
      <td>Services</td>
      <td>Energy</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>131293</th>
      <td>SEN</td>
      <td>Personal Use</td>
      <td>Home Appliances</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_iso = df_iso.dropna()
df_iso.loc[df_iso["funded_amount"]==0, :]
df_iso = df_iso.sort_values(by=["funded_amount"])
df_iso["amount_projects"] = df_iso["amount_projects"].astype("int")
#df_iso["amount_projects"] = df_iso["funded_amount"].astype("int")
#df_iso = df_iso.loc[df_iso["funded_amount"]< 100000, :]
df_iso
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
      <th>ISO_ALPHA3</th>
      <th>sector</th>
      <th>activity</th>
      <th>funded_amount</th>
      <th>amount_projects</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>92354</th>
      <td>MEX</td>
      <td>Retail</td>
      <td>Mobile Phones</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>131293</th>
      <td>SEN</td>
      <td>Personal Use</td>
      <td>Home Appliances</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>124259</th>
      <td>PRI</td>
      <td>Services</td>
      <td>Energy</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>70447</th>
      <td>JOR</td>
      <td>Services</td>
      <td>Cleaning Services</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>77875</th>
      <td>KHM</td>
      <td>Services</td>
      <td>Printing</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>170121</th>
      <td>VNM</td>
      <td>Housing</td>
      <td>Personal Housing Expenses</td>
      <td>5690050.0</td>
      <td>7358</td>
    </tr>
    <tr>
      <th>119925</th>
      <td>PHL</td>
      <td>Agriculture</td>
      <td>Pigs</td>
      <td>6039975.0</td>
      <td>19985</td>
    </tr>
    <tr>
      <th>75852</th>
      <td>KHM</td>
      <td>Agriculture</td>
      <td>Farming</td>
      <td>7109100.0</td>
      <td>8545</td>
    </tr>
    <tr>
      <th>70962</th>
      <td>KEN</td>
      <td>Agriculture</td>
      <td>Farming</td>
      <td>10745600.0</td>
      <td>20555</td>
    </tr>
    <tr>
      <th>121670</th>
      <td>PHL</td>
      <td>Retail</td>
      <td>General Store</td>
      <td>15025550.0</td>
      <td>42960</td>
    </tr>
  </tbody>
</table>
<p>5743 rows × 5 columns</p>
</div>




```python
fig = px.choropleth(df_iso, locations="ISO_ALPHA3",
                    color="amount_projects", # lifeExp is a column of gapminder
                    #hover_name="country", # column to add to hover information
                    color_continuous_scale=px.colors.sequential.Plasma)
fig.show()
```


<div>                            <div id="ab7a2717-1da5-433b-9c18-742015489729" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("ab7a2717-1da5-433b-9c18-742015489729")) {                    Plotly.newPlot(                        "ab7a2717-1da5-433b-9c18-742015489729",                        [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ALB", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " ARM", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " AZE", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BDI", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BEN", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BFA", " BLZ", " BLZ", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BOL", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " BRA", " CHL", " CHL", " CHL", " CHL", " CHL", " CHL", " CHL", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CHN", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " CMR", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COG", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " COL", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " CRI", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " DOM", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " ECU", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " EGY", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GEO", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GHA", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GTM", " GUM", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HND", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " HTI", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IDN", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IND", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " IRQ", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " ISR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " JOR", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KEN", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KGZ", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " KHM", " LAO", " LAO", " LAO", " LAO", " LAO", " LAO", " LAO", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBN", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LBR", " LSO", " LSO", " LSO", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MDG", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MEX", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MLI", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MNG", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MOZ", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " MWI", " NAM", " NAM", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NGA", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NIC", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " NPL", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAK", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PAN", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PER", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PHL", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRI", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " PRY", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " RWA", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SEN", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLB", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLE", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SLV", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SOM", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SSD", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " SUR", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " TGO", " THA", " THA", " THA", " THA", " THA", " THA", " THA", " THA", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TJK", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TLS", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " TUR", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UGA", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " UKR", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " USA", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VCT", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VNM", " VUT", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " WSM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " YEM", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZAF", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZMB", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE", " ZWE"], "name": "", "type": "choropleth", "z": [176, 7, 1, 17, 130, 22, 177, 3, 6, 156, 11, 13, 1, 3, 1, 7, 15, 59, 6, 1, 1, 12, 12, 2, 2, 85, 8, 2, 3, 2, 1, 13, 7, 4, 3, 4, 3, 5, 25, 3, 9, 11, 20, 29, 211, 372, 6, 1, 11, 1, 2, 6, 2, 10, 7, 14, 26, 6, 24, 1, 1, 2, 1, 1, 6, 1, 2, 2, 1, 1, 1, 2, 16, 2, 8, 1, 7, 4, 1, 1, 1, 1, 5, 11, 17, 6, 5, 1, 1, 6, 8, 26, 1, 6, 600, 3, 26, 359, 96, 109, 2451, 12, 6, 743, 202, 22, 1, 20, 8, 1, 4, 14, 2, 5, 257, 1, 1, 4, 2, 116, 19, 2, 6, 733, 554, 13, 58, 6, 15, 27, 5, 1, 3, 1, 14, 19, 33, 7, 110, 48, 8, 6, 15, 22, 11, 695, 5, 45, 5, 46, 1, 16, 7, 9, 7, 29, 24, 45, 16, 32, 1, 20, 50, 8, 3, 2, 3, 7, 26, 2, 18, 10, 1, 1, 2, 2, 2, 3, 57, 21, 10, 10, 1, 1, 64, 5, 78, 1, 16, 17, 4, 11, 1, 3, 1, 1, 2, 10, 2, 3, 97, 18, 15, 4, 3, 1, 30, 1, 117, 67, 13, 2, 71, 63, 229, 74, 22, 504, 4, 139, 42, 1, 2, 1, 6, 29, 3, 2, 36, 6, 3, 30, 4, 4, 11, 21, 4, 4, 35, 9, 1, 120, 20, 6, 66, 1, 44, 1, 1, 14, 3, 3, 3, 4, 4, 1, 3, 1, 1, 3, 18, 7, 1, 4, 1, 2, 2, 1, 28, 3, 1, 1, 1, 20, 5, 4, 2, 4, 1, 4, 1, 26, 6, 6, 1, 1, 28, 93, 12, 1, 1, 6, 15, 5, 1, 5, 2, 2, 41, 43, 1, 13, 3, 1, 3, 4, 2, 15, 13, 3, 20, 8, 200, 22, 49, 3, 43, 10, 90, 2, 21, 1, 3, 1, 2, 4, 4, 1, 2, 9, 3, 1, 3, 2, 173, 5, 3, 1, 4, 1, 5, 2, 4, 2, 1, 2, 10, 3, 2, 13, 5, 130, 11, 33, 1, 8, 6, 1, 2, 2, 1, 10, 21, 14, 1, 5, 3, 1, 6, 3, 2, 2, 1, 156, 7, 2, 2, 2, 6, 10, 10, 244, 10, 1, 3, 70, 229, 13, 7, 4, 9, 1, 2, 33, 185, 100, 15, 3, 1, 1, 1, 2, 5, 4, 27, 2, 23, 3, 170, 15, 52, 22, 238, 25, 128, 13, 46, 6, 197, 1, 3, 2, 1, 22, 2, 2, 1, 2, 1, 1, 1, 21, 37, 35, 1, 2, 37, 10, 8, 52, 5, 8, 3, 18, 2, 2, 3, 1, 3, 133, 16, 8, 1, 50, 2, 1, 1, 2, 1, 1, 2, 2, 4, 33, 2, 3, 1, 70, 55, 794, 126, 2, 4, 334, 602, 50, 440, 32, 4, 98, 14, 10, 4, 26, 246, 14, 158, 6, 4, 120, 230, 186, 918, 310, 22, 10, 28, 16, 136, 158, 20, 2, 2, 18, 846, 16, 28, 8, 4, 428, 94, 138, 34, 70, 26, 328, 40, 182, 226, 320, 608, 1092, 346, 500, 34, 186, 4, 114, 14, 148, 14, 56, 292, 10, 822, 36, 6, 198, 2, 126, 58, 14, 6, 6, 110, 30, 2, 88, 2, 50, 222, 48, 28, 22, 2, 10, 398, 46, 256, 50, 40, 32, 18, 14, 10, 10, 20, 56, 14, 6, 26, 18, 484, 132, 4, 48, 4, 14, 90, 18, 142, 4, 14, 6, 4, 68, 4, 20, 40, 22, 2, 38, 8, 2, 6, 6, 10, 40, 20, 8, 428, 722, 236, 10, 20, 20, 32, 2, 10, 294, 322, 6, 1, 1, 19, 1, 1, 1, 7, 54, 1, 2, 3, 4, 1, 3, 1, 10, 2, 8, 4, 3, 6, 3, 2, 1, 1, 1, 18, 2, 3, 2, 7, 12, 2, 1, 2, 2, 3, 3, 1, 10, 2, 1, 39, 1, 1, 1, 1, 5, 15, 3, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 4, 3, 5, 1, 1, 1, 1, 18, 3, 3, 2, 3, 4, 1, 3, 1, 1, 2, 2, 4, 4, 4, 8, 1, 1, 5, 1, 3, 1, 2, 1, 7, 1, 3, 1, 6, 2, 2, 4, 3, 1, 6, 1, 1, 1, 5, 363, 3, 1, 1, 5, 275, 33, 31, 32, 1, 4, 2, 1, 1, 49, 44, 25, 15, 6, 2, 2, 1, 3, 10, 3, 3, 2, 7, 32, 1, 91, 94, 56, 55, 55, 55, 26, 1, 42, 100, 1, 1, 1, 1, 6, 7, 1, 1, 4, 4, 6, 12, 1, 1, 7, 16, 65, 13, 11, 5, 1, 1, 3, 4, 4, 13, 1, 1, 195, 12, 11, 5, 7, 83, 14, 2, 3, 1, 6, 1, 2, 4, 6, 1, 2, 2, 4, 1, 15, 44, 41, 19, 1, 2, 2, 9, 4, 1, 3, 2, 1, 1, 1, 1, 1, 3, 10, 1, 1, 3, 8, 5, 1, 8, 6, 1, 9, 10, 7, 11, 6, 2, 2, 1, 1, 2, 2, 2, 1, 1, 3, 1, 1, 2, 1, 1, 3, 2, 1, 2, 1, 1, 1104, 194, 14, 3, 355, 154, 90, 634, 70, 2, 143, 1305, 914, 5, 43, 401, 7, 3, 1, 6, 13, 117, 724, 5, 7, 62, 2, 101, 50, 1, 11, 1, 37, 767, 3, 27, 52, 12, 207, 1, 139, 142, 116, 80, 3, 14, 84, 70, 572, 41, 1501, 443, 375, 303, 77, 21, 129, 405, 10, 8, 2, 33, 46, 9, 53, 28, 131, 3, 132, 80, 2, 4, 11, 14, 7, 15, 121, 38, 16, 15, 17, 1455, 36, 157, 41, 30, 2, 20, 131, 7, 15, 664, 38, 8, 16, 19, 3, 2047, 209, 2, 29, 5, 6, 1, 3, 99, 98, 681, 14, 1, 5, 27, 3, 120, 2, 18, 14, 69, 27, 169, 85, 40, 79, 9, 23, 19, 45, 477, 1472, 154, 5, 13, 3, 46, 3, 74, 2, 19, 135, 14, 29, 366, 6, 116, 16, 21, 164, 5, 79, 95, 66, 2, 9, 1, 1, 6, 66, 1, 18, 2, 2, 9, 1, 14, 2, 2, 9, 1, 1, 3, 3, 1, 16, 14, 4, 12, 7, 2, 1, 58, 3, 8, 3, 3, 2, 3, 13, 1, 6, 12, 7, 25, 6, 8, 2, 1, 5, 45, 1, 73, 5, 3, 1, 1, 8, 22, 1, 1, 1, 1, 1, 2, 2, 1, 2, 24, 23, 5, 3, 11, 9, 2, 2, 1, 1, 1, 11, 52, 9, 2, 178, 2, 5, 3, 1, 3, 1, 20, 13, 17, 21, 4, 22, 2, 8, 1, 8, 7, 11, 4, 6, 1, 6, 1, 34, 7, 1, 16, 1, 4, 3, 9, 1055, 444, 350, 166, 95, 846, 5, 2, 518, 1010, 805, 4, 16, 135, 11, 21, 1, 1, 9, 11, 61, 1417, 6, 20, 22, 5, 24, 20, 9, 5, 7, 29, 8, 2, 5, 1, 78, 84, 53, 19, 58, 12, 9, 174, 78, 218, 41, 371, 276, 235, 567, 3, 21, 33, 182, 2, 10, 1, 35, 2, 10, 36, 8, 1, 66, 1, 88, 26, 8, 12, 7, 1, 3, 9, 32, 474, 14, 4, 6, 1, 11, 359, 13, 125, 40, 26, 6, 9, 15, 46, 7, 6, 82, 20, 8, 20, 8, 2, 1071, 124, 1, 24, 1, 10, 1, 71, 2, 153, 4, 1, 5, 9, 22, 13, 14, 6, 29, 7, 2, 4, 6, 6, 7, 10, 79, 258, 60, 5, 4, 1, 22, 24, 4, 69, 67, 1, 6, 5, 6, 313, 63, 10, 3, 101, 131, 4, 28, 11, 1, 9, 139, 1, 49, 4, 5, 8, 1, 1, 9, 2, 11, 15, 1, 2, 1, 10, 2, 4, 4, 56, 285, 11, 1, 4, 3, 2, 1, 5, 29, 3, 13, 19, 5, 2, 5, 3, 1, 70, 7, 3, 10, 7, 14, 1, 2, 1, 2, 3, 2, 2, 3, 2, 1, 1, 19, 75, 2, 1, 1, 1, 1, 6, 109, 15, 97, 174, 2, 85, 2, 18, 22, 43, 2, 2, 1, 23, 350, 47, 1, 1, 1, 5, 5, 6, 1, 7, 59, 1, 98, 5, 5, 31, 5, 3, 2, 33, 52, 41, 4, 159, 33, 2, 22, 7, 1, 1, 6, 5, 75, 11, 11, 8, 2, 5, 3, 1, 3, 68, 62, 17, 1, 1, 1, 4, 2, 1, 2, 75, 6, 1, 6, 1, 1, 23, 32, 2, 1, 1, 1, 8, 1, 1, 2, 4, 1, 1, 62, 11, 9, 4, 1, 2, 1, 191, 88, 88, 1, 92, 544, 8, 1, 5, 2, 118, 1, 1, 2, 7, 67, 252, 111, 16, 1, 2, 3, 9, 8, 1, 2, 2, 6, 93, 7, 2, 32, 114, 5, 1, 14, 58, 203, 2, 120, 173, 324, 69, 90, 251, 4, 8, 17, 11, 11, 11, 19, 2, 1, 28, 8, 2, 14, 58, 1, 15, 41, 62, 90, 1, 25, 15, 13, 124, 27, 58, 33, 6, 5, 1, 3, 31, 10, 20, 37, 1, 22, 284, 46, 26, 3, 6, 1, 45, 2, 1, 1, 1, 11, 3, 4, 21, 31, 31, 2, 9, 94, 5, 1, 2, 1, 573, 140, 3, 9, 2, 59, 589, 22, 7, 30, 53, 84, 22, 162, 88, 1, 18, 235, 308, 158, 873, 62, 2, 2, 4, 1, 40, 60, 1, 1, 5, 10, 8, 4, 62, 15, 21, 8, 10, 5, 4, 6, 4, 146, 18, 260, 220, 207, 353, 4, 18, 9, 16, 5, 26, 575, 198, 6, 22, 27, 1, 5, 1, 13, 4, 79, 9, 46, 32, 141, 4, 1, 49, 340, 12, 45, 20, 3, 3, 3, 4, 1, 18, 9, 5, 1, 1, 1, 171, 80, 3, 1, 2, 5, 7, 34, 3, 4, 2, 1, 11, 1, 1, 4, 1, 44, 75, 72, 1, 4, 5, 1, 6, 1, 15, 21, 1, 1, 1, 1760, 19, 34, 11, 105, 654, 18, 86, 21, 4, 25, 4, 2, 33, 311, 113, 2, 7, 6, 1, 21, 14, 2, 1, 98, 1, 33, 9, 7, 5, 5, 9, 11, 75, 31, 43, 166, 496, 60, 71, 822, 5, 1, 3, 8, 6, 1, 7, 62, 4, 16, 9, 9, 499, 6, 2, 1, 5, 43, 1, 1, 33, 122, 12, 21, 1, 3, 2, 5, 14, 6, 14, 7, 3, 158, 20, 1, 8, 1, 21, 14, 17, 4, 7, 6, 1, 2, 2, 7, 8, 6, 1, 24, 40, 20, 1, 4, 3, 3, 3, 9, 1, 22, 13, 2, 15, 30, 2, 8, 34, 1, 13, 15, 2, 28, 2, 18, 113, 128, 36, 2, 1, 1, 43, 1, 4, 20, 91, 7, 1, 1, 31, 2, 104, 174, 240, 126, 38, 467, 20, 1, 29, 2, 21, 10, 16, 1, 413, 6, 5, 11, 178, 2, 16, 2, 65, 13, 98, 1, 5, 4, 1, 1, 20, 6, 1, 6, 2, 89, 539, 88, 21, 2, 1, 1, 14, 2, 1, 2, 1, 1, 1, 4, 2, 6, 1, 19, 19, 1, 1, 6, 1, 1, 6, 32, 65, 4, 1, 12, 158, 3, 7, 500, 20, 6, 100, 1, 2, 62, 12, 2, 6, 2, 5, 6, 1, 89, 100, 2, 30, 1, 2, 2, 6, 40, 8, 9, 6, 142, 41, 93, 5, 1, 1973, 2, 1, 2, 2, 1, 26, 35, 306, 2, 1, 3, 2, 1, 44, 214, 212, 21, 7, 744, 2, 2, 64, 38, 2, 1, 5, 1, 7, 1, 1, 5, 1, 4, 875, 16, 5, 7, 2, 1, 13, 9, 1, 67, 147, 26, 1238, 23, 1631, 7, 181, 8, 88, 37, 179, 113, 3, 4, 7, 16, 432, 28, 491, 5, 10, 5, 5, 43, 35, 6, 27, 2, 298, 260, 2, 3, 16, 11, 7, 18, 7, 46, 1, 80, 13, 60, 13, 394, 219, 333, 689, 4, 11, 22, 11, 1, 10, 3, 18, 1294, 1, 13, 49, 1, 216, 10, 223, 92, 5, 1, 11, 44, 28, 15, 1, 6, 41, 151, 5, 19, 153, 5, 11, 1, 4, 18, 1, 8, 8, 10, 8, 116, 18, 2, 5, 8, 6, 42, 16, 2, 4, 3, 8, 2, 18, 1, 3, 1, 7, 8, 4, 71, 140, 699, 1, 7, 2, 5, 65, 3, 99, 2, 2, 6, 1, 1, 1, 1, 20, 2, 5, 4, 8, 35, 225, 1, 2, 10, 2, 3, 4, 4, 3, 2, 6, 65, 6, 1, 1, 1, 6, 6, 4, 1, 2, 2, 1, 10, 5, 7, 3, 2, 9, 14, 1, 1, 4, 4, 12, 24, 25, 3, 4, 1, 2, 1, 1, 2, 3, 5, 381, 22, 1, 3, 1, 2, 1, 1, 3, 29, 1, 5, 6, 4, 1, 6, 1, 3, 2, 8, 2, 2, 1, 4, 1, 5, 1, 1, 1, 2, 1, 2, 3, 1, 3, 6, 3, 6, 40, 10, 1, 2, 10, 3, 4, 1, 1, 6, 30, 9, 17, 2, 17, 3, 65, 24, 17, 44, 28, 3, 1, 19, 300, 10, 1, 3, 5, 8, 3, 3, 3, 2019, 3, 3, 1, 19, 8, 38, 5, 6, 1, 13, 107, 57, 12, 19, 179, 2, 28, 2, 1, 1, 1, 13, 12, 1, 15, 3, 1, 1, 1, 4, 5, 54, 14, 8, 5, 8, 37, 1, 209, 6, 11, 1, 3, 3, 4, 9, 1, 175, 12, 4, 3, 1, 25, 30, 88, 1, 6, 1, 1, 2, 7, 3, 6, 3, 1, 10, 6, 1, 16, 104, 55, 1, 1, 2, 2, 11, 8, 1, 2, 2, 5244, 338, 1, 1, 156, 3983, 324, 20555, 8, 6, 548, 83, 2340, 57, 11, 200, 6, 1, 14, 3, 112, 1766, 2372, 559, 56, 109, 224, 3, 175, 91, 85, 110, 116, 445, 768, 17, 12, 3, 84, 145, 416, 221, 14, 2647, 788, 9, 1091, 454, 804, 1272, 2989, 2578, 16, 168, 39, 337, 486, 28, 14, 5, 98, 116, 170, 30, 110, 2, 53, 100, 2, 4, 1, 254, 1716, 145, 8, 28, 924, 43, 721, 5, 46, 91, 280, 3830, 114, 238, 14, 21, 17, 4, 25, 7, 25, 51, 13, 59, 28, 186, 2918, 293, 1, 196, 2, 5, 50, 146, 938, 13, 1, 6, 6, 4, 49, 2, 33, 10, 27, 172, 79, 35, 11, 7, 13, 30, 3, 66, 43, 7, 3, 1946, 41, 1280, 1, 2, 47, 276, 113, 1443, 5, 70, 325, 6, 15, 285, 47, 2, 237, 120, 30, 870, 2, 3222, 1, 3, 1, 1, 2, 101, 1, 1, 2, 1, 335, 6, 1, 8, 1, 51, 21, 5, 26, 22, 1, 205, 1, 1, 3, 3, 1, 9, 3, 1, 5, 2, 22, 4, 7, 3, 1, 2, 1, 32, 7, 1, 2, 12, 3, 1, 4, 28, 2, 2, 412, 142, 9, 334, 1, 297, 8545, 3, 41, 76, 606, 92, 1, 9, 23, 2, 24, 190, 5, 35, 1, 2, 2, 2, 84, 71, 1, 17, 9, 1360, 21, 5, 2, 10, 87, 3, 9, 1, 21, 79, 182, 107, 10, 106, 36, 143, 386, 5, 2, 2, 30, 1, 8, 2, 3, 2073, 36, 15, 13, 11, 3, 15, 7, 16362, 966, 114, 736, 41, 1, 4, 5, 10, 1, 1, 1, 13, 127, 24, 3, 1, 1, 1, 31, 81, 4, 3, 3, 4, 5, 5, 24, 2, 3, 3, 5, 4, 26, 2, 3, 1, 8, 44, 16, 52, 1, 7, 10, 5, 165, 10, 27, 33, 3, 4, 18, 1, 1459, 1, 2, 3, 2, 80, 16, 13, 13, 7, 76, 8, 12, 13, 7, 20, 39, 41, 16, 3, 1, 9, 5, 39, 588, 20, 5, 3, 10, 16, 131, 21, 1, 3, 172, 557, 1876, 16, 6, 6, 183, 2, 15, 101, 12, 4, 2, 7, 10, 40, 21, 289, 4, 85, 360, 1, 4, 25, 102, 15, 5, 2, 410, 3, 308, 15, 26, 39, 29, 27, 22, 3, 57, 7, 234, 114, 61, 6, 6, 6, 163, 6, 1, 9, 3, 4, 38, 22, 100, 33, 76, 6, 1, 2, 1, 1, 34, 18, 14, 10, 13, 7, 203, 32, 18, 15, 2, 1, 10, 73, 62, 518, 1, 6, 5, 4, 8, 29, 10, 1, 17, 6, 2, 29, 4, 25, 6, 3, 152, 55, 242, 1, 2, 3, 4, 19, 3, 59, 82, 3, 4, 1, 4, 12, 120, 234, 6, 4, 1, 5, 2, 13, 3, 6, 1, 17, 72, 1, 7, 234, 114, 1083, 101, 121, 40, 49, 18, 1, 114, 26, 3, 3, 2, 2, 3, 1, 188, 4, 43, 2, 4, 19, 138, 27, 52, 4, 2, 1, 41, 1, 3, 1, 123, 514, 47, 1, 2, 2, 25, 1, 1, 2, 1, 3, 1, 1, 3, 1, 55, 366, 1, 186, 12, 60, 23, 2, 38, 3, 130, 479, 167, 1, 8, 34, 8, 2, 1, 18, 14, 23, 85, 277, 81, 27, 15, 7, 6, 9, 3, 88, 1, 40, 9, 41, 2, 49, 22, 111, 118, 182, 254, 2, 25, 6, 1, 1, 13, 43, 1, 1, 49, 16, 7, 1, 2, 17, 203, 76, 70, 7, 29, 4, 3, 15, 1, 3, 11, 25, 6, 25, 2, 1, 253, 74, 2, 15, 6, 2, 37, 1, 14, 1, 3, 3, 15, 60, 4, 1, 1, 18, 3, 2, 4, 390, 165, 28, 24, 6, 113, 10, 46, 40, 19, 7, 60, 26, 2, 1, 3, 17, 41, 130, 1, 5, 15, 103, 2, 2, 9, 33, 2, 3, 1, 4, 45, 7, 11, 7, 13, 1, 5, 17, 12, 138, 7, 150, 69, 35, 91, 1, 24, 5, 52, 5, 16, 1495, 8, 500, 55, 9, 26, 2, 40, 3, 2, 14, 459, 9, 17, 5, 8, 45, 11, 2, 3, 218, 10, 58, 26, 1, 13, 35, 7, 4, 16, 4, 1, 17, 6, 3, 2, 176, 63, 6, 5, 1, 2, 16, 54, 1, 1, 2, 6, 1, 3, 3, 21, 7, 2, 3, 1, 3, 11, 2, 47, 19, 11, 3, 6, 15, 1, 4, 2, 59, 26, 1, 4, 59, 676, 46, 14, 417, 1642, 1, 670, 1, 11, 2, 21, 28, 5, 3, 353, 71, 81, 1, 2, 1, 2, 6, 34, 4, 6, 371, 67, 17, 37, 768, 3, 181, 13, 6, 2, 185, 1, 7, 4, 5, 1, 7, 1, 1, 42, 54, 63, 1, 53, 15, 1, 35, 5, 1, 1, 8, 3, 14, 418, 55, 12, 1, 1, 2, 6, 7, 2, 1, 1, 1, 1, 1, 7, 1, 7, 16, 1, 1, 6, 4, 56, 3, 1, 1, 8, 3, 2, 7, 74, 3, 3, 16, 13, 1, 2, 2, 4, 1, 14, 25, 1, 2, 1, 1, 1, 1, 374, 5, 2, 19, 6, 2, 6, 9, 4, 19, 19, 1, 3, 1, 1, 3, 3, 3, 1, 1, 17, 8, 2, 3, 8, 4, 3, 2, 12, 2, 7, 6, 9, 14, 4, 1, 1, 1, 55, 33, 1, 62, 2, 7, 39, 54, 1, 1, 180, 1, 2, 49, 61, 5, 9, 5, 2, 27, 25, 1, 22, 2, 3, 7, 5, 1, 2, 8, 5, 12, 8, 23, 16, 8, 63, 6, 3, 1, 1, 1, 1, 1573, 8, 1, 2, 2, 74, 40, 2, 7, 27, 6, 1, 1, 3, 1, 1, 3, 2, 462, 14, 20, 2, 4, 1, 1, 307, 5, 1, 3, 2, 39, 2, 2, 1, 1, 1, 3, 86, 2, 8, 7, 3, 2, 2, 15, 1, 1, 2, 2, 1, 1, 4, 192, 3, 41, 14, 1, 4, 34, 72, 8, 1, 1, 1, 1, 1, 25, 6, 8, 6, 69, 68, 14, 20, 25, 19, 78, 131, 2, 22, 1, 2, 1, 2, 1, 1, 1, 2, 2, 13, 8, 9, 4, 1, 2, 8, 1, 1, 5, 113, 261, 3, 1, 1, 7, 75, 32, 5889, 1, 1, 1063, 2, 83, 21, 2962, 1, 3, 3, 569, 8, 66, 13, 59, 290, 11, 14, 162, 16, 7, 60, 5, 6, 51, 792, 238, 6, 4, 6, 1, 21, 144, 2, 7, 1, 10, 933, 73, 8, 8, 1, 97, 59, 52, 33, 47, 7, 12, 96, 22, 134, 87, 732, 169, 362, 735, 22, 2, 11, 7, 11, 7, 127, 16, 1682, 28, 46, 38, 14, 7, 1, 36, 629, 107, 15, 1, 29, 2, 25, 206, 4, 4, 1, 2, 20, 488, 24, 113, 84, 12, 1, 8, 6, 4, 9, 14, 48, 15, 3, 36, 3, 909, 100, 10, 6, 1, 2, 1, 46, 11, 65, 12, 49, 2, 4, 8, 6, 11, 3, 9, 3, 6, 4, 66, 75, 51, 2, 11, 4, 16, 2, 12, 5, 24, 32, 5, 4, 35, 4, 20, 13, 1, 132, 12, 1, 37, 7, 12, 4, 7, 1, 2, 1, 9, 1, 1, 1, 21, 9, 10, 3, 12, 238, 32, 7, 4, 1, 12, 1, 4, 1, 2, 4, 1, 1, 1, 2, 1, 7, 1, 32, 5, 3, 1, 87, 1677, 367, 599, 11, 206, 23, 1, 296, 50, 74, 80, 1823, 2, 33, 1, 6, 31, 133, 74, 1351, 28, 1, 1, 27, 4, 187, 51, 1, 3, 3, 1, 1216, 6, 6, 16, 5, 40, 9, 124, 22, 20, 13, 1, 14, 105, 4, 90, 230, 807, 244, 1131, 156, 3, 39, 10, 38, 1, 6, 142, 167, 131, 512, 77, 1, 11, 16, 8, 5, 24, 1, 864, 88, 46, 62, 37, 1, 30, 1675, 35, 201, 153, 105, 4, 6, 9, 1, 1, 20, 23, 10, 146, 3, 2, 440, 94, 109, 49, 4, 5, 120, 124, 817, 16, 11, 3, 1, 59, 12, 83, 11, 14, 1, 19, 3, 31, 4, 14, 39, 68, 1, 412, 778, 3723, 1, 4, 2, 53, 1, 224, 2134, 585, 466, 45, 72, 13, 15, 1, 12, 1, 4, 14, 13, 3, 2, 1, 4, 1, 2, 1, 2, 3, 2, 1, 1, 3, 3, 4, 31, 2, 1, 1, 6, 2, 16, 2, 1, 1, 5, 2, 1, 2, 1, 1, 2, 1, 2, 1, 1, 5, 2253, 2054, 3, 363, 77, 158, 1331, 13, 8, 147, 79, 92, 11, 29, 466, 19, 63, 9, 43, 70, 52, 846, 15, 45, 69, 5, 170, 154, 2, 15, 2, 165, 924, 84, 10, 9, 19, 214, 99, 53, 51, 123, 207, 10, 40, 11, 371, 67, 870, 290, 524, 1299, 84, 32, 39, 668, 48, 11, 17, 64, 13, 58, 517, 34, 2, 87, 1, 47, 39, 228, 1, 408, 3, 307, 35, 69, 1, 34, 363, 15, 12, 22, 4, 31, 644, 105, 54, 50, 18, 5, 8, 25, 2, 8, 33, 46, 8, 8, 19, 2, 3, 3, 1299, 91, 14, 150, 1, 58, 2, 95, 3, 5, 2, 18, 30, 7, 14, 1, 61, 52, 12, 14, 11, 4, 10, 64, 7, 6, 356, 155, 47, 115, 6, 8, 17, 1, 220, 100, 286, 432, 1, 20, 1298, 164, 40, 274, 1, 281, 11924, 479, 7, 1718, 19985, 544, 61, 145, 546, 4, 17, 1, 2, 42, 1233, 73, 2105, 515, 68, 33, 9, 16, 209, 360, 11, 158, 2, 131, 1563, 78, 76, 30, 33, 443, 60, 277, 232, 79, 53, 2891, 12, 9060, 8682, 2959, 562, 8675, 1499, 4486, 1313, 635, 7, 2, 773, 10, 38, 246, 281, 56, 6591, 194, 39, 532, 4, 796, 73, 15, 6, 364, 497, 1022, 34, 5, 3, 13, 1678, 13, 191, 22, 10, 50, 9, 795, 42960, 70, 503, 213, 37, 14, 1, 56, 11, 15, 8, 2125, 39, 107, 58, 400, 22, 1, 3873, 115, 33, 124, 1, 11, 7, 109, 61, 415, 7, 2, 7, 3, 10, 130, 1, 73, 3, 505, 3, 33, 27, 1, 80, 12, 18, 89, 125, 1, 924, 340, 1175, 7, 36, 2, 94, 2, 177, 3002, 261, 44, 1261, 38, 36, 13, 4, 6, 3, 4, 10, 1, 7, 1, 1, 2, 5, 1, 2, 1, 7, 83, 199, 9, 24, 15, 46, 13, 27, 24, 25, 3, 18, 111, 30, 4, 3, 3, 581, 490, 63, 49, 27, 2, 40, 15, 1, 1, 1, 1, 3819, 1, 69, 34, 33, 2, 4, 1, 11, 27, 651, 64, 608, 107, 388, 259, 62, 63, 20, 1, 1, 132, 9, 17, 6, 6, 3, 6, 2, 1, 2, 1, 21, 39, 3, 124, 10, 4, 5, 2, 811, 7, 66, 94, 7, 2, 73, 7, 3, 1, 38, 914, 55, 11, 23, 28, 374, 1, 2, 5, 4, 2, 1, 28, 11, 2, 3, 3, 1, 1, 184, 490, 45, 5, 17, 27, 29, 2, 228, 63, 61, 24, 19, 976, 1, 33, 2, 3, 6, 6, 14, 2, 28, 424, 46, 63, 11, 5, 1, 6, 51, 3, 11, 3, 350, 153, 3, 7, 135, 43, 6, 2, 117, 29, 1, 276, 736, 142, 59, 405, 111, 30, 12, 87, 71, 1, 7, 4, 1, 10, 1, 2, 5, 1, 16, 5, 1, 6, 87, 2, 5, 5, 5, 8, 590, 7, 36, 1, 8, 1, 3, 1, 3, 4, 8, 1, 1, 5, 500, 38, 1, 37, 3, 1, 27, 1, 1, 2, 2, 1, 2, 3, 1, 2, 1, 3, 257, 4, 77, 1, 1, 25, 5, 1, 26, 2, 3, 1, 159, 72, 30, 2, 4, 43, 224, 21, 88, 2, 4, 2, 8, 71, 55, 15, 1, 4, 1, 2, 2, 6, 1, 3, 2, 8, 9, 4, 2, 2, 80, 133, 9, 65, 97, 479, 27, 177, 35, 2, 5, 65, 2, 1, 2, 7, 5, 4, 3, 25, 2, 1, 2, 1, 10, 3, 5, 74, 34, 1, 2, 5, 4, 1, 27, 1, 5, 2, 5, 4, 5, 1, 840, 23, 8, 4, 1, 13, 1, 1, 1, 2, 3, 4, 27, 15, 4, 1, 11, 11, 30, 1, 3, 6, 3, 57, 38, 44, 6, 3, 5, 10, 13, 1, 2, 28, 1, 16, 1, 3, 28, 17, 8, 7, 4, 2, 18, 7, 1, 1, 1, 5, 182, 1, 8, 1, 1, 2, 11, 7, 4, 1, 5, 1, 1, 1, 2, 40, 2, 1, 6, 1, 14, 403, 175, 15, 1, 14, 6, 54, 1, 12, 6, 11, 24, 3, 1, 17, 168, 3, 41, 174, 1, 157, 1267, 243, 136, 81, 107, 11, 7, 8, 37, 8, 3, 5, 2, 116, 36, 1, 1, 10, 7, 3, 1, 3, 16, 1, 172, 47, 113, 8, 22, 26, 65, 548, 14, 118, 14, 6, 8, 1, 13, 3, 4, 48, 18, 6, 51, 3, 1, 1, 329, 147, 41, 8, 3, 1, 39, 1, 2, 1, 2, 1, 2, 1, 11, 1, 27, 1, 4, 6, 4, 2, 4554, 110, 5, 2, 1419, 220, 1328, 2939, 33, 154, 1464, 862, 919, 48, 379, 11, 11, 2, 11, 92, 67, 1763, 73, 3, 18, 60, 6, 60, 40, 3, 7, 1, 4, 64, 43, 8, 5, 6, 606, 170, 110, 131, 16, 26, 64, 298, 366, 270, 146, 4318, 388, 992, 330, 33, 1, 51, 4, 27, 7, 107, 78, 57, 5925, 52, 20, 186, 4, 68, 219, 4, 1, 31, 51, 168, 81, 2, 4, 50, 2, 31, 251, 47, 5, 9, 4, 40, 3061, 17, 83, 116, 27, 10, 13, 8, 22, 24, 243, 29, 2, 23, 23, 3, 1333, 159, 23, 1, 7, 2, 174, 65, 183, 41, 31, 17, 30, 47, 38, 3, 27, 14, 38, 13, 2, 312, 181, 279, 7, 68, 62, 2, 1, 87, 3, 34, 239, 2, 1, 1, 1, 3, 2, 3, 2, 13, 1, 1, 3, 1, 1, 1, 1, 4, 1, 2, 1, 1, 1, 3, 3, 1, 20, 1, 2, 1, 5, 8, 2, 3, 2, 2, 1, 1, 2, 4, 2, 1, 2, 1, 1, 6, 3, 5, 4, 4, 1, 1, 4, 1, 2, 1, 4, 12, 1, 2, 1, 22, 32, 1, 3, 1, 1, 1, 1, 1, 6, 2, 48, 7, 127, 1, 1, 11, 2, 1, 7, 1, 1, 2, 3, 2, 1, 2, 1, 2, 1, 2, 10, 9, 2, 3, 3, 1, 13, 14, 11, 49, 100, 255, 7, 7, 4, 15, 26, 8, 6, 12, 226, 7, 65, 1, 437, 218, 922, 111, 168, 146, 75, 2, 8, 59, 1, 10, 1, 10, 1, 6, 6, 3, 1, 4, 416, 294, 120, 2, 6, 7, 27, 42, 7, 162, 64, 3, 2, 2, 19, 2, 105, 3, 2, 57, 2, 2, 912, 164, 9, 9, 1, 5, 108, 6, 2, 4, 3, 2, 2, 5, 2, 21, 71, 21, 1, 6, 2, 2, 1, 1, 33, 63, 1, 36, 2, 42, 1, 2, 1200, 1600, 2, 4, 1351, 96, 165, 1260, 3, 12, 755, 11, 1, 6, 15, 2, 6, 11, 4, 6, 32, 823, 1, 2, 6, 4, 56, 55, 1, 37, 2366, 59, 5, 2, 2, 184, 6, 10, 68, 13, 6, 2, 56, 307, 189, 77, 386, 260, 13, 10, 45, 171, 74, 1959, 27, 1232, 5, 4, 19, 50, 6, 2, 98, 3, 142, 62, 390, 32, 220, 1, 1, 4, 120, 16, 2, 3, 5, 1, 4, 30, 6, 60, 22, 10, 9, 10, 9, 1, 22, 5, 7, 3, 176, 98, 3, 10, 50, 6, 55, 1, 1, 2, 2, 2, 1, 10, 4, 81, 2101, 207, 1, 3, 7, 222, 95, 1, 1, 25, 84, 7, 3, 70, 3, 4, 21, 14, 4, 3, 10, 28, 100, 104, 5, 28, 4, 5, 4, 2, 146, 95, 2, 2, 5, 11, 34, 8, 16, 25, 38, 57, 11, 1, 13, 1, 73, 1, 1, 1, 2, 1, 1, 4, 1, 76, 1178, 7, 7, 1, 1, 1, 3, 244, 20, 1, 1, 3, 5, 1, 10, 15, 6, 6, 14, 1, 7, 2, 2, 2, 2, 1, 1, 5, 8, 206, 121, 39, 87, 5, 2, 6, 140, 4, 1, 6, 33, 1, 25, 19, 152, 2, 2, 2, 2, 1, 10, 3, 5, 4, 1, 3, 2, 106, 18, 4, 8, 92, 26, 1, 1, 44, 5, 15, 223, 16, 4, 72, 6, 4, 2, 23, 25, 104, 1, 429, 166, 1, 624, 95, 92, 2487, 2, 9, 231, 85, 366, 32, 4, 35, 2, 7, 4, 5, 3, 148, 682, 656, 124, 68, 19, 5, 51, 40, 15, 63, 1144, 118, 649, 21, 5, 2, 50, 131, 146, 9, 30, 548, 190, 28, 570, 301, 660, 343, 648, 193, 195, 42, 417, 417, 3, 19, 51, 12, 3, 148, 781, 207, 7, 53, 27, 36, 5, 10, 257, 29, 17, 5, 11, 386, 26, 70, 12, 13, 82, 60, 1901, 92, 127, 4, 26, 12, 3, 39, 15, 3, 6, 40, 2, 7, 7, 1, 15, 671, 107, 1, 213, 6, 26, 446, 2, 1, 6, 4, 7, 34, 7, 6, 6, 21, 1, 18, 9, 3, 29, 126, 8, 195, 1, 1, 2, 13, 5, 5, 388, 89, 35, 30, 42, 14, 1, 1, 3, 150, 12, 1, 2, 3, 188, 9, 1, 4, 1, 2, 12, 2, 6, 6, 4, 9, 10, 1, 1, 291, 3, 3, 23, 18, 11, 3, 1, 3, 3, 6, 4, 2, 2, 1, 11, 5, 2, 2, 4, 52, 44, 1, 1, 1, 5, 1, 1, 12, 1, 2, 1, 398, 1, 2, 2, 73, 209, 4, 1, 372, 47, 1, 5, 90, 177, 1, 174, 1, 8, 10, 7, 7, 2, 8, 3, 709, 8, 2, 30, 1, 414, 71, 2, 2, 13, 33, 24, 13, 2, 13, 62, 2, 1, 1, 2, 299, 4, 2, 23, 364, 4, 25, 4, 3, 2, 11, 1, 1, 173, 2, 1, 3, 2, 14, 4, 45, 75, 115, 86, 18, 2, 5, 15, 27, 1, 3, 1, 9, 10, 2, 3, 1323, 1, 4, 172, 8, 1, 1, 37, 2, 24, 1, 13, 118, 11, 10, 3, 3, 3, 3, 3, 6, 3, 3, 6, 3, 3, 3, 6, 320, 84, 60, 428, 12, 42, 856, 14, 602, 1362, 1448, 4, 10, 2, 2, 44, 238, 4, 2, 18, 26, 2, 16, 72, 4, 2, 2, 174, 234, 2280, 10, 2, 28, 208, 32, 86, 56, 758, 276, 208, 20, 290, 582, 362, 262, 10, 2, 2, 98, 70, 4, 2, 28, 52, 7358, 24, 2, 106, 10, 138, 60, 2, 4, 58, 40, 42, 18, 4, 12, 14, 12, 10, 30, 22, 2, 22, 842, 4, 32, 2, 12, 2, 12, 8, 8, 4, 4, 34, 10, 204, 44, 4, 2, 10, 24, 116, 6, 4, 2, 12, 4, 8, 24, 2, 2, 38, 2, 8, 8, 8, 92, 104, 136, 8, 2, 2, 26, 4, 4, 4, 719, 1, 17, 34, 1682, 11, 2, 11, 2, 1, 130, 85, 438, 3, 12, 44, 6, 3, 35, 6, 43, 398, 8, 26, 9, 180, 161, 13, 1614, 216, 149, 6, 2, 6, 6, 1, 1, 1, 31, 3, 1, 616, 1, 24, 52, 1, 3, 1, 1, 3, 2, 3, 69, 189, 105, 3, 5, 195, 5, 1, 1, 112, 1, 4, 2, 38, 2, 2, 5, 195, 11, 91, 2, 1, 1, 7, 5, 2, 6, 1, 14, 2, 55, 28, 16, 13, 127, 4, 4, 10, 375, 7, 8, 5, 1, 45, 266, 230, 87, 4, 2, 2, 65, 1, 5, 5, 1, 3, 2, 10, 12, 1, 22, 22, 17, 11, 2, 71, 2, 3, 4, 1, 8, 18, 2, 1, 2, 3, 1, 2, 2, 1, 1, 1, 28, 17, 62, 1, 27, 2, 46, 33, 1, 2, 1, 5, 3, 11, 2, 1, 1, 2, 2, 2, 4, 1, 23, 177, 1, 9, 3, 2, 5, 7, 1, 4, 1, 4, 6, 2, 7, 2, 3, 1, 3, 1, 10, 1, 1, 3, 4, 1, 1, 1, 3, 5, 2, 1, 3, 1, 1, 1, 8, 9, 3, 3, 1, 5, 10, 1, 66, 1, 1, 43, 1, 1, 42, 7, 9, 2, 7, 2, 35, 1, 8, 6, 10, 5, 1, 44, 2, 3, 1, 1, 3, 3, 1, 2, 2, 1, 76, 383, 14, 40, 4, 1, 1, 9, 16, 108, 1, 41, 8, 849, 5, 7, 3, 1, 72, 971, 50, 2, 10, 1, 3, 5, 1, 28, 1, 1, 20, 4, 14, 7, 7, 10, 45, 1, 24, 33, 47, 65, 71, 344, 4, 1, 1, 11, 1, 1, 1, 2, 16, 2, 33, 8, 1, 1, 3, 1, 18, 16, 2, 18, 5, 10, 350, 42, 71, 4, 9, 3, 1, 1, 15, 4, 4, 2, 6, 1, 4, 148, 11, 10, 3, 3, 3, 2, 65, 1, 2, 3, 1, 1, 1, 3, 6, 1, 24, 49, 64, 4, 3, 6, 8, 8]}],                        {"coloraxis": {"colorbar": {"title": {"text": "amount_projects"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}}, "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('ab7a2717-1da5-433b-9c18-742015489729');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.choropleth(df_iso, locations="ISO_ALPHA3", color="amount_projects", animation_frame="sector")
fig.show()
```


<div>                            <div id="ecc77a9c-640e-4608-8392-11ae8a19b09b" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("ecc77a9c-640e-4608-8392-11ae8a19b09b")) {                    Plotly.newPlot(                        "ecc77a9c-640e-4608-8392-11ae8a19b09b",                        [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Retail<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" MEX", " BFA", " KEN", " PHL", " IDN", " SEN", " CMR", " HND", " SSD", " SLE", " SSD", " NPL", " IND", " SEN", " MDG", " IND", " LBR", " MDG", " TUR", " NPL", " NIC", " TGO", " LBR", " ZMB", " MLI", " CMR", " TGO", " CMR", " UGA", " KHM", " SLV", " MEX", " EGY", " NIC", " LBR", " MOZ", " ECU", " HTI", " PHL", " MOZ", " AZE", " YEM", " HND", " GUM", " WSM", " UGA", " TGO", " KHM", " SSD", " PAK", " GTM", " KHM", " TGO", " GHA", " SEN", " BEN", " MLI", " KEN", " ALB", " ZAF", " HTI", " SLB", " NGA", " NPL", " SEN", " NPL", " TJK", " MDG", " NPL", " IND", " PAK", " LBN", " RWA", " MDG", " USA", " PAK", " ZWE", " PAK", " PAN", " CMR", " AZE", " CMR", " HND", " CMR", " TJK", " GEO", " MOZ", " COL", " BEN", " HTI", " TLS", " BEN", " ZAF", " MDG", " IDN", " PHL", " TLS", " MOZ", " BFA", " MOZ", " IND", " TUR", " IDN", " GHA", " GEO", " MLI", " PAN", " ARM", " LBN", " MOZ", " LBN", " MEX", " BFA", " HND", " RWA", " MEX", " GEO", " NPL", " ALB", " YEM", " PAN", " TGO", " SEN", " KGZ", " BFA", " HTI", " MDG", " NIC", " SEN", " UKR", " IDN", " ZWE", " GEO", " TLS", " TLS", " TGO", " TLS", " HTI", " MNG", " KGZ", " SLB", " LBR", " MDG", " BFA", " TUR", " WSM", " ALB", " NPL", " GTM", " SLV", " PER", " ALB", " MOZ", " MOZ", " MOZ", " MDG", " CRI", " BEN", " SLE", " SSD", " YEM", " UGA", " ALB", " TLS", " ALB", " KHM", " PAN", " MLI", " EGY", " MEX", " TJK", " JOR", " ZWE", " ZAF", " COL", " ECU", " MLI", " MOZ", " MNG", " LBR", " HND", " KHM", " ZWE", " MWI", " IND", " AZE", " GHA", " GEO", " HND", " MOZ", " PRY", " GEO", " JOR", " KHM", " SLV", " ZMB", " DOM", " PER", " SUR", " TJK", " HTI", " GEO", " GEO", " TGO", " MWI", " PAK", " UGA", " COL", " KEN", " ZWE", " IND", " MDG", " CMR", " ALB", " PAK", " CMR", " KEN", " MWI", " NPL", " ZAF", " ZMB", " MNG", " MWI", " MDG", " AZE", " AZE", " CRI", " YEM", " MDG", " KHM", " ECU", " KHM", " PAN", " VNM", " ARM", " ARM", " ARM", " JOR", " PHL", " SLE", " TGO", " ALB", " IND", " TGO", " ZWE", " WSM", " ZAF", " HND", " ZMB", " VNM", " YEM", " USA", " GHA", " YEM", " ARM", " MWI", " AZE", " SLE", " IDN", " SLV", " ZWE", " LBR", " MOZ", " NPL", " JOR", " ZAF", " ZWE", " ZMB", " HTI", " YEM", " IRQ", " HTI", " MEX", " LBR", " KGZ", " ZAF", " EGY", " CRI", " PRY", " HTI", " EGY", " MOZ", " IND", " GTM", " TUR", " CRI", " KEN", " HTI", " IDN", " LBR", " PAK", " KGZ", " ALB", " TGO", " IRQ", " LBN", " BEN", " CMR", " AZE", " BOL", " ARM", " EGY", " CMR", " NIC", " TJK", " TJK", " TUR", " BFA", " DOM", " PER", " ALB", " TJK", " SLV", " JOR", " PAK", " TGO", " HND", " COL", " ARM", " ECU", " AZE", " MDG", " GHA", " MEX", " MDG", " GTM", " BDI", " TUR", " BFA", " KGZ", " UKR", " BDI", " CMR", " IDN", " MDG", " NIC", " ARM", " GTM", " BEN", " PHL", " PER", " MNG", " LBR", " TGO", " SUR", " HND", " CMR", " HND", " IRQ", " EGY", " ZWE", " KHM", " MDG", " EGY", " IND", " BFA", " HND", " GTM", " CMR", " RWA", " NIC", " NIC", " BFA", " HTI", " JOR", " DOM", " COL", " ALB", " ALB", " VNM", " IND", " USA", " TLS", " CRI", " VNM", " YEM", " PHL", " NIC", " VNM", " HND", " CMR", " IND", " GEO", " NIC", " ARM", " BOL", " COL", " MNG", " TJK", " BRA", " MWI", " GEO", " KHM", " MNG", " GEO", " USA", " CHN", " KGZ", " AZE", " GEO", " SLE", " SLE", " CHN", " CHN", " KEN", " ECU", " UKR", " KHM", " KHM", " HTI", " ARM", " MLI", " AZE", " UGA", " LBR", " CRI", " YEM", " EGY", " IDN", " UGA", " SLV", " IND", " BFA", " LBN", " UKR", " NIC", " MEX", " TGO", " TLS", " SLB", " SLV", " IND", " MLI", " JOR", " UKR", " MDG", " ZAF", " PHL", " ARM", " TGO", " KHM", " ARM", " SLV", " ECU", " PAK", " UKR", " BEN", " KHM", " ARM", " DOM", " VNM", " JOR", " EGY", " MWI", " LBR", " AZE", " PHL", " MLI", " PER", " BEN", " PER", " BRA", " KEN", " UGA", " TJK", " LBN", " KGZ", " ZWE", " YEM", " SLE", " PRY", " TJK", " BEN", " GTM", " USA", " MNG", " AZE", " ZWE", " YEM", " MLI", " COL", " GEO", " JOR", " SUR", " ZMB", " COL", " CMR", " ZWE", " UKR", " RWA", " TJK", " IND", " PHL", " ECU", " UGA", " JOR", " TUR", " BFA", " TJK", " JOR", " SEN", " MWI", " GEO", " UKR", " TJK", " ZWE", " TGO", " TGO", " NIC", " CHN", " TJK", " COG", " TJK", " NIC", " UGA", " BRA", " BDI", " IRQ", " RWA", " CHN", " SLB", " UKR", " MNG", " KGZ", " COG", " VNM", " UGA", " SEN", " SLV", " SEN", " COG", " PAK", " MNG", " PRI", " BRA", " ZAF", " NAM", " NIC", " SLV", " COL", " ECU", " ISR", " CMR", " JOR", " ISR", " GHA", " VNM", " EGY", " ALB", " CMR", " PHL", " CMR", " CMR", " TJK", " PHL", " UKR", " CMR", " SLV", " BRA", " AZE", " USA", " NIC", " UGA", " HND", " JOR", " MWI", " SEN", " SOM", " PER", " BEN", " ISR", " IDN", " PAK", " TLS", " BRA", " TUR", " PER", " MEX", " AZE", " BFA", " MWI", " LBN", " MDG", " CMR", " LBN", " MEX", " GTM", " SLE", " MNG", " HTI", " COG", " ZWE", " PRY", " BFA", " PRY", " NIC", " YEM", " ZWE", " BDI", " COL", " UKR", " UGA", " CRI", " BEN", " COG", " JOR", " SLE", " COG", " ECU", " RWA", " JOR", " BDI", " MDG", " COL", " IND", " BRA", " PHL", " MDG", " YEM", " KGZ", " LBN", " YEM", " ALB", " NIC", " PHL", " ARM", " HTI", " YEM", " RWA", " MDG", " USA", " TJK", " HND", " LBN", " GHA", " UKR", " KEN", " VNM", " PRI", " ISR", " BRA", " SEN", " RWA", " RWA", " HND", " TGO", " COL", " MDG", " TGO", " PAN", " COL", " BDI", " TLS", " UGA", " SLE", " VNM", " ECU", " SLE", " COL", " LBN", " VNM", " YEM", " LBN", " MNG", " BEN", " IRQ", " BDI", " MLI", " KHM", " KGZ", " BRA", " RWA", " SLE", " UKR", " MLI", " ECU", " TJK", " ZWE", " BRA", " LBR", " BFA", " COG", " RWA", " PRY", " IND", " LBR", " SEN", " PHL", " HND", " BOL", " LBN", " COG", " BFA", " BEN", " GTM", " GTM", " IND", " TUR", " BDI", " CRI", " CRI", " ARM", " ECU", " RWA", " HND", " ECU", " SLV", " PHL", " CHN", " NIC", " UKR", " RWA", " USA", " TUR", " BRA", " MWI", " HTI", " ZWE", " RWA", " CRI", " MWI", " BOL", " PRY", " BDI", " UGA", " TJK", " SLE", " KEN", " CRI", " GTM", " GTM", " IDN", " USA", " DOM", " CRI", " PAK", " ECU", " NIC", " LBN", " COL", " ZWE", " PRY", " KGZ", " HND", " PAK", " GEO", " BOL", " SOM", " ISR", " GTM", " UGA", " MEX", " LAO", " BDI", " LBR", " ZWE", " VCT", " EGY", " IND", " JOR", " GHA", " KEN", " BOL", " MOZ", " KEN", " KHM", " TGO", " HTI", " IRQ", " KGZ", " USA", " COG", " BDI", " USA", " ZWE", " COL", " TGO", " JOR", " HND", " SSD", " ECU", " ISR", " ZWE", " JOR", " LBN", " ECU", " SLV", " MEX", " ZAF", " LBR", " ARM", " COL", " COG", " SEN", " AZE", " MOZ", " COG", " IND", " VNM", " KEN", " TUR", " KEN", " LBR", " PAK", " PRI", " COG", " JOR", " COG", " VNM", " NIC", " EGY", " GHA", " MDG", " IND", " LBN", " SSD", " PER", " RWA", " BDI", " IRQ", " KGZ", " BOL", " PER", " TGO", " MEX", " VNM", " LBN", " ECU", " SLV", " ZWE", " CRI", " SLV", " MLI", " GTM", " KEN", " PRY", " KEN", " MNG", " GEO", " UGA", " LBR", " MLI", " PHL", " SEN", " HTI", " SLE", " PHL", " PAK", " PER", " COL", " IND", " COL", " YEM", " HTI", " IRQ", " SLV", " GHA", " MEX", " YEM", " ARM", " HTI", " GTM", " MDG", " TJK", " SOM", " BRA", " SLV", " BFA", " HND", " NIC", " CHN", " ZWE", " VNM", " USA", " PER", " WSM", " PRY", " PER", " TLS", " NIC", " MWI", " MEX", " PAK", " TJK", " KHM", " VNM", " CHL", " UKR", " SLV", " SLE", " ZWE", " IRQ", " MEX", " SLV", " JOR", " CHN", " ISR", " TUR", " PAK", " PAK", " VNM", " BEN", " COG", " SLV", " COL", " UGA", " ECU", " RWA", " LBN", " ECU", " GHA", " PER", " PHL", " LBN", " PER", " KEN", " USA", " TJK", " IRQ", " DOM", " COL", " NIC", " RWA", " BEN", " KEN", " PHL", " PER", " KHM", " RWA", " GTM", " YEM", " ECU", " IRQ", " ECU", " ALB", " MWI", " MEX", " MEX", " ECU", " SLV", " DOM", " COL", " HND", " SLV", " BDI", " RWA", " PAK", " LBN", " BOL", " SLE", " BOL", " ARM", " MDG", " NIC", " LBN", " MDG", " KEN", " DOM", " PHL", " TJK", " GEO", " UGA", " AZE", " SLE", " PAN", " IND", " LBN", " EGY", " TGO", " PER", " SLV", " MEX", " DOM", " BFA", " NIC", " UGA", " DOM", " PRY", " DOM", " PRY", " BFA", " MNG", " GHA", " PHL", " IDN", " VNM", " CMR", " ISR", " ARM", " SLE", " VNM", " SLE", " GTM", " PHL", " BRA", " MEX", " IDN", " KGZ", " VNM", " MEX", " COL", " IDN", " IRQ", " ARM", " NIC", " GHA", " BOL", " MLI", " BOL", " BRA", " MDG", " SLE", " NGA", " CRI", " BOL", " TGO", " UGA", " BOL", " BFA", " GTM", " KEN", " GHA", " PAK", " BFA", " ECU", " SLE", " IND", " LBR", " SLV", " PRY", " KGZ", " SEN", " ECU", " GHA", " PER", " GHA", " SEN", " USA", " TGO", " GHA", " TUR", " GHA", " PHL", " IND", " ECU", " TGO", " BOL", " JOR", " GHA", " SLE", " BOL", " ARM", " TUR", " VNM", " AZE", " PER", " UGA", " VNM", " SEN", " YEM", " PAK", " SLE", " BFA", " PAK", " BOL", " WSM", " LBN", " EGY", " BRA", " GTM", " PAK", " LBN", " PER", " LBR", " MLI", " PAK", " LBN", " JOR", " PHL", " ARM", " LBN", " PER", " ZMB", " MOZ", " YEM", " KEN", " ZWE", " ZWE", " SLV", " HTI", " SOM", " ECU", " GTM", " IND", " PAK", " UGA", " GHA", " GHA", " BOL", " HTI", " PER", " UGA", " SLV", " TLS", " PER", " ARM", " MLI", " USA", " GTM", " GHA", " GEO", " TJK", " MLI", " COL", " MEX", " MEX", " KHM", " RWA", " LBR", " BFA", " CRI", " MEX", " PHL", " PHL", " SLE", " IND", " UGA", " GHA", " NIC", " VNM", " PAK", " PAK", " USA", " MLI", " VNM", " MDG", " RWA", " CMR", " MLI", " COL", " UKR", " PRY", " HTI", " TGO", " ZWE", " PER", " MDG", " BOL", " TJK", " IDN", " NIC", " PHL", " SLE", " UKR", " KEN", " SLE", " COL", " PER", " NIC", " BOL", " KEN", " TUR", " ECU", " UGA", " SLV", " PAK", " GEO", " HTI", " KEN", " BOL", " RWA", " KHM", " BOL", " DOM", " BOL", " MEX", " TGO", " BOL", " TJK", " GTM", " LBN", " KEN", " GHA", " SLV", " HND", " UGA", " ECU", " HND", " COL", " ZWE", " SEN", " SLE", " PRY", " LBN", " CRI", " PRY", " SLV", " ECU", " PHL", " UGA", " GEO", " IDN", " BOL", " GTM", " PER", " GHA", " LBR", " SLB", " PER", " BOL", " TJK", " BEN", " BOL", " UGA", " SLE", " KEN", " NIC", " GTM", " IND", " MEX", " HTI", " PER", " PER", " MOZ", " JOR", " PRY", " PHL", " JOR", " BFA", " TLS", " PAK", " LBN", " GTM", " BOL", " PRY", " IDN", " PHL", " MOZ", " RWA", " SLE", " TGO", " PRY", " HTI", " LBN", " ZWE", " COL", " VNM", " GHA", " KEN", " BOL", " PAK", " NIC", " PRY", " ECU", " IDN", " MLI", " PRY", " UGA", " BDI", " USA", " ECU", " PHL", " MEX", " PER", " MEX", " BOL", " KEN", " NIC", " SLV", " BOL", " GTM", " PAK", " WSM", " PHL", " PER", " COL", " USA", " TLS", " COL", " VNM", " KEN", " BOL", " USA", " UGA", " KEN", " ECU", " PHL", " BOL", " RWA", " RWA", " SEN", " SLV", " PER", " PRY", " PRY"], "name": "", "type": "choropleth", "z": [1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 2.0, 2.0, 4.0, 1.0, 1.0, 1.0, 4.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 5.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 3.0, 4.0, 1.0, 2.0, 3.0, 1.0, 3.0, 3.0, 3.0, 4.0, 1.0, 4.0, 2.0, 3.0, 1.0, 4.0, 4.0, 5.0, 1.0, 4.0, 1.0, 1.0, 1.0, 1.0, 6.0, 1.0, 1.0, 1.0, 2.0, 3.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 5.0, 4.0, 2.0, 3.0, 1.0, 2.0, 3.0, 2.0, 1.0, 1.0, 3.0, 2.0, 2.0, 1.0, 1.0, 8.0, 3.0, 2.0, 3.0, 3.0, 4.0, 4.0, 1.0, 1.0, 2.0, 2.0, 2.0, 2.0, 1.0, 2.0, 3.0, 2.0, 1.0, 1.0, 3.0, 1.0, 3.0, 6.0, 2.0, 5.0, 6.0, 3.0, 4.0, 1.0, 5.0, 6.0, 2.0, 2.0, 4.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 4.0, 1.0, 2.0, 2.0, 3.0, 5.0, 2.0, 1.0, 3.0, 4.0, 2.0, 1.0, 2.0, 2.0, 2.0, 4.0, 1.0, 5.0, 9.0, 3.0, 6.0, 3.0, 2.0, 1.0, 6.0, 3.0, 4.0, 7.0, 2.0, 1.0, 5.0, 3.0, 2.0, 2.0, 1.0, 4.0, 2.0, 7.0, 2.0, 2.0, 1.0, 3.0, 9.0, 3.0, 2.0, 4.0, 3.0, 2.0, 6.0, 4.0, 3.0, 1.0, 5.0, 3.0, 3.0, 11.0, 3.0, 5.0, 2.0, 5.0, 3.0, 6.0, 1.0, 2.0, 3.0, 3.0, 5.0, 3.0, 1.0, 8.0, 2.0, 2.0, 2.0, 1.0, 1.0, 3.0, 2.0, 2.0, 3.0, 8.0, 4.0, 4.0, 5.0, 7.0, 10.0, 4.0, 4.0, 1.0, 2.0, 5.0, 1.0, 4.0, 1.0, 1.0, 3.0, 3.0, 1.0, 1.0, 2.0, 1.0, 2.0, 3.0, 3.0, 3.0, 3.0, 1.0, 1.0, 7.0, 3.0, 2.0, 4.0, 4.0, 6.0, 2.0, 1.0, 2.0, 4.0, 4.0, 3.0, 2.0, 5.0, 5.0, 3.0, 5.0, 8.0, 3.0, 2.0, 2.0, 3.0, 3.0, 6.0, 4.0, 5.0, 7.0, 5.0, 3.0, 4.0, 1.0, 15.0, 3.0, 11.0, 3.0, 7.0, 4.0, 7.0, 4.0, 4.0, 10.0, 1.0, 1.0, 5.0, 3.0, 1.0, 4.0, 3.0, 5.0, 2.0, 4.0, 2.0, 14.0, 1.0, 2.0, 5.0, 5.0, 1.0, 13.0, 7.0, 5.0, 3.0, 3.0, 3.0, 4.0, 4.0, 4.0, 7.0, 3.0, 1.0, 2.0, 3.0, 3.0, 3.0, 3.0, 5.0, 7.0, 1.0, 4.0, 2.0, 3.0, 7.0, 13.0, 4.0, 2.0, 8.0, 6.0, 8.0, 10.0, 6.0, 7.0, 6.0, 8.0, 8.0, 4.0, 4.0, 4.0, 2.0, 5.0, 1.0, 3.0, 4.0, 7.0, 19.0, 6.0, 2.0, 9.0, 1.0, 5.0, 4.0, 6.0, 2.0, 1.0, 2.0, 2.0, 2.0, 8.0, 3.0, 3.0, 3.0, 1.0, 4.0, 5.0, 5.0, 8.0, 2.0, 1.0, 6.0, 3.0, 1.0, 2.0, 4.0, 1.0, 6.0, 10.0, 15.0, 6.0, 1.0, 12.0, 5.0, 2.0, 5.0, 8.0, 7.0, 2.0, 12.0, 13.0, 11.0, 11.0, 10.0, 13.0, 4.0, 13.0, 9.0, 2.0, 4.0, 2.0, 8.0, 12.0, 7.0, 3.0, 2.0, 5.0, 1.0, 8.0, 7.0, 1.0, 7.0, 9.0, 7.0, 2.0, 15.0, 8.0, 6.0, 3.0, 5.0, 4.0, 2.0, 15.0, 16.0, 4.0, 2.0, 4.0, 6.0, 3.0, 16.0, 1.0, 4.0, 2.0, 8.0, 5.0, 10.0, 11.0, 6.0, 2.0, 14.0, 3.0, 11.0, 5.0, 2.0, 1.0, 8.0, 8.0, 1.0, 8.0, 5.0, 5.0, 2.0, 25.0, 16.0, 11.0, 2.0, 11.0, 25.0, 5.0, 5.0, 6.0, 12.0, 6.0, 9.0, 22.0, 10.0, 2.0, 10.0, 3.0, 16.0, 2.0, 10.0, 14.0, 6.0, 6.0, 4.0, 17.0, 4.0, 2.0, 3.0, 2.0, 8.0, 1.0, 3.0, 12.0, 9.0, 15.0, 17.0, 27.0, 5.0, 15.0, 2.0, 7.0, 12.0, 6.0, 4.0, 7.0, 14.0, 17.0, 6.0, 12.0, 5.0, 7.0, 3.0, 6.0, 3.0, 3.0, 8.0, 10.0, 4.0, 3.0, 6.0, 8.0, 5.0, 12.0, 8.0, 10.0, 9.0, 3.0, 27.0, 10.0, 1.0, 1.0, 2.0, 15.0, 19.0, 5.0, 15.0, 14.0, 4.0, 6.0, 1.0, 8.0, 10.0, 5.0, 3.0, 11.0, 16.0, 3.0, 6.0, 7.0, 10.0, 9.0, 1.0, 8.0, 9.0, 13.0, 22.0, 3.0, 12.0, 6.0, 7.0, 3.0, 18.0, 3.0, 8.0, 5.0, 10.0, 5.0, 6.0, 8.0, 2.0, 7.0, 3.0, 13.0, 9.0, 14.0, 21.0, 5.0, 1.0, 4.0, 21.0, 1.0, 4.0, 8.0, 24.0, 10.0, 14.0, 6.0, 16.0, 5.0, 3.0, 7.0, 20.0, 23.0, 6.0, 6.0, 3.0, 3.0, 3.0, 15.0, 5.0, 3.0, 4.0, 43.0, 15.0, 3.0, 13.0, 19.0, 9.0, 10.0, 25.0, 14.0, 14.0, 28.0, 13.0, 57.0, 20.0, 4.0, 7.0, 4.0, 2.0, 4.0, 3.0, 11.0, 19.0, 64.0, 8.0, 21.0, 22.0, 15.0, 3.0, 16.0, 11.0, 9.0, 14.0, 24.0, 5.0, 10.0, 41.0, 10.0, 20.0, 2.0, 5.0, 7.0, 20.0, 2.0, 18.0, 12.0, 14.0, 26.0, 25.0, 47.0, 20.0, 5.0, 2.0, 12.0, 2.0, 12.0, 15.0, 19.0, 13.0, 29.0, 28.0, 13.0, 32.0, 8.0, 4.0, 5.0, 5.0, 9.0, 4.0, 14.0, 42.0, 8.0, 8.0, 10.0, 6.0, 22.0, 10.0, 12.0, 23.0, 15.0, 12.0, 28.0, 7.0, 43.0, 8.0, 6.0, 26.0, 52.0, 14.0, 37.0, 5.0, 11.0, 13.0, 33.0, 30.0, 4.0, 30.0, 41.0, 29.0, 22.0, 13.0, 4.0, 17.0, 15.0, 7.0, 17.0, 8.0, 21.0, 9.0, 49.0, 16.0, 3.0, 7.0, 23.0, 16.0, 33.0, 24.0, 6.0, 18.0, 10.0, 4.0, 15.0, 24.0, 11.0, 8.0, 20.0, 20.0, 9.0, 6.0, 49.0, 22.0, 24.0, 10.0, 4.0, 11.0, 29.0, 18.0, 18.0, 7.0, 4.0, 27.0, 14.0, 7.0, 6.0, 44.0, 37.0, 46.0, 12.0, 14.0, 3.0, 23.0, 38.0, 26.0, 20.0, 5.0, 14.0, 13.0, 20.0, 19.0, 39.0, 15.0, 18.0, 46.0, 4.0, 22.0, 10.0, 6.0, 38.0, 36.0, 5.0, 21.0, 59.0, 50.0, 12.0, 31.0, 8.0, 18.0, 22.0, 24.0, 9.0, 20.0, 16.0, 13.0, 13.0, 11.0, 26.0, 31.0, 7.0, 41.0, 43.0, 40.0, 9.0, 8.0, 35.0, 18.0, 10.0, 26.0, 10.0, 18.0, 74.0, 25.0, 22.0, 70.0, 51.0, 8.0, 58.0, 30.0, 17.0, 40.0, 18.0, 22.0, 16.0, 18.0, 18.0, 29.0, 105.0, 22.0, 47.0, 16.0, 11.0, 21.0, 29.0, 39.0, 6.0, 7.0, 7.0, 23.0, 18.0, 17.0, 25.0, 56.0, 44.0, 14.0, 65.0, 6.0, 21.0, 47.0, 22.0, 1.0, 1.0, 107.0, 10.0, 26.0, 2.0, 22.0, 22.0, 10.0, 36.0, 38.0, 14.0, 20.0, 48.0, 15.0, 26.0, 42.0, 22.0, 12.0, 76.0, 65.0, 1.0, 25.0, 14.0, 164.0, 60.0, 10.0, 37.0, 20.0, 91.0, 26.0, 62.0, 35.0, 11.0, 41.0, 8.0, 123.0, 50.0, 10.0, 32.0, 23.0, 32.0, 27.0, 31.0, 33.0, 34.0, 11.0, 120.0, 41.0, 92.0, 31.0, 70.0, 44.0, 40.0, 162.0, 20.0, 37.0, 46.0, 51.0, 18.0, 26.0, 106.0, 30.0, 28.0, 25.0, 70.0, 32.0, 27.0, 65.0, 88.0, 48.0, 37.0, 2.0, 14.0, 52.0, 32.0, 70.0, 18.0, 46.0, 105.0, 38.0, 34.0, 188.0, 35.0, 94.0, 34.0, 54.0, 115.0, 50.0, 33.0, 3.0, 3.0, 3.0, 71.0, 114.0, 42.0, 4.0, 83.0, 88.0, 20.0, 46.0, 45.0, 116.0, 109.0, 107.0, 58.0, 62.0, 18.0, 98.0, 33.0, 82.0, 116.0, 76.0, 50.0, 57.0, 53.0, 25.0, 49.0, 37.0, 62.0, 60.0, 54.0, 121.0, 17.0, 45.0, 81.0, 38.0, 138.0, 52.0, 45.0, 35.0, 191.0, 124.0, 118.0, 153.0, 92.0, 22.0, 84.0, 44.0, 146.0, 153.0, 23.0, 63.0, 34.0, 203.0, 36.0, 195.0, 55.0, 157.0, 44.0, 21.0, 65.0, 294.0, 71.0, 54.0, 253.0, 32.0, 98.0, 64.0, 113.0, 213.0, 113.0, 52.0, 196.0, 147.0, 131.0, 46.0, 100.0, 28.0, 238.0, 223.0, 82.0, 127.0, 159.0, 201.0, 75.0, 178.0, 280.0, 40.0, 37.0, 127.0, 46.0, 34.0, 56.0, 63.0, 416.0, 48.0, 120.0, 32.0, 76.0, 293.0, 90.0, 251.0, 158.0, 15.0, 124.0, 122.0, 209.0, 148.0, 74.0, 172.0, 39.0, 100.0, 73.0, 38.0, 243.0, 125.0, 400.0, 213.0, 68.0, 214.0, 48.0, 80.0, 69.0, 124.0, 514.0, 182.0, 91.0, 50.0, 176.0, 156.0, 50.0, 386.0, 329.0, 721.0, 206.0, 141.0, 151.0, 58.0, 89.0, 105.0, 150.0, 307.0, 175.0, 66.0, 503.0, 209.0, 133.0, 244.0, 440.0, 163.0, 171.0, 88.0, 55.0, 212.0, 795.0, 462.0, 87.0, 548.0, 912.0, 73.0, 539.0, 203.0, 350.0, 664.0, 204.0, 284.0, 924.0, 132.0, 864.0, 488.0, 94.0, 359.0, 744.0, 418.0, 124.0, 671.0, 173.0, 173.0, 474.0, 1678.0, 176.0, 363.0, 218.0, 222.0, 186.0, 909.0, 1333.0, 256.0, 340.0, 1675.0, 616.0, 2125.0, 644.0, 1455.0, 299.0, 1178.0, 2047.0, 842.0, 2918.0, 398.0, 364.0, 1901.0, 3830.0, 1071.0, 3873.0, 484.0, 500.0, 590.0, 840.0, 3061.0, 1299.0, 811.0, 914.0]}],                        {"coloraxis": {"colorbar": {"title": {"text": "amount_projects"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}}, "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "sliders": [{"active": 0, "currentvalue": {"prefix": "sector="}, "len": 0.9, "pad": {"b": 10, "t": 60}, "steps": [{"args": [["Retail"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Retail", "method": "animate"}, {"args": [["Personal Use"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Personal Use", "method": "animate"}, {"args": [["Services"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Services", "method": "animate"}, {"args": [["Agriculture"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Agriculture", "method": "animate"}, {"args": [["Education"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Education", "method": "animate"}, {"args": [["Health"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Health", "method": "animate"}, {"args": [["Wholesale"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Wholesale", "method": "animate"}, {"args": [["Arts"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Arts", "method": "animate"}, {"args": [["Food"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Food", "method": "animate"}, {"args": [["Transportation"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Transportation", "method": "animate"}, {"args": [["Construction"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Construction", "method": "animate"}, {"args": [["Manufacturing"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Manufacturing", "method": "animate"}, {"args": [["Entertainment"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Entertainment", "method": "animate"}, {"args": [["Clothing"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Clothing", "method": "animate"}, {"args": [["Housing"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "Housing", "method": "animate"}], "x": 0.1, "xanchor": "left", "y": 0, "yanchor": "top"}], "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "updatemenus": [{"buttons": [{"args": [null, {"frame": {"duration": 500, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 500, "easing": "linear"}}], "label": "&#9654;", "method": "animate"}, {"args": [[null], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "&#9724;", "method": "animate"}], "direction": "left", "pad": {"r": 10, "t": 70}, "showactive": false, "type": "buttons", "x": 0.1, "xanchor": "right", "y": 0, "yanchor": "top"}]},                        {"responsive": true}                    ).then(function(){
                            Plotly.addFrames('ecc77a9c-640e-4608-8392-11ae8a19b09b', [{"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Retail<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" MEX", " BFA", " KEN", " PHL", " IDN", " SEN", " CMR", " HND", " SSD", " SLE", " SSD", " NPL", " IND", " SEN", " MDG", " IND", " LBR", " MDG", " TUR", " NPL", " NIC", " TGO", " LBR", " ZMB", " MLI", " CMR", " TGO", " CMR", " UGA", " KHM", " SLV", " MEX", " EGY", " NIC", " LBR", " MOZ", " ECU", " HTI", " PHL", " MOZ", " AZE", " YEM", " HND", " GUM", " WSM", " UGA", " TGO", " KHM", " SSD", " PAK", " GTM", " KHM", " TGO", " GHA", " SEN", " BEN", " MLI", " KEN", " ALB", " ZAF", " HTI", " SLB", " NGA", " NPL", " SEN", " NPL", " TJK", " MDG", " NPL", " IND", " PAK", " LBN", " RWA", " MDG", " USA", " PAK", " ZWE", " PAK", " PAN", " CMR", " AZE", " CMR", " HND", " CMR", " TJK", " GEO", " MOZ", " COL", " BEN", " HTI", " TLS", " BEN", " ZAF", " MDG", " IDN", " PHL", " TLS", " MOZ", " BFA", " MOZ", " IND", " TUR", " IDN", " GHA", " GEO", " MLI", " PAN", " ARM", " LBN", " MOZ", " LBN", " MEX", " BFA", " HND", " RWA", " MEX", " GEO", " NPL", " ALB", " YEM", " PAN", " TGO", " SEN", " KGZ", " BFA", " HTI", " MDG", " NIC", " SEN", " UKR", " IDN", " ZWE", " GEO", " TLS", " TLS", " TGO", " TLS", " HTI", " MNG", " KGZ", " SLB", " LBR", " MDG", " BFA", " TUR", " WSM", " ALB", " NPL", " GTM", " SLV", " PER", " ALB", " MOZ", " MOZ", " MOZ", " MDG", " CRI", " BEN", " SLE", " SSD", " YEM", " UGA", " ALB", " TLS", " ALB", " KHM", " PAN", " MLI", " EGY", " MEX", " TJK", " JOR", " ZWE", " ZAF", " COL", " ECU", " MLI", " MOZ", " MNG", " LBR", " HND", " KHM", " ZWE", " MWI", " IND", " AZE", " GHA", " GEO", " HND", " MOZ", " PRY", " GEO", " JOR", " KHM", " SLV", " ZMB", " DOM", " PER", " SUR", " TJK", " HTI", " GEO", " GEO", " TGO", " MWI", " PAK", " UGA", " COL", " KEN", " ZWE", " IND", " MDG", " CMR", " ALB", " PAK", " CMR", " KEN", " MWI", " NPL", " ZAF", " ZMB", " MNG", " MWI", " MDG", " AZE", " AZE", " CRI", " YEM", " MDG", " KHM", " ECU", " KHM", " PAN", " VNM", " ARM", " ARM", " ARM", " JOR", " PHL", " SLE", " TGO", " ALB", " IND", " TGO", " ZWE", " WSM", " ZAF", " HND", " ZMB", " VNM", " YEM", " USA", " GHA", " YEM", " ARM", " MWI", " AZE", " SLE", " IDN", " SLV", " ZWE", " LBR", " MOZ", " NPL", " JOR", " ZAF", " ZWE", " ZMB", " HTI", " YEM", " IRQ", " HTI", " MEX", " LBR", " KGZ", " ZAF", " EGY", " CRI", " PRY", " HTI", " EGY", " MOZ", " IND", " GTM", " TUR", " CRI", " KEN", " HTI", " IDN", " LBR", " PAK", " KGZ", " ALB", " TGO", " IRQ", " LBN", " BEN", " CMR", " AZE", " BOL", " ARM", " EGY", " CMR", " NIC", " TJK", " TJK", " TUR", " BFA", " DOM", " PER", " ALB", " TJK", " SLV", " JOR", " PAK", " TGO", " HND", " COL", " ARM", " ECU", " AZE", " MDG", " GHA", " MEX", " MDG", " GTM", " BDI", " TUR", " BFA", " KGZ", " UKR", " BDI", " CMR", " IDN", " MDG", " NIC", " ARM", " GTM", " BEN", " PHL", " PER", " MNG", " LBR", " TGO", " SUR", " HND", " CMR", " HND", " IRQ", " EGY", " ZWE", " KHM", " MDG", " EGY", " IND", " BFA", " HND", " GTM", " CMR", " RWA", " NIC", " NIC", " BFA", " HTI", " JOR", " DOM", " COL", " ALB", " ALB", " VNM", " IND", " USA", " TLS", " CRI", " VNM", " YEM", " PHL", " NIC", " VNM", " HND", " CMR", " IND", " GEO", " NIC", " ARM", " BOL", " COL", " MNG", " TJK", " BRA", " MWI", " GEO", " KHM", " MNG", " GEO", " USA", " CHN", " KGZ", " AZE", " GEO", " SLE", " SLE", " CHN", " CHN", " KEN", " ECU", " UKR", " KHM", " KHM", " HTI", " ARM", " MLI", " AZE", " UGA", " LBR", " CRI", " YEM", " EGY", " IDN", " UGA", " SLV", " IND", " BFA", " LBN", " UKR", " NIC", " MEX", " TGO", " TLS", " SLB", " SLV", " IND", " MLI", " JOR", " UKR", " MDG", " ZAF", " PHL", " ARM", " TGO", " KHM", " ARM", " SLV", " ECU", " PAK", " UKR", " BEN", " KHM", " ARM", " DOM", " VNM", " JOR", " EGY", " MWI", " LBR", " AZE", " PHL", " MLI", " PER", " BEN", " PER", " BRA", " KEN", " UGA", " TJK", " LBN", " KGZ", " ZWE", " YEM", " SLE", " PRY", " TJK", " BEN", " GTM", " USA", " MNG", " AZE", " ZWE", " YEM", " MLI", " COL", " GEO", " JOR", " SUR", " ZMB", " COL", " CMR", " ZWE", " UKR", " RWA", " TJK", " IND", " PHL", " ECU", " UGA", " JOR", " TUR", " BFA", " TJK", " JOR", " SEN", " MWI", " GEO", " UKR", " TJK", " ZWE", " TGO", " TGO", " NIC", " CHN", " TJK", " COG", " TJK", " NIC", " UGA", " BRA", " BDI", " IRQ", " RWA", " CHN", " SLB", " UKR", " MNG", " KGZ", " COG", " VNM", " UGA", " SEN", " SLV", " SEN", " COG", " PAK", " MNG", " PRI", " BRA", " ZAF", " NAM", " NIC", " SLV", " COL", " ECU", " ISR", " CMR", " JOR", " ISR", " GHA", " VNM", " EGY", " ALB", " CMR", " PHL", " CMR", " CMR", " TJK", " PHL", " UKR", " CMR", " SLV", " BRA", " AZE", " USA", " NIC", " UGA", " HND", " JOR", " MWI", " SEN", " SOM", " PER", " BEN", " ISR", " IDN", " PAK", " TLS", " BRA", " TUR", " PER", " MEX", " AZE", " BFA", " MWI", " LBN", " MDG", " CMR", " LBN", " MEX", " GTM", " SLE", " MNG", " HTI", " COG", " ZWE", " PRY", " BFA", " PRY", " NIC", " YEM", " ZWE", " BDI", " COL", " UKR", " UGA", " CRI", " BEN", " COG", " JOR", " SLE", " COG", " ECU", " RWA", " JOR", " BDI", " MDG", " COL", " IND", " BRA", " PHL", " MDG", " YEM", " KGZ", " LBN", " YEM", " ALB", " NIC", " PHL", " ARM", " HTI", " YEM", " RWA", " MDG", " USA", " TJK", " HND", " LBN", " GHA", " UKR", " KEN", " VNM", " PRI", " ISR", " BRA", " SEN", " RWA", " RWA", " HND", " TGO", " COL", " MDG", " TGO", " PAN", " COL", " BDI", " TLS", " UGA", " SLE", " VNM", " ECU", " SLE", " COL", " LBN", " VNM", " YEM", " LBN", " MNG", " BEN", " IRQ", " BDI", " MLI", " KHM", " KGZ", " BRA", " RWA", " SLE", " UKR", " MLI", " ECU", " TJK", " ZWE", " BRA", " LBR", " BFA", " COG", " RWA", " PRY", " IND", " LBR", " SEN", " PHL", " HND", " BOL", " LBN", " COG", " BFA", " BEN", " GTM", " GTM", " IND", " TUR", " BDI", " CRI", " CRI", " ARM", " ECU", " RWA", " HND", " ECU", " SLV", " PHL", " CHN", " NIC", " UKR", " RWA", " USA", " TUR", " BRA", " MWI", " HTI", " ZWE", " RWA", " CRI", " MWI", " BOL", " PRY", " BDI", " UGA", " TJK", " SLE", " KEN", " CRI", " GTM", " GTM", " IDN", " USA", " DOM", " CRI", " PAK", " ECU", " NIC", " LBN", " COL", " ZWE", " PRY", " KGZ", " HND", " PAK", " GEO", " BOL", " SOM", " ISR", " GTM", " UGA", " MEX", " LAO", " BDI", " LBR", " ZWE", " VCT", " EGY", " IND", " JOR", " GHA", " KEN", " BOL", " MOZ", " KEN", " KHM", " TGO", " HTI", " IRQ", " KGZ", " USA", " COG", " BDI", " USA", " ZWE", " COL", " TGO", " JOR", " HND", " SSD", " ECU", " ISR", " ZWE", " JOR", " LBN", " ECU", " SLV", " MEX", " ZAF", " LBR", " ARM", " COL", " COG", " SEN", " AZE", " MOZ", " COG", " IND", " VNM", " KEN", " TUR", " KEN", " LBR", " PAK", " PRI", " COG", " JOR", " COG", " VNM", " NIC", " EGY", " GHA", " MDG", " IND", " LBN", " SSD", " PER", " RWA", " BDI", " IRQ", " KGZ", " BOL", " PER", " TGO", " MEX", " VNM", " LBN", " ECU", " SLV", " ZWE", " CRI", " SLV", " MLI", " GTM", " KEN", " PRY", " KEN", " MNG", " GEO", " UGA", " LBR", " MLI", " PHL", " SEN", " HTI", " SLE", " PHL", " PAK", " PER", " COL", " IND", " COL", " YEM", " HTI", " IRQ", " SLV", " GHA", " MEX", " YEM", " ARM", " HTI", " GTM", " MDG", " TJK", " SOM", " BRA", " SLV", " BFA", " HND", " NIC", " CHN", " ZWE", " VNM", " USA", " PER", " WSM", " PRY", " PER", " TLS", " NIC", " MWI", " MEX", " PAK", " TJK", " KHM", " VNM", " CHL", " UKR", " SLV", " SLE", " ZWE", " IRQ", " MEX", " SLV", " JOR", " CHN", " ISR", " TUR", " PAK", " PAK", " VNM", " BEN", " COG", " SLV", " COL", " UGA", " ECU", " RWA", " LBN", " ECU", " GHA", " PER", " PHL", " LBN", " PER", " KEN", " USA", " TJK", " IRQ", " DOM", " COL", " NIC", " RWA", " BEN", " KEN", " PHL", " PER", " KHM", " RWA", " GTM", " YEM", " ECU", " IRQ", " ECU", " ALB", " MWI", " MEX", " MEX", " ECU", " SLV", " DOM", " COL", " HND", " SLV", " BDI", " RWA", " PAK", " LBN", " BOL", " SLE", " BOL", " ARM", " MDG", " NIC", " LBN", " MDG", " KEN", " DOM", " PHL", " TJK", " GEO", " UGA", " AZE", " SLE", " PAN", " IND", " LBN", " EGY", " TGO", " PER", " SLV", " MEX", " DOM", " BFA", " NIC", " UGA", " DOM", " PRY", " DOM", " PRY", " BFA", " MNG", " GHA", " PHL", " IDN", " VNM", " CMR", " ISR", " ARM", " SLE", " VNM", " SLE", " GTM", " PHL", " BRA", " MEX", " IDN", " KGZ", " VNM", " MEX", " COL", " IDN", " IRQ", " ARM", " NIC", " GHA", " BOL", " MLI", " BOL", " BRA", " MDG", " SLE", " NGA", " CRI", " BOL", " TGO", " UGA", " BOL", " BFA", " GTM", " KEN", " GHA", " PAK", " BFA", " ECU", " SLE", " IND", " LBR", " SLV", " PRY", " KGZ", " SEN", " ECU", " GHA", " PER", " GHA", " SEN", " USA", " TGO", " GHA", " TUR", " GHA", " PHL", " IND", " ECU", " TGO", " BOL", " JOR", " GHA", " SLE", " BOL", " ARM", " TUR", " VNM", " AZE", " PER", " UGA", " VNM", " SEN", " YEM", " PAK", " SLE", " BFA", " PAK", " BOL", " WSM", " LBN", " EGY", " BRA", " GTM", " PAK", " LBN", " PER", " LBR", " MLI", " PAK", " LBN", " JOR", " PHL", " ARM", " LBN", " PER", " ZMB", " MOZ", " YEM", " KEN", " ZWE", " ZWE", " SLV", " HTI", " SOM", " ECU", " GTM", " IND", " PAK", " UGA", " GHA", " GHA", " BOL", " HTI", " PER", " UGA", " SLV", " TLS", " PER", " ARM", " MLI", " USA", " GTM", " GHA", " GEO", " TJK", " MLI", " COL", " MEX", " MEX", " KHM", " RWA", " LBR", " BFA", " CRI", " MEX", " PHL", " PHL", " SLE", " IND", " UGA", " GHA", " NIC", " VNM", " PAK", " PAK", " USA", " MLI", " VNM", " MDG", " RWA", " CMR", " MLI", " COL", " UKR", " PRY", " HTI", " TGO", " ZWE", " PER", " MDG", " BOL", " TJK", " IDN", " NIC", " PHL", " SLE", " UKR", " KEN", " SLE", " COL", " PER", " NIC", " BOL", " KEN", " TUR", " ECU", " UGA", " SLV", " PAK", " GEO", " HTI", " KEN", " BOL", " RWA", " KHM", " BOL", " DOM", " BOL", " MEX", " TGO", " BOL", " TJK", " GTM", " LBN", " KEN", " GHA", " SLV", " HND", " UGA", " ECU", " HND", " COL", " ZWE", " SEN", " SLE", " PRY", " LBN", " CRI", " PRY", " SLV", " ECU", " PHL", " UGA", " GEO", " IDN", " BOL", " GTM", " PER", " GHA", " LBR", " SLB", " PER", " BOL", " TJK", " BEN", " BOL", " UGA", " SLE", " KEN", " NIC", " GTM", " IND", " MEX", " HTI", " PER", " PER", " MOZ", " JOR", " PRY", " PHL", " JOR", " BFA", " TLS", " PAK", " LBN", " GTM", " BOL", " PRY", " IDN", " PHL", " MOZ", " RWA", " SLE", " TGO", " PRY", " HTI", " LBN", " ZWE", " COL", " VNM", " GHA", " KEN", " BOL", " PAK", " NIC", " PRY", " ECU", " IDN", " MLI", " PRY", " UGA", " BDI", " USA", " ECU", " PHL", " MEX", " PER", " MEX", " BOL", " KEN", " NIC", " SLV", " BOL", " GTM", " PAK", " WSM", " PHL", " PER", " COL", " USA", " TLS", " COL", " VNM", " KEN", " BOL", " USA", " UGA", " KEN", " ECU", " PHL", " BOL", " RWA", " RWA", " SEN", " SLV", " PER", " PRY", " PRY"], "name": "", "z": [1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 2.0, 2.0, 4.0, 1.0, 1.0, 1.0, 4.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 5.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 3.0, 4.0, 1.0, 2.0, 3.0, 1.0, 3.0, 3.0, 3.0, 4.0, 1.0, 4.0, 2.0, 3.0, 1.0, 4.0, 4.0, 5.0, 1.0, 4.0, 1.0, 1.0, 1.0, 1.0, 6.0, 1.0, 1.0, 1.0, 2.0, 3.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 5.0, 4.0, 2.0, 3.0, 1.0, 2.0, 3.0, 2.0, 1.0, 1.0, 3.0, 2.0, 2.0, 1.0, 1.0, 8.0, 3.0, 2.0, 3.0, 3.0, 4.0, 4.0, 1.0, 1.0, 2.0, 2.0, 2.0, 2.0, 1.0, 2.0, 3.0, 2.0, 1.0, 1.0, 3.0, 1.0, 3.0, 6.0, 2.0, 5.0, 6.0, 3.0, 4.0, 1.0, 5.0, 6.0, 2.0, 2.0, 4.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 4.0, 1.0, 2.0, 2.0, 3.0, 5.0, 2.0, 1.0, 3.0, 4.0, 2.0, 1.0, 2.0, 2.0, 2.0, 4.0, 1.0, 5.0, 9.0, 3.0, 6.0, 3.0, 2.0, 1.0, 6.0, 3.0, 4.0, 7.0, 2.0, 1.0, 5.0, 3.0, 2.0, 2.0, 1.0, 4.0, 2.0, 7.0, 2.0, 2.0, 1.0, 3.0, 9.0, 3.0, 2.0, 4.0, 3.0, 2.0, 6.0, 4.0, 3.0, 1.0, 5.0, 3.0, 3.0, 11.0, 3.0, 5.0, 2.0, 5.0, 3.0, 6.0, 1.0, 2.0, 3.0, 3.0, 5.0, 3.0, 1.0, 8.0, 2.0, 2.0, 2.0, 1.0, 1.0, 3.0, 2.0, 2.0, 3.0, 8.0, 4.0, 4.0, 5.0, 7.0, 10.0, 4.0, 4.0, 1.0, 2.0, 5.0, 1.0, 4.0, 1.0, 1.0, 3.0, 3.0, 1.0, 1.0, 2.0, 1.0, 2.0, 3.0, 3.0, 3.0, 3.0, 1.0, 1.0, 7.0, 3.0, 2.0, 4.0, 4.0, 6.0, 2.0, 1.0, 2.0, 4.0, 4.0, 3.0, 2.0, 5.0, 5.0, 3.0, 5.0, 8.0, 3.0, 2.0, 2.0, 3.0, 3.0, 6.0, 4.0, 5.0, 7.0, 5.0, 3.0, 4.0, 1.0, 15.0, 3.0, 11.0, 3.0, 7.0, 4.0, 7.0, 4.0, 4.0, 10.0, 1.0, 1.0, 5.0, 3.0, 1.0, 4.0, 3.0, 5.0, 2.0, 4.0, 2.0, 14.0, 1.0, 2.0, 5.0, 5.0, 1.0, 13.0, 7.0, 5.0, 3.0, 3.0, 3.0, 4.0, 4.0, 4.0, 7.0, 3.0, 1.0, 2.0, 3.0, 3.0, 3.0, 3.0, 5.0, 7.0, 1.0, 4.0, 2.0, 3.0, 7.0, 13.0, 4.0, 2.0, 8.0, 6.0, 8.0, 10.0, 6.0, 7.0, 6.0, 8.0, 8.0, 4.0, 4.0, 4.0, 2.0, 5.0, 1.0, 3.0, 4.0, 7.0, 19.0, 6.0, 2.0, 9.0, 1.0, 5.0, 4.0, 6.0, 2.0, 1.0, 2.0, 2.0, 2.0, 8.0, 3.0, 3.0, 3.0, 1.0, 4.0, 5.0, 5.0, 8.0, 2.0, 1.0, 6.0, 3.0, 1.0, 2.0, 4.0, 1.0, 6.0, 10.0, 15.0, 6.0, 1.0, 12.0, 5.0, 2.0, 5.0, 8.0, 7.0, 2.0, 12.0, 13.0, 11.0, 11.0, 10.0, 13.0, 4.0, 13.0, 9.0, 2.0, 4.0, 2.0, 8.0, 12.0, 7.0, 3.0, 2.0, 5.0, 1.0, 8.0, 7.0, 1.0, 7.0, 9.0, 7.0, 2.0, 15.0, 8.0, 6.0, 3.0, 5.0, 4.0, 2.0, 15.0, 16.0, 4.0, 2.0, 4.0, 6.0, 3.0, 16.0, 1.0, 4.0, 2.0, 8.0, 5.0, 10.0, 11.0, 6.0, 2.0, 14.0, 3.0, 11.0, 5.0, 2.0, 1.0, 8.0, 8.0, 1.0, 8.0, 5.0, 5.0, 2.0, 25.0, 16.0, 11.0, 2.0, 11.0, 25.0, 5.0, 5.0, 6.0, 12.0, 6.0, 9.0, 22.0, 10.0, 2.0, 10.0, 3.0, 16.0, 2.0, 10.0, 14.0, 6.0, 6.0, 4.0, 17.0, 4.0, 2.0, 3.0, 2.0, 8.0, 1.0, 3.0, 12.0, 9.0, 15.0, 17.0, 27.0, 5.0, 15.0, 2.0, 7.0, 12.0, 6.0, 4.0, 7.0, 14.0, 17.0, 6.0, 12.0, 5.0, 7.0, 3.0, 6.0, 3.0, 3.0, 8.0, 10.0, 4.0, 3.0, 6.0, 8.0, 5.0, 12.0, 8.0, 10.0, 9.0, 3.0, 27.0, 10.0, 1.0, 1.0, 2.0, 15.0, 19.0, 5.0, 15.0, 14.0, 4.0, 6.0, 1.0, 8.0, 10.0, 5.0, 3.0, 11.0, 16.0, 3.0, 6.0, 7.0, 10.0, 9.0, 1.0, 8.0, 9.0, 13.0, 22.0, 3.0, 12.0, 6.0, 7.0, 3.0, 18.0, 3.0, 8.0, 5.0, 10.0, 5.0, 6.0, 8.0, 2.0, 7.0, 3.0, 13.0, 9.0, 14.0, 21.0, 5.0, 1.0, 4.0, 21.0, 1.0, 4.0, 8.0, 24.0, 10.0, 14.0, 6.0, 16.0, 5.0, 3.0, 7.0, 20.0, 23.0, 6.0, 6.0, 3.0, 3.0, 3.0, 15.0, 5.0, 3.0, 4.0, 43.0, 15.0, 3.0, 13.0, 19.0, 9.0, 10.0, 25.0, 14.0, 14.0, 28.0, 13.0, 57.0, 20.0, 4.0, 7.0, 4.0, 2.0, 4.0, 3.0, 11.0, 19.0, 64.0, 8.0, 21.0, 22.0, 15.0, 3.0, 16.0, 11.0, 9.0, 14.0, 24.0, 5.0, 10.0, 41.0, 10.0, 20.0, 2.0, 5.0, 7.0, 20.0, 2.0, 18.0, 12.0, 14.0, 26.0, 25.0, 47.0, 20.0, 5.0, 2.0, 12.0, 2.0, 12.0, 15.0, 19.0, 13.0, 29.0, 28.0, 13.0, 32.0, 8.0, 4.0, 5.0, 5.0, 9.0, 4.0, 14.0, 42.0, 8.0, 8.0, 10.0, 6.0, 22.0, 10.0, 12.0, 23.0, 15.0, 12.0, 28.0, 7.0, 43.0, 8.0, 6.0, 26.0, 52.0, 14.0, 37.0, 5.0, 11.0, 13.0, 33.0, 30.0, 4.0, 30.0, 41.0, 29.0, 22.0, 13.0, 4.0, 17.0, 15.0, 7.0, 17.0, 8.0, 21.0, 9.0, 49.0, 16.0, 3.0, 7.0, 23.0, 16.0, 33.0, 24.0, 6.0, 18.0, 10.0, 4.0, 15.0, 24.0, 11.0, 8.0, 20.0, 20.0, 9.0, 6.0, 49.0, 22.0, 24.0, 10.0, 4.0, 11.0, 29.0, 18.0, 18.0, 7.0, 4.0, 27.0, 14.0, 7.0, 6.0, 44.0, 37.0, 46.0, 12.0, 14.0, 3.0, 23.0, 38.0, 26.0, 20.0, 5.0, 14.0, 13.0, 20.0, 19.0, 39.0, 15.0, 18.0, 46.0, 4.0, 22.0, 10.0, 6.0, 38.0, 36.0, 5.0, 21.0, 59.0, 50.0, 12.0, 31.0, 8.0, 18.0, 22.0, 24.0, 9.0, 20.0, 16.0, 13.0, 13.0, 11.0, 26.0, 31.0, 7.0, 41.0, 43.0, 40.0, 9.0, 8.0, 35.0, 18.0, 10.0, 26.0, 10.0, 18.0, 74.0, 25.0, 22.0, 70.0, 51.0, 8.0, 58.0, 30.0, 17.0, 40.0, 18.0, 22.0, 16.0, 18.0, 18.0, 29.0, 105.0, 22.0, 47.0, 16.0, 11.0, 21.0, 29.0, 39.0, 6.0, 7.0, 7.0, 23.0, 18.0, 17.0, 25.0, 56.0, 44.0, 14.0, 65.0, 6.0, 21.0, 47.0, 22.0, 1.0, 1.0, 107.0, 10.0, 26.0, 2.0, 22.0, 22.0, 10.0, 36.0, 38.0, 14.0, 20.0, 48.0, 15.0, 26.0, 42.0, 22.0, 12.0, 76.0, 65.0, 1.0, 25.0, 14.0, 164.0, 60.0, 10.0, 37.0, 20.0, 91.0, 26.0, 62.0, 35.0, 11.0, 41.0, 8.0, 123.0, 50.0, 10.0, 32.0, 23.0, 32.0, 27.0, 31.0, 33.0, 34.0, 11.0, 120.0, 41.0, 92.0, 31.0, 70.0, 44.0, 40.0, 162.0, 20.0, 37.0, 46.0, 51.0, 18.0, 26.0, 106.0, 30.0, 28.0, 25.0, 70.0, 32.0, 27.0, 65.0, 88.0, 48.0, 37.0, 2.0, 14.0, 52.0, 32.0, 70.0, 18.0, 46.0, 105.0, 38.0, 34.0, 188.0, 35.0, 94.0, 34.0, 54.0, 115.0, 50.0, 33.0, 3.0, 3.0, 3.0, 71.0, 114.0, 42.0, 4.0, 83.0, 88.0, 20.0, 46.0, 45.0, 116.0, 109.0, 107.0, 58.0, 62.0, 18.0, 98.0, 33.0, 82.0, 116.0, 76.0, 50.0, 57.0, 53.0, 25.0, 49.0, 37.0, 62.0, 60.0, 54.0, 121.0, 17.0, 45.0, 81.0, 38.0, 138.0, 52.0, 45.0, 35.0, 191.0, 124.0, 118.0, 153.0, 92.0, 22.0, 84.0, 44.0, 146.0, 153.0, 23.0, 63.0, 34.0, 203.0, 36.0, 195.0, 55.0, 157.0, 44.0, 21.0, 65.0, 294.0, 71.0, 54.0, 253.0, 32.0, 98.0, 64.0, 113.0, 213.0, 113.0, 52.0, 196.0, 147.0, 131.0, 46.0, 100.0, 28.0, 238.0, 223.0, 82.0, 127.0, 159.0, 201.0, 75.0, 178.0, 280.0, 40.0, 37.0, 127.0, 46.0, 34.0, 56.0, 63.0, 416.0, 48.0, 120.0, 32.0, 76.0, 293.0, 90.0, 251.0, 158.0, 15.0, 124.0, 122.0, 209.0, 148.0, 74.0, 172.0, 39.0, 100.0, 73.0, 38.0, 243.0, 125.0, 400.0, 213.0, 68.0, 214.0, 48.0, 80.0, 69.0, 124.0, 514.0, 182.0, 91.0, 50.0, 176.0, 156.0, 50.0, 386.0, 329.0, 721.0, 206.0, 141.0, 151.0, 58.0, 89.0, 105.0, 150.0, 307.0, 175.0, 66.0, 503.0, 209.0, 133.0, 244.0, 440.0, 163.0, 171.0, 88.0, 55.0, 212.0, 795.0, 462.0, 87.0, 548.0, 912.0, 73.0, 539.0, 203.0, 350.0, 664.0, 204.0, 284.0, 924.0, 132.0, 864.0, 488.0, 94.0, 359.0, 744.0, 418.0, 124.0, 671.0, 173.0, 173.0, 474.0, 1678.0, 176.0, 363.0, 218.0, 222.0, 186.0, 909.0, 1333.0, 256.0, 340.0, 1675.0, 616.0, 2125.0, 644.0, 1455.0, 299.0, 1178.0, 2047.0, 842.0, 2918.0, 398.0, 364.0, 1901.0, 3830.0, 1071.0, 3873.0, 484.0, 500.0, 590.0, 840.0, 3061.0, 1299.0, 811.0, 914.0], "type": "choropleth"}], "name": "Retail"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Personal Use<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" SEN", " BFA", " USA", " PER", " KEN", " KEN", " PAK", " YEM", " HTI", " LAO", " NIC", " SLV", " PAN", " SOM", " ECU", " GTM", " SUR", " NIC", " ZMB", " MOZ", " PER", " GEO", " LBR", " SLB", " BFA", " IDN", " SLV", " PRY", " RWA", " SUR", " TJK", " KEN", " BFA", " CMR", " JOR", " HND", " KGZ", " SLB", " RWA", " GHA", " GTM", " LSO", " ZWE", " PRY", " SEN", " CHN", " SEN", " LBR", " MEX", " YEM", " TJK", " GHA", " SLE", " CRI", " KHM", " COL", " UGA", " LAO", " PHL", " ALB", " COL", " KGZ", " MOZ", " PHL", " PAK", " MOZ", " PRY", " NIC", " SLV", " IRQ", " GTM", " VNM", " LBN", " PHL", " SUR", " PAK", " SUR", " UKR", " NGA", " AZE", " RWA", " ALB", " AZE", " KEN", " UGA", " GHA", " COL", " IRQ", " ECU", " SEN", " HND", " AZE", " ISR", " GTM", " IRQ", " RWA", " AZE", " PAK", " KHM", " PAN", " BOL", " MNG", " MEX", " ALB", " MNG", " ECU", " MEX", " MNG", " LBN", " ARM", " VUT", " ECU", " BOL", " ARM", " NIC", " IDN", " GTM", " UKR", " MOZ", " BOL", " ALB", " PHL", " USA", " ARM", " ALB", " CRI", " UGA", " MOZ", " MEX", " NIC", " UGA", " MNG", " SLV", " PER", " UKR", " SLE", " ALB", " ARM", " GHA", " TJK", " VNM", " ALB", " ARM", " LBN", " SLV", " WSM", " BOL", " KEN", " VNM", " MOZ", " YEM", " UKR", " ARM", " ARM", " UGA", " VNM", " VNM", " TJK", " PHL", " KHM", " MNG", " LSO", " PHL", " YEM", " KEN", " TJK", " IND", " NIC", " GTM", " USA", " SLV", " LBN", " ZMB", " TJK", " LBN", " IND", " KHM", " IDN", " YEM", " SLV", " TJK", " HTI", " BOL", " IDN", " LBN", " MEX", " PER", " PHL", " YEM", " TJK", " PER", " KEN", " LSO", " LBN", " USA", " PER", " NIC", " HND", " NGA", " KHM", " KHM", " LAO", " KHM"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 3.0, 2.0, 2.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 1.0, 2.0, 4.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 4.0, 1.0, 1.0, 1.0, 3.0, 1.0, 2.0, 3.0, 2.0, 4.0, 3.0, 2.0, 3.0, 3.0, 7.0, 2.0, 5.0, 2.0, 6.0, 2.0, 4.0, 3.0, 6.0, 5.0, 8.0, 7.0, 2.0, 7.0, 4.0, 1.0, 5.0, 4.0, 3.0, 15.0, 2.0, 11.0, 3.0, 3.0, 21.0, 4.0, 5.0, 7.0, 3.0, 8.0, 10.0, 14.0, 11.0, 2.0, 7.0, 10.0, 6.0, 4.0, 2.0, 9.0, 2.0, 16.0, 3.0, 16.0, 15.0, 6.0, 14.0, 6.0, 9.0, 6.0, 4.0, 12.0, 14.0, 9.0, 7.0, 7.0, 4.0, 8.0, 6.0, 9.0, 15.0, 26.0, 13.0, 11.0, 27.0, 6.0, 14.0, 34.0, 2.0, 16.0, 10.0, 13.0, 29.0, 40.0, 17.0, 36.0, 17.0, 19.0, 31.0, 35.0, 18.0, 16.0, 26.0, 24.0, 58.0, 32.0, 58.0, 24.0, 29.0, 22.0, 51.0, 31.0, 30.0, 254.0, 40.0, 74.0, 45.0, 23.0, 32.0, 45.0, 257.0, 42.0, 18.0, 62.0, 364.0, 41.0, 19.0, 55.0, 497.0, 87.0, 145.0, 98.0, 223.0, 107.0, 79.0, 13.0, 81.0, 57.0, 44.0, 142.0, 61.0, 92.0, 114.0, 35.0, 230.0, 168.0, 220.0, 413.0, 110.0, 306.0, 114.0, 459.0, 307.0, 1022.0, 266.0, 390.0, 228.0, 1716.0, 366.0, 234.0, 62.0, 408.0, 629.0, 499.0, 2962.0, 966.0, 736.0, 1459.0, 16362.0], "type": "choropleth"}], "name": "Personal Use"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Services<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" PRI", " JOR", " KHM", " MDG", " SEN", " SSD", " SSD", " SSD", " MDG", " IDN", " MDG", " GHA", " UGA", " TGO", " LBR", " MDG", " SEN", " TGO", " SLE", " HTI", " IND", " PHL", " IDN", " PAN", " MOZ", " HND", " MOZ", " TUR", " EGY", " GTM", " PAK", " PAK", " MLI", " EGY", " EGY", " HTI", " IND", " CMR", " CMR", " UGA", " IND", " JOR", " CMR", " CMR", " YEM", " YEM", " PHL", " SSD", " NPL", " HTI", " CRI", " PAK", " MDG", " WSM", " IND", " LBR", " GTM", " PAK", " ZAF", " ZAF", " ZMB", " CMR", " MLI", " JOR", " GHA", " PHL", " SLE", " MOZ", " HTI", " ZWE", " KEN", " ZWE", " PAK", " PAN", " KHM", " LBR", " SLB", " ECU", " ARM", " BRA", " KEN", " RWA", " PHL", " JOR", " EGY", " CMR", " SEN", " IND", " TGO", " ECU", " CMR", " ZAF", " EGY", " LBR", " ARM", " MNG", " GEO", " AZE", " BFA", " TJK", " MLI", " MEX", " SLB", " RWA", " PER", " IND", " TUR", " JOR", " UKR", " BRA", " MOZ", " SUR", " TJK", " HTI", " TJK", " TLS", " TGO", " MNG", " KEN", " GHA", " TGO", " PRY", " LBR", " IDN", " IDN", " UGA", " BFA", " CMR", " ECU", " ZAF", " AZE", " HTI", " PER", " PHL", " BRA", " HND", " HND", " CRI", " SEN", " CRI", " YEM", " SLV", " YEM", " CMR", " YEM", " UGA", " SLE", " PHL", " TLS", " ECU", " KHM", " ZWE", " ZWE", " SLV", " SLE", " ZWE", " HTI", " NPL", " LBN", " PAK", " NIC", " CMR", " SLV", " SLE", " SEN", " CRI", " COL", " YEM", " GTM", " EGY", " YEM", " EGY", " GEO", " SSD", " ECU", " KEN", " IND", " CRI", " MDG", " ZAF", " VNM", " AZE", " SLE", " BEN", " SLE", " CMR", " MEX", " JOR", " BFA", " BRA", " KHM", " BFA", " PRY", " PER", " NIC", " TJK", " YEM", " MOZ", " CMR", " CMR", " EGY", " PAK", " COL", " GEO", " BFA", " EGY", " LBR", " NIC", " WSM", " GTM", " PAN", " GHA", " AZE", " HND", " TJK", " BDI", " GEO", " SLB", " UKR", " RWA", " ARM", " LBN", " WSM", " BFA", " HND", " GEO", " SSD", " COL", " LBR", " BRA", " SEN", " GTM", " ALB", " KGZ", " COL", " ALB", " ALB", " EGY", " BFA", " WSM", " TJK", " TLS", " RWA", " ALB", " ALB", " TGO", " CMR", " UGA", " ALB", " ALB", " LBN", " CHN", " GHA", " IDN", " HTI", " LBR", " COL", " KEN", " MEX", " SEN", " MEX", " KEN", " CRI", " AZE", " MDG", " SUR", " MOZ", " YEM", " IDN", " TGO", " ARM", " EGY", " EGY", " KHM", " AZE", " ARM", " BOL", " LBR", " MEX", " PHL", " BFA", " NIC", " MEX", " MDG", " PHL", " CRI", " IDN", " MOZ", " GEO", " COL", " PAK", " YEM", " NGA", " BRA", " TUR", " KHM", " ZWE", " SLE", " MOZ", " PHL", " CRI", " PHL", " MLI", " UGA", " COL", " KHM", " MEX", " PAK", " MOZ", " HND", " WSM", " BOL", " JOR", " ARM", " SLE", " PAN", " PAN", " ARM", " SEN", " SLV", " KEN", " TGO", " CRI", " WSM", " NPL", " ARM", " GEO", " PER", " TGO", " ZAF", " JOR", " WSM", " GHA", " ZMB", " CMR", " HND", " ZWE", " CMR", " EGY", " SLE", " IDN", " BOL", " YEM", " CMR", " HTI", " IRQ", " PAN", " ZWE", " KHM", " TGO", " HND", " CMR", " ZWE", " BRA", " BDI", " JOR", " IDN", " ZWE", " SLV", " NIC", " UGA", " EGY", " LBN", " HND", " KHM", " BFA", " KEN", " LBR", " PAK", " GEO", " YEM", " PAK", " EGY", " HTI", " KEN", " USA", " USA", " IND", " NIC", " VNM", " ARM", " PHL", " LBN", " SEN", " JOR", " GEO", " JOR", " PRY", " MDG", " HTI", " PER", " ZWE", " HTI", " IRQ", " IRQ", " IRQ", " IDN", " CHN", " CHN", " UKR", " NIC", " HND", " IND", " IND", " SLE", " EGY", " KGZ", " ZMB", " VNM", " IND", " PAN", " UGA", " UGA", " COL", " MEX", " GEO", " UGA", " JOR", " PRY", " TJK", " ECU", " UGA", " KEN", " BRA", " SSD", " RWA", " TJK", " RWA", " PAK", " KGZ", " TLS", " IDN", " YEM", " TUR", " ZAF", " ARM", " SLE", " ZAF", " SUR", " MOZ", " UKR", " ZAF", " MEX", " WSM", " NIC", " KHM", " MNG", " LBN", " BEN", " ARM", " LBN", " GEO", " CMR", " MNG", " NIC", " RWA", " GHA", " PHL", " PHL", " KHM", " KGZ", " PRY", " PAN", " MNG", " TGO", " GTM", " GTM", " PRY", " GTM", " KHM", " COL", " IRQ", " HND", " IND", " LBN", " BOL", " ZAF", " UGA", " ARM", " UGA", " ECU", " KEN", " LBN", " MOZ", " MEX", " ECU", " PRY", " PRY", " KEN", " KHM", " PER", " TJK", " MEX", " ALB", " PER", " ISR", " GHA", " MEX", " LBN", " THA", " AZE", " PHL", " LBN", " ALB", " GHA", " MEX", " BFA", " DOM", " USA", " USA", " COG", " ECU", " BRA", " CRI", " RWA", " NIC", " ARM", " TLS", " KEN", " MEX", " TLS", " PHL", " PAK", " USA", " MDG", " KEN", " ALB", " ARM", " HND", " IRQ", " TJK", " PER", " HND", " ECU", " UGA", " MLI", " PAK", " MLI", " COL", " HND", " BFA", " ISR", " HND", " KGZ", " MWI", " COL", " ALB", " LBN", " MLI", " UGA", " AZE", " PAK", " RWA", " PRI", " MNG", " GTM", " MEX", " KEN", " TGO", " BFA", " PER", " ECU", " CMR", " HTI", " SOM", " NIC", " UGA", " RWA", " CHN", " MNG", " TLS", " PER", " EGY", " PRY", " ISR", " IND", " IRQ", " VNM", " CRI", " IND", " AZE", " ARM", " KHM", " SEN", " CMR", " NIC", " JOR", " NIC", " GTM", " GTM", " PRY", " IRQ", " PAK", " ZWE", " ECU", " NIC", " VNM", " RWA", " GEO", " PAK", " BOL", " COG", " LBN", " RWA", " IND", " ZAF", " UGA", " MWI", " CRI", " PER", " ECU", " CHN", " PER", " RWA", " JOR", " LBN", " PHL", " LBN", " CRI", " BEN", " COG", " ALB", " SLB", " TLS", " USA", " HTI", " SLV", " USA", " RWA", " GTM", " ECU", " LBN", " BEN", " GTM", " NIC", " COL", " AZE", " UKR", " DOM", " UGA", " KHM", " HND", " PAK", " COL", " ECU", " SLV", " SOM", " LBR", " ZAF", " VNM", " GTM", " NIC", " BOL", " BOL", " GEO", " BDI", " MEX", " RWA", " KGZ", " KHM", " WSM", " GEO", " EGY", " VNM", " TJK", " JOR", " GTM", " IRQ", " BOL", " BOL", " IND", " IND", " SLE", " NIC", " PRY", " KEN", " VNM", " ISR", " COL", " CHN", " PER", " COG", " MEX", " USA", " GEO", " COG", " IND", " CRI", " BOL", " SLV", " ALB", " NPL", " PRY", " PER", " TUR", " JOR", " BDI", " MDG", " DOM", " UGA", " PHL", " LBN", " MNG", " RWA", " NPL", " KEN", " VNM", " PAK", " PER", " LBN", " JOR", " AZE", " VCT", " TLS", " GHA", " HND", " KEN", " USA", " BOL", " ECU", " SEN", " MNG", " HTI", " IDN", " BRA", " VNM", " VNM", " VNM", " NIC", " USA", " AZE", " SLV", " SLB", " EGY", " JOR", " USA", " TLS", " HND", " YEM", " VNM", " TGO", " ALB", " BDI", " HTI", " ECU", " KEN", " JOR", " IRQ", " MRT", " HND", " MEX", " LBN", " GHA", " CHN", " ECU", " KEN", " RWA", " ARM", " BDI", " MNG", " VNM", " ARM", " COL", " RWA", " MDG", " PHL", " BOL", " CMR", " HND", " MNG", " ARM", " RWA", " JOR", " UGA", " USA", " SEN", " MNG", " MOZ", " ECU", " ZAF", " KEN", " ARM", " ARM", " BOL", " PER", " TGO", " ISR", " SLV", " COG", " KEN", " SLV", " ALB", " PHL", " PER", " CMR", " YEM", " UGA", " USA", " KHM", " USA", " IND", " KEN", " COL", " PER", " IRQ", " SLV", " ECU", " SLE", " GEO", " PHL", " PRY", " ARM", " MEX", " BOL", " UGA", " VNM", " COL", " PAK", " ZWE", " MEX", " COL", " COL", " HND", " SLV", " BOL", " PAK", " KEN", " VCT", " BRA", " ECU", " KHM", " UGA", " AZE", " MNG", " BOL", " HTI", " CMR", " LBN", " CRI", " ARM", " LBN", " HTI", " COL", " PAK", " KEN", " YEM", " PRI", " EGY", " GEO", " PER", " IRQ", " SLV", " SLV", " TGO", " PAN", " JOR", " USA", " PAK", " VNM", " UKR", " KGZ", " PHL", " TUR", " ECU", " BOL", " PRY", " PER", " GHA", " COL", " SLV", " PHL", " SLE", " GHA", " SEN", " JOR", " PAK", " AZE", " TJK", " NIC", " BOL", " MEX", " LBN", " MEX", " CRI", " IRQ", " USA", " ZWE", " ECU", " ISR", " COL", " CRI", " KEN", " LBN", " AZE", " KHM", " BOL", " COL", " GTM", " ISR", " VNM", " LBN", " NIC", " SLV", " GHA", " PHL", " IND", " TUR", " BFA", " YEM", " USA", " UGA", " BFA", " KHM", " TUR", " SLV", " PHL", " KEN", " NIC", " GEO", " NIC", " BOL", " GHA", " BOL", " IRQ", " COL", " TJK", " ZWE", " COL", " MEX", " PAK", " KEN", " PHL", " IRQ", " NIC", " COL", " VNM", " PHL", " SLV", " USA", " PAK", " DOM", " ZMB", " PER", " USA", " BOL", " NIC", " GTM", " LBN", " DOM", " BOL", " USA", " ARM", " COL", " JOR", " PHL", " ECU", " KEN", " USA", " PER", " IND", " ZWE", " RWA", " ECU", " TJK", " ECU", " BOL", " MWI", " WSM", " PER", " PHL", " LBN", " MOZ", " BOL", " PRY", " PER", " COL", " UGA", " PER", " GEO", " WSM", " BRA", " PRY", " GHA", " ARM", " MEX", " ARM", " TJK", " UGA", " SLV", " LBN", " MEX", " JOR", " USA", " GTM", " BOL", " IRQ", " VNM", " SLV", " VNM", " JOR", " PHL", " USA", " SLV", " COL", " PER", " VNM", " ISR", " PER", " PHL", " WSM", " RWA", " PRY", " KEN", " ECU", " SLV", " GTM", " PAK", " USA", " USA", " SLV", " LBN", " UGA", " USA", " PER", " PHL", " VNM", " USA", " BOL", " ECU", " RWA", " PAK", " PAK", " KEN", " IND", " LBN", " COL", " MWI", " PHL", " PER", " COL", " USA", " PHL", " PRY", " KEN", " KEN", " BOL", " BOL", " IDN", " ZMB", " LBN", " IRQ", " COL", " BOL", " TJK", " PAK", " PRY", " PRY", " USA"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 4.0, 1.0, 4.0, 1.0, 6.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 2.0, 1.0, 3.0, 1.0, 2.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 2.0, 2.0, 3.0, 2.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 2.0, 4.0, 1.0, 4.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 7.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 4.0, 2.0, 2.0, 3.0, 3.0, 1.0, 2.0, 2.0, 2.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 4.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 3.0, 2.0, 1.0, 3.0, 1.0, 2.0, 2.0, 4.0, 2.0, 2.0, 2.0, 2.0, 5.0, 2.0, 1.0, 2.0, 3.0, 2.0, 2.0, 1.0, 2.0, 3.0, 3.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 3.0, 3.0, 3.0, 1.0, 4.0, 3.0, 1.0, 2.0, 3.0, 3.0, 2.0, 7.0, 2.0, 6.0, 5.0, 3.0, 2.0, 5.0, 3.0, 3.0, 3.0, 2.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 4.0, 3.0, 2.0, 5.0, 1.0, 3.0, 7.0, 1.0, 1.0, 4.0, 2.0, 1.0, 2.0, 3.0, 4.0, 2.0, 7.0, 3.0, 3.0, 6.0, 1.0, 2.0, 5.0, 2.0, 1.0, 5.0, 1.0, 1.0, 2.0, 2.0, 3.0, 3.0, 4.0, 6.0, 3.0, 1.0, 1.0, 2.0, 7.0, 3.0, 2.0, 3.0, 2.0, 3.0, 2.0, 4.0, 2.0, 2.0, 7.0, 3.0, 4.0, 1.0, 2.0, 4.0, 7.0, 4.0, 6.0, 1.0, 1.0, 3.0, 4.0, 2.0, 3.0, 7.0, 3.0, 4.0, 1.0, 4.0, 3.0, 1.0, 14.0, 1.0, 2.0, 4.0, 2.0, 1.0, 1.0, 1.0, 5.0, 1.0, 1.0, 1.0, 4.0, 3.0, 6.0, 3.0, 4.0, 3.0, 2.0, 1.0, 2.0, 7.0, 2.0, 5.0, 7.0, 3.0, 1.0, 2.0, 4.0, 3.0, 1.0, 2.0, 4.0, 7.0, 6.0, 1.0, 6.0, 1.0, 1.0, 2.0, 11.0, 2.0, 3.0, 7.0, 8.0, 6.0, 3.0, 5.0, 3.0, 3.0, 2.0, 8.0, 1.0, 2.0, 1.0, 3.0, 6.0, 5.0, 2.0, 1.0, 6.0, 4.0, 3.0, 1.0, 14.0, 4.0, 4.0, 1.0, 2.0, 10.0, 7.0, 5.0, 4.0, 3.0, 2.0, 3.0, 5.0, 7.0, 1.0, 2.0, 4.0, 4.0, 5.0, 2.0, 6.0, 4.0, 2.0, 4.0, 3.0, 6.0, 2.0, 6.0, 4.0, 7.0, 3.0, 7.0, 6.0, 4.0, 4.0, 5.0, 11.0, 5.0, 5.0, 4.0, 6.0, 5.0, 3.0, 1.0, 6.0, 3.0, 4.0, 2.0, 4.0, 7.0, 4.0, 6.0, 3.0, 3.0, 2.0, 1.0, 1.0, 1.0, 1.0, 6.0, 3.0, 2.0, 1.0, 6.0, 3.0, 6.0, 13.0, 3.0, 5.0, 12.0, 14.0, 1.0, 15.0, 13.0, 4.0, 3.0, 7.0, 2.0, 6.0, 7.0, 7.0, 5.0, 6.0, 6.0, 14.0, 7.0, 9.0, 6.0, 3.0, 1.0, 8.0, 3.0, 3.0, 14.0, 5.0, 5.0, 2.0, 8.0, 5.0, 11.0, 3.0, 2.0, 2.0, 3.0, 3.0, 10.0, 21.0, 4.0, 6.0, 6.0, 19.0, 2.0, 1.0, 2.0, 9.0, 1.0, 2.0, 4.0, 6.0, 6.0, 10.0, 2.0, 3.0, 8.0, 3.0, 2.0, 3.0, 8.0, 4.0, 4.0, 10.0, 4.0, 15.0, 8.0, 6.0, 11.0, 5.0, 11.0, 5.0, 3.0, 16.0, 6.0, 7.0, 12.0, 2.0, 5.0, 2.0, 12.0, 8.0, 1.0, 6.0, 4.0, 18.0, 9.0, 13.0, 5.0, 5.0, 11.0, 6.0, 3.0, 7.0, 1.0, 7.0, 6.0, 18.0, 4.0, 8.0, 10.0, 1.0, 7.0, 7.0, 10.0, 2.0, 14.0, 7.0, 2.0, 2.0, 4.0, 7.0, 6.0, 10.0, 2.0, 11.0, 13.0, 4.0, 5.0, 3.0, 18.0, 8.0, 14.0, 19.0, 14.0, 9.0, 13.0, 2.0, 25.0, 5.0, 4.0, 5.0, 9.0, 6.0, 8.0, 11.0, 2.0, 7.0, 1.0, 12.0, 16.0, 5.0, 4.0, 14.0, 8.0, 10.0, 6.0, 4.0, 4.0, 4.0, 6.0, 8.0, 7.0, 11.0, 16.0, 2.0, 30.0, 8.0, 2.0, 18.0, 4.0, 10.0, 2.0, 11.0, 2.0, 9.0, 2.0, 16.0, 11.0, 6.0, 14.0, 8.0, 5.0, 3.0, 8.0, 25.0, 8.0, 4.0, 37.0, 4.0, 26.0, 27.0, 10.0, 9.0, 3.0, 32.0, 27.0, 8.0, 31.0, 14.0, 10.0, 6.0, 6.0, 3.0, 14.0, 9.0, 17.0, 33.0, 4.0, 4.0, 13.0, 15.0, 12.0, 6.0, 16.0, 5.0, 8.0, 6.0, 10.0, 11.0, 3.0, 6.0, 17.0, 11.0, 19.0, 11.0, 3.0, 15.0, 24.0, 18.0, 4.0, 21.0, 11.0, 5.0, 19.0, 5.0, 41.0, 10.0, 1.0, 1.0, 20.0, 4.0, 8.0, 11.0, 6.0, 10.0, 35.0, 3.0, 10.0, 4.0, 7.0, 8.0, 16.0, 19.0, 1.0, 60.0, 36.0, 10.0, 41.0, 21.0, 8.0, 11.0, 25.0, 16.0, 21.0, 4.0, 13.0, 6.0, 39.0, 14.0, 8.0, 43.0, 15.0, 17.0, 18.0, 17.0, 71.0, 4.0, 31.0, 3.0, 49.0, 27.0, 17.0, 61.0, 12.0, 44.0, 17.0, 29.0, 5.0, 24.0, 2.0, 42.0, 47.0, 23.0, 14.0, 3.0, 30.0, 22.0, 27.0, 8.0, 33.0, 11.0, 18.0, 11.0, 10.0, 3.0, 12.0, 27.0, 53.0, 24.0, 16.0, 40.0, 27.0, 40.0, 38.0, 22.0, 39.0, 50.0, 6.0, 15.0, 22.0, 26.0, 34.0, 20.0, 14.0, 14.0, 6.0, 83.0, 17.0, 22.0, 30.0, 19.0, 19.0, 45.0, 59.0, 79.0, 28.0, 7.0, 75.0, 23.0, 30.0, 12.0, 47.0, 41.0, 108.0, 1.0, 25.0, 8.0, 68.0, 24.0, 12.0, 28.0, 73.0, 72.0, 29.0, 20.0, 28.0, 18.0, 31.0, 46.0, 38.0, 80.0, 39.0, 31.0, 27.0, 30.0, 83.0, 26.0, 50.0, 46.0, 20.0, 19.0, 25.0, 15.0, 24.0, 5.0, 9.0, 49.0, 24.0, 10.0, 85.0, 23.0, 66.0, 29.0, 28.0, 52.0, 20.0, 69.0, 34.0, 10.0, 24.0, 29.0, 51.0, 65.0, 45.0, 94.0, 71.0, 104.0, 33.0, 62.0, 10.0, 5.0, 50.0, 44.0, 23.0, 62.0, 89.0, 113.0, 49.0, 32.0, 65.0, 32.0, 21.0, 20.0, 22.0, 98.0, 55.0, 65.0, 79.0, 21.0, 124.0, 146.0, 109.0, 24.0, 66.0, 99.0, 38.0, 125.0, 68.0, 15.0, 120.0, 16.0, 76.0, 58.0, 14.0, 38.0, 75.0, 44.0, 55.0, 9.0, 40.0, 24.0, 64.0, 120.0, 55.0, 130.0, 60.0, 172.0, 18.0, 61.0, 140.0, 64.0, 27.0, 71.0, 81.0, 79.0, 68.0, 113.0, 69.0, 52.0, 177.0, 62.0, 86.0, 40.0, 17.0, 47.0, 154.0, 126.0, 64.0, 62.0, 105.0, 39.0, 28.0, 94.0, 78.0, 47.0, 97.0, 207.0, 195.0, 174.0, 73.0, 54.0, 88.0, 27.0, 75.0, 90.0, 25.0, 116.0, 183.0, 104.0, 104.0, 340.0, 37.0, 181.0, 169.0, 95.0, 92.0, 40.0, 115.0, 415.0, 189.0, 257.0, 45.0, 276.0, 153.0, 279.0, 72.0, 412.0, 45.0, 86.0, 312.0, 152.0, 446.0, 75.0, 155.0, 505.0, 136.0, 115.0, 142.0, 258.0, 77.0, 817.0, 778.0, 938.0, 699.0, 242.0, 477.0, 261.0, 924.0, 356.0, 681.0, 172.0, 1175.0, 184.0, 1280.0, 1946.0, 236.0, 428.0, 875.0, 383.0, 518.0, 381.0, 1472.0, 722.0, 2101.0, 3723.0, 374.0, 490.0, 1323.0], "type": "choropleth"}], "name": "Services"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Agriculture<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" RWA", " ARM", " PHL", " KEN", " KEN", " MOZ", " CMR", " ZMB", " CMR", " TUR", " NPL", " SOM", " TGO", " TUR", " YEM", " SLE", " LBR", " ZMB", " TUR", " WSM", " MDG", " WSM", " SLE", " SLE", " MDG", " KHM", " NPL", " PAK", " PAN", " ZWE", " MNG", " MNG", " MOZ", " MDG", " ZWE", " UKR", " UGA", " UKR", " GTM", " TGO", " LBR", " IDN", " COL", " TUR", " TJK", " CHN", " MLI", " KHM", " PER", " MNG", " MWI", " HTI", " SOM", " SLE", " MWI", " SEN", " COL", " SSD", " ZAF", " YEM", " CMR", " TJK", " ZWE", " TGO", " WSM", " TUR", " BOL", " KGZ", " GHA", " UGA", " IRQ", " SUR", " NPL", " WSM", " YEM", " MLI", " EGY", " ECU", " SUR", " GTM", " UKR", " GHA", " KHM", " USA", " PAN", " MOZ", " TLS", " KGZ", " CMR", " BFA", " JOR", " ARM", " KGZ", " IDN", " JOR", " CHN", " TJK", " TGO", " KGZ", " HTI", " SLB", " EGY", " SLE", " MLI", " BRA", " ECU", " TLS", " ALB", " GHA", " YEM", " TUR", " MWI", " SLV", " SLV", " EGY", " YEM", " CHN", " GHA", " MWI", " DOM", " GEO", " IDN", " TGO", " ZAF", " CHN", " KEN", " COL", " BFA", " TLS", " CHL", " DOM", " RWA", " SSD", " BOL", " MDG", " EGY", " KGZ", " TGO", " ZWE", " KEN", " MOZ", " CRI", " ALB", " SLB", " MWI", " IDN", " ALB", " UKR", " BDI", " NPL", " MWI", " PHL", " UGA", " PAK", " BFA", " NPL", " ZAF", " BDI", " LBN", " ZWE", " PAN", " USA", " GTM", " PRY", " AZE", " COG", " TLS", " CHN", " SEN", " GEO", " TGO", " EGY", " NIC", " PER", " ZWE", " NGA", " WSM", " MDG", " JOR", " ISR", " IDN", " VCT", " NPL", " COL", " TJK", " CHN", " COG", " LBN", " ZAF", " COG", " NIC", " NIC", " MDG", " TJK", " RWA", " VNM", " KHM", " PRY", " SUR", " WSM", " HTI", " ZMB", " PAN", " GTM", " IRQ", " PER", " LBN", " HND", " NPL", " ZAF", " MLI", " ALB", " TLS", " UKR", " VCT", " MNG", " PAK", " IND", " BFA", " MNG", " HTI", " IND", " CMR", " BDI", " JOR", " CRI", " HND", " NIC", " USA", " ECU", " ARM", " BFA", " IND", " NPL", " HTI", " PHL", " CMR", " IND", " CMR", " BOL", " MEX", " SLE", " TLS", " MLI", " HND", " ALB", " CHN", " PAN", " NIC", " LBN", " ZWE", " ALB", " BDI", " YEM", " MDG", " HND", " CRI", " IDN", " PAN", " UKR", " BDI", " BOL", " GHA", " SUR", " RWA", " TLS", " GEO", " BOL", " LBN", " MOZ", " JOR", " LBN", " GEO", " LBN", " ARM", " PAK", " ALB", " SLV", " THA", " UGA", " LBN", " CRI", " HTI", " PER", " KGZ", " BOL", " MWI", " PAN", " WSM", " JOR", " GEO", " ARM", " AZE", " HTI", " MNG", " ZWE", " PHL", " MWI", " ZMB", " MWI", " JOR", " HND", " BRA", " KEN", " PRY", " VNM", " PAN", " KEN", " WSM", " UKR", " ZWE", " MOZ", " MEX", " ALB", " SLB", " PAK", " JOR", " BDI", " RWA", " SLB", " PRY", " ARM", " MDG", " KHM", " MEX", " MDG", " SLB", " BLZ", " NPL", " EGY", " MOZ", " GTM", " CHN", " NIC", " YEM", " BEN", " SEN", " RWA", " PRY", " BOL", " NIC", " IND", " TLS", " ZMB", " COL", " EGY", " EGY", " MLI", " GEO", " PHL", " GTM", " PER", " KGZ", " HND", " HTI", " SEN", " NGA", " GTM", " VNM", " TJK", " SLV", " UGA", " UGA", " KHM", " AZE", " BLZ", " KEN", " TLS", " IND", " SEN", " VNM", " AZE", " PER", " MEX", " PRY", " RWA", " PER", " JOR", " BOL", " PRI", " PRY", " BFA", " UGA", " ZWE", " ECU", " LBN", " AZE", " LBN", " PRY", " COL", " RWA", " MEX", " KHM", " CMR", " MDG", " IND", " PHL", " SLV", " HND", " CRI", " COL", " AZE", " CRI", " GEO", " PHL", " MLI", " COL", " GTM", " MOZ", " PAK", " SLE", " MDG", " SUR", " TJK", " SEN", " UGA", " KEN", " PRY", " MEX", " GEO", " CRI", " ARM", " BOL", " VNM", " RWA", " COL", " PAK", " KHM", " CMR", " IDN", " BOL", " GTM", " IND", " SLV", " MEX", " PAK", " ECU", " KGZ", " CRI", " ARM", " KEN", " PER", " NIC", " UGA", " GEO", " PER", " AZE", " PHL", " MLI", " CRI", " PHL", " MEX", " ALB", " SEN", " KEN", " IDN", " GEO", " SEN", " BFA", " THA", " ALB", " ALB", " ALB", " ARM", " EGY", " KHM", " NIC", " GHA", " PRY", " GHA", " UGA", " COL", " BFA", " KGZ", " ECU", " KGZ", " AZE", " PAK", " SUR", " UKR", " COL", " KHM", " GTM", " CRI", " KHM", " UGA", " PER", " COL", " ARM", " PHL", " ECU", " TJK", " UGA", " SLV", " WSM", " SEN", " ECU", " VNM", " SLV", " IND", " NIC", " PHL", " KHM", " COL", " MEX", " RWA", " BOL", " ZWE", " MEX", " SLV", " HND", " AZE", " PAK", " PRY", " IDN", " COL", " VNM", " ECU", " TJK", " KEN", " BOL", " ECU", " GTM", " GTM", " ARM", " ECU", " BOL", " NGA", " KGZ", " TJK", " SLV", " IND", " ECU", " TJK", " VNM", " SLV", " ARM", " BOL", " TJK", " WSM", " RWA", " MLI", " MLI", " MLI", " SLV", " PER", " KEN", " UGA", " VNM", " HND", " PER", " VNM", " VNM", " USA", " SLV", " KEN", " PER", " ARM", " KGZ", " PHL"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 4.0, 1.0, 3.0, 2.0, 2.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 2.0, 5.0, 2.0, 2.0, 3.0, 4.0, 1.0, 3.0, 2.0, 5.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 4.0, 2.0, 1.0, 1.0, 3.0, 2.0, 1.0, 3.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 3.0, 2.0, 5.0, 1.0, 2.0, 3.0, 1.0, 4.0, 3.0, 1.0, 3.0, 9.0, 2.0, 1.0, 3.0, 4.0, 2.0, 1.0, 1.0, 5.0, 3.0, 1.0, 8.0, 2.0, 8.0, 4.0, 2.0, 5.0, 6.0, 4.0, 1.0, 5.0, 2.0, 1.0, 2.0, 3.0, 13.0, 1.0, 1.0, 8.0, 5.0, 3.0, 4.0, 1.0, 1.0, 2.0, 8.0, 4.0, 12.0, 5.0, 3.0, 3.0, 4.0, 6.0, 7.0, 5.0, 6.0, 6.0, 2.0, 7.0, 3.0, 2.0, 1.0, 13.0, 3.0, 7.0, 9.0, 11.0, 7.0, 12.0, 5.0, 2.0, 7.0, 8.0, 4.0, 2.0, 7.0, 3.0, 4.0, 1.0, 7.0, 3.0, 4.0, 2.0, 10.0, 10.0, 11.0, 8.0, 9.0, 32.0, 11.0, 38.0, 6.0, 2.0, 12.0, 3.0, 20.0, 14.0, 12.0, 3.0, 1.0, 7.0, 3.0, 1.0, 8.0, 13.0, 23.0, 11.0, 6.0, 12.0, 9.0, 9.0, 11.0, 11.0, 13.0, 1.0, 13.0, 9.0, 6.0, 11.0, 8.0, 11.0, 35.0, 11.0, 11.0, 7.0, 14.0, 3.0, 3.0, 7.0, 23.0, 7.0, 13.0, 7.0, 15.0, 8.0, 31.0, 5.0, 9.0, 6.0, 21.0, 14.0, 2.0, 4.0, 6.0, 10.0, 26.0, 37.0, 8.0, 40.0, 33.0, 23.0, 32.0, 4.0, 6.0, 5.0, 21.0, 14.0, 19.0, 11.0, 5.0, 14.0, 16.0, 12.0, 16.0, 13.0, 6.0, 38.0, 60.0, 18.0, 21.0, 20.0, 12.0, 12.0, 5.0, 10.0, 92.0, 7.0, 3.0, 25.0, 22.0, 4.0, 13.0, 54.0, 17.0, 13.0, 15.0, 16.0, 12.0, 50.0, 17.0, 33.0, 33.0, 32.0, 13.0, 16.0, 30.0, 13.0, 30.0, 14.0, 14.0, 15.0, 34.0, 17.0, 18.0, 22.0, 22.0, 15.0, 16.0, 41.0, 61.0, 41.0, 43.0, 192.0, 24.0, 34.0, 1.0, 83.0, 24.0, 14.0, 13.0, 57.0, 17.0, 14.0, 40.0, 39.0, 10.0, 22.0, 38.0, 87.0, 30.0, 15.0, 19.0, 44.0, 15.0, 26.0, 130.0, 41.0, 24.0, 167.0, 57.0, 55.0, 132.0, 101.0, 62.0, 22.0, 18.0, 66.0, 112.0, 2.0, 43.0, 24.0, 13.0, 32.0, 59.0, 88.0, 70.0, 66.0, 90.0, 63.0, 131.0, 59.0, 43.0, 164.0, 59.0, 79.0, 47.0, 86.0, 34.0, 30.0, 75.0, 30.0, 60.0, 96.0, 110.0, 85.0, 92.0, 76.0, 63.0, 70.0, 156.0, 84.0, 67.0, 21.0, 42.0, 42.0, 77.0, 19.0, 27.0, 61.0, 92.0, 65.0, 50.0, 13.0, 24.0, 70.0, 95.0, 108.0, 95.0, 80.0, 71.0, 76.0, 25.0, 70.0, 33.0, 28.0, 92.0, 275.0, 186.0, 147.0, 274.0, 154.0, 105.0, 66.0, 194.0, 74.0, 79.0, 97.0, 281.0, 46.0, 143.0, 53.0, 180.0, 296.0, 40.0, 479.0, 48.0, 165.0, 88.0, 166.0, 338.0, 46.0, 46.0, 85.0, 95.0, 96.0, 126.0, 84.0, 63.0, 154.0, 367.0, 142.0, 363.0, 65.0, 98.0, 84.0, 181.0, 220.0, 40.0, 206.0, 166.0, 120.0, 116.0, 109.0, 324.0, 147.0, 162.0, 231.0, 109.0, 158.0, 139.0, 544.0, 417.0, 164.0, 479.0, 113.0, 130.0, 72.0, 548.0, 158.0, 174.0, 159.0, 229.0, 63.0, 156.0, 176.0, 177.0, 202.0, 313.0, 297.0, 290.0, 88.0, 83.0, 544.0, 366.0, 355.0, 244.0, 237.0, 350.0, 285.0, 229.0, 599.0, 127.0, 150.0, 634.0, 334.0, 140.0, 366.0, 412.0, 429.0, 363.0, 914.0, 359.0, 1298.0, 518.0, 755.0, 624.0, 862.0, 719.0, 224.0, 444.0, 320.0, 919.0, 1238.0, 569.0, 1718.0, 606.0, 1305.0, 390.0, 228.0, 440.0, 849.0, 165.0, 1328.0, 654.0, 504.0, 1677.0, 199.0, 500.0, 1104.0, 428.0, 846.0, 1200.0, 2340.0, 334.0, 805.0, 573.0, 589.0, 600.0, 1010.0, 602.0, 5889.0, 870.0, 1260.0, 1419.0, 1631.0, 1055.0, 1351.0, 602.0, 1464.0, 743.0, 794.0, 1600.0, 1682.0, 976.0, 1642.0, 670.0, 676.0, 2939.0, 1331.0, 3983.0, 2487.0, 856.0, 1760.0, 2054.0, 1362.0, 1448.0, 398.0, 4554.0, 5244.0, 2253.0, 2451.0, 3222.0, 11924.0], "type": "choropleth"}], "name": "Agriculture"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Education<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" NGA", " GTM", " PRY", " MOZ", " MOZ", " PAN", " TUR", " YEM", " YEM", " CRI", " ZWE", " YEM", " SSD", " PER", " MEX", " ZWE", " NPL", " COL", " SSD", " SEN", " IND", " MEX", " SEN", " VNM", " TUR", " PAK", " PAN", " SEN", " HND", " KGZ", " USA", " MWI", " SOM", " LBN", " CHN", " JOR", " PAN", " PAK", " ECU", " SLV", " GTM", " PRY", " CHN", " AZE", " GTM", " JOR", " AZE", " ARM", " SOM", " RWA", " GHA", " MNG", " ALB", " KHM", " GHA", " MNG", " GEO", " WSM", " KHM", " MOZ", " CRI", " MEX", " ECU", " HTI", " ISR", " NIC", " BOL", " ECU", " WSM", " NPL", " SLE", " BOL", " GTM", " ISR", " PRI", " SLV", " TJK", " PHL", " SLE", " ECU", " ZAF", " NIC", " ZWE", " TJK", " AZE", " SLV", " RWA", " GEO", " COL", " SLE", " MEX", " IDN", " PHL", " PER", " KEN", " UGA", " IDN", " TLS", " MNG", " IRQ", " ALB", " ZMB", " GHA", " RWA", " VNM", " IND", " TLS", " LBN", " IND", " COL", " KEN", " HND", " PER", " ZAF", " NGA", " VNM", " KGZ", " UGA", " IRQ", " KEN", " PHL", " USA", " ARM", " LBN", " ARM", " BOL", " NIC", " DOM", " PER", " PAK", " UGA", " KHM", " TJK", " JOR", " VNM", " LBN", " PRY"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 3.0, 4.0, 2.0, 2.0, 2.0, 3.0, 2.0, 4.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 3.0, 1.0, 3.0, 2.0, 6.0, 5.0, 4.0, 8.0, 1.0, 3.0, 4.0, 10.0, 3.0, 3.0, 6.0, 13.0, 3.0, 7.0, 3.0, 8.0, 21.0, 6.0, 7.0, 7.0, 6.0, 9.0, 22.0, 9.0, 9.0, 8.0, 4.0, 6.0, 10.0, 16.0, 7.0, 35.0, 1.0, 6.0, 18.0, 5.0, 5.0, 3.0, 43.0, 37.0, 78.0, 11.0, 29.0, 23.0, 73.0, 28.0, 59.0, 30.0, 64.0, 153.0, 59.0, 37.0, 24.0, 33.0, 100.0, 131.0, 84.0, 116.0, 118.0, 89.0, 95.0, 74.0, 35.0, 85.0, 42.0, 93.0, 350.0, 174.0, 260.0, 146.0, 172.0, 298.0, 767.0, 768.0, 98.0, 165.0, 177.0, 1063.0, 234.0, 335.0, 649.0, 225.0, 445.0, 1563.0, 177.0, 554.0, 557.0, 733.0, 846.0, 933.0, 178.0, 924.0, 1216.0, 1144.0, 1360.0, 2366.0, 2019.0, 2280.0, 1876.0, 3819.0], "type": "choropleth"}], "name": "Education"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Health<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" TGO", " MOZ", " GEO", " PRI", " BFA", " IND", " EGY", " MOZ", " CMR", " JOR", " UKR", " SSD", " NPL", " CMR", " SEN", " IDN", " SSD", " TGO", " MLI", " MOZ", " SEN", " CMR", " TLS", " UGA", " TGO", " ECU", " PRY", " AZE", " PRY", " ZAF", " MNG", " MOZ", " KHM", " KHM", " TUR", " COL", " PAK", " JOR", " JOR", " IND", " HND", " BFA", " CHN", " ZWE", " BRA", " SLE", " BDI", " ECU", " GEO", " CRI", " BRA", " MNG", " LBN", " SLE", " ECU", " KHM", " UGA", " ZWE", " MNG", " JOR", " CRI", " KEN", " IND", " TGO", " SLE", " HTI", " SOM", " MEX", " ZWE", " IRQ", " TUR", " CHN", " SEN", " MLI", " HND", " HND", " YEM", " HND", " ISR", " SLE", " TGO", " GTM", " LBN", " VNM", " MDG", " MNG", " SLV", " KHM", " ISR", " MEX", " SSD", " IDN", " KEN", " UGA", " ISR", " IND", " VNM", " RWA", " NIC", " GTM", " COL", " PHL", " ALB", " SLV", " PER", " AZE", " USA", " HND", " GEO", " COL", " ZMB", " KHM", " KHM", " USA", " IRQ", " ECU", " PAK", " LBN", " MEX", " NIC", " MLI", " ARM", " NIC", " ARM", " ECU", " TUR", " PRY", " KEN", " PER", " NIC", " BOL", " PHL", " PER", " LAO", " IND", " COG", " SLV", " LBN", " GEO", " COL", " ARM", " LBR", " TJK", " ARM", " PAK", " UGA", " GHA", " BOL", " RWA", " ALB", " PAK", " GHA", " GTM", " YEM", " TJK", " NPL", " HTI", " CHL", " MEX", " COL", " PHL", " BOL", " ALB", " UGA", " TJK", " MEX", " SLV", " VNM", " KEN", " PER", " ECU", " SSD", " MWI", " SLV", " VNM", " GHA", " GTM", " NIC", " SLV", " PER", " PER", " AZE", " SLE", " PHL", " VNM", " PHL", " UGA", " BOL", " LBN", " TJK", " ALB", " USA", " BOL", " PRY", " BOL", " LBN", " KEN", " ARM", " MEX", " TJK"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 4.0, 2.0, 3.0, 1.0, 1.0, 3.0, 1.0, 3.0, 1.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 2.0, 3.0, 1.0, 1.0, 2.0, 2.0, 5.0, 10.0, 10.0, 5.0, 2.0, 2.0, 5.0, 1.0, 1.0, 5.0, 1.0, 2.0, 4.0, 3.0, 7.0, 4.0, 6.0, 1.0, 8.0, 10.0, 5.0, 3.0, 2.0, 6.0, 1.0, 4.0, 3.0, 1.0, 5.0, 4.0, 5.0, 14.0, 12.0, 1.0, 11.0, 4.0, 1.0, 7.0, 9.0, 8.0, 10.0, 11.0, 7.0, 13.0, 6.0, 2.0, 8.0, 6.0, 10.0, 10.0, 8.0, 30.0, 2.0, 1.0, 10.0, 10.0, 5.0, 8.0, 16.0, 7.0, 11.0, 11.0, 5.0, 10.0, 3.0, 9.0, 28.0, 11.0, 7.0, 10.0, 38.0, 17.0, 18.0, 18.0, 2.0, 27.0, 15.0, 7.0, 33.0, 15.0, 26.0, 45.0, 22.0, 39.0, 19.0, 11.0, 14.0, 7.0, 20.0, 38.0, 11.0, 16.0, 10.0, 27.0, 4.0, 21.0, 1.0, 52.0, 46.0, 56.0, 14.0, 29.0, 51.0, 74.0, 16.0, 57.0, 70.0, 98.0, 48.0, 35.0, 1.0, 1.0, 78.0, 28.0, 11.0, 26.0, 127.0, 107.0, 58.0, 64.0, 66.0, 116.0, 246.0, 52.0, 281.0, 148.0, 56.0, 102.0, 171.0, 211.0, 71.0, 148.0, 132.0, 292.0, 410.0, 486.0, 695.0, 1495.0, 1959.0], "type": "choropleth"}], "name": "Health"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Wholesale<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" TGO", " GHA", " EGY", " WSM", " TJK", " SLV", " MOZ", " SEN", " YEM", " SLB", " MDG", " CMR", " MEX", " ECU", " IND", " IDN", " MLI", " TGO", " LBR", " CRI", " CMR", " MDG", " MOZ", " AZE", " ALB", " AZE", " JOR", " MWI", " HND", " TJK", " MNG", " SLV", " SLE", " UKR", " PRY", " NIC", " JOR", " ZAF", " CRI", " KHM", " PER", " KHM", " IND", " GHA", " BRA", " LBN", " MEX", " ARM", " GTM", " NIC", " SEN", " GTM", " KEN", " ECU", " EGY", " BOL", " VNM", " LBN", " KEN", " ZWE", " COL", " ALB", " ZWE", " VNM", " RWA", " ARM", " COL", " PAK", " PHL", " PHL", " NAM", " ZMB", " USA", " USA", " UGA", " PER", " MLI", " PAK", " UGA", " HTI", " HTI"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 3.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 4.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 2.0, 1.0, 2.0, 4.0, 2.0, 1.0, 2.0, 3.0, 1.0, 4.0, 2.0, 2.0, 2.0, 3.0, 4.0, 2.0, 1.0, 5.0, 3.0, 1.0, 15.0, 6.0, 6.0, 6.0, 4.0, 4.0, 6.0, 8.0, 14.0, 6.0, 8.0, 4.0, 1.0, 13.0, 29.0, 45.0, 38.0, 36.0, 7.0, 14.0, 10.0, 11.0, 42.0, 20.0, 1.0, 72.0, 30.0, 6.0, 32.0], "type": "choropleth"}], "name": "Wholesale"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Arts<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" MDG", " EGY", " CMR", " SLE", " PAK", " KEN", " BEN", " TJK", " ZAF", " TJK", " CMR", " CMR", " HTI", " GHA", " CMR", " MEX", " ARM", " ECU", " CRI", " TUR", " ECU", " PHL", " MOZ", " IND", " SLV", " ALB", " MDG", " BRA", " GHA", " CRI", " IDN", " PAK", " COL", " GHA", " PAN", " PHL", " SLE", " BRA", " KHM", " KEN", " NPL", " GEO", " EGY", " VNM", " PHL", " ZWE", " ARM", " LBN", " HND", " WSM", " MWI", " TGO", " ALB", " CMR", " HND", " HND", " JOR", " SSD", " PAK", " ARM", " GTM", " UGA", " JOR", " NPL", " KEN", " NIC", " BFA", " BFA", " SEN", " NPL", " TGO", " TUR", " UGA", " TJK", " MDG", " COL", " AZE", " IND", " TLS", " SLB", " MEX", " TJK", " ZAF", " USA", " COL", " TJK", " THA", " MDG", " EGY", " CRI", " AZE", " GEO", " AZE", " THA", " IND", " MDG", " BFA", " GEO", " MEX", " BRA", " ZWE", " TJK", " UGA", " KEN", " VCT", " IDN", " ZAF", " TLS", " YEM", " UGA", " COL", " SLB", " ISR", " SEN", " BDI", " BOL", " NIC", " MLI", " COL", " LBN", " SLE", " VNM", " MDG", " CHL", " UGA", " ISR", " NPL", " ALB", " KEN", " PHL", " VNM", " NIC", " KHM", " SLV", " COG", " IND", " KGZ", " ZWE", " MDG", " SLV", " PRY", " SEN", " RWA", " HTI", " TLS", " LBN", " SLV", " PER", " CRI", " VNM", " KHM", " PRY", " ZWE", " IRQ", " IRQ", " ECU", " BFA", " ECU", " PAK", " MNG", " ARM", " LBN", " TJK", " TUR", " PRY", " AFG", " UGA", " PHL", " RWA", " MLI", " HND", " PRI", " PAK", " BTN", " JOR", " CHN", " KGZ", " USA", " CHN", " IND", " GHA", " ECU", " ECU", " BOL", " LBN", " ARM", " ECU", " IDN", " MLI", " SEN", " MEX", " TJK", " CHN", " COL", " BRA", " BOL", " LBN", " SLV", " UGA", " ARM", " TUR", " BFA", " PAK", " PAK", " BOL", " IDN", " PER", " KEN", " JOR", " KHM", " GHA", " GTM", " PHL", " RWA", " TUR", " PER", " MEX", " GTM", " MLI", " JOR", " PAK", " SLV", " KEN", " WSM", " PRY", " LBN", " PER", " LBN", " NIC", " IND", " TUR", " HTI", " THA", " WSM", " THA", " GHA", " MEX", " PRY", " IND", " KHM", " PER", " PER", " ECU", " PHL", " MEX", " COL", " GTM", " SLV", " IDN", " GTM", " USA", " IND", " WSM", " PHL", " BOL", " PRY", " BOL", " BOL", " GTM", " BOL", " GTM", " PER", " PAK", " USA"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 1.0, 4.0, 2.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 3.0, 1.0, 1.0, 1.0, 2.0, 3.0, 4.0, 2.0, 11.0, 2.0, 4.0, 1.0, 1.0, 1.0, 2.0, 3.0, 1.0, 11.0, 1.0, 4.0, 4.0, 4.0, 1.0, 2.0, 6.0, 4.0, 1.0, 4.0, 3.0, 7.0, 6.0, 5.0, 2.0, 1.0, 2.0, 7.0, 14.0, 5.0, 2.0, 4.0, 8.0, 3.0, 2.0, 3.0, 3.0, 3.0, 3.0, 6.0, 1.0, 1.0, 6.0, 6.0, 1.0, 14.0, 28.0, 2.0, 1.0, 1.0, 1.0, 2.0, 7.0, 8.0, 4.0, 2.0, 2.0, 1.0, 3.0, 6.0, 5.0, 11.0, 3.0, 2.0, 2.0, 4.0, 2.0, 7.0, 7.0, 6.0, 1.0, 2.0, 2.0, 4.0, 6.0, 3.0, 13.0, 3.0, 6.0, 4.0, 18.0, 1.0, 3.0, 1.0, 12.0, 3.0, 14.0, 17.0, 2.0, 7.0, 9.0, 11.0, 1.0, 16.0, 1.0, 5.0, 34.0, 11.0, 3.0, 4.0, 2.0, 2.0, 10.0, 5.0, 11.0, 9.0, 9.0, 10.0, 24.0, 4.0, 7.0, 1.0, 1.0, 9.0, 9.0, 11.0, 31.0, 6.0, 8.0, 9.0, 15.0, 39.0, 3.0, 2.0, 4.0, 42.0, 6.0, 5.0, 25.0, 4.0, 33.0, 2.0, 17.0, 2.0, 2.0, 4.0, 3.0, 37.0, 7.0, 21.0, 16.0, 6.0, 16.0, 14.0, 11.0, 6.0, 21.0, 8.0, 7.0, 11.0, 3.0, 43.0, 19.0, 14.0, 20.0, 48.0, 35.0, 20.0, 87.0, 33.0, 74.0, 80.0, 26.0, 62.0, 19.0, 112.0, 28.0, 23.0, 2.0, 18.0, 145.0, 14.0, 121.0, 29.0, 17.0, 22.0, 28.0, 44.0, 133.0, 92.0, 200.0, 85.0, 18.0, 41.0, 43.0, 39.0, 60.0, 113.0, 206.0, 28.0, 36.0, 130.0, 42.0, 118.0, 26.0, 30.0, 179.0, 190.0, 63.0, 70.0, 135.0, 546.0, 60.0, 401.0, 88.0, 379.0, 100.0, 162.0, 73.0, 432.0, 438.0, 1233.0, 120.0, 111.0, 230.0, 158.0, 308.0, 246.0, 235.0, 466.0, 1823.0, 209.0], "type": "choropleth"}], "name": "Arts"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Food<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" HTI", " LBR", " TGO", " NGA", " CMR", " SSD", " SLE", " IND", " SSD", " MDG", " SSD", " HTI", " KGZ", " GHA", " TUR", " SSD", " SSD", " MDG", " ZMB", " MOZ", " PAK", " MEX", " COL", " CMR", " PRI", " ALB", " KHM", " SLB", " IDN", " SOM", " PHL", " MDG", " EGY", " LBR", " TUR", " TLS", " BEN", " GHA", " TUR", " SSD", " ZWE", " CMR", " EGY", " SSD", " ZMB", " SSD", " JOR", " SSD", " MNG", " TUR", " SSD", " SLV", " CRI", " YEM", " KGZ", " EGY", " IDN", " CMR", " TGO", " MOZ", " SSD", " MLI", " TUR", " TGO", " BEN", " AZE", " ZMB", " CMR", " ZWE", " TUR", " SSD", " BEN", " BRA", " ZWE", " CMR", " TGO", " SSD", " EGY", " UKR", " MDG", " TGO", " PRY", " UKR", " SOM", " SOM", " SOM", " NPL", " LBN", " ZAF", " YEM", " BEN", " KGZ", " SOM", " SLB", " BRA", " MNG", " DOM", " COL", " EGY", " TLS", " ISR", " PAN", " MWI", " HTI", " ZAF", " TUR", " VNM", " YEM", " CRI", " EGY", " CRI", " LBN", " SLB", " TJK", " LBR", " MOZ", " ARM", " ARM", " LBN", " BFA", " PAN", " PAN", " ARM", " VNM", " IND", " SEN", " SUR", " MOZ", " MOZ", " UKR", " IND", " PAN", " SLB", " CRI", " HTI", " SLE", " GTM", " BRA", " MDG", " GEO", " WSM", " HND", " EGY", " BFA", " NPL", " NPL", " ZAF", " MNG", " PHL", " ALB", " HND", " YEM", " CRI", " GEO", " MNG", " KHM", " ECU", " PAN", " HND", " ALB", " TGO", " SUR", " MNG", " BEN", " TJK", " CHL", " MDG", " CMR", " IDN", " ZAF", " BFA", " NPL", " SLB", " ZWE", " MEX", " PAK", " ZMB", " HND", " IDN", " GEO", " ZAF", " SEN", " UGA", " NIC", " MLI", " PAK", " USA", " IND", " PHL", " CHN", " CHL", " LBN", " CMR", " IRQ", " JOR", " GTM", " ALB", " MOZ", " MOZ", " ISR", " BRA", " NPL", " KHM", " ALB", " UKR", " WSM", " IDN", " ZMB", " MLI", " ZWE", " BEN", " BEN", " IDN", " ISR", " MOZ", " MOZ", " UKR", " MLI", " KEN", " KEN", " TGO", " MOZ", " DOM", " ISR", " PAN", " ALB", " TLS", " IRQ", " MOZ", " GEO", " BRA", " SEN", " AZE", " TGO", " MLI", " TUR", " GHA", " BRA", " IDN", " PAN", " LBN", " CRI", " COG", " ZMB", " LBR", " USA", " KEN", " SLE", " ARM", " SLB", " ISR", " SEN", " CRI", " VCT", " ZAF", " YEM", " ARM", " MNG", " GHA", " WSM", " DOM", " ZAF", " YEM", " KHM", " ZMB", " TGO", " ECU", " TLS", " PRY", " GEO", " MNG", " SOM", " CRI", " BEN", " YEM", " IND", " LBN", " ZAF", " KGZ", " PAK", " WSM", " UKR", " HND", " KGZ", " TJK", " JOR", " ARM", " CHN", " MOZ", " BDI", " VCT", " MOZ", " TJK", " PAK", " AZE", " ARM", " ISR", " AZE", " IRQ", " HND", " LBN", " JOR", " COG", " GHA", " EGY", " NIC", " HND", " BRA", " PER", " RWA", " SOM", " SLB", " JOR", " BDI", " BRA", " NPL", " ISR", " TLS", " ALB", " EGY", " GEO", " ALB", " ZWE", " MLI", " TUR", " IND", " CHN", " YEM", " MDG", " IRQ", " WSM", " ALB", " EGY", " BDI", " SEN", " CHN", " RWA", " CMR", " UKR", " BEN", " EGY", " SLE", " HTI", " GEO", " ZAF", " MDG", " SLE", " HND", " TJK", " MWI", " SEN", " PAK", " ZWE", " GTM", " CHN", " MEX", " ZWE", " GTM", " LBR", " EGY", " VNM", " IRQ", " DOM", " LBR", " KHM", " HTI", " MNG", " TUR", " HTI", " COL", " GTM", " IND", " IND", " YEM", " IRQ", " ALB", " ARM", " NPL", " ZAF", " SLV", " CRI", " BRA", " LBN", " ARM", " KHM", " CMR", " TLS", " DOM", " PAK", " NIC", " MEX", " TLS", " IRQ", " MOZ", " TJK", " MEX", " USA", " MDG", " BOL", " KGZ", " VCT", " BEN", " JOR", " MWI", " ZWE", " MDG", " IRQ", " BRA", " TGO", " MWI", " MEX", " RWA", " NIC", " MDG", " BFA", " JOR", " TUR", " TJK", " ZWE", " SLE", " MLI", " CRI", " UGA", " WSM", " RWA", " SLV", " BFA", " BRA", " SLB", " USA", " LBN", " TLS", " JOR", " SLB", " WSM", " COL", " DOM", " ISR", " MWI", " AZE", " ECU", " CHN", " TGO", " ALB", " GHA", " UGA", " SLV", " SOM", " SOM", " DOM", " KEN", " VCT", " SLE", " UKR", " MLI", " UKR", " IND", " BFA", " IDN", " CMR", " GTM", " CMR", " UKR", " NIC", " CRI", " PER", " ZMB", " ISR", " HND", " YEM", " ZWE", " AZE", " BFA", " PRY", " TLS", " ZMB", " ZAF", " CMR", " CMR", " ARM", " CMR", " HTI", " SLB", " BRA", " DOM", " CRI", " LBR", " YEM", " NIC", " TGO", " HTI", " GTM", " HND", " SEN", " SEN", " ECU", " IND", " KHM", " KHM", " PRI", " NIC", " PHL", " COL", " ZWE", " LBN", " PAK", " CRI", " ISR", " HND", " MNG", " LBR", " JOR", " CRI", " IND", " LBN", " UGA", " MNG", " VNM", " TLS", " BDI", " KGZ", " PHL", " MEX", " ECU", " PRY", " ZWE", " COG", " IDN", " BFA", " COG", " PHL", " LBR", " SLB", " TGO", " MNG", " MWI", " MWI", " LBR", " LBR", " BFA", " MWI", " TGO", " BEN", " ZWE", " KHM", " GHA", " AZE", " GTM", " BDI", " TLS", " MDG", " MLI", " ISR", " LBN", " ARM", " SLV", " KGZ", " KEN", " MDG", " COG", " USA", " USA", " SLE", " EGY", " PER", " YEM", " MEX", " ALB", " MEX", " MDG", " ECU", " PER", " COG", " AZE", " GTM", " HTI", " USA", " CMR", " RWA", " NIC", " GEO", " GTM", " CMR", " TJK", " IDN", " COL", " IND", " KGZ", " JOR", " JOR", " HND", " LBR", " MWI", " GEO", " MEX", " BDI", " ARM", " CMR", " HTI", " MWI", " IND", " VNM", " COL", " COG", " PAK", " BFA", " DOM", " IRQ", " MOZ", " HND", " DOM", " SLE", " COL", " ARM", " COG", " NIC", " TGO", " PER", " HND", " GHA", " HTI", " USA", " SLV", " MLI", " GTM", " LBN", " TLS", " COG", " BDI", " GEO", " NIC", " KHM", " PAK", " UGA", " USA", " GEO", " ECU", " MNG", " IDN", " KEN", " ZWE", " BOL", " COL", " BDI", " BFA", " PRY", " MEX", " DOM", " SEN", " AZE", " COG", " MDG", " TJK", " DOM", " COG", " KEN", " PRI", " BDI", " NIC", " ZWE", " PAK", " SEN", " TUR", " ZWE", " IND", " ARM", " LBR", " UGA", " PRY", " PER", " DOM", " SLE", " ECU", " BDI", " HTI", " KGZ", " BOL", " BOL", " COG", " GEO", " COL", " ECU", " KHM", " RWA", " VNM", " GHA", " BFA", " TJK", " TGO", " SLV", " ECU", " TGO", " VNM", " COL", " HTI", " JOR", " PAK", " COL", " ARM", " COL", " KHM", " PAK", " MEX", " PER", " KEN", " MDG", " PRY", " NIC", " NPL", " SLV", " BOL", " SLV", " SLV", " PER", " MEX", " TGO", " KHM", " MWI", " GHA", " WSM", " PAK", " NGA", " MWI", " ECU", " MLI", " PER", " USA", " NIC", " GEO", " SLE", " UGA", " GHA", " BOL", " PHL", " WSM", " UGA", " SEN", " MWI", " BEN", " NIC", " GHA", " IDN", " ECU", " GTM", " PER", " PRY", " HND", " LBN", " ARM", " RWA", " PHL", " VNM", " KHM", " SLE", " JOR", " SLE", " BDI", " NIC", " YEM", " RWA", " KEN", " BFA", " KHM", " UGA", " LBN", " UGA", " IND", " WSM", " SLE", " HTI", " PHL", " BOL", " SEN", " COL", " PER", " BDI", " SLV", " WSM", " UGA", " KEN", " TJK", " TJK", " GEO", " UGA", " GHA", " KEN", " SLE", " SEN", " VNM", " RWA", " MEX", " TGO", " SLV", " MLI", " COL", " GHA", " IRQ", " EGY", " PRY", " COL", " PRY", " AZE", " PRY", " JOR", " SLV", " GEO", " ECU", " MWI", " IND", " UGA", " PHL", " GHA", " RWA", " PHL", " ZWE", " SEN", " MEX", " TJK", " BFA", " PRY", " RWA", " PER", " BDI", " SLV", " COL", " SLV", " ECU", " VNM", " IND", " HTI", " ECU", " MLI", " BFA", " WSM", " GTM", " UGA", " SEN", " GTM", " ECU", " LBN", " KEN", " HND", " KEN", " TJK", " COL", " TJK", " COL", " BOL", " GHA", " RWA", " BOL", " BFA", " RWA", " RWA", " LBR", " GTM", " ECU", " PAK", " VNM", " BOL", " LBN", " GTM", " PRY", " UGA", " PHL", " KEN", " NIC", " UGA", " KHM", " RWA", " GHA", " PER", " UGA", " VNM", " GTM", " IND", " MEX", " BOL", " SEN", " ECU", " KEN", " SLV", " BOL", " MEX", " VNM", " VNM", " PER", " PHL", " UGA", " MLI", " PAK", " LBN", " HND", " SLV", " BDI", " PHL", " BOL", " PER", " VNM", " KEN", " RWA", " NIC", " COL", " NIC", " SLE", " PRY", " BOL", " PER", " VNM", " MLI", " ECU", " PHL", " KEN", " WSM", " BOL", " KEN", " PHL", " BOL", " BOL", " RWA", " VNM", " SEN", " PER", " PER", " PRY", " PHL", " BOL", " USA", " RWA", " PRY", " PRY", " SLV", " PHL", " PER", " PHL", " BOL", " PHL", " USA"], "name": "", "z": [1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 2.0, 4.0, 1.0, 1.0, 1.0, 4.0, 2.0, 5.0, 1.0, 4.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 1.0, 2.0, 2.0, 3.0, 7.0, 2.0, 1.0, 2.0, 2.0, 2.0, 2.0, 1.0, 2.0, 3.0, 1.0, 2.0, 6.0, 1.0, 1.0, 1.0, 3.0, 12.0, 1.0, 2.0, 1.0, 3.0, 6.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 3.0, 4.0, 2.0, 1.0, 3.0, 2.0, 7.0, 1.0, 6.0, 2.0, 2.0, 1.0, 2.0, 1.0, 2.0, 3.0, 2.0, 7.0, 7.0, 3.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 2.0, 7.0, 4.0, 1.0, 3.0, 5.0, 1.0, 7.0, 3.0, 1.0, 3.0, 1.0, 3.0, 4.0, 1.0, 9.0, 2.0, 2.0, 5.0, 4.0, 3.0, 9.0, 7.0, 1.0, 2.0, 7.0, 3.0, 5.0, 5.0, 2.0, 3.0, 1.0, 3.0, 3.0, 2.0, 1.0, 3.0, 15.0, 2.0, 2.0, 2.0, 6.0, 1.0, 9.0, 7.0, 8.0, 1.0, 4.0, 10.0, 4.0, 4.0, 1.0, 4.0, 7.0, 9.0, 6.0, 2.0, 3.0, 2.0, 9.0, 2.0, 6.0, 9.0, 2.0, 11.0, 12.0, 2.0, 1.0, 4.0, 10.0, 2.0, 2.0, 4.0, 4.0, 3.0, 8.0, 2.0, 3.0, 12.0, 5.0, 3.0, 4.0, 6.0, 6.0, 8.0, 3.0, 4.0, 6.0, 8.0, 2.0, 1.0, 8.0, 8.0, 2.0, 4.0, 14.0, 9.0, 8.0, 6.0, 1.0, 1.0, 4.0, 2.0, 5.0, 2.0, 5.0, 4.0, 4.0, 2.0, 4.0, 8.0, 6.0, 10.0, 4.0, 2.0, 9.0, 3.0, 4.0, 3.0, 1.0, 6.0, 17.0, 1.0, 16.0, 7.0, 6.0, 7.0, 2.0, 2.0, 2.0, 3.0, 4.0, 4.0, 6.0, 2.0, 8.0, 9.0, 2.0, 4.0, 6.0, 9.0, 9.0, 26.0, 9.0, 8.0, 2.0, 5.0, 4.0, 1.0, 4.0, 5.0, 14.0, 4.0, 4.0, 5.0, 5.0, 14.0, 8.0, 6.0, 5.0, 6.0, 13.0, 5.0, 5.0, 4.0, 16.0, 2.0, 3.0, 12.0, 6.0, 13.0, 4.0, 7.0, 3.0, 4.0, 3.0, 9.0, 7.0, 6.0, 1.0, 5.0, 11.0, 7.0, 7.0, 3.0, 11.0, 2.0, 1.0, 8.0, 8.0, 3.0, 4.0, 21.0, 2.0, 11.0, 4.0, 11.0, 5.0, 5.0, 11.0, 6.0, 19.0, 13.0, 4.0, 13.0, 22.0, 3.0, 6.0, 7.0, 9.0, 3.0, 9.0, 2.0, 1.0, 26.0, 6.0, 11.0, 15.0, 8.0, 20.0, 5.0, 7.0, 25.0, 11.0, 11.0, 10.0, 6.0, 5.0, 22.0, 7.0, 5.0, 4.0, 7.0, 7.0, 4.0, 40.0, 10.0, 10.0, 4.0, 2.0, 18.0, 10.0, 2.0, 3.0, 25.0, 38.0, 14.0, 6.0, 16.0, 11.0, 16.0, 4.0, 9.0, 8.0, 32.0, 6.0, 16.0, 7.0, 3.0, 10.0, 15.0, 10.0, 32.0, 11.0, 3.0, 20.0, 12.0, 7.0, 13.0, 6.0, 23.0, 10.0, 7.0, 2.0, 40.0, 4.0, 8.0, 6.0, 13.0, 12.0, 8.0, 10.0, 49.0, 6.0, 6.0, 59.0, 6.0, 5.0, 7.0, 11.0, 41.0, 6.0, 13.0, 33.0, 13.0, 14.0, 17.0, 13.0, 9.0, 30.0, 26.0, 6.0, 26.0, 23.0, 10.0, 17.0, 3.0, 12.0, 16.0, 19.0, 16.0, 13.0, 21.0, 4.0, 4.0, 14.0, 11.0, 12.0, 8.0, 65.0, 13.0, 14.0, 42.0, 33.0, 4.0, 3.0, 3.0, 39.0, 6.0, 37.0, 12.0, 17.0, 10.0, 22.0, 15.0, 30.0, 55.0, 8.0, 42.0, 9.0, 22.0, 12.0, 10.0, 7.0, 6.0, 31.0, 7.0, 24.0, 9.0, 13.0, 4.0, 25.0, 35.0, 9.0, 55.0, 55.0, 14.0, 56.0, 31.0, 28.0, 8.0, 5.0, 14.0, 49.0, 28.0, 22.0, 75.0, 20.0, 15.0, 33.0, 8.0, 9.0, 21.0, 13.0, 36.0, 21.0, 7.0, 33.0, 60.0, 41.0, 33.0, 15.0, 40.0, 14.0, 5.0, 43.0, 13.0, 72.0, 19.0, 16.0, 18.0, 21.0, 50.0, 14.0, 20.0, 34.0, 8.0, 21.0, 53.0, 11.0, 19.0, 11.0, 20.0, 5.0, 40.0, 25.0, 3.0, 79.0, 101.0, 28.0, 168.0, 16.0, 25.0, 19.0, 114.0, 114.0, 22.0, 25.0, 111.0, 33.0, 45.0, 2.0, 17.0, 20.0, 10.0, 10.0, 38.0, 88.0, 34.0, 8.0, 25.0, 19.0, 51.0, 22.0, 84.0, 118.0, 6.0, 8.0, 8.0, 41.0, 56.0, 39.0, 55.0, 12.0, 25.0, 13.0, 111.0, 33.0, 32.0, 7.0, 21.0, 18.0, 104.0, 7.0, 91.0, 12.0, 52.0, 33.0, 21.0, 94.0, 56.0, 41.0, 84.0, 46.0, 26.0, 38.0, 28.0, 60.0, 121.0, 20.0, 22.0, 24.0, 15.0, 27.0, 100.0, 29.0, 22.0, 60.0, 28.0, 70.0, 6.0, 90.0, 27.0, 13.0, 10.0, 63.0, 71.0, 17.0, 81.0, 77.0, 33.0, 8.0, 47.0, 146.0, 40.0, 75.0, 32.0, 126.0, 7.0, 64.0, 37.0, 18.0, 40.0, 57.0, 8.0, 13.0, 31.0, 59.0, 87.0, 105.0, 28.0, 10.0, 41.0, 41.0, 25.0, 93.0, 145.0, 65.0, 26.0, 80.0, 21.0, 46.0, 20.0, 17.0, 20.0, 27.0, 35.0, 10.0, 182.0, 77.0, 22.0, 9.0, 168.0, 10.0, 20.0, 87.0, 47.0, 124.0, 35.0, 152.0, 71.0, 80.0, 48.0, 234.0, 131.0, 33.0, 51.0, 21.0, 136.0, 58.0, 22.0, 91.0, 51.0, 34.0, 34.0, 11.0, 33.0, 139.0, 53.0, 79.0, 30.0, 32.0, 69.0, 52.0, 68.0, 218.0, 110.0, 78.0, 226.0, 56.0, 142.0, 174.0, 57.0, 156.0, 116.0, 58.0, 129.0, 107.0, 244.0, 35.0, 84.0, 221.0, 254.0, 27.0, 96.0, 238.0, 146.0, 70.0, 170.0, 131.0, 53.0, 69.0, 437.0, 106.0, 68.0, 90.0, 149.0, 230.0, 83.0, 69.0, 78.0, 67.0, 67.0, 30.0, 97.0, 52.0, 107.0, 146.0, 58.0, 40.0, 277.0, 161.0, 190.0, 65.0, 78.0, 130.0, 134.0, 114.0, 142.0, 84.0, 62.0, 123.0, 34.0, 166.0, 85.0, 110.0, 43.0, 232.0, 86.0, 143.0, 157.0, 107.0, 174.0, 43.0, 169.0, 127.0, 29.0, 337.0, 128.0, 182.0, 193.0, 101.0, 343.0, 219.0, 180.0, 168.0, 240.0, 635.0, 114.0, 65.0, 207.0, 99.0, 49.0, 270.0, 216.0, 301.0, 454.0, 189.0, 184.0, 98.0, 195.0, 120.0, 416.0, 243.0, 80.0, 98.0, 59.0, 45.0, 922.0, 298.0, 185.0, 303.0, 173.0, 65.0, 285.0, 64.0, 375.0, 69.0, 120.0, 62.0, 179.0, 330.0, 159.0, 182.0, 131.0, 394.0, 417.0, 443.0, 203.0, 71.0, 562.0, 344.0, 97.0, 91.0, 260.0, 170.0, 63.0, 142.0, 214.0, 90.0, 366.0, 443.0, 388.0, 174.0, 208.0, 333.0, 467.0, 235.0, 181.0, 197.0, 398.0, 220.0, 417.0, 133.0, 146.0, 218.0, 183.0, 788.0, 496.0, 804.0, 307.0, 405.0, 386.0, 572.0, 94.0, 251.0, 87.0, 186.0, 238.0, 135.0, 117.0, 1083.0, 207.0, 276.0, 807.0, 208.0, 182.0, 289.0, 353.0, 107.0, 648.0, 773.0, 1091.0, 362.0, 570.0, 386.0, 111.0, 324.0, 290.0, 548.0, 262.0, 260.0, 689.0, 150.0, 138.0, 177.0, 371.0, 1272.0, 606.0, 328.0, 138.0, 276.0, 290.0, 207.0, 1313.0, 660.0, 371.0, 1131.0, 360.0, 822.0, 992.0, 200.0, 1499.0, 500.0, 371.0, 362.0, 2578.0, 276.0, 732.0, 1501.0, 735.0, 1267.0, 259.0, 226.0, 524.0, 582.0, 768.0, 567.0, 2959.0, 2647.0, 1614.0, 346.0, 2989.0, 2891.0, 320.0, 428.0, 405.0, 758.0, 479.0, 870.0, 668.0, 388.0, 4486.0, 608.0, 414.0, 736.0, 608.0, 651.0, 4318.0, 8675.0, 1299.0, 9060.0, 1092.0, 8682.0, 709.0], "type": "choropleth"}], "name": "Food"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Transportation<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" NPL", " CMR", " HTI", " HND", " MDG", " GHA", " IND", " GEO", " MOZ", " BFA", " JOR", " BRA", " MEX", " GTM", " COL", " IND", " HTI", " TGO", " TGO", " SOM", " MNG", " MLI", " VNM", " UKR", " TGO", " TLS", " ISR", " MOZ", " KEN", " CHN", " TLS", " CRI", " YEM", " SUR", " RWA", " ECU", " GHA", " TJK", " KGZ", " SLV", " ZWE", " CMR", " NIC", " SLB", " HND", " SLE", " SEN", " USA", " SLE", " IDN", " CMR", " TLS", " IDN", " RWA", " MDG", " ALB", " MOZ", " PAN", " ALB", " WSM", " ZWE", " ZAF", " GTM", " KHM", " SEN", " NIC", " CRI", " BOL", " HND", " CHN", " COL", " HND", " NIC", " YEM", " AZE", " GTM", " IND", " PHL", " GTM", " SEN", " SLV", " IND", " NIC", " ZAF", " PRY", " UGA", " PRY", " KHM", " MEX", " KHM", " KEN", " VNM", " ALB", " YEM", " USA", " COL", " PER", " LAO", " UGA", " SLV", " MNG", " ECU", " ARM", " ECU", " RWA", " LBN", " PHL", " MNG", " YEM", " PAK", " COL", " TJK", " AZE", " ARM", " KEN", " LBN", " TJK", " KHM", " MEX", " SLV", " GEO", " PAK", " PER", " UGA", " WSM", " PAK", " BOL", " GEO", " PER", " BOL", " USA", " PER", " PHL", " KEN", " PAK", " PHL"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 3.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 5.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 2.0, 2.0, 6.0, 2.0, 1.0, 2.0, 5.0, 1.0, 2.0, 2.0, 2.0, 2.0, 3.0, 4.0, 5.0, 7.0, 2.0, 3.0, 3.0, 4.0, 5.0, 4.0, 9.0, 4.0, 11.0, 1.0, 6.0, 9.0, 9.0, 7.0, 13.0, 2.0, 18.0, 6.0, 15.0, 5.0, 8.0, 5.0, 6.0, 5.0, 6.0, 10.0, 11.0, 12.0, 9.0, 10.0, 13.0, 5.0, 19.0, 22.0, 24.0, 27.0, 12.0, 15.0, 65.0, 44.0, 21.0, 30.0, 34.0, 99.0, 32.0, 10.0, 27.0, 35.0, 29.0, 27.0, 26.0, 33.0, 70.0, 26.0, 26.0, 33.0, 13.0, 74.0, 100.0, 2.0, 89.0, 87.0, 33.0, 67.0, 67.0, 69.0, 26.0, 59.0, 261.0, 55.0, 46.0, 224.0, 135.0, 95.0, 93.0, 117.0, 325.0, 82.0, 222.0, 165.0, 59.0, 239.0, 88.0, 466.0, 220.0, 388.0, 195.0, 585.0, 322.0, 191.0, 432.0, 294.0, 118.0, 286.0, 1261.0, 1443.0, 2134.0, 3002.0], "type": "choropleth"}], "name": "Transportation"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Construction<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" HTI", " GEO", " PAK", " SEN", " GHA", " HTI", " PAK", " ARM", " NIC", " HTI", " BFA", " PAN", " LBR", " SLE", " CMR", " PHL", " COL", " ALB", " ZWE", " HND", " SLE", " GHA", " LBN", " BFA", " GTM", " TJK", " IDN", " ALB", " SSD", " MWI", " SLB", " SLB", " PRY", " COL", " KEN", " SLV", " NPL", " PER", " CMR", " MLI", " ZAF", " KHM", " BFA", " ARM", " MOZ", " SSD", " EGY", " MLI", " PAK", " GTM", " IND", " BFA", " PRY", " TLS", " MWI", " SEN", " NGA", " ALB", " IDN", " TGO", " KHM", " NIC", " CHN", " MLI", " BDI", " GEO", " IND", " LBR", " LBR", " CRI", " COL", " HND", " MDG", " CRI", " SEN", " GHA", " GEO", " PAK", " PRY", " SLV", " PAK", " KHM", " MWI", " UKR", " GHA", " DOM", " TGO", " TJK", " BRA", " CMR", " TJK", " ZWE", " KHM", " ZWE", " GTM", " IND", " GTM", " ZAF", " UGA", " MWI", " MOZ", " MNG", " LBN", " ALB", " PHL", " KGZ", " VNM", " NIC", " SEN", " GHA", " AZE", " TLS", " PAN", " HND", " LBR", " IDN", " IDN", " NIC", " MEX", " NPL", " TJK", " RWA", " PER", " MNG", " VNM", " NIC", " MDG", " ARM", " TLS", " JOR", " EGY", " IDN", " SEN", " SLV", " RWA", " ZWE", " NIC", " MOZ", " SLV", " MNG", " COL", " HND", " TLS", " COG", " TLS", " BFA", " JOR", " PHL", " GHA", " COG", " LBR", " VNM", " SLE", " ARM", " ECU", " WSM", " ZAF", " EGY", " MNG", " AZE", " MDG", " PHL", " EGY", " GHA", " BDI", " UGA", " IND", " GEO", " WSM", " IND", " BDI", " BRA", " AZE", " JOR", " GEO", " UKR", " LBR", " COL", " JOR", " JOR", " BOL", " SOM", " MEX", " GEO", " RWA", " BDI", " HTI", " ALB", " IRQ", " BOL", " SLV", " IND", " ZWE", " BDI", " HND", " BOL", " ECU", " KHM", " MOZ", " PER", " IND", " PHL", " MOZ", " MNG", " SLE", " MEX", " PAK", " IRQ", " VNM", " LBN", " HND", " SOM", " UGA", " ALB", " ARM", " NIC", " RWA", " GHA", " CRI", " ECU", " BRA", " ECU", " YEM", " IRQ", " USA", " PRY", " LBN", " RWA", " VNM", " ECU", " SLE", " TLS", " LBN", " SLV", " PAK", " PER", " TJK", " KEN", " RWA", " KEN", " COL", " UGA", " GTM", " BOL", " BOL", " ECU", " KEN", " UGA", " EGY", " UGA", " TJK", " SLV", " SLV", " COL", " PRY", " GTM", " BOL", " PRY", " AZE", " KEN", " HTI", " KEN", " VNM", " SLE", " COL", " UGA", " KHM", " KHM", " PHL", " YEM", " PHL", " PAK", " KEN", " NIC", " PER", " VNM", " ARM", " PRY", " PHL", " MEX", " BOL", " LBN", " RWA", " PER", " PER", " BOL", " USA"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 2.0, 2.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 1.0, 2.0, 2.0, 1.0, 1.0, 2.0, 3.0, 2.0, 5.0, 2.0, 1.0, 2.0, 1.0, 2.0, 1.0, 2.0, 2.0, 4.0, 1.0, 2.0, 1.0, 2.0, 1.0, 1.0, 5.0, 3.0, 1.0, 1.0, 1.0, 2.0, 7.0, 2.0, 2.0, 2.0, 1.0, 4.0, 2.0, 3.0, 3.0, 2.0, 1.0, 1.0, 2.0, 2.0, 7.0, 6.0, 1.0, 6.0, 1.0, 1.0, 2.0, 3.0, 4.0, 10.0, 1.0, 2.0, 5.0, 1.0, 5.0, 1.0, 3.0, 2.0, 9.0, 2.0, 2.0, 1.0, 4.0, 2.0, 2.0, 4.0, 4.0, 7.0, 5.0, 5.0, 6.0, 4.0, 2.0, 9.0, 4.0, 6.0, 5.0, 1.0, 2.0, 6.0, 15.0, 2.0, 5.0, 3.0, 5.0, 6.0, 6.0, 6.0, 1.0, 5.0, 7.0, 9.0, 7.0, 2.0, 7.0, 6.0, 4.0, 1.0, 5.0, 5.0, 3.0, 11.0, 3.0, 1.0, 4.0, 4.0, 6.0, 4.0, 5.0, 6.0, 4.0, 8.0, 3.0, 3.0, 27.0, 16.0, 4.0, 9.0, 2.0, 15.0, 27.0, 5.0, 3.0, 6.0, 3.0, 3.0, 6.0, 3.0, 5.0, 4.0, 13.0, 11.0, 5.0, 8.0, 2.0, 2.0, 5.0, 6.0, 5.0, 4.0, 2.0, 12.0, 4.0, 2.0, 18.0, 43.0, 10.0, 3.0, 14.0, 10.0, 9.0, 17.0, 25.0, 15.0, 35.0, 33.0, 27.0, 8.0, 12.0, 15.0, 27.0, 5.0, 16.0, 10.0, 21.0, 3.0, 19.0, 12.0, 19.0, 21.0, 3.0, 8.0, 18.0, 20.0, 2.0, 22.0, 11.0, 8.0, 5.0, 15.0, 16.0, 11.0, 18.0, 20.0, 14.0, 28.0, 21.0, 40.0, 51.0, 45.0, 56.0, 85.0, 11.0, 109.0, 50.0, 40.0, 40.0, 16.0, 20.0, 24.0, 91.0, 68.0, 49.0, 51.0, 55.0, 60.0, 60.0, 62.0, 40.0, 60.0, 28.0, 27.0, 36.0, 110.0, 43.0, 175.0, 26.0, 54.0, 101.0, 63.0, 84.0, 71.0, 158.0, 91.0, 209.0, 187.0, 224.0, 144.0, 69.0, 72.0, 116.0, 49.0, 360.0, 103.0, 136.0, 131.0, 51.0, 154.0, 170.0, 158.0, 90.0], "type": "choropleth"}], "name": "Construction"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Manufacturing<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" SLE", " MDG", " MDG", " BEN", " SEN", " CMR", " WSM", " MLI", " SSD", " KGZ", " CMR", " NPL", " PAN", " ALB", " ECU", " SLE", " BFA", " MOZ", " BRA", " TGO", " MOZ", " JOR", " KGZ", " PER", " RWA", " LBR", " BFA", " JOR", " WSM", " RWA", " PER", " TLS", " TLS", " SOM", " ECU", " TGO", " TGO", " BFA", " AZE", " IDN", " SEN", " IDN", " TLS", " EGY", " ARM", " COL", " ZWE", " TJK", " LBR", " TUR", " IDN", " MOZ", " SEN", " KEN", " VNM", " KHM", " AZE", " IDN", " ALB", " PHL", " ZWE", " TLS", " IND", " UKR", " PRY", " BEN", " BFA", " EGY", " MEX", " ALB", " ZAF", " PRI", " MEX", " CMR", " GHA", " YEM", " SEN", " SLE", " NPL", " ZAF", " UGA", " LBR", " TJK", " EGY", " MNG", " CRI", " JOR", " CRI", " MDG", " CMR", " MNG", " SLV", " GTM", " UKR", " GTM", " SLE", " YEM", " AZE", " IND", " CRI", " VNM", " HND", " BOL", " MWI", " ZAF", " KGZ", " IND", " MLI", " HND", " PRY", " CHN", " BDI", " SOM", " PRY", " KHM", " KHM", " ARM", " IRQ", " GEO", " CRI", " HND", " NIC", " ALB", " ZWE", " SLE", " ARM", " MNG", " USA", " VCT", " PRY", " ZMB", " MDG", " KHM", " BFA", " JOR", " KEN", " MLI", " SLV", " IRQ", " GEO", " JOR", " BOL", " UGA", " PHL", " HTI", " TJK", " COL", " JOR", " ALB", " ZWE", " GHA", " GEO", " IRQ", " AZE", " VNM", " ECU", " UGA", " ARM", " RWA", " RWA", " PHL", " IND", " GTM", " ZWE", " MNG", " UGA", " LBN", " MEX", " PER", " LBN", " NIC", " NIC", " GTM", " PAK", " LBN", " HTI", " KEN", " SLV", " TJK", " SEN", " GHA", " LBN", " USA", " ECU", " PAK", " KEN", " COL", " MEX", " ARM", " PER", " PAK", " COL", " VNM", " PAK", " COL", " USA", " ECU", " MEX", " PER", " KEN", " BOL", " IND", " SLV", " SLV", " BOL", " VNM", " VNM", " PHL", " PAK", " PHL", " BOL"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 3.0, 1.0, 1.0, 1.0, 2.0, 2.0, 3.0, 2.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 6.0, 6.0, 2.0, 1.0, 1.0, 2.0, 1.0, 1.0, 2.0, 1.0, 3.0, 2.0, 4.0, 2.0, 3.0, 2.0, 2.0, 3.0, 2.0, 2.0, 3.0, 1.0, 2.0, 2.0, 4.0, 2.0, 2.0, 1.0, 3.0, 2.0, 2.0, 2.0, 3.0, 3.0, 1.0, 1.0, 1.0, 2.0, 6.0, 1.0, 5.0, 4.0, 3.0, 12.0, 2.0, 7.0, 3.0, 6.0, 4.0, 2.0, 2.0, 3.0, 3.0, 13.0, 7.0, 2.0, 4.0, 1.0, 3.0, 6.0, 7.0, 8.0, 3.0, 10.0, 3.0, 2.0, 9.0, 2.0, 2.0, 3.0, 3.0, 13.0, 5.0, 9.0, 6.0, 3.0, 3.0, 1.0, 6.0, 11.0, 13.0, 5.0, 4.0, 11.0, 8.0, 16.0, 14.0, 6.0, 8.0, 10.0, 7.0, 6.0, 2.0, 3.0, 3.0, 5.0, 43.0, 15.0, 22.0, 13.0, 30.0, 7.0, 20.0, 6.0, 8.0, 12.0, 6.0, 27.0, 39.0, 10.0, 19.0, 28.0, 15.0, 11.0, 16.0, 8.0, 11.0, 6.0, 14.0, 10.0, 26.0, 36.0, 16.0, 10.0, 5.0, 73.0, 49.0, 22.0, 33.0, 19.0, 53.0, 26.0, 9.0, 39.0, 27.0, 38.0, 46.0, 27.0, 77.0, 29.0, 16.0, 100.0, 68.0, 50.0, 25.0, 28.0, 39.0, 13.0, 66.0, 142.0, 53.0, 80.0, 40.0, 46.0, 47.0, 167.0, 131.0, 60.0, 131.0, 132.0, 24.0, 88.0, 26.0, 87.0, 110.0, 58.0, 216.0, 186.0, 219.0, 126.0, 106.0, 138.0, 532.0, 512.0, 796.0, 198.0], "type": "choropleth"}], "name": "Manufacturing"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Entertainment<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" LBR", " NPL", " EGY", " EGY", " CMR", " HND", " IND", " SLE", " JOR", " CHN", " NIC", " PRY", " MEX", " ECU", " TLS", " ECU", " IDN", " MDG", " UGA", " KEN", " MEX", " KHM", " IRQ", " ALB", " CRI", " PAK", " PAK", " GHA", " SLE", " IND", " ALB", " GEO", " JOR", " BOL", " KHM", " VNM", " TJK", " KEN", " TJK", " SLV", " PER", " SLV", " TJK", " ECU", " IRQ", " NIC", " USA", " LBR", " NIC", " GTM", " PAK", " ISR", " PER", " LBN", " SLV", " MEX", " UGA", " KEN", " LBN", " RWA", " COL", " UGA", " PHL", " VCT", " PHL", " ISR", " PRI", " ARM", " VNM", " LBN", " PER", " USA", " COL", " WSM", " PHL", " COL", " BOL", " BOL", " USA"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 3.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 6.0, 2.0, 3.0, 3.0, 2.0, 1.0, 3.0, 1.0, 6.0, 5.0, 2.0, 3.0, 2.0, 2.0, 1.0, 3.0, 4.0, 5.0, 2.0, 5.0, 12.0, 2.0, 6.0, 9.0, 5.0, 2.0, 5.0, 2.0, 8.0, 1.0, 6.0, 8.0, 4.0, 16.0, 1.0, 10.0, 6.0, 8.0, 4.0, 5.0, 17.0, 6.0, 3.0, 12.0, 21.0, 30.0, 3.0, 33.0, 4.0, 4.0, 13.0, 10.0, 16.0, 19.0, 8.0, 27.0, 43.0, 76.0, 52.0, 8.0, 28.0, 174.0], "type": "choropleth"}], "name": "Entertainment"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Clothing<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" PAK", " NPL", " SSD", " KHM", " JOR", " SSD", " PAN", " NPL", " TJK", " PAN", " TGO", " MOZ", " SSD", " GEO", " ZMB", " COL", " BEN", " LBR", " ZWE", " KGZ", " VNM", " TUR", " BFA", " KGZ", " HND", " IDN", " ARM", " MLI", " SEN", " MOZ", " THA", " ISR", " MEX", " SLV", " ZAF", " ZAF", " IND", " BDI", " GTM", " SLB", " YEM", " USA", " UKR", " IRQ", " NIC", " MNG", " VNM", " IRQ", " KHM", " LBR", " ARM", " EGY", " BEN", " CMR", " ECU", " CRI", " MWI", " CHL", " MDG", " COG", " WSM", " CHN", " AZE", " LBN", " ALB", " JOR", " SLE", " MNG", " CMR", " SLB", " MWI", " VCT", " ISR", " SLB", " IDN", " PAK", " IND", " SOM", " UKR", " GHA", " KEN", " TGO", " BRA", " BFA", " ALB", " HTI", " CMR", " CMR", " COG", " PRI", " SLE", " HTI", " HND", " DOM", " JOR", " GEO", " MOZ", " LBN", " SEN", " MDG", " TLS", " TJK", " PER", " PHL", " TGO", " PHL", " WSM", " MDG", " KHM", " MOZ", " BDI", " PAK", " SLV", " AZE", " SLV", " DOM", " ZWE", " IRQ", " NIC", " LBN", " GEO", " HTI", " LBR", " TGO", " ALB", " MWI", " VNM", " COG", " ECU", " UGA", " TUR", " RWA", " LBR", " COL", " ALB", " HND", " BOL", " GTM", " GHA", " ZWE", " TLS", " CRI", " MDG", " PER", " MLI", " MNG", " TLS", " UGA", " BRA", " HTI", " EGY", " MLI", " MEX", " SEN", " MWI", " GHA", " SEN", " ISR", " BDI", " RWA", " BFA", " YEM", " BDI", " KGZ", " SLE", " USA", " DOM", " HND", " RWA", " NIC", " KEN", " BFA", " PHL", " PRY", " GHA", " IND", " ARM", " MEX", " UGA", " GTM", " JOR", " SLE", " UKR", " GEO", " VNM", " COL", " UGA", " MLI", " KEN", " BOL", " PAK", " ZWE", " TJK", " NIC", " PHL", " BOL", " LBN", " KEN", " SLV", " GTM", " USA", " RWA", " PER", " ECU", " PRY", " PRY", " BOL"], "name": "", "z": [1.0, 1.0, 2.0, 1.0, 1.0, 3.0, 1.0, 2.0, 1.0, 2.0, 7.0, 2.0, 2.0, 1.0, 1.0, 5.0, 2.0, 6.0, 2.0, 1.0, 2.0, 6.0, 3.0, 1.0, 2.0, 2.0, 1.0, 1.0, 1.0, 5.0, 1.0, 1.0, 1.0, 3.0, 2.0, 2.0, 5.0, 1.0, 2.0, 5.0, 5.0, 1.0, 3.0, 1.0, 6.0, 3.0, 4.0, 2.0, 5.0, 12.0, 5.0, 9.0, 10.0, 15.0, 6.0, 6.0, 4.0, 1.0, 23.0, 1.0, 12.0, 4.0, 6.0, 5.0, 6.0, 10.0, 15.0, 4.0, 25.0, 10.0, 8.0, 3.0, 3.0, 13.0, 12.0, 28.0, 28.0, 3.0, 9.0, 16.0, 56.0, 49.0, 7.0, 15.0, 15.0, 36.0, 49.0, 44.0, 3.0, 6.0, 14.0, 18.0, 33.0, 9.0, 19.0, 23.0, 49.0, 20.0, 15.0, 81.0, 28.0, 32.0, 15.0, 68.0, 100.0, 73.0, 44.0, 85.0, 35.0, 61.0, 13.0, 74.0, 67.0, 29.0, 73.0, 11.0, 50.0, 20.0, 51.0, 39.0, 47.0, 128.0, 120.0, 255.0, 7.0, 34.0, 44.0, 10.0, 61.0, 124.0, 140.0, 28.0, 234.0, 117.0, 59.0, 113.0, 22.0, 62.0, 67.0, 72.0, 100.0, 66.0, 277.0, 52.0, 71.0, 56.0, 104.0, 148.0, 54.0, 113.0, 139.0, 81.0, 41.0, 55.0, 72.0, 111.0, 71.0, 29.0, 41.0, 46.0, 100.0, 195.0, 43.0, 101.0, 175.0, 47.0, 52.0, 311.0, 63.0, 238.0, 559.0, 185.0, 515.0, 63.0, 252.0, 491.0, 257.0, 130.0, 656.0, 158.0, 300.0, 403.0, 188.0, 350.0, 238.0, 724.0, 682.0, 353.0, 1766.0, 186.0, 1351.0, 971.0, 823.0, 792.0, 2105.0, 310.0, 588.0, 2372.0, 1763.0, 873.0, 372.0, 424.0, 846.0, 1417.0, 490.0, 581.0, 918.0], "type": "choropleth"}], "name": "Clothing"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "sector=Housing<br>ISO_ALPHA3=%{location}<br>amount_projects=%{z}<extra></extra>", "locations": [" MDG", " IND", " PAK", " TUR", " TLS", " PAN", " WSM", " MWI", " LAO", " IDN", " SEN", " TJK", " SLE", " PAK", " GHA", " RWA", " RWA", " IRQ", " CHN", " SEN", " MOZ", " HND", " PRY", " YEM", " CRI", " COL", " ALB", " MWI", " SLB", " DOM", " SSD", " MNG", " CHN", " GEO", " ECU", " ZAF", " WSM", " GHA", " SUR", " SLB", " LBN", " NIC", " PRY", " PAN", " VNM", " ECU", " BOL", " KHM", " SLV", " PER", " ARM", " COL", " KEN", " HND", " AZE", " TLS", " SLE", " CRI", " USA", " KEN", " USA", " PHL", " MEX", " UGA", " GEO", " GTM", " KGZ", " YEM", " PER", " UKR", " GTM", " ALB", " LBN", " MEX", " UGA", " MNG", " IND", " MOZ", " TJK", " BOL", " IDN", " NIC", " PHL", " KHM", " SLV", " VNM"], "name": "", "z": [1.0, 1.0, 1.0, 1.0, 1.0, 2.0, 1.0, 1.0, 1.0, 2.0, 7.0, 5.0, 1.0, 6.0, 2.0, 1.0, 4.0, 1.0, 1.0, 5.0, 8.0, 4.0, 6.0, 7.0, 3.0, 9.0, 6.0, 2.0, 7.0, 8.0, 12.0, 5.0, 5.0, 5.0, 8.0, 7.0, 6.0, 19.0, 7.0, 18.0, 15.0, 28.0, 17.0, 31.0, 24.0, 36.0, 36.0, 36.0, 52.0, 34.0, 45.0, 53.0, 116.0, 62.0, 44.0, 73.0, 36.0, 58.0, 33.0, 170.0, 13.0, 194.0, 55.0, 207.0, 75.0, 198.0, 205.0, 375.0, 517.0, 291.0, 575.0, 372.0, 308.0, 500.0, 781.0, 374.0, 1294.0, 1573.0, 1232.0, 822.0, 1973.0, 1682.0, 6591.0, 2073.0, 5925.0, 7358.0], "type": "choropleth"}], "name": "Housing"}]);
                        }).then(function(){

var gd = document.getElementById('ecc77a9c-640e-4608-8392-11ae8a19b09b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


# Beeinflusst die Darlehenshöhe die Anzahl der Darlehensgeber?


```python
# Daten für ax1 vorbereiten

df_funding_sector = df_final.groupby("sector").agg({"funded_amount":np.mean})
df_fund_sector_sorted = df_funding_sector.sort_values(by=["funded_amount"], ascending=True)
df_fund_sector_sorted

# Daten für ax2 vorbereiten

df_funding_lender = df_final.groupby("sector").agg({"lender_count":np.mean,"funded_amount":np.mean})
df_funding_lender_sorted = df_funding_lender.sort_values(by=["funded_amount"], ascending=True)
df_funding_lender_sorted

# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_sector_sorted.index, y=df_fund_sector_sorted["funded_amount"], palette="Blues",data=df_fund_sector_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Durchschnitt Darlehenshöhe in US-Dollar (Balken)", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_funding_lender_sorted.index, y=df_funding_lender_sorted["lender_count"], data=df_funding_lender_sorted, color="darkred")
ax2.set_ylabel('Durchschnitt Anzahl Darlehensgeber (Linie)', fontsize=14)

plt.show()
```


    
![png](output_51_0.png)
    


# Überzeugen weniger erfolgreiche Teams auch weniger die Investoren?


```python
# Daten vorbereiten

# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Virgin Islands entfernen, da keine 
# Projekte gefördert wurden. Die Entferrnungen haben keinen Einfluss auf die Aussage der Graphik.

df_final = df_final.loc[df_final["country_code"]!="CI",:]
df_final = df_final.loc[df_final["country_code"]!="MR",:]
df_final = df_final.loc[df_final["country_code"]!="BT",:]
df_final = df_final.loc[df_final["country_code"]!="AF",:]
df_final = df_final.loc[df_final["country_code"]!="VI",:]

# DataFrame mit erfolgreichen Fundings (Erfolgsquote = 100%), mit wenig erfolgreichen (Erfolgsquote < 50%) 
# und dazwischen liegenden erstellen

df_success = df_final.loc[df_final["success_classes"]=="Gleich100",:]
df_no_success = df_final.loc[df_final["success_classes"]=="KleinerGleich50",:]


# Daten gruppieren

# Daten für ax1
df_fund_country = df_final.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_fund_country_sorted = df_fund_country.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax2_success 
df_succ_sect = df_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_succ_sect_sorted = df_succ_sect.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax3_no_success 
df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["funded_amount"], ascending=True)


# Daten plotten

fig, ax1 = plt.subplots(figsize=(14,14))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_country_sorted.index, y=df_fund_country_sorted["lender_count"], color="grey",data=df_fund_country_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=90, labelsize=9)
ax1.set_ylabel("Median Anzahl Darlehensgeber, alle Projekte (graue Balken)", fontsize=14)
ax1.set_xlabel("Länder", fontsize=14)

ax2 = sns.lineplot(x=df_succ_sect_sorted.index, y=df_succ_sect_sorted["lender_count"], data=df_succ_sect_sorted, color="lightcoral")
ax3 = sns.lineplot(x=df_no_succ_sect_sorted.index, y=df_no_succ_sect_sorted["lender_count"], data=df_no_succ_sect_sorted, color="darkred")

ax2.set_ylabel('Durchschnitt Anzahl Darlehensgeber: alle (Balken), erfolgreiche (rosa), nicht erfolgreiche (rot) Projekte', fontsize=14)

plt.show()
```


    
![png](output_53_0.png)
    


# Ausgewählte Zahlen


```python
# Übersicht bzgl. der Verteilungen
df_final.describe()
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
      <th>funded_amount</th>
      <th>loan_amount</th>
      <th>success_ratio</th>
      <th>term_in_months</th>
      <th>lender_count</th>
      <th>team_count</th>
      <th>female</th>
      <th>male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>690877.000000</td>
      <td>690877.000000</td>
      <td>690877.000000</td>
      <td>690877.000000</td>
      <td>690877.000000</td>
      <td>686581.000000</td>
      <td>686581.000000</td>
      <td>686581.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>809.703522</td>
      <td>868.526959</td>
      <td>96.002208</td>
      <td>13.853947</td>
      <td>21.284744</td>
      <td>2.043319</td>
      <td>1.633354</td>
      <td>0.409965</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1139.139167</td>
      <td>1208.798815</td>
      <td>15.977007</td>
      <td>8.625613</td>
      <td>28.735068</td>
      <td>3.403661</td>
      <td>3.037324</td>
      <td>1.107721</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>25.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>250.000000</td>
      <td>275.000000</td>
      <td>100.000000</td>
      <td>8.000000</td>
      <td>7.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>475.000000</td>
      <td>500.000000</td>
      <td>100.000000</td>
      <td>13.000000</td>
      <td>13.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>925.000000</td>
      <td>1000.000000</td>
      <td>100.000000</td>
      <td>14.000000</td>
      <td>25.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>50000.000000</td>
      <td>50000.000000</td>
      <td>113.333333</td>
      <td>158.000000</td>
      <td>1765.000000</td>
      <td>50.000000</td>
      <td>50.000000</td>
      <td>44.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# sector_size: absolute Anzahl Projekte
# Alle weiteren Variablen: MITTELWERT

df_group_activity = df_final.groupby("activity").agg({"sector":np.size, "funded_amount":np.mean, "loan_amount":np.mean, "success_ratio":np.mean, "team_count":np.mean, "term_in_months":np.mean, "lender_count":np.mean})
df_group_sector_rename = df_group_activity.rename(columns={"sector": "activity_size"})
df_group_activity = df_group_sector_rename.reset_index()
df_group_activity
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
      <th>activity</th>
      <th>activity_size</th>
      <th>funded_amount</th>
      <th>loan_amount</th>
      <th>success_ratio</th>
      <th>team_count</th>
      <th>term_in_months</th>
      <th>lender_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adult Care</td>
      <td>2</td>
      <td>1687.500000</td>
      <td>1687.500000</td>
      <td>100.000000</td>
      <td>1.000000</td>
      <td>22.500000</td>
      <td>51.500000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Agriculture</td>
      <td>27578</td>
      <td>952.666437</td>
      <td>1024.297447</td>
      <td>95.237045</td>
      <td>2.242566</td>
      <td>15.049097</td>
      <td>27.148633</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Air Conditioning</td>
      <td>36</td>
      <td>1141.666667</td>
      <td>1225.694444</td>
      <td>98.845815</td>
      <td>1.000000</td>
      <td>15.694444</td>
      <td>32.194444</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Animal Sales</td>
      <td>9342</td>
      <td>1083.001499</td>
      <td>1141.016378</td>
      <td>96.657719</td>
      <td>2.706021</td>
      <td>13.740420</td>
      <td>27.825412</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aquaculture</td>
      <td>139</td>
      <td>752.517986</td>
      <td>900.719424</td>
      <td>90.589361</td>
      <td>2.467626</td>
      <td>13.712230</td>
      <td>22.812950</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Water Distribution</td>
      <td>564</td>
      <td>1086.170213</td>
      <td>1180.097518</td>
      <td>98.328759</td>
      <td>1.204380</td>
      <td>12.742908</td>
      <td>31.381206</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Weaving</td>
      <td>3074</td>
      <td>777.756994</td>
      <td>781.717632</td>
      <td>99.874787</td>
      <td>1.980431</td>
      <td>12.824333</td>
      <td>23.132401</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Wedding Expenses</td>
      <td>405</td>
      <td>800.000000</td>
      <td>964.938272</td>
      <td>86.168179</td>
      <td>1.056931</td>
      <td>17.832099</td>
      <td>23.019753</td>
    </tr>
    <tr>
      <th>161</th>
      <td>Well digging</td>
      <td>43</td>
      <td>1309.302326</td>
      <td>1309.302326</td>
      <td>100.000000</td>
      <td>1.209302</td>
      <td>20.697674</td>
      <td>42.883721</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Wholesale</td>
      <td>362</td>
      <td>1289.779006</td>
      <td>1335.013812</td>
      <td>98.742165</td>
      <td>1.459384</td>
      <td>13.640884</td>
      <td>39.994475</td>
    </tr>
  </tbody>
</table>
<p>163 rows × 8 columns</p>
</div>




```python
# sector_size: absolute Anzahl Projekte
# Alle weiteren Variablen: MEDIAN

df_group_sector_median = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.median, "loan_amount":np.median, "success_ratio":np.median, "team_count":np.median, "term_in_months":np.median, "lender_count":np.median})
df_group_sector_rename = df_group_sector_median.rename(columns={"sector": "sector_size"})
df_group_sector_median = df_group_sector_rename.reset_index()
df_group_sector_median
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
      <th>sector</th>
      <th>sector_size</th>
      <th>funded_amount</th>
      <th>loan_amount</th>
      <th>success_ratio</th>
      <th>team_count</th>
      <th>term_in_months</th>
      <th>lender_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agriculture</td>
      <td>184175</td>
      <td>500</td>
      <td>500</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Arts</td>
      <td>12469</td>
      <td>500</td>
      <td>500</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Clothing</td>
      <td>33606</td>
      <td>575</td>
      <td>625</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>11</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Construction</td>
      <td>6524</td>
      <td>675</td>
      <td>725</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Education</td>
      <td>32798</td>
      <td>700</td>
      <td>725</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>15</td>
      <td>21</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Entertainment</td>
      <td>858</td>
      <td>675</td>
      <td>900</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>21</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Food</td>
      <td>140694</td>
      <td>450</td>
      <td>475</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>11</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Health</td>
      <td>9568</td>
      <td>675</td>
      <td>750</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>21</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Housing</td>
      <td>37851</td>
      <td>500</td>
      <td>525</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>19</td>
      <td>16</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Manufacturing</td>
      <td>6564</td>
      <td>550</td>
      <td>550</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>17</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Personal Use</td>
      <td>36549</td>
      <td>200</td>
      <td>200</td>
      <td>100.0</td>
      <td>2.0</td>
      <td>9</td>
      <td>7</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Retail</td>
      <td>126260</td>
      <td>400</td>
      <td>450</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>9</td>
      <td>11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Services</td>
      <td>46475</td>
      <td>525</td>
      <td>575</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Transportation</td>
      <td>15845</td>
      <td>425</td>
      <td>450</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>13</td>
      <td>12</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wholesale</td>
      <td>641</td>
      <td>950</td>
      <td>950</td>
      <td>100.0</td>
      <td>1.0</td>
      <td>14</td>
      <td>27</td>
    </tr>
  </tbody>
</table>
</div>



# Kontinente


```python
df_cont = df_dropna_funding.groupby("continent").agg({"continent":np.size, "funded_amount":np.sum})
df_cont_rename = df_cont.rename(columns={"continent": "amount_projects"})
df_cont = df_cont_rename.reset_index()
df_cont_sorted = df_cont.sort_values(by=["amount_projects"])
df_cont_sorted_fund = df_cont.sort_values(by=["funded_amount"])
df_cont_sorted
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
      <th>continent</th>
      <th>amount_projects</th>
      <th>funded_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Europe</td>
      <td>5938</td>
      <td>6908025.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oceania</td>
      <td>7367</td>
      <td>5737670.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>N_America</td>
      <td>77756</td>
      <td>84778390.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>S_America</td>
      <td>82011</td>
      <td>117102550.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Africa</td>
      <td>161493</td>
      <td>120477480.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Asia</td>
      <td>309295</td>
      <td>192273700.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = px.scatter(df_cont_count, x="amount_projects", y="funded_amount", color="continent",
           hover_name="country", log_x=True, size_max=60)
fig.show()
```


<div>                            <div id="4deb68d4-9a5a-4216-b2e4-a4e7bf6de090" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("4deb68d4-9a5a-4216-b2e4-a4e7bf6de090")) {                    Plotly.newPlot(                        "4deb68d4-9a5a-4216-b2e4-a4e7bf6de090",                        [{"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Oceania<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Guam", "Vanuatu", "Solomon Islands", "Samoa"], "legendgroup": "Oceania", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Oceania", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1.0, 4.0, 554.0, 7396.0], "xaxis": "x", "y": [395.0, 9250.0, 493875.0, 5641825.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Asia<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Afghanistan", "Bhutan", "Nepal", "China", "Thailand", "Israel", "Lao People's Democratic Republic", "Mongolia", "Yemen", "Timor-Leste", "Iraq", "Azerbaijan", "Myanmar (Burma)", "Georgia", "Jordan", "Indonesia", "India", "Kyrgyzstan", "Armenia", "Lebanon", "Palestine", "Pakistan", "Tajikistan", "Cambodia", "Vietnam", "Philippines"], "legendgroup": "Asia", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Asia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2.0, 2.0, 717.0, 134.0, 180.0, 190.0, 1486.0, 964.0, 2313.0, 2690.0, 993.0, 1945.0, 1870.0, 2409.0, 4167.0, 6214.0, 11237.0, 5774.0, 8631.0, 8792.0, 8167.0, 26857.0, 19580.0, 34836.0, 21686.0, 160441.0], "xaxis": "x", "y": [14000.0, 15625.0, 307625.0, 373475.0, 423350.0, 719450.0, 1160425.0, 1591400.0, 1784075.0, 2326900.0, 2611225.0, 2699575.0, 3035850.0, 3369500.0, 4425525.0, 4547025.0, 6466850.0, 6728550.0, 11186675.0, 11556350.0, 12032025.0, 12467100.0, 13781775.0, 18817100.0, 27323400.0, 54476375.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Africa<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Namibia", "South Sudan", "Somalia", "Lesotho", "Benin", "South Africa", "Congo", "Cameroon", "Egypt", "Zambia", "Liberia", "Madagascar", "Togo", "Malawi", "Nigeria", "Mozambique", "Burundi", "Burkina Faso", "Zimbabwe", "Sierra Leone", "Ghana", "Tanzania", "Senegal", "Mali", "The Democratic Republic of the Congo", "Uganda", "Rwanda", "Kenya"], "legendgroup": "Africa", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Africa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [8.0, 160.0, 75.0, 422.0, 497.0, 378.0, 128.0, 2230.0, 1639.0, 784.0, 3682.0, 3821.0, 5749.0, 1320.0, 10136.0, 3483.0, 880.0, 2460.0, 4034.0, 5415.0, 4374.0, 5219.0, 3269.0, 6639.0, 3073.0, 20601.0, 6735.0, 75825.0], "xaxis": "x", "y": [32375.0, 120900.0, 225875.0, 359525.0, 516825.0, 574025.0, 786250.0, 875500.0, 1084925.0, 1147950.0, 1168225.0, 1223575.0, 1486800.0, 1612500.0, 1905325.0, 1918825.0, 2558550.0, 2909975.0, 3372725.0, 3961050.0, 4792475.0, 6518075.0, 6819675.0, 8340850.0, 11020275.0, 14142675.0, 15505600.0, 32248405.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=S_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Chile", "Suriname", "Brazil", "Colombia", "Ecuador", "Paraguay", "Peru", "Bolivia"], "legendgroup": "S_America", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "S_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [10.0, 223.0, 284.0, 21995.0, 13521.0, 11903.0, 22233.0, 17612.0], "xaxis": "x", "y": [76250.0, 540475.0, 661025.0, 12473775.0, 14598900.0, 29412700.0, 30394850.0, 36552400.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=N_America<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Belize", "Saint Vincent and the Grenadines", "Panama", "Puerto Rico", "Costa Rica", "Dominican Republic", "Haiti", "Honduras", "Mexico", "Nicaragua", "Guatemala", "United States", "El Salvador"], "legendgroup": "N_America", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "N_America", "orientation": "v", "showlegend": true, "type": "scatter", "x": [125.0, 48.0, 193.0, 68.0, 1561.0, 496.0, 3617.0, 6557.0, 5741.0, 11781.0, 7310.0, 6093.0, 39875.0], "xaxis": "x", "y": [114025.0, 147675.0, 273275.0, 299825.0, 2015525.0, 2083500.0, 2549475.0, 5667925.0, 9391225.0, 9854375.0, 10934975.0, 23158540.0, 23357725.0], "yaxis": "y"}, {"hovertemplate": "<b>%{hovertext}</b><br><br>continent=Europe<br>amount_projects=%{x}<br>funded_amount=%{y}<extra></extra>", "hovertext": ["Moldova", "Turkey", "Ukraine", "Kosovo", "Albania"], "legendgroup": "Europe", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Europe", "orientation": "v", "showlegend": true, "type": "scatter", "x": [348.0, 1703.0, 963.0, 1419.0, 1934.0], "xaxis": "x", "y": [686850.0, 743625.0, 1561350.0, 1778600.0, 2490000.0], "yaxis": "y"}],                        {"legend": {"title": {"text": "continent"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}, "type": "log"}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "funded_amount"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('4deb68d4-9a5a-4216-b2e4-a4e7bf6de090');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.bar(df_cont_count_sorted, x="amount_projects", y="country", barmode="group")
fig.update_layout(
    font_size = 8,
    autosize=False,
    width=900,
    height=1000,
    margin=dict(
        l=50,
        r=50,
        b=100,
        t=100,
        pad=4
    ),
    paper_bgcolor="LightSteelBlue",
)
fig.show()

```


<div>                            <div id="05d67086-6f06-446f-beb9-050b789997c4" class="plotly-graph-div" style="height:1000px; width:900px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("05d67086-6f06-446f-beb9-050b789997c4")) {                    Plotly.newPlot(                        "05d67086-6f06-446f-beb9-050b789997c4",                        [{"alignmentgroup": "True", "hovertemplate": "amount_projects=%{x}<br>country=%{y}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa"}, "name": "", "offsetgroup": "", "orientation": "h", "showlegend": false, "textposition": "auto", "type": "bar", "x": [1.0, 1.0, 1.0, 2.0, 2.0, 2.0, 4.0, 8.0, 10.0, 48.0, 68.0, 75.0, 125.0, 128.0, 134.0, 160.0, 180.0, 190.0, 193.0, 223.0, 284.0, 348.0, 378.0, 422.0, 496.0, 497.0, 554.0, 717.0, 784.0, 880.0, 963.0, 964.0, 993.0, 1320.0, 1419.0, 1486.0, 1561.0, 1639.0, 1703.0, 1870.0, 1934.0, 1945.0, 2230.0, 2313.0, 2409.0, 2460.0, 2690.0, 3073.0, 3269.0, 3483.0, 3617.0, 3682.0, 3821.0, 4034.0, 4167.0, 4374.0, 5219.0, 5415.0, 5741.0, 5749.0, 5774.0, 6093.0, 6214.0, 6557.0, 6639.0, 6735.0, 7310.0, 7396.0, 8167.0, 8631.0, 8792.0, 10136.0, 11237.0, 11781.0, 11903.0, 13521.0, 17612.0, 19580.0, 20601.0, 21686.0, 21995.0, 22233.0, 26857.0, 34836.0, 39875.0, 75825.0, 160441.0, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null], "xaxis": "x", "y": ["Mauritania", "Guam", "Cote D'Ivoire", "Afghanistan", "Virgin Islands", "Bhutan", "Vanuatu", "Namibia", "Chile", "Saint Vincent and the Grenadines", "Puerto Rico", "Somalia", "Belize", "Congo", "China", "South Sudan", "Thailand", "Israel", "Panama", "Suriname", "Brazil", "Moldova", "South Africa", "Lesotho", "Dominican Republic", "Benin", "Solomon Islands", "Nepal", "Zambia", "Burundi", "Ukraine", "Mongolia", "Iraq", "Malawi", "Kosovo", "Lao People's Democratic Republic", "Costa Rica", "Egypt", "Turkey", "Myanmar (Burma)", "Albania", "Azerbaijan", "Cameroon", "Yemen", "Georgia", "Burkina Faso", "Timor-Leste", "The Democratic Republic of the Congo", "Senegal", "Mozambique", "Haiti", "Liberia", "Madagascar", "Zimbabwe", "Jordan", "Ghana", "Tanzania", "Sierra Leone", "Mexico", "Togo", "Kyrgyzstan", "United States", "Indonesia", "Honduras", "Mali", "Rwanda", "Guatemala", "Samoa", "Palestine", "Armenia", "Lebanon", "Nigeria", "India", "Nicaragua", "Paraguay", "Ecuador", "Bolivia", "Tajikistan", "Uganda", "Vietnam", "Colombia", "Peru", "Pakistan", "Cambodia", "El Salvador", "Kenya", "Philippines", "Afghanistan", "Albania", "Armenia", "Azerbaijan", "Belize", "Bhutan", "Bolivia", "Brazil", "Cambodia", "Chile", "China", "Colombia", "Costa Rica", "Dominican Republic", "Ecuador", "El Salvador", "Georgia", "Guam", "Guatemala", "Haiti", "Honduras", "India", "Indonesia", "Iraq", "Israel", "Jordan", "Kosovo", "Kyrgyzstan", "Lao People's Democratic Republic", "Lebanon", "Mexico", "Moldova", "Mongolia", "Myanmar (Burma)", "Nepal", "Nicaragua", "Pakistan", "Palestine", "Panama", "Paraguay", "Peru", "Philippines", "Puerto Rico", "Saint Vincent and the Grenadines", "Samoa", "Solomon Islands", "Suriname", "Tajikistan", "Thailand", "Timor-Leste", "Turkey", "Ukraine", "United States", "Vanuatu", "Vietnam", "Virgin Islands", "Yemen", "Albania", "Belize", "Benin", "Bolivia", "Brazil", "Burkina Faso", "Burundi", "Cameroon", "Chile", "Colombia", "Congo", "Costa Rica", "Cote D'Ivoire", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Ghana", "Guam", "Guatemala", "Haiti", "Honduras", "Kenya", "Kosovo", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mexico", "Moldova", "Mozambique", "Namibia", "Nicaragua", "Nigeria", "Panama", "Paraguay", "Peru", "Puerto Rico", "Rwanda", "Saint Vincent and the Grenadines", "Samoa", "Senegal", "Sierra Leone", "Solomon Islands", "Somalia", "South Africa", "South Sudan", "Suriname", "Tanzania", "The Democratic Republic of the Congo", "Togo", "Turkey", "Uganda", "Ukraine", "United States", "Vanuatu", "Virgin Islands", "Zambia", "Zimbabwe", "Afghanistan", "Armenia", "Azerbaijan", "Belize", "Benin", "Bhutan", "Bolivia", "Brazil", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Chile", "China", "Colombia", "Congo", "Costa Rica", "Cote D'Ivoire", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Georgia", "Ghana", "Guam", "Guatemala", "Haiti", "Honduras", "India", "Indonesia", "Iraq", "Israel", "Jordan", "Kenya", "Kyrgyzstan", "Lao People's Democratic Republic", "Lebanon", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mexico", "Mongolia", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Nicaragua", "Nigeria", "Pakistan", "Palestine", "Panama", "Paraguay", "Peru", "Philippines", "Puerto Rico", "Rwanda", "Saint Vincent and the Grenadines", "Samoa", "Senegal", "Sierra Leone", "Solomon Islands", "Somalia", "South Africa", "South Sudan", "Suriname", "Tajikistan", "Tanzania", "Thailand", "The Democratic Republic of the Congo", "Timor-Leste", "Togo", "Uganda", "United States", "Vanuatu", "Vietnam", "Virgin Islands", "Yemen", "Zambia", "Zimbabwe", "Afghanistan", "Albania", "Armenia", "Azerbaijan", "Benin", "Bhutan", "Bolivia", "Brazil", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Chile", "China", "Colombia", "Congo", "Cote D'Ivoire", "Ecuador", "Egypt", "Georgia", "Ghana", "Guam", "India", "Indonesia", "Iraq", "Israel", "Jordan", "Kenya", "Kosovo", "Kyrgyzstan", "Lao People's Democratic Republic", "Lebanon", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Moldova", "Mongolia", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Nigeria", "Pakistan", "Palestine", "Paraguay", "Peru", "Philippines", "Rwanda", "Samoa", "Senegal", "Sierra Leone", "Solomon Islands", "Somalia", "South Africa", "South Sudan", "Suriname", "Tajikistan", "Tanzania", "Thailand", "The Democratic Republic of the Congo", "Timor-Leste", "Togo", "Turkey", "Uganda", "Ukraine", "Vanuatu", "Vietnam", "Yemen", "Zambia", "Zimbabwe", "Afghanistan", "Albania", "Armenia", "Azerbaijan", "Belize", "Benin", "Bhutan", "Bolivia", "Brazil", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Chile", "China", "Colombia", "Congo", "Costa Rica", "Cote D'Ivoire", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Georgia", "Ghana", "Guatemala", "Haiti", "Honduras", "India", "Indonesia", "Iraq", "Israel", "Jordan", "Kenya", "Kosovo", "Kyrgyzstan", "Lao People's Democratic Republic", "Lebanon", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mexico", "Moldova", "Mongolia", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Nicaragua", "Nigeria", "Pakistan", "Palestine", "Panama", "Paraguay", "Peru", "Philippines", "Puerto Rico", "Rwanda", "Saint Vincent and the Grenadines", "Senegal", "Sierra Leone", "Somalia", "South Africa", "South Sudan", "Suriname", "Tajikistan", "Tanzania", "Thailand", "The Democratic Republic of the Congo", "Timor-Leste", "Togo", "Turkey", "Uganda", "Ukraine", "United States", "Vietnam", "Virgin Islands", "Yemen", "Zambia", "Zimbabwe", "Afghanistan", "Albania", "Armenia", "Azerbaijan", "Belize", "Benin", "Bhutan", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "China", "Congo", "Costa Rica", "Cote D'Ivoire", "Dominican Republic", "Egypt", "El Salvador", "Georgia", "Ghana", "Guam", "Guatemala", "Haiti", "Honduras", "India", "Indonesia", "Iraq", "Israel", "Jordan", "Kenya", "Kosovo", "Kyrgyzstan", "Lao People's Democratic Republic", "Lebanon", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mexico", "Moldova", "Mongolia", "Mozambique", "Myanmar (Burma)", "Namibia", "Nepal", "Nicaragua", "Nigeria", "Pakistan", "Palestine", "Panama", "Philippines", "Puerto Rico", "Rwanda", "Saint Vincent and the Grenadines", "Samoa", "Senegal", "Sierra Leone", "Solomon Islands", "Somalia", "South Africa", "South Sudan", "Tajikistan", "Tanzania", "Thailand", "The Democratic Republic of the Congo", "Timor-Leste", "Togo", "Turkey", "Uganda", "Ukraine", "United States", "Vanuatu", "Vietnam", "Virgin Islands", "Yemen", "Zambia", "Zimbabwe"], "yaxis": "y"}],                        {"autosize": false, "barmode": "group", "font": {"size": 8}, "height": 1000, "legend": {"tracegroupgap": 0}, "margin": {"b": 100, "l": 50, "pad": 4, "r": 50, "t": 100}, "paper_bgcolor": "LightSteelBlue", "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "width": 900, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "amount_projects"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "country"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('05d67086-6f06-446f-beb9-050b789997c4');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.pie(df_cont, values='amount_projects', names=['Africa', 'Asia', 'Europe', 'North America', "Oceania", "South America"], title='payment interval')
fig.show()
```


<div>                            <div id="be300d50-e364-4beb-87ab-66dbf64eace5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("be300d50-e364-4beb-87ab-66dbf64eace5")) {                    Plotly.newPlot(                        "be300d50-e364-4beb-87ab-66dbf64eace5",                        [{"domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "hovertemplate": "label=%{label}<br>amount_projects=%{value}<extra></extra>", "labels": ["Africa", "Asia", "Europe", "North America", "Oceania", "South America"], "legendgroup": "", "name": "", "showlegend": true, "type": "pie", "values": [161493, 309295, 5938, 77756, 7367, 82011]}],                        {"legend": {"tracegroupgap": 0}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "payment interval"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('be300d50-e364-4beb-87ab-66dbf64eace5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.pie(df_cont, values='funded_amount', names=['Africa', 'Asia', 'Europe', 'North America', "Oceania", "South America"], title='payment interval')
fig.show()
```


<div>                            <div id="e7b76d01-36d2-462f-bb73-834657a41cef" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e7b76d01-36d2-462f-bb73-834657a41cef")) {                    Plotly.newPlot(                        "e7b76d01-36d2-462f-bb73-834657a41cef",                        [{"domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "hovertemplate": "label=%{label}<br>funded_amount=%{value}<extra></extra>", "labels": ["Africa", "Asia", "Europe", "North America", "Oceania", "South America"], "legendgroup": "", "name": "", "showlegend": true, "type": "pie", "values": [120477480.0, 192273700.0, 6908025.0, 84778390.0, 5737670.0, 117102550.0]}],                        {"legend": {"tracegroupgap": 0}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "autotypenumbers": "strict", "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "payment interval"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e7b76d01-36d2-462f-bb73-834657a41cef');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.scatter(df_group_sector_mean, x="lender_count", y="funded_amount", size="sector_size", color="team_count",
           hover_name="sector", size_max=60)
fig.show()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-41-31a89961b943> in <module>
    ----> 1 fig = px.scatter(df_group_sector_mean, x="lender_count", y="funded_amount", size="sector_size", color="team_count",
          2            hover_name="sector", size_max=60)
          3 fig.show()
    

    ~\anaconda3\lib\site-packages\plotly\express\_chart_types.py in scatter(data_frame, x, y, color, symbol, size, hover_name, hover_data, custom_data, text, facet_row, facet_col, facet_col_wrap, facet_row_spacing, facet_col_spacing, error_x, error_x_minus, error_y, error_y_minus, animation_frame, animation_group, category_orders, labels, orientation, color_discrete_sequence, color_discrete_map, color_continuous_scale, range_color, color_continuous_midpoint, symbol_sequence, symbol_map, opacity, size_max, marginal_x, marginal_y, trendline, trendline_color_override, log_x, log_y, range_x, range_y, render_mode, title, template, width, height)
         62     mark in 2D space.
         63     """
    ---> 64     return make_figure(args=locals(), constructor=go.Scatter)
         65 
         66 
    

    ~\anaconda3\lib\site-packages\plotly\express\_core.py in make_figure(args, constructor, trace_patch, layout_patch)
       1859     apply_default_cascade(args)
       1860 
    -> 1861     args = build_dataframe(args, constructor)
       1862     if constructor in [go.Treemap, go.Sunburst] and args["path"] is not None:
       1863         args = process_dataframe_hierarchy(args)
    

    ~\anaconda3\lib\site-packages\plotly\express\_core.py in build_dataframe(args, constructor)
       1375     # now that things have been prepped, we do the systematic rewriting of `args`
       1376 
    -> 1377     df_output, wide_id_vars = process_args_into_dataframe(
       1378         args, wide_mode, var_name, value_name
       1379     )
    

    ~\anaconda3\lib\site-packages\plotly\express\_core.py in process_args_into_dataframe(args, wide_mode, var_name, value_name)
       1181                         if argument == "index":
       1182                             err_msg += "\n To use the index, pass it in directly as `df.index`."
    -> 1183                         raise ValueError(err_msg)
       1184                 elif length and len(df_input[argument]) != length:
       1185                     raise ValueError(
    

    ValueError: Value of 'size' is not the name of a column in 'data_frame'. Expected one of ['sector', 'amount_projects', 'funded_amount', 'loan_amount', 'success_ratio', 'team_count', 'term_in_months', 'lender_count'] but received: sector_size


##### ERKENNTNISSE

Es besteht augenscheinlich ein Zusammenhang zwischen Anzahl Darlehnensgeber und ausgezahltem Fundingbetrag. Je höher der Betrag, desto mehr 
Kein Zusammenhang zwischen Teamgröße ausgezahltem Fundingbetrag, als auch Anzahl Darlehensgeber


```python
# Daten zur Graphik - MITTELWERTE
# sector_size: absolute Anzahl Projekte
# Alle weiteren Variablen: Mittelwert

df_group_sector_mean
```


```python
# Daten wie oben, jedoch zum Vergleich mit MEDIANEN
# sector_size: absolute Anzahl Projekte
# Alle weiteren Variablen: Median

df_group_sector_median = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.median, "loan_amount":np.median, "success_ratio":np.median, "team_count":np.median, "term_in_months":np.median, "lender_count":np.median})
df_group_sector_rename = df_group_sector_median.rename(columns={"sector": "sector_size"})
df_group_sector_median = df_group_sector_rename.reset_index()
df_group_sector_median
```


```python

```


```python
fig = px.scatter(df_group_sector_mean, x="loan_amount", y="funded_amount", size="sector_size", color="lender_count",
           hover_name="sector", size_max=60)
fig.show()
```


```python
fig = px.scatter(df_group_sector_mean, x="team_count", y="funded_amount", size="sector_size", color="lender_count",
           hover_name="sector", size_max=60)
fig.show()
```


```python
fig = px.scatter(df_group_sector_mean, x="lender_count", y="funded_amount", size="sector_size", color="team_count",
           hover_name="sector", size_max=60)
fig.show()
```


```python
# Nach Aktivität groupieren gruppieren

df_group_activity = df_final.groupby("activity").agg({"sector":np.size, "funded_amount":np.mean, "loan_amount":np.mean, "success_ratio":np.mean, "team_count":np.mean, "term_in_months":np.mean, "lender_count":np.mean})
df_group_sector_rename = df_group_activity.rename(columns={"sector": "activity_size"})
df_group_activity = df_group_sector_rename.reset_index()
df_group_activity
```


```python
fig = px.scatter(df_group_activity, x="funded_amount", y="lender_count", color="term_in_months",
           hover_name="activity", size_max=70)
fig.show()
```

## Länder bzgl. Projektanzahl, Funding-Höhe und Funding Erfolg


```python
df_final.info()
```


```python
# Nach Aktivität groupieren gruppieren

df_group_activity = df_final.groupby("activity").agg({"sector":np.size, "funded_amount":np.mean, "loan_amount":np.mean, "success_ratio":np.mean, "team_count":np.mean, "term_in_months":np.mean, "lender_count":np.mean})
df_group_activity
```


```python
df_final.loc[df_final["activity"]=="Adult Care", :]
```


```python
# Nach Aktivität groupieren gruppieren

df_group_activity = df_final.groupby(["sector", "activity"]).agg({"sector":np.size, "funded_amount":np.mean, "loan_amount":np.mean, "success_ratio":np.mean, "team_count":np.mean, "term_in_months":np.mean, "lender_count":np.mean})
df_group_activity
```


```python

```


```python
fig = px.scatter(df_group_sector, x="loan_amount", y="funded_amount", size="sector_size", color="lender_count",
           hover_name="sector", size_max=60)
fig.show()
```



## Länder bzgl. Projektanzahl, Funding-Höhe und Funding Erfolg


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```

## Verteilung Länder und Sektoren 


```python
fig = px.treemap(df_dropna_funding, path=[px.Constant('world'), 'continent', 'country', "sector"], values='funded_amount',
                  color='funded_amount')
fig.show()
```

# Drei Haupt-Visualisierungen 

## Beeinflusst der Sektor die Anzahl Darlehensgeber und -höhe?


```python
# Daten für ax1 vorbereiten

df_funding_sector = df_final.groupby("sector").agg({"funded_amount":np.mean})
df_fund_sector_sorted = df_funding_sector.sort_values(by=["funded_amount"], ascending=True)
df_fund_sector_sorted

# Daten für ax2 vorbereiten

df_funding_lender = df_final.groupby("sector").agg({"lender_count":np.mean,"funded_amount":np.mean})
df_funding_lender_sorted = df_funding_lender.sort_values(by=["funded_amount"], ascending=True)
df_funding_lender_sorted

# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_sector_sorted.index, y=df_fund_sector_sorted["funded_amount"], palette="Blues",data=df_fund_sector_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Median Höhe Darlehen", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_funding_lender_sorted.index, y=df_funding_lender_sorted["lender_count"], data=df_funding_lender_sorted, color="darkred")
ax2.set_ylabel('Median Anzahl Darlehensgeber', fontsize=14)

plt.show()
```


```python
# Daten für ax1 vorbereiten

df_funding_sector = df_final.groupby("sector").agg({"funded_amount":np.median})
df_fund_sector_sorted = df_funding_sector.sort_values(by=["funded_amount"], ascending=True)
df_fund_sector_sorted
```


```python
# Daten für ax2 vorbereiten

df_funding_lender = df_final.groupby("sector").agg({"lender_count":np.median,"funded_amount":np.median})
df_funding_lender_sorted = df_funding_lender.sort_values(by=["funded_amount"], ascending=True)
df_funding_lender_sorted
```


```python

```


```python
# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_sector_sorted.index, y=df_fund_sector_sorted["funded_amount"], palette="Blues",data=df_fund_sector_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Median Höhe Darlehen", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_funding_lender_sorted.index, y=df_funding_lender_sorted["lender_count"], data=df_funding_lender_sorted, color="darkred")
ax2.set_ylabel('Median Anzahl Darlehensgeber', fontsize=14)

plt.show()
```

##### Fragestellung bei Erstellung der Graphik: 

Beeinflusst der Sektor die Anzahl Darlehensgeber und -höhe?

Aus dem Data Preprocessing Pairplot ist bereits bekant, dass Anzahl Darlehensgeber steigt, je höher das Darlehenen ist.

Nun interessiert, ob ...
- dieser Zusammenhang je Sektor unterschiedlich ist. 
- Müssen also Gründer je Sektor gegebenfalls weniger oder mehr Darlehensgeber überzeugen, um die für den Sektor übliche erforderliche Summe zu erhalten?  
 

##### Erkenntnisse

- Dieser Zusammenhang besteht auch je Sektor überwiegend weiterhin.

- Einzig bei "Personal Use" braucht es im Verhältnis zur Darlehenshöhe sichtbar weniger Darlehensgeber sowie bei "Wholesale" im Verhältnis mehr. Dies könnte sich aus dem insgesamt niedrigen Darlehensnivau bei "Personal Use" bzw. bei dem hohen von "Wholesale" erklären. Für einen geringe Darlehenshöhe werden auch weniger Darlehensgeber benötigt.


## Überzeugen weniger erfolgreiche Teams auch weniger die Investoren? 


```python
df_success = df_final.loc[df_final["success_ratio"]==100,:]
df_success_medium = df_final.loc[(df_final["success_ratio"]<100) & (df_final["success_ratio"]>50)]
df_no_success = df_final.loc[(df_final["success_ratio"]<=50),:]
```


```python
df_success = df_final.loc[df_final["success_ratio"]==100,:]
df_success
```


```python
df_success_medium = df_final.loc[(df_final["success_ratio"]<100) & (df_final["success_ratio"]>50)]
df_success_medium
```


```python
# DataFrame mit wenig erfolgreichen Fundings erstellen (Erfolgsquote kleiner gleich fünfzig)

df_no_success = df_final.loc[(df_final["success_ratio"]<=50),:]
df_no_success
```


```python
# DataFrame mit erfolgreichen Fundings (Erfolgsquote = 100%), mit wenig erfolgreichen (Erfolgsquote < 50%) 
# und dazwischen liegenden erstellen

df_success = df_final.loc[df_final["success_classes"]=="Gleich100",:]
df_success_medium = df_final.loc[df_final["success_classes"]=="Größer50Kleiner100",:]
df_no_success = df_final.loc[df_final["success_classes"]=="KleinerGleich50",:]

# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Virgin Islands entfernen, da keine 
# Projekte gefördert wurden. Die Entferrnungen haben keinen Einfluss auf die Aussage der Graphik.

df_final = df_final.loc[df_final["country_code"]!="CI",:]
df_final = df_final.loc[df_final["country_code"]!="MR",:]
df_final = df_final.loc[df_final["country_code"]!="BT",:]
df_final = df_final.loc[df_final["country_code"]!="AF",:]
df_final = df_final.loc[df_final["country_code"]!="VI",:]

df_success = df_success.loc[df_success["country_code"]!="CI",:]
df_success = df_success.loc[df_success["country_code"]!="MR",:]
df_success = df_success.loc[df_success["country_code"]!="BT",:]
df_success = df_success.loc[df_success["country_code"]!="AF",:]
df_success = df_success.loc[df_success["country_code"]!="VI",:]

df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="CI",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="MR",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="BT",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="AF",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="VI",:]

df_no_success = df_no_success.loc[df_no_success["country_code"]!="CI",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="MR",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="BT",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="AF",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="VI",:]
```


```python
# Daten für ax1 vorbereiten
df_fund_country = df_final.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_fund_country_sorted = df_fund_country.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax2_success vorbereiten
df_succ_sect = df_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_succ_sect_sorted = df_succ_sect.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax3_no_success vorbereiten
df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["funded_amount"], ascending=True)
```


```python
df_final = df_final.loc[df_final["country_code"]!=("CI", "MR", "BT", "AF", "VI"),:]
df_success = df_success.loc[df_success["country_code"]!=("CI", "MR", "BT", "AF", "VI"),:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!=("CI", "MR", "BT", "AF", "VI"),:]

671204
```


```python
df_final
```


```python
df_test = df_final.copy()
```


```python
indexNames = df_test[(df_test['country_code'] == "CI") | (df_test['country_code'] == "MR") | 
                      (df_test['country_code'] == "BT") | (df_test['country_code'] == "AF") |
                     (df_test['country_code'] == "VI")].index 
df_test.drop(indexNames, inplace=True)
```


```python
df_test
```


```python

df_final = df_final.loc[(df_final["country_code"]!="CI",:]
df_final = df_final.loc[df_final["country_code"]!="MR",:]
df_final = df_final.loc[df_final["country_code"]!="BT",:]
df_final = df_final.loc[df_final["country_code"]!="AF",:]
df_final = df_final.loc[df_final["country_code"]!="VI",:]

df_success = df_success.loc[df_success["country_code"]!="CI",:]
df_success = df_success.loc[df_success["country_code"]!="MR",:]
df_success = df_success.loc[df_success["country_code"]!="BT",:]
df_success = df_success.loc[df_success["country_code"]!="AF",:]
df_success = df_success.loc[df_success["country_code"]!="VI",:]

df_no_success = df_no_success.loc[df_no_success["country_code"]!="CI",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="MR",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="BT",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="AF",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="VI",:]
```


```python
# Daten vorbereiten
# DataFrame mit erfolgreichen Fundings (Erfolgsquote = 100%), mit wenig erfolgreichen (Erfolgsquote < 50%) 
# und dazwischen liegenden erstellen

df_success = df_final.loc[df_final["success_classes"]=="Gleich100",:]
df_no_success = df_final.loc[df_final["success_classes"]=="KleinerGleich50",:]

# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Virgin Islands entfernen, da keine 
# Projekte gefördert wurden. Die Entferrnungen haben keinen Einfluss auf die Aussage der Graphik.

df_final = df_final.loc[df_final["country_code"]!="CI",:]
df_final = df_final.loc[df_final["country_code"]!="MR",:]
df_final = df_final.loc[df_final["country_code"]!="BT",:]
df_final = df_final.loc[df_final["country_code"]!="AF",:]
df_final = df_final.loc[df_final["country_code"]!="VI",:]

df_success = df_success.loc[df_success["country_code"]!="CI",:]
df_success = df_success.loc[df_success["country_code"]!="MR",:]
df_success = df_success.loc[df_success["country_code"]!="BT",:]
df_success = df_success.loc[df_success["country_code"]!="AF",:]
df_success = df_success.loc[df_success["country_code"]!="VI",:]

df_no_success = df_no_success.loc[df_no_success["country_code"]!="CI",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="MR",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="BT",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="AF",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="VI",:]

# Daten gruppieren
# Daten für ax1
df_fund_country = df_final.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_fund_country_sorted = df_fund_country.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax2_success 
df_succ_sect = df_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_succ_sect_sorted = df_succ_sect.sort_values(by=["funded_amount"], ascending=True)

# Daten für ax3_no_success 
df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.mean, "funded_amount":np.mean})
df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["funded_amount"], ascending=True)


# Plotten
fig, ax1 = plt.subplots(figsize=(14,14))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_country_sorted.index, y=df_fund_country_sorted["lender_count"], color="grey",data=df_fund_country_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=90, labelsize=9)
ax1.set_ylabel("Median Anzahl Darlehensgeber, alle Projekte (graue Balken)", fontsize=14)
ax1.set_xlabel("Länder", fontsize=14)

ax2 = sns.lineplot(x=df_succ_sect_sorted.index, y=df_succ_sect_sorted["lender_count"], data=df_succ_sect_sorted, color="lightcoral")
ax3 = sns.lineplot(x=df_no_succ_sect_sorted.index, y=df_no_succ_sect_sorted["lender_count"], data=df_no_succ_sect_sorted, color="darkred")

ax2.set_ylabel('Median Anzahl Darlehensgeber | alle Projekte (Balken) vs. Projekte Erfolgsquote <= 50% (Linie)', fontsize=14)

plt.show()
```


```python
# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Die Entferrnung hat keinen Einfluss auf 
# die Aussage der Graphik

df_final = df_final.loc[df_final["country_code"]!="CI",:]
df_final = df_final.loc[df_final["country_code"]!="MR",:]
df_final = df_final.loc[df_final["country_code"]!="BT",:]
df_final = df_final.loc[df_final["country_code"]!="AF",:]

```


```python
# DataFrame df_success
# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Die Entferrnung hat keinen Einfluss auf 
# die Aussage der Graphik

df_success = df_success.loc[df_success["country_code"]!="CI",:]
df_success = df_success.loc[df_success["country_code"]!="MR",:]
df_success = df_success.loc[df_success["country_code"]!="BT",:]
df_success = df_success.loc[df_success["country_code"]!="AF",:]
```


```python
# DataFrame df_success_medium
# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Die Entferrnung hat keinen Einfluss auf 
# die Aussage der Graphik

df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="CI",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="MR",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="BT",:]
df_success_medium = df_success_medium.loc[df_success_medium["country_code"]!="AF",:]
```


```python
# DataFrame df_no_success
# Elfenbeinküste, Mauretanien, Buthan und Afghanistan entfernen, da sie bis zu 17 mal so viele Darlehensgeber hat, 
# wie die anderen 95% der Länder und somit die Graphik stark verzerrt. Die Entferrnung hat keinen Einfluss auf 
# die Aussage der Graphik

df_no_success = df_no_success.loc[df_no_success["country_code"]!="CI",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="MR",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="BT",:]
df_no_success = df_no_success.loc[df_no_success["country_code"]!="AF",:]
```


```python
# Daten für ax1 vorbereiten
df_fund_country = df_final.groupby("country").agg({"lender_count":np.mean})
df_fund_country_sorted = df_fund_country.sort_values(by=["lender_count"], ascending=False)
df_fund_country_sorted
```


```python
# Daten für ax2_success vorbereiten

df_succ_sect = df_success.groupby("country").agg({"lender_count":np.mean})
df_succ_sect_sorted = df_succ_sect.sort_values(by=["lender_count"], ascending=False)
df_succ_sect_sorted
```


```python
# Daten für ax2 vorbereiten

df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.median})
df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["lender_count"], ascending=False)
df_no_succ_sect_sorted
```


```python
fig, ax1 = plt.subplots(figsize=(14,14))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_country_sorted.index, y=df_fund_country_sorted["lender_count"], color="wheat",data=df_fund_country_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=90, labelsize=9)
ax1.set_ylabel("Median Anzahl Darlehensgeber, alle Projekte (graue Balken)", fontsize=14)
ax1.set_xlabel("Länder", fontsize=14)

#ax2 = ax1.twinx() # Verzerrt aufgrund der unterschiedlichen Skalierung die Darstellung.
ax2 = sns.lineplot(x=df_succ_sect_sorted.index, y=df_succ_sect_sorted["lender_count"], data=df_succ_sect_sorted, color="goldenrod")
ax2.set_ylabel('Median Anzahl Darlehensgeber | alle Projekte (Balken) vs. Projekte Erfolgsquote <= 50% (Linie)', fontsize=14)

#ax2 = ax1.twinx() # Verzerrt aufgrund der unterschiedlichen Skalierung die Darstellung.
ax3 = sns.lineplot(x=df_no_succ_sect_sorted.index, y=df_no_succ_sect_sorted["lender_count"], data=df_no_succ_sect_sorted, color="goldenrod")
ax3.set_ylabel('Median Anzahl Darlehensgeber | alle Projekte (Balken) vs. Projekte Erfolgsquote <= 50% (Linie)', fontsize=14)


plt.show()
```


```python
# Daten für ax2 vorbereiten

df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.mean})
df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["lender_count"], ascending=True)
df_no_succ_sect_sorted
```


```python
# Daten für ax1 vorbereiten
df_fund_country = df_final.groupby("country").agg({"lender_count":np.mean})
# df_fund_country_sorted = df_fund_country.sort_values(by=["lender_count"], ascending=True)
# df_fund_country_sorted

# Daten für ax2 vorbereiten
df_no_succ_sect = df_no_success.groupby("country").agg({"lender_count":np.mean})
#df_no_succ_sect_sorted = df_no_succ_sect.sort_values(by=["lender_count"], ascending=False)
#df_no_succ_sect_sorted

# Plotten
fig, ax1 = plt.subplots(figsize=(14,14))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_country_sorted.index, y=df_fund_country_sorted["lender_count"], color="wheat",data=df_fund_country_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=90, labelsize=9)
ax1.set_ylabel("Median Anzahl Darlehensgeber, alle Projekte (graue Balken)", fontsize=14)
ax1.set_xlabel("Länder", fontsize=14)

#ax2 = ax1.twinx() # Verzerrt aufgrund der unterschiedlichen Skalierung die Darstellung.
ax2 = sns.lineplot(x=df_no_succ_sect.index, y=df_no_succ_sect["lender_count"], data=df_no_succ_sect, color="goldenrod")
ax2.set_ylabel('Median Anzahl Darlehensgeber | alle Projekte (Balken) vs. Projekte Erfolgsquote <= 50% (Linie)', fontsize=14)

#ax3 = sns.lineplot(x=df_succ_sect_sorted.index, y=df_succ_sect_sorted["lender_count"], data=df_succ_sect_sorted, color="darkred")

plt.show()
```

##### Fragestellung bei Erstellung der Graphik: 

Überzeugen weniger erfolgreiche Teams auch weniger die Investoren? 
Gibt je Land einen Unterschied bzgl. Anzahl der Darlehensgeber bei weniger "erfolgreichen" Teams (bzgl. Darlehenserhalt) gegenüber allen Teams? 


##### Erkenntnisse

- Weniger erfolgreiche Teams haben in fast allen Ländern __deutlich weniger Darlehensgeber__. Dies scheint schlüssig, da sie ggf. weniger Personen für Ihre Idee gewinnen konnten. Ein anderer Grund könnte sein, dass mehr Personen eher in der Lage sind, eine Idee angemessen zu bewerten. 

- Die __Unterschiede je Land sind sehr hetegerogen__. Hier könnte weitere Analyse, z.B. spielt höchstwahrscheinlich die Darlehenshöhe hier wiederum eine Rollen. Eine Betrachtung je Kontinent und somit kulturelle Hintergründe könnte weiteren auf Schluss geben.

## Inwieweit beinflusst Team & Zahlungsmodalitäten die Darlehenshöhe?


```python
# Datensatzkopie erstellen

df_facetplot = df_final.copy()
```


```python
# Test, ob erfolgreich

df_facetplot.info()
```


```python
# nicht notwendige Zeilen entfernen

df_facetplot_drop =df_facetplot.drop(["loan_amount", "activity", "sector", "country", "currency", "country_code", "term_in_months", "lender_count", "female", "male"], axis=1)
```


```python
# Test, ob erfolgreich

df_facetplot_drop.info()
```


```python
# Zeilen mit NaN's löschen
# NaN's existieren nur in den Spalten team_count, female, male, sex_majority und team_category und stellen 
# mit 4221 NaNs nur 0,62% der Gesamtdaten dar. 
# Da für die folgende Graphike besonders "Team-Merkmale" (team_category und sex_majority) wichtig sind, 
# wurde beschlossen für den DataFrame für diese Graphik die NaNs zu löschen.

df_facetplot_oNaN = df_facetplot_drop.dropna(0)
```


```python
# "Downcasting" von float Datensätzen

df_facetplot_oNaN["success_ratio"] = df_facetplot_oNaN["success_ratio"].apply(pd.to_numeric, downcast="float")
```


```python
# Kontrolle, ob erfolgreich

df_facetplot_oNaN.info()
```


```python
# Daten groupieren, damit sie im Plot leicht geladen werden können.

df_team_credit_cond = df_facetplot_oNaN.groupby(["sex_majority", "team_category", "repayment_interval"]).agg({"funded_amount":np.size})
```


```python
# Test, ob erfolgreich

df_team_credit_cond
```


```python
# Spaltennamen "auf eine Ebene" bringen

df_team_credit_cond.reset_index(inplace=True)
```


```python
# Test, ob erfolgreich

df_team_credit_cond.head(2)
```


```python
fig = px.bar(df_team_credit_cond, x="team_category", y="funded_amount", color="sex_majority", barmode="group", facet_col="repayment_interval", 
       category_orders={"repayment_interval": ["bullet", "irregular", "monthly", "weekly"]})
fig.show()
```

##### Fragestellung bei Erstellung der Graphik: 

Welche Zusammenhänge gibt es zwischen Teamgröße, Geschlechterverteilung, Auszahlungmodaliäten und der Darlehenshöhe?


##### Erkenntnisse

- Frauen scheinen mehr individuelle Bedingungen zu erhalten, während Männer eher einen monatlichen Kreditrückzahlungsmodus oder einen Bullet-Kredit erhalten. 
- Bulletkredite gehen weniger an Einzelpersonen als bei den anderen Kreditformen. Bulletkredite werden eher für Gründungen mit unregelmäßigen Rückflüssen verwendet. Dies könnte für komplexere Projekte sprechen, bei denen  spezifischen Fachwissen und somit mehr Personen von Nöten sind.  

# Anhang - weitere Visualisierungen

## Verschiedene Plots bzgl. Erfolgsquote


```python
# Scatterplot für Zusammenhang Erfolgsquote/Funding Höhe mit lendercount, teams größe und Zusammensetzung
```


```python
df_final.info()
```


```python
df_final.describe()
```

Datenvorbereitung:

Aus dem Data Preprocessing Pairplot und der deskriptiven Statistik sehen wir, dass ...


```python
# DataFrame vorbereiten: funded_amount reduzieren

df_success_team = df_final.loc[(df_final["funded_amount"]<=10000) & (df_final["loan_amount"]<=10000),:]
```


```python
# Test of erfolgreich

df_success_team.describe()
```


```python
# Scatterplot erstellen

size = df_success_team["team_count"]**5

f, ax = plt.subplots(figsize=(8, 8))
sns.set_theme(style="whitegrid")

sns.scatterplot(x="loan_amount", y="funded_amount",
                hue="sex_majority", size=size,
                palette="deep",
                linewidth=0,
                data=df_success_team, ax=ax)

plt.show()
```

##### Erkenntnisse

Duch die hohe Menge der Projekte, macht Darstellung wenig Sinn.

Mögliche Lösugn: Entertainment untersuchen, da niedrigste Erfolgsquote und zweitkleinste Segment


```python
# DataFrame vorbereiten

df_success_team_ent = df_success_team.loc[(df_success_team["sector"]=="Entertainment")]
df_success_team_ent
```


```python
# Scatterplot erstellen

size = df_success_team_ent["team_count"]**5

f, ax = plt.subplots(figsize=(8, 8))
sns.set_theme(style="whitegrid")

sns.scatterplot(x="loan_amount", y="funded_amount",
                hue="team_category", size=size,
                palette="deep",
                linewidth=0,
                data=df_success_team_ent, ax=ax)

plt.show()
```

##### Erkenntnisse

Die Team Kategorie bringt mehr Informationen. Alle Projekte, die nicht oder nur teilweis Darlehnen erhalten haben, waren ausschliesslich one (wo)man shows.

Die Größe bietet bisher aufgrund der Menge der Fälle und der ungleichen Verteilung bisher keinen Mehrwert

Nun Versuch mit etwas mehr Fällen. Manufacturing ist etwas größer, jedoch noch nicht so groß


```python
# DataFrame vorbereiten

df_success_team_man = df_success_team.loc[(df_success_team["sector"]=="Housing")]
df_success_team_man
```


```python
# Scatterplot erstellen

size = df_success_team_man["team_count"]**5

f, ax = plt.subplots(figsize=(8, 8))
sns.set_theme(style="whitegrid")

sns.scatterplot(x="loan_amount", y="funded_amount",
                hue="team_category", size=size,
                palette="deep",
                linewidth=0,
                data=df_success_team_man, ax=ax)

plt.show()
```

## Funding

### Anzahl Fundingprojekte bzgl. Dauer Kreditauszahlung


```python
# DataFrame für Verteilung erstellen
df_term_in_months = df_final.groupby(["term_in_months"]).size()

# Barplot erstellen
fig = px.bar(df_term_in_months, 
             labels={'value':'Anzahl Fundingprojekte', 'term_in_months':'Dauer Kreditauszahlung in Monaten'}, 
             height=500)
fig.show()
```

##### Erkenntnisse: 
- Der Großteil der Funding Projekte wird nicht über einen längeren Zeitraum als 15 Monate ausgezahlt.
- 14 Monate und 8 Monate sind mit großem Abstand die am häufigsten gewählten Zeiträume.
- Mehr als 27 Monate wird nur bei wenigen Ausnahmen ausgezahlt. den wenigsten  ausgezahlt

### Kumulierte Fundinghöhe je Country


```python
# DataFrame für Verteilung erstellen

df_funding_country = df_final.groupby("country_code").agg({"funded_amount":np.sum})
df_fund_contry_sorted = df_funding_country.sort_values(by=["funded_amount"], ascending=False)
```


```python
# Barplot erstellen

fig = px.bar(df_fund_contry_sorted)
fig.show()
```

### Durchschnittliche Fundinghöhe je Country


```python
# DataFrame für Verteilung erstellen

df_funding_country = df_final.groupby("country_code").agg({"funded_amount":np.mean})
df_fund_contry_sorted = df_funding_country.sort_values(by=["funded_amount"], ascending=False)
```


```python
# Barplot erstellen

fig = px.bar(df_fund_contry_sorted)
fig.show()
```

### Kumulierte Funding Höhe je Sector


```python
# DataFrame für Verteilung erstellen

df_funding_sector = df_final.groupby("sector").agg({"funded_amount":np.sum})
df_fund_sector_sorted = df_funding_sector.sort_values(by=["funded_amount"], ascending=False)
df_fund_sector_sorted
```


```python
# Barplot erstellen

fig = px.bar(df_fund_sector_sorted)
fig.show()
```

## Erfolgsquote

### Erfolgsquote je Sektor


```python
# DataFrame für Verteilung erstellen

df_success_ratio_sector = df_final.groupby("sector").agg({"success_ratio":np.mean})
df_success_ratio_sector
```


```python
# Barplot erstellen
fig = px.bar(df_success_ratio_sector, 
             labels={'value':'Mittelwert:<br>Verhältnis loan_amount zu funded_amount', 'country_code':'Länder'}, 
             height=500).update_xaxes(categoryorder="total ascending")
fig.show()
```

### Erfolgsquote je Land


```python
# DataFrame für Verteilung erstellen

df_success_ratio = df_final.groupby("country_code").agg({"success_ratio":np.mean})
df_success_ratio
```


```python
# Barplot erstellen
fig = px.bar(df_success_ratio, 
             labels={'value':'Mittelwert:<br>Verhältnis loan_amount zu funded_amount', 'country_code':'Länder'}, 
             height=500).update_xaxes(categoryorder="total ascending")
fig.show()
```

##### Erkenntnisse:
Der Großteil der Projekte hat über 90% der gewünschten Fundingsumme erhalten

### Nicht erfolgreiche Projekte


```python
# DataFrame erstellen mit wenig erfolgreichen erstellen (Erfolgsquote kleiner gleich fünfzig)

df_no_success = df_final.loc[(df_final["success_ratio"]<=50),:]
df_no_success
```

#### Bzgl. Teamgröße


```python
df_no_success_group = df_no_success.groupby("team_category").size()

fig = px.bar(df_no_success_group).update_xaxes(categoryorder="total ascending")
fig.show()
```

#### Bzgl. Geschlecht


```python
df_no_success_sex = df_no_success.groupby("sex_majority").size()

fig = px.bar(df_no_success_sex,
            labels={'value':'numer of funding projects', 'sex_majority':'sex majority per team'}, 
             height=500
            ).update_xaxes(categoryorder="total ascending")
fig.show()
```

## Sektoren

### Erfolgsquote je Sektor und Teamgröße


```python
# DataFrame erstellen

df_sector_team_cat = df_final.groupby(["sector", "team_category"], as_index=False).agg({"success_ratio":np.mean})
df_sector_team_cat
```


```python
df_sector_team_cat.columns
```


```python
fig = px.bar(df_sector_team_cat, x="sector", y="success_ratio", color="team_category", barmode="group")
fig.show()
```

### Sektor bzgl. Darlehenshöhe & Geber - Mittelwert


```python
# Daten für ax1 vorbereiten

df_funding_sector_avg = df_final.groupby("sector").agg({"funded_amount":np.mean})
df_fund_sector_avg_sorted = df_funding_sector_avg.sort_values(by=["funded_amount"], ascending=False)
df_fund_sector_avg_sorted
```


```python
# Daten für ax2 vorbereiten

df_funding_lender_avg = df_final.groupby("sector").agg({"lender_count":np.mean})
df_funding_lender_avg_sorted = df_funding_lender_avg.sort_values(by=["lender_count"], ascending=False)
df_funding_lender_avg_sorted
```


```python
# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_sector_avg_sorted.index, y=df_fund_sector_avg_sorted["funded_amount"], palette="Blues_r",data=df_fund_sector_avg_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Durchschnittliche Höhe Darlehen", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_funding_lender_avg_sorted.index, y=df_funding_lender_avg_sorted["lender_count"], data=df_funding_lender_avg_sorted, color="darkred")
ax2.set_ylabel('Durchschnitt Anzahl Darlehensgeber', fontsize=14)

plt.show()
```

## Team Verteilungen

### Geschlechterverhältnis 


```python
df_all_sex = df_final.groupby("sex_majority").size()

fig = px.bar(df_all_sex,
            labels={'value':'numer of funding projects', 'sex_majority':'sex majority per team'}, 
             height=500
            ).update_xaxes(categoryorder="total ascending")
fig.show()
```

### team_member_count


```python
# DataFrame für Verteilung erstellen
df_team_member = df_final.groupby("team_count").size()

# Barplot erstellen
fig = px.bar(df_team_member,
             labels={'value':'Anzahl Funding Projekte', 'team_member_count':'Anzahl Teammitglieder'}, 
             height=500).update_xaxes(categoryorder="total ascending")
fig.show()
```

##### Erkenntnisse:
Der größte Teil der Funding Projekte, werden von einer Person durchgeführt.


### Teamgröße und Erfolgsquote


```python
df_sector_team_cat = df_final.groupby(["sector", "team_category"], as_index=False).agg({"success_ratio":np.mean})
df_sector_team_cat
```


```python
df_success_team = df_final.groupby("team_category").agg({"success_ratio":np.mean})
df_success_team
```


```python
fig = px.bar(df_success_team)
fig.show()
```

## Kreditkonditionen

### repayment_interval


```python
# Ausprägungen und Häufigkeiten zu repayment_interval erhalten

df_final.groupby("repayment_interval").size()
```


```python
# Zusammenhang bzgl. funded_amount und repayment_interval betrachten
# DataFrame anlegen

df_repayment_sorted = df_final.groupby("repayment_interval").agg({"funded_amount":np.mean}).sort_values(by=["funded_amount"], ascending=False)
df_repayment_sorted
```


```python
df_repayment_sorted.index
```


```python
# Barplot erstellen

fig = px.bar(df_repayment_sorted)
fig.show()
```


```python
# Pieplot erstellen

fig = px.pie(df_repayment_sorted, values='funded_amount', names=['monthly', 'bullet', 'irregular', 'weekly'], title='payment interval')
fig.show()
```

##### Erkenntnis: 
Weekly interval kommen wie erwartet nur für kleinere Kredite in Betracht


```python
# Daten für ax1 vorbereiten

df_funding_country = df_final.groupby("country").agg({"funded_amount":np.median})
df_fund_country_sorted = df_funding_country.sort_values(by=["funded_amount"], ascending=True)
df_fund_country_sorted

# Daten für ax2 vorbereiten

df_funding_lender = df_final.groupby("country").agg({"lender_count":np.median})
df_funding_lender_sorted = df_funding_lender.sort_values(by=["lender_count"], ascending=True)
df_funding_lender_sorted

# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_fund_country_sorted.index, y=df_fund_country_sorted["funded_amount"], palette="Blues",data=df_fund_country_sorted, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Median Höhe Darlehen", fontsize=14)
ax1.set_xlabel("Länder", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_funding_lender_sorted.index, y=df_funding_lender_sorted["lender_count"], data=df_funding_lender_sorted, color="darkred")
ax2.set_ylabel('Median Anzahl Darlehensgeber', fontsize=14)

plt.show()
```


```python
# ax1
df_sector = df_final.groupby("sector").agg({"funded_amount":np.sum})
df_sector_sorted_funded = df_sector.sort_values(by=["funded_amount"], ascending=True)

# ax2
df_sector = df_final.groupby("sector").agg({"sector":np.size})
df_sector = df_sector.rename(columns={"sector": "amount_projects"})
df_sector = df_sector_rename.reset_index()
df_sector_sorted_amount = df_sector.sort_values(by=["amount_projects"])

# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_sector_sorted_funded.index, y=df_sector_sorted_funded["funded_amount"], palette="Blues",data=df_sector_sorted_funded, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Durchschnitt Darlehenshöhe in US-Dollar (Balken)", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_sector_sorted_amount.index, y=df_sector_sorted_amount["amount_projects"], data=df_sector_sorted_amount, color="darkred")
ax2.set_ylabel('Durchschnitt Anzahl Darlehensgeber (Linie)', fontsize=14)

plt.show()
```


```python
# nach Funding-Summe

df_cont = df_final.groupby("continent").agg({"continent":np.size, "funded_amount":np.sum})
df_cont_rename = df_cont.rename(columns={"continent": "amount_projects"})
df_cont = df_cont_rename.reset_index()
df_cont_sorted = df_cont.sort_values(by=["amount_projects"])
#df_cont_sorted_fund = df_cont.sort_values(by=["funded_amount"])

sns.set(rc={'figure.figsize':(10,8)})
sns.set_theme(style="whitegrid")
ax = sns.barplot(x="continent", y="funded_amount", data=df_cont_sorted)
```


```python
fig = px.bar(df_cont_count_sorted, x="amount_projects", y="country", barmode="group")
fig.update_layout(
    font_size = 8,
    autosize=False,
    width=900,
    height=1000,
    margin=dict(
        l=50,
        r=50,
        b=100,
        t=100,
        pad=4
    ),
)
fig.show()
```


```python
# Daten für ax1 vorbereiten

df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.sum})
df_sector = df_sector.rename(columns={"sector": "amount_projects"})
df_sector_sum = df_sector.reset_index()

# Daten für ax2 vorbereiten

df_sector = df_final.groupby("sector").agg({"sector":np.size, "funded_amount":np.mean})
df_sector = df_sector.rename(columns={"sector": "amount_projects"})
df_sector_mean = df_sector.reset_index()

# Plot erstellen
 
fig, ax1 = plt.subplots(figsize=(10,10))
sns.set_style("whitegrid")
sns.barplot(x=df_sector_sum.index, y=df_sector_sum["funded_amount"], palette="Blues",data=df_sector_sum, ax=ax1)

ax1.xaxis.set_tick_params(rotation=70, labelsize=10)
ax1.set_ylabel("Absolute Fundinghöhe in US-Dollar (Balken)", fontsize=14)
ax1.set_xlabel("Sektor", fontsize=14)

ax2 = ax1.twinx()
ax2 = sns.lineplot(x=df_sector_mean["sector"], y=df_sector_mean["funded_amount"], data=df_sector_mean, color="darkred")
ax2.set_ylabel('Durchschnitt Fundinghöhe in US-Dollar (Linie)', fontsize=14)

plt.show()
```


```python
df_cont_count = df_dropna_funding.groupby(["continent", "country"]).agg({"continent":np.size, "funded_amount":np.sum})
df_cont_count_rename = df_cont_count.rename(columns={"continent": "amount_projects"})
df_cont_count = df_cont_count_rename.reset_index()
df_cont_count = df_cont_count.sort_values(by=["amount_projects"])
df_cont_count = df_cont_count.loc[df_cont_count["funded_amount"]!=0, :]
df_cont_count

fig = px.scatter(df_cont_count, x="amount_projects", y="funded_amount", color="continent",
           hover_name="country", log_x=True, size_max=60)
fig.show()
```

# Learnings

Bei Balkendiagrammen auf gleiche Reihenfolge achten