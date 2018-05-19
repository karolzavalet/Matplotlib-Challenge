
# Pymaceuticals Inc.

### Analysis
- Observed Trend 1:
- Observed Trend 2:
- Observed Trend 3:


```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from scipy.stats import sem
from scipy.stats import linregress
```


```python
clinical_data = "raw_data/clinicaltrial_data.csv"
clinical_data_df = pd.read_csv(clinical_data)
clinical_data_df.head()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>f932</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>g107</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a457</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>c819</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
clinical_data_df.describe()
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
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1893.000000</td>
      <td>1893.000000</td>
      <td>1893.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>19.572108</td>
      <td>50.455258</td>
      <td>1.021659</td>
    </tr>
    <tr>
      <th>std</th>
      <td>14.079460</td>
      <td>8.888824</td>
      <td>1.137974</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>22.050126</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>5.000000</td>
      <td>45.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>20.000000</td>
      <td>48.957919</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>30.000000</td>
      <td>56.292200</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>45.000000</td>
      <td>78.567014</td>
      <td>4.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Mouse "g989"
clinical_data_df[clinical_data_df["Mouse ID"]=="g989"].count()
```




    Mouse ID              13
    Timepoint             13
    Tumor Volume (mm3)    13
    Metastatic Sites      13
    dtype: int64




```python
clinical_df = clinical_data_df.set_index("Mouse ID")
```


```python
mouse_data = "raw_data/mouse_drug_data.csv"
mouse_data_df = pd.read_csv(mouse_data)
mouse_data_df.head()
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
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>f234</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>1</th>
      <td>x402</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a492</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>3</th>
      <td>w540</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>4</th>
      <td>v764</td>
      <td>Stelasyn</td>
    </tr>
  </tbody>
</table>
</div>




```python
mouse_data_df.describe()
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
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>250</td>
      <td>250</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>249</td>
      <td>10</td>
    </tr>
    <tr>
      <th>top</th>
      <td>g989</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>




```python
mouse_data_df[mouse_data_df["Mouse ID"]=="g989"]
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
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>g989</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>173</th>
      <td>g989</td>
      <td>Propriva</td>
    </tr>
  </tbody>
</table>
</div>




```python
mouse_df = mouse_data_df.set_index("Mouse ID")
```


```python
# Concatenating both tables mouse_data_df and clinical_data_df
# !It didnt work!!!!!!!!!!!!!!!!!
# clinic_mouse_concat = pd.concat([clinical_df, mouse_df], axis=1)
# clinic_mouse_concat
```


```python
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
# Mouse "g989" has been dupplicated because he has been taken 2 types of drugs
clinic_mouse_merge[clinic_mouse_merge["Mouse ID"]=="g989"].count()
```




    Mouse ID              26
    Timepoint             26
    Tumor Volume (mm3)    26
    Metastatic Sites      26
    Drug                  26
    dtype: int64



### Tumor Response to Treatment


```python
# Grouping the merged table into Drug and Timepoint
grouped_drug_time = clinic_mouse_merge.groupby(['Drug', 'Timepoint'])
grouped_drug_time.count()
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
      <th rowspan="10" valign="top">Capomulin</th>
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
    <tr>
      <th>25</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>30</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>35</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>40</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>45</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">Ceftamin</th>
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>10</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>15</th>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
    <tr>
      <th>20</th>
      <td>18</td>
      <td>18</td>
      <td>18</td>
    </tr>
    <tr>
      <th>25</th>
      <td>18</td>
      <td>18</td>
      <td>18</td>
    </tr>
    <tr>
      <th>30</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>35</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>40</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>45</th>
      <td>13</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">Infubinol</th>
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
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>15</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>25</th>
      <td>18</td>
      <td>18</td>
      <td>18</td>
    </tr>
    <tr>
      <th>30</th>
      <td>17</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>35</th>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>40</th>
      <td>10</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>45</th>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">Ramicane</th>
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
      <td>24</td>
      <td>24</td>
      <td>24</td>
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
    <tr>
      <th>25</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>30</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>35</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>40</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>45</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">Stelasyn</th>
      <th>0</th>
      <td>26</td>
      <td>26</td>
      <td>26</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>10</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>25</th>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
    <tr>
      <th>30</th>
      <td>18</td>
      <td>18</td>
      <td>18</td>
    </tr>
    <tr>
      <th>35</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>40</th>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>45</th>
      <td>11</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">Zoniferol</th>
      <th>0</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24</td>
      <td>24</td>
      <td>24</td>
    </tr>
    <tr>
      <th>10</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>20</th>
      <td>17</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>25</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>30</th>
      <td>15</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>35</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>40</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>45</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 3 columns</p>
