
# Pymaceuticals Inc.

### Analysis
- Observed Trend 1: In the analysis of the four drugs: Capomulin, Infubinol, Ketapril, and Placebo; we can observe that Carpolium is the most effective one. Having the highest rate of survival and also the most effective in terms of reducing the volume of the tumor (19.48%) over the 45 days of the study.
- Observed Trend 2: The other 3 drugs analyzed (Infubinol, Ketapril, and Placebo), does not seem to cause a positive outcome. The tumor keep growing and the survival rate of the mouses are around 10%. 
- Observed Trend 3: Even though Capomulin did not cure the mice it definitely performed better than the other 3 drugs analyzed.


```python
# Importing Dependencies
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from scipy.stats import sem
from scipy.stats import linregress
```


```python
# Reading the files
clinical_data = "raw_data/clinicaltrial_data.csv"
clinical_data_df = pd.read_csv(clinical_data)
#clinical_data_df.head()

mouse_data = "raw_data/mouse_drug_data.csv"
mouse_data_df = pd.read_csv(mouse_data)
#mouse_data_df.head()
```


```python
# Concatenating both tables mouse_data_df and clinical_data_df
clinic_mouse_merge = pd.merge(clinical_data_df, mouse_data_df, on="Mouse ID", how="outer")
clinic_mouse_merge.head()
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
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b128</td>
      <td>5</td>
      <td>45.651331</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>2</th>
      <td>b128</td>
      <td>10</td>
      <td>43.270852</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b128</td>
      <td>15</td>
      <td>43.784893</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b128</td>
      <td>20</td>
      <td>42.731552</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Selecting the drugs we are going to analyze: (Capomulin, Infubinol, Ketapril, and Placebo)
clinic_mouse_analysis = clinic_mouse_merge.loc[(clinic_mouse_merge["Drug"]=="Capomulin") | (
    clinic_mouse_merge["Drug"]=="Infubinol") | (
    clinic_mouse_merge["Drug"]=="Ketapril") | (
    clinic_mouse_merge["Drug"]=="Placebo")]
#clinic_mouse_analysis
```

### Tumor Response to Treatment