</div>




```python
tumor_vol_avrg = grouped_drug_time["Tumor Volume (mm3)"].mean()
tumor_vol_avrg
```




    Drug       Timepoint
    Capomulin  0            45.000000
               5            44.266086
               10           43.084291
               15           42.064317
               20           40.716325
               25           39.939528
               30           38.769339
               35           37.816839
               40           36.958001
               45           36.236114
    Ceftamin   0            45.000000
               5            46.503051
               10           48.285125
               15           50.094055
               20           52.157049
               25           54.287674
               30           56.769517
               35           58.827548
               40           61.467895
               45           64.132421
    Infubinol  0            45.000000
               5            47.062001
               10           49.403909
               15           51.296397
               20           53.197691
               25           55.715252
               30           58.299397
               35           60.742461
               40           63.162824
               45           65.755562
                              ...    
    Ramicane   0            45.000000
               5            43.944859
               10           42.531957
               15           41.495061
               20           40.238325
               25           38.974300
               30           38.703137
               35           37.451996
               40           36.574081
               45           34.955595
    Stelasyn   0            45.000000
               5            47.527452
               10           49.463844
               15           51.529409
               20           54.067395
               25           56.166123
               30           59.826738
               35           62.440699
               40           65.356386
               45           68.438310
    Zoniferol  0            45.000000
               5            46.851818
               10           48.689881
               15           50.779059
               20           53.170334
               25           55.432935
               30           57.713531
               35           60.089372
               40           62.916692
               45           65.960888
    Name: Tumor Volume (mm3), Length: 100, dtype: float64




```python
tumor_vol_df = tumor_vol_avrg.reset_index(level=['Drug','Timepoint'])
tumor_vol_df.head()
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
      <th>Tumor Volume (mm3)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>0</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Capomulin</td>
      <td>5</td>
      <td>44.266086</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Capomulin</td>
      <td>10</td>
      <td>43.084291</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Capomulin</td>
      <td>15</td>
      <td>42.064317</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Capomulin</td>
      <td>20</td>
      <td>40.716325</td>
    </tr>
  </tbody>
</table>
</div>




```python
tumor_vol_df["Drug"].value_counts()
```




    Naftisol     10
    Placebo      10
    Ketapril     10
    Stelasyn     10
    Ceftamin     10
    Ramicane     10
    Propriva     10
    Capomulin    10
    Infubinol    10
    Zoniferol    10
    Name: Drug, dtype: int64




```python
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
tumor_pivot.values[:,0]
```




    array([45.        , 44.26608642, 43.08429058, 42.06431735, 40.71632532,
           39.93952783, 38.76933929, 37.81683888, 36.95800081, 36.2361138 ])




```python
tumor_pivot.columns
```




    Index(['Capomulin', 'Ceftamin', 'Infubinol', 'Ketapril', 'Naftisol', 'Placebo',
           'Propriva', 'Ramicane', 'Stelasyn', 'Zoniferol'],
          dtype='object', name='Drug')




```python
tumor_pivot.index
```




    Int64Index([0, 5, 10, 15, 20, 25, 30, 35, 40, 45], dtype='int64', name='Timepoint')




```python
c = 0
volumen = []
```


```python
c = 0
marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"1", u"2", u"3", u"4"]
x_axis = tumor_pivot.index
for a in tumor_pivot.index:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = tumor_pivot.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=labels, edgecolors="black", alpha=0.75)
    c +=1
print(c)
```

    10
    


![png](output_24_1.png)



```python
c=0
for a in tumor_pivot.index:
    #fig, ax = plt.subplots()
    # Set regression line
    y_axis = tumor_pivot.values[:,c]
    slope, intercept, *_ = linregress(x_axis, y_axis)
    fit = slope * x_axis + intercept
    plt.plot(x_axis, fit, '--')
    c +=1
print(c)
```

    10
    


![png](output_25_1.png)



```python
c=0
for a in tumor_pivot.index:
    #fig, ax = plt.subplots()
    # Set error bars
    y_axis = tumor_pivot.values[:,c]
    standard_errors = sem(tumor_pivot.values[:,0])
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3)
    #tumor_vol_df.plot(kind='scatter', x='Timepoint', y='Tumor Volume (mm3)', yerr=standard_errors)
#     slope, intercept, *_ = linregress(x_axis, y_axis)
#     fit = slope * x_axis + intercept
#     plt.plot(x_axis, fit, '--')
    c +=1
print(c)
```

    10
    


![png](output_26_1.png)



```python

marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["pink", "orange", "lightblue", "lightgreen", "red", "purple", "darkgreen", "blue", "gold", "brown"]
x_axis = tumor_pivot.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in tumor_pivot.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

# Creating the scatterplot
c = 0
for a in tumor_pivot.index:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = tumor_pivot.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=labels, edgecolors="black", alpha=0.75, c=colors[c], s=50)

#     # Set regression line
#     y_axis = tumor_pivot.values[:,c]
#     slope, intercept, *_ = linregress(x_axis, y_axis)
#     fit = slope * x_axis + intercept
#     plt.plot(x_axis, fit, '--', c=colors[c])
    
    # Set error bars
    y_axis = tumor_pivot.values[:,c]
    standard_errors = sem(tumor_pivot.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])  
    c +=1
    
# Set the Titles
plt.title("Tumor Response to Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Tumor Volume (mm3)", fontsize=13)
c = 0

leg_vol
print(c)
```

    0
    


![png](output_27_1.png)



```python
c = 0
for i in tumor_pivot.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=15, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)
```




    <matplotlib.legend.Legend at 0x221767337f0>




![png](output_28_1.png)


### Metastatic Response to Treatment


```python
# From the group table we will get the mean of "Metastatic Sites"
tumor_met_avrg = grouped_drug_time["Metastatic Sites"].mean()
tumor_met_avrg
```




    Drug       Timepoint
    Capomulin  0            0.000000
               5            0.160000
               10           0.320000
               15           0.375000
               20           0.652174
               25           0.818182
               30           1.090909
               35           1.181818
               40           1.380952
               45           1.476190
    Ceftamin   0            0.000000
               5            0.380952
               10           0.600000
               15           0.789474
               20           1.111111
               25           1.500000
               30           1.937500
               35           2.071429
               40           2.357143
               45           2.692308
    Infubinol  0            0.000000
               5            0.280000
               10           0.666667
               15           0.904762
               20           1.050000
               25           1.277778
               30           1.588235
               35           1.666667
               40           2.100000
               45           2.111111
                              ...   
    Ramicane   0            0.000000
               5            0.120000
               10           0.250000
               15           0.333333
               20           0.347826
               25           0.652174
               30           0.782609
               35           0.952381
               40           1.100000
               45           1.250000
    Stelasyn   0            0.000000
               5            0.240000
               10           0.478261
               15           0.782609
               20           0.952381
               25           1.157895
               30           1.388889
               35           1.562500
               40           1.583333
               45           1.727273
    Zoniferol  0            0.000000
               5            0.166667
               10           0.500000
               15           0.809524
               20           1.294118
               25           1.687500
               30           1.933333
               35           2.285714
               40           2.785714
               45           3.071429
    Name: Metastatic Sites, Length: 100, dtype: float64




```python
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

marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["pink", "orange", "lightblue", "lightgreen", "red", "purple", "darkgreen", "blue", "gold", "brown"]
x_axis = metastatic_pivot.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in metastatic_pivot.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

c = 0
for a in metastatic_pivot.index:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = metastatic_pivot.values[:,c]
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=labels, edgecolors="black", alpha=0.75, c=colors[c], s=50)

#     # Set regression line
#     y_axis = tumor_pivot.values[:,c]
#     slope, intercept, *_ = linregress(x_axis, y_axis)
#     fit = slope * x_axis + intercept
#     plt.plot(x_axis, fit, '--', c=colors[c])
    
    # Set error bars
    y_axis = metastatic_pivot.values[:,c]
    standard_errors = sem(metastatic_pivot.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])  
    c +=1
    
# Set the Titles
plt.title("Metastatic Spread During Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Met. Sites", fontsize=13)
print(c)
```

    10
    


![png](output_33_1.png)


### Survival Rates


```python
# From the group table we will get the mean of "Metastatic Sites"
surv_rate = grouped_drug_time["Mouse ID"].count()
surv_rate
```




    Drug       Timepoint
    Capomulin  0            25
               5            25
               10           25
               15           24
               20           23
               25           22
               30           22
               35           22
               40           21
               45           21
    Ceftamin   0            25
               5            21
               10           20
               15           19
               20           18
               25           18
               30           16
               35           14
               40           14
               45           13
    Infubinol  0            25
               5            25
               10           21
               15           21
               20           20
               25           18
               30           17
               35           12
               40           10
               45            9
                            ..
    Ramicane   0            25
               5            25
               10           24
               15           24
               20           23
               25           23
               30           23
               35           21
               40           20
               45           20
    Stelasyn   0            26
               5            25
               10           23
               15           23
               20           21
               25           19
               30           18
               35           16
               40           12
               45           11
    Zoniferol  0            25
               5            24
               10           22
               15           21
               20           17
               25           16
               30           15
               35           14
               40           14
               45           14
    Name: Mouse ID, Length: 100, dtype: int64




```python
# Renaming the series
surv_rate = surv_rate.rename("Mouse Count")
surv_rate
```




    Drug       Timepoint
    Capomulin  0            25
               5            25
               10           25
               15           24
               20           23
               25           22
               30           22
               35           22
               40           21
               45           21
    Ceftamin   0            25
               5            21
               10           20
               15           19
               20           18
               25           18
               30           16
               35           14
               40           14
               45           13
    Infubinol  0            25
               5            25
               10           21
               15           21
               20           20
               25           18
               30           17
               35           12
               40           10
               45            9
                            ..
    Ramicane   0            25
               5            25
               10           24
               15           24
               20           23
               25           23
               30           23
               35           21
               40           20
               45           20
    Stelasyn   0            26
               5            25
               10           23
               15           23
               20           21
               25           19
               30           18
               35           16
               40           12
               45           11
    Zoniferol  0            25
               5            24
               10           22
               15           21
               20           17
               25           16
               30           15
               35           14
               40           14
               45           14
    Name: Mouse Count, Length: 100, dtype: int64




```python
surv_rate_df = surv_rate.reset_index(level=['Drug','Timepoint'])
surv_rate_df.head()
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
      <th>Mouse Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Capomulin</td>
      <td>0</td>
      <td>25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Capomulin</td>
      <td>5</td>
      <td>25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Capomulin</td>
      <td>10</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Capomulin</td>
      <td>15</td>
      <td>24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Capomulin</td>
      <td>20</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
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
marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"h", u"*", u"3", u"4"]
colors = ["pink", "orange", "lightblue", "lightgreen", "red", "purple", "darkgreen", "blue", "gold", "brown"]
x_axis = survival_pivot.index

plt.figure(figsize=(10,10))
plt.grid(True)