```python
# Grouping the merged table into Drug and Timepoint
grouped_drug_time = clinic_mouse_merge.groupby(['Drug', 'Timepoint'])
grouped_drug_time.count().head()
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
      <th>Mouse ID</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
    <tr>
      <th>Drug</th>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Capomulin</th>
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
      <td>24</td>
      <td>24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculating the average size of the tumor per drug and timepoint.
tumor_vol_avrg = grouped_drug_time["Tumor Volume (mm3)"].mean()
#tumor_vol_avrg

# Reset the index and covert it into a dataframe
tumor_vol_df = tumor_vol_avrg.reset_index(level=['Drug','Timepoint'])
#tumor_vol_df.head()

# Make a pivot table
tumor_pivot = tumor_vol_df.pivot(index='Timepoint', columns='Drug', values='Tumor Volume (mm3)')
tumor_pivot
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>44.266086</td>
      <td>46.503051</td>
      <td>47.062001</td>
      <td>47.389175</td>
      <td>46.796098</td>
      <td>47.125589</td>
      <td>47.248967</td>
      <td>43.944859</td>
      <td>47.527452</td>
      <td>46.851818</td>
    </tr>
    <tr>
      <th>10</th>
      <td>43.084291</td>
      <td>48.285125</td>
      <td>49.403909</td>
      <td>49.582269</td>
      <td>48.694210</td>
      <td>49.423329</td>
      <td>49.101541</td>
      <td>42.531957</td>
      <td>49.463844</td>
      <td>48.689881</td>
    </tr>
    <tr>
      <th>15</th>
      <td>42.064317</td>
      <td>50.094055</td>
      <td>51.296397</td>
      <td>52.399974</td>
      <td>50.933018</td>
      <td>51.359742</td>
      <td>51.067318</td>
      <td>41.495061</td>
      <td>51.529409</td>
      <td>50.779059</td>
    </tr>
    <tr>
      <th>20</th>
      <td>40.716325</td>
      <td>52.157049</td>
      <td>53.197691</td>
      <td>54.920935</td>
      <td>53.644087</td>
      <td>54.364417</td>
      <td>53.346737</td>
      <td>40.238325</td>
      <td>54.067395</td>
      <td>53.170334</td>
    </tr>
    <tr>
      <th>25</th>
      <td>39.939528</td>
      <td>54.287674</td>
      <td>55.715252</td>
      <td>57.678982</td>
      <td>56.731968</td>
      <td>57.482574</td>
      <td>55.504138</td>
      <td>38.974300</td>
      <td>56.166123</td>
      <td>55.432935</td>
    </tr>
    <tr>
      <th>30</th>
      <td>38.769339</td>
      <td>56.769517</td>
      <td>58.299397</td>
      <td>60.994507</td>
      <td>59.559509</td>
      <td>59.809063</td>
      <td>58.196374</td>
      <td>38.703137</td>
      <td>59.826738</td>
      <td>57.713531</td>
    </tr>
    <tr>
      <th>35</th>
      <td>37.816839</td>
      <td>58.827548</td>
      <td>60.742461</td>
      <td>63.371686</td>
      <td>62.685087</td>
      <td>62.420615</td>
      <td>60.350199</td>
      <td>37.451996</td>
      <td>62.440699</td>
      <td>60.089372</td>
    </tr>
    <tr>
      <th>40</th>
      <td>36.958001</td>
      <td>61.467895</td>
      <td>63.162824</td>
      <td>66.068580</td>
      <td>65.600754</td>
      <td>65.052675</td>
      <td>63.045537</td>
      <td>36.574081</td>
      <td>65.356386</td>
      <td>62.916692</td>
    </tr>
    <tr>
      <th>45</th>
      <td>36.236114</td>
      <td>64.132421</td>
      <td>65.755562</td>
      <td>70.662958</td>
      <td>69.265506</td>
      <td>68.084082</td>
      <td>66.258529</td>
      <td>34.955595</td>
      <td>68.438310</td>
      <td>65.960888</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Selecting the drugs we are going to analyze: (Capomulin, Infubinol, Ketapril, and Placebo)
tumor_pivot_analysis = tumor_pivot.loc[:,["Capomulin", "Infubinol", "Ketapril", "Placebo"]]
#tumor_pivot_analysis


marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["orange", "purple", "blue", "red"]
x_axis = tumor_pivot_analysis.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in tumor_pivot_analysis.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

# Creating the scatterplot
c = 0
# for a in tumor_pivot_analysis.index:
for a in tumor_pivot_analysis.columns:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = tumor_pivot_analysis.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=a, edgecolors="black", alpha=0.75, c=colors[c], s=50)
    
    # Set error bars
    y_axis = tumor_pivot_analysis.values[:,c]
    standard_errors = sem(tumor_pivot_analysis.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])  
    c +=1
    
# Set the Titles
plt.title("Tumor Response to Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Tumor Volume (mm3)", fontsize=13)
c = 0
plt.savefig("Images/Tumor_Response_to_Treatment.png")
```


![png](output_9_0.png)


### Metastatic Response to Treatment


```python
# From the group table we will get the mean of "Metastatic Sites"
tumor_met_avrg = grouped_drug_time["Metastatic Sites"].mean()
#tumor_met_avrg
tumor_met_df = tumor_met_avrg.reset_index(level=['Drug','Timepoint'])
tumor_met_df.head()
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
      <th>Drug</th>
      <th>Timepoint</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Capomulin</td>
      <td>5</td>
      <td>0.160000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Capomulin</td>
      <td>10</td>
      <td>0.320000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Capomulin</td>
      <td>15</td>
      <td>0.375000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Capomulin</td>
      <td>20</td>
      <td>0.652174</td>
    </tr>
  </tbody>
</table>
</div>




```python
metastatic_pivot = tumor_met_df.pivot(index='Timepoint', columns='Drug', values='Metastatic Sites')
metastatic_pivot
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.160000</td>
      <td>0.380952</td>
      <td>0.280000</td>
      <td>0.304348</td>
      <td>0.260870</td>
      <td>0.375000</td>
      <td>0.320000</td>
      <td>0.120000</td>
      <td>0.240000</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.320000</td>
      <td>0.600000</td>
      <td>0.666667</td>
      <td>0.590909</td>
      <td>0.523810</td>
      <td>0.833333</td>
      <td>0.565217</td>
      <td>0.250000</td>
      <td>0.478261</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.375000</td>
      <td>0.789474</td>
      <td>0.904762</td>
      <td>0.842105</td>
      <td>0.857143</td>
      <td>1.250000</td>
      <td>0.764706</td>
      <td>0.333333</td>
      <td>0.782609</td>
      <td>0.809524</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.652174</td>
      <td>1.111111</td>
      <td>1.050000</td>
      <td>1.210526</td>
      <td>1.150000</td>
      <td>1.526316</td>
      <td>1.000000</td>
      <td>0.347826</td>
      <td>0.952381</td>
      <td>1.294118</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.818182</td>
      <td>1.500000</td>
      <td>1.277778</td>
      <td>1.631579</td>
      <td>1.500000</td>
      <td>1.941176</td>
      <td>1.357143</td>
      <td>0.652174</td>
      <td>1.157895</td>
      <td>1.687500</td>
    </tr>
    <tr>
      <th>30</th>
      <td>1.090909</td>
      <td>1.937500</td>
      <td>1.588235</td>
      <td>2.055556</td>
      <td>2.066667</td>
      <td>2.266667</td>
      <td>1.615385</td>
      <td>0.782609</td>
      <td>1.388889</td>
      <td>1.933333</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1.181818</td>
      <td>2.071429</td>
      <td>1.666667</td>
      <td>2.294118</td>
      <td>2.266667</td>
      <td>2.642857</td>
      <td>2.300000</td>
      <td>0.952381</td>
      <td>1.562500</td>
      <td>2.285714</td>
    </tr>
    <tr>
      <th>40</th>
      <td>1.380952</td>
      <td>2.357143</td>
      <td>2.100000</td>
      <td>2.733333</td>
      <td>2.466667</td>
      <td>3.166667</td>
      <td>2.777778</td>
      <td>1.100000</td>
      <td>1.583333</td>
      <td>2.785714</td>
    </tr>
    <tr>
      <th>45</th>
      <td>1.476190</td>
      <td>2.692308</td>
      <td>2.111111</td>
      <td>3.363636</td>
      <td>2.538462</td>
      <td>3.272727</td>
      <td>2.571429</td>
      <td>1.250000</td>
      <td>1.727273</td>
      <td>3.071429</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Selecting the drugs we are going to analyze: (Capomulin, Infubinol, Ketapril, and Placebo)
metastatic_pivot_analysis = metastatic_pivot.loc[:,["Capomulin", "Infubinol", "Ketapril", "Placebo"]]
#tumor_pivot_analysis


marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["orange", "purple", "blue", "red"]
x_axis = metastatic_pivot_analysis.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in metastatic_pivot_analysis.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

# Creating the scatterplot
c = 0
# for a in tumor_pivot_analysis.index:
for a in metastatic_pivot_analysis.columns:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = metastatic_pivot_analysis.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=a, edgecolors="black", alpha=0.75, c=colors[c], s=50)
    
    # Set error bars
    y_axis = metastatic_pivot_analysis.values[:,c]
    standard_errors = sem(metastatic_pivot_analysis.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])  
    c +=1
    
# Set the Titles
plt.title("Metastatic Spread During Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Met. Sites", fontsize=13)
c = 0
plt.savefig("Images/Metastatic_Spread_During_Treatment.png")
```


![png](output_13_0.png)


### Survival Rates


```python
# From the group table we will get the mean of "Metastatic Sites"
surv_rate = grouped_drug_time["Mouse ID"].count()

# Renaming the series
surv_rate = surv_rate.rename("Mouse Count")

surv_rate_df = surv_rate.reset_index(level=['Drug','Timepoint'])
surv_rate_df.head()