# Creating the legend for the graph
c = 0
for i in survival_pivot.columns:
    plt.scatter([],[], c=colors[c], alpha=0.5, s=50, linewidths=1, label=i, linestyle='solid', marker=marker[c])
    c +=1
plt.legend(scatterpoints=2, labelspacing=1, title='Drug', ncol=2)

c = 0
for a in metastatic_pivot.index:
    # Plotting the tumor volume for every drug in a determined timepoint
    y_axis = (survival_pivot.values[:,c]/survival_pivot.values[0,c])*100
    plt.scatter(x_axis, y_axis, marker=marker[c], linewidth=0.5, label=labels, edgecolors="black", alpha=0.75, c=colors[c], s=50)
    
    # Set error bars
    y_axis = (survival_pivot.values[:,c]/survival_pivot.values[0,c])*100
    standard_errors = sem(survival_pivot.values[:,c])
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='None', capsize=3, c=colors[c])   
    #plt.errorbar(x_axis, y_axis, standard_errors, linestyle='--', capsize=3, c=colors[c])
    plt.errorbar(x_axis, y_axis, linestyle='--', c=colors[c]) 
    c +=1
    
# Set the Titles
plt.title("Survival During Treatment", fontsize=20)
plt.xlabel("Time (Days)", fontsize=13)
plt.ylabel("Survival Rate (%)", fontsize=13)
print(c)
```

    10
    


![png](output_39_1.png)


### Summary Bar Graph


```python
# Calculating the % Tumor Change for each drug across the full 45 days
#Ultimo menos el primero entre el primero (volumen del tumor)

```


```python
tumor_chg = []
colum_drug = []
c = 0
for i in tumor_pivot.columns:
    first_val = tumor_pivot.values[0,c]
    last_val = tumor_pivot.values[9,c]
    tum_chg = round(((last_val - first_val)/first_val)*100,2)
#     print(first_val)
#     print(last_val)
    print(tum_chg)
    tumor_chg.append(tum_chg)
    colum_drug.append(i)
    #tumor_chg.append([i,tum_chg])
    #tumor_chg.append({i:tum_chg})
    c+=1
```

    -19.48
    42.52
    46.12
    57.03
    53.92
    51.3
    47.24
    -22.32
    52.09
    46.58
    


```python
tumor_chg
```




    [-19.48, 42.52, 46.12, 57.03, 53.92, 51.3, 47.24, -22.32, 52.09, 46.58]




```python
colum_drug
```




    ['Capomulin',
     'Ceftamin',
     'Infubinol',
     'Ketapril',
     'Naftisol',
     'Placebo',
     'Propriva',
     'Ramicane',
     'Stelasyn',
     'Zoniferol']




```python
plt.figure(figsize=(10,10))
c=0
for i in tumor_pivot.columns:
    if tumor_chg[c]>0:
        plt.bar(colum_drug[c], tumor_chg[c], color='b', alpha=0.5, align="center")
    if tumor_chg[c]<0:
        plt.bar(colum_drug[c], tumor_chg[c], color='r', alpha=0.5, align="center")
    c+=1
```


![png](output_45_0.png)



```python
c=0
for i in tumor_pivot.columns:
    if tumor_chg[c]<0:
        print("si!")
    c+=1
```

    si!
    si!
    


```python
standard_errors
```




    0.9709032345000089




```python
y_axis
```




    array([45.        , 46.85181827, 48.68988143, 50.77905905, 53.17033369,
           55.43293487, 57.71353092, 60.08937222, 62.91669188, 65.96088789])




```python
x_axis = tumor_vol_df["Timepoint"]
y_axis = tumor_vol_df["Tumor Volume (mm3)"]
labels = tumor_vol_df["Drug"]
marker = [u"o", u"^", u"s", u"x", u"8", u"p", u"1", u"2", u"3", u"4"]
```


```python
# Set regression line
slope, intercept, *_ = linregress(x_axis, y_axis)
fit = slope * x_axis + intercept


fig, ax = plt.subplots()
ax.scatter(x_axis, y_axis, marker="s", linewidth=0.5, label=labels, edgecolors="black", alpha=0.75)
ax.plot(x_axis, fit, 'b--')

#Title
fig.suptitle("Tumor Response to Treatment", fontsize=16, fontweight="bold")
```




    Text(0.5,0.98,'Tumor Response to Treatment')




![png](output_50_1.png)



```python
for x, y, m in x_axis, y_axis, marker:
    plt.scatter(x, y, marker=m, edgecolors="black", alpha=0.75)
plt.show()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-140-94c2c2c48899> in <module>()
    ----> 1 for x, y, m in x_axis, y_axis, marker:
          2     plt.scatter(x, y, marker=m, edgecolors="black", alpha=0.75)
          3 plt.show()
    

    ValueError: too many values to unpack (expected 3)



```python
for x, y in x_axis, y_axis:
    slope, intercept, *_ = linregress(x, y)
    fit = slope * x + intercept
plt.show()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-141-976009d14558> in <module>()
    ----> 1 for x, y in x_axis, y_axis:
          2     slope, intercept, *_ = linregress(x, y)
          3     fit = slope * x + intercept
          4 plt.show()
    

    ValueError: too many values to unpack (expected 2)



```python
a = tumor_vol_df[tumor_vol_df["Drug"]=="Naftisol"]["Timepoint"]
b = tumor_vol_df[tumor_vol_df["Drug"]=="Naftisol"]["Tumor Volume (mm3)"]
```


```python
fig, ax = plt.subplots()
slope, intercept, *_ = linregress(a, b)
ax.scatter(a, b, marker="s", linewidth=0.5, label=labels, edgecolors="black", alpha=0.75)
fit = slope * a + intercept

ax.plot(a, fit, 'b--')
```




    [<matplotlib.lines.Line2D at 0x22173939470>]




![png](output_54_1.png)



```python
means = np.mean(b)
means
```




    55.891023645458006




```python
means = [np.mean(s) for s in b]
standard_errors = [sem(s) for s in b]
standard_errors
```

    C:\Users\karol\Anaconda3\envs\PythonData\lib\site-packages\numpy\core\_methods.py:135: RuntimeWarning: Degrees of freedom <= 0 for slice
      keepdims=keepdims)
    C:\Users\karol\Anaconda3\envs\PythonData\lib\site-packages\numpy\core\_methods.py:127: RuntimeWarning: invalid value encountered in double_scalars
      ret = ret.dtype.type(ret / rcount)
    




    [nan, nan, nan, nan, nan, nan, nan, nan, nan, nan]




```python
df.plot(kind='scatter', x="Sample Number", y="Population of People Voting Republican", yerr="sem")
```


```python
tumor_pivot.values[:,0]
```




    array([45.        , 44.26608642, 43.08429058, 42.06431735, 40.71632532,
           39.93952783, 38.76933929, 37.81683888, 36.95800081, 36.2361138 ])




```python
type(means)
```




    list




```python
test = Arrays.asList(tumor_pivot.values[:,0])
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-213-2817fcb8c5f4> in <module>()
    ----> 1 test = Arrays.asList(tumor_pivot.values[:,0])
    

    NameError: name 'Arrays' is not defined



```python
val = tumor_pivot.values[:,0].tolist()
val
```




    [45.0,
     44.26608641544399,
     43.08429058188399,
     42.0643173468125,
     40.71632532212173,
     39.939527826868186,
     38.76933928855454,
     37.81683888251364,
     36.958000810895236,
     36.23611379944763]




```python
standard_errors = sem(tumor_pivot.values[:,0])
standard_errors
```




    0.9709032345000089




```python
means = [np.mean(s) for s in val]
means
```




    [45.0,
     44.26608641544399,
     43.08429058188399,
     42.0643173468125,
     40.71632532212173,
     39.939527826868186,
     38.76933928855454,
     37.81683888251364,
     36.958000810895236,
     36.23611379944763]