#Creating the pivot table
survival_pivot = surv_rate_df.pivot(index='Timepoint', columns='Drug', values='Mouse Count')
survival_pivot
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
      <th>Drug</th>
      <th>Capomulin</th>
      <th>Ceftamin</th>
      <th>Infubinol</th>
      <th>Ketapril</th>
      <th>Naftisol</th>
      <th>Placebo</th>
      <th>Propriva</th>
      <th>Ramicane</th>
      <th>Stelasyn</th>
      <th>Zoniferol</th>
    </tr>
    <tr>
      <th>Timepoint</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>26</td>
      <td>25</td>
      <td>26</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>21</td>
      <td>25</td>
      <td>23</td>
      <td>23</td>
      <td>24</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>24</td>
    </tr>
    <tr>
      <th>10</th>
      <td>25</td>
      <td>20</td>
      <td>21</td>
      <td>22</td>
      <td>21</td>
      <td>24</td>
      <td>23</td>
      <td>24</td>
      <td>23</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15</th>
      <td>24</td>
      <td>19</td>
      <td>21</td>
      <td>19</td>
      <td>21</td>
      <td>20</td>
      <td>17</td>
      <td>24</td>
      <td>23</td>
      <td>21</td>
    </tr>
    <tr>
      <th>20</th>
      <td>23</td>
      <td>18</td>
      <td>20</td>
      <td>19</td>
      <td>20</td>
      <td>19</td>
      <td>17</td>
      <td>23</td>
      <td>21</td>
      <td>17</td>
    </tr>
    <tr>
      <th>25</th>
      <td>22</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>18</td>
      <td>17</td>
      <td>14</td>
      <td>23</td>
      <td>19</td>
      <td>16</td>
    </tr>
    <tr>
      <th>30</th>
      <td>22</td>
      <td>16</td>
      <td>17</td>
      <td>18</td>
      <td>15</td>
      <td>15</td>
      <td>13</td>
      <td>23</td>
      <td>18</td>
      <td>15</td>
    </tr>
    <tr>
      <th>35</th>
      <td>22</td>
      <td>14</td>
      <td>12</td>
      <td>17</td>
      <td>15</td>
      <td>14</td>
      <td>10</td>
      <td>21</td>
      <td>16</td>
      <td>14</td>
    </tr>
    <tr>
      <th>40</th>
      <td>21</td>
      <td>14</td>
      <td>10</td>
      <td>15</td>
      <td>15</td>
      <td>12</td>
      <td>9</td>
      <td>20</td>
      <td>12</td>
      <td>14</td>
    </tr>
    <tr>
      <th>45</th>
      <td>21</td>
      <td>13</td>
      <td>9</td>
      <td>11</td>
      <td>13</td>
      <td>11</td>
      <td>7</td>
      <td>20</td>
      <td>11</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Selecting the drugs we are going to analyze: (Capomulin, Infubinol, Ketapril, and Placebo)
survival_pivot_analysis = survival_pivot.loc[:,["Capomulin", "Infubinol", "Ketapril", "Placebo"]]
#tumor_pivot_analysis


marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["orange", "purple", "blue", "red"]
x_axis = survival_pivot_analysis.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in survival_pivot_analysis.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

# Creating the scatterplot
c = 0
# for a in survival_pivot_analysis.index:
for a in survival_pivot_analysis.columns:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = survival_pivot_analysis.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=a, edgecolors="black", alpha=0.75, c=colors[c], s=50)
    
    # Set error bars
    y_axis = survival_pivot_analysis.values[:,c]
    standard_errors = sem(survival_pivot_analysis.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])  
    c +=1
    
# Set the Titles
plt.title("Survival During Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Survival Rate (%)", fontsize=13)
c = 0
plt.savefig("Images/Survival_During_Treatment.png")
```


![png](output_16_0.png)


### Summary Bar Graph


```python
# Calculating the % Tumor Change for each drug across the full 45 days
tumor_chg = []
colum_drug = []
c = 0
for i in tumor_pivot_analysis.columns:
    first_val = tumor_pivot_analysis.values[0,c]
    last_val = tumor_pivot_analysis.values[9,c]
    tum_chg = round(((last_val - first_val)/first_val)*100,2)
#     print(first_val)
#     print(last_val)
    print(tum_chg)
    tumor_chg.append(tum_chg)
    colum_drug.append(i)
    c+=1
```

    -19.48
    46.12
    57.03
    51.3
    


```python
plt.figure(figsize=(10,10))
plt.hlines(0, 0, 3, alpha=0.25)
plt.grid(True)

c=0
for i in tumor_pivot_analysis.columns:
    if tumor_chg[c]>0:
        plt.bar(colum_drug[c], tumor_chg[c], color='b', width=1, edgecolor="black", linewidth=1, alpha=0.65, align="center")
    if tumor_chg[c]<0:
        plt.bar(colum_drug[c], tumor_chg[c], color='r', width=1, edgecolor="black", linewidth=1,  alpha=0.65, align="center")
    
    c+=1

# Set the Titles
plt.title("Tumor Change Over 45 Day Treatment", fontsize=20)
plt.xlabel("Drug", fontsize=13)
plt.ylabel("% Volume Change", fontsize=13)

for a,b in zip(colum_drug, tumor_chg):
    plt.text(a, b, str(b), verticalalignment="bottom", horizontalalignment='center', fontsize=14, color="black")

plt.savefig("Images/Tumor_Change_45Day_Treatment.png")
```


![png](output_19_0.png)

