```python
# title: tfcb_2020_capstone_cwc
# author: Cara Chao
# date: 2020-12-13

# About the data: 
# Glaucoma diagnosis takes into acount multiple measurements including age, ocular pressur, PSD, GHT, cornea thickness, and in this case the RNFL4.mean (see README for a discription of every variable in this dataset)
# Since glaucoma is known to manifest at higher ages, I was interested in seeing whether PCA analysis of age and two other measurements (ocular pressure and cornea thickness) would indicate any level of relatedness.
# This data tackles my first question from the README: Is age related to ocular pressure or cornea thickness? What does that mean for age and glaucoma diagnosis?

# I first generated scatterplots between age and ocular pressure or cornea thickness.
# These scatterplots don't tell us if there is any apparent relationship or trend between these variables. However, there may be a trend when it comes to ocular pressure and age.
# PCA analyis, along with their respective scaled plots, show that neither ocular pressure or cornea thickness are related with age.
# Thus within this dataset, there is no correlation between age and both ocular pressure and cornea thickness, based on PCA.
# This suggests that age has no role in determining the ocular pressure or cornea thickness within these data. 
```


```python
import re
import Bio.SeqIO
import matplotlib.pyplot as plt # for plotting
import pandas as pd # pandas
import seaborn as sns
import numpy as np
from sklearn.decomposition import PCA

glaucdata_whole = 'dataset/ds_whole.csv'
df_whole = pd.read_csv(glaucdata_whole) #dataframe of the whole glaucoma dataset, including training and testing data. 
df_whole
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
      <th>RL</th>
      <th>glaucoma</th>
      <th>age</th>
      <th>ocular_pressure</th>
      <th>MD</th>
      <th>PSD</th>
      <th>GHT</th>
      <th>cornea_thickness</th>
      <th>RNFL4.mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>OD</td>
      <td>0</td>
      <td>62</td>
      <td>17</td>
      <td>-0.54</td>
      <td>1.81</td>
      <td>0</td>
      <td>558</td>
      <td>103.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OS</td>
      <td>0</td>
      <td>62</td>
      <td>17</td>
      <td>-0.64</td>
      <td>1.38</td>
      <td>0</td>
      <td>564</td>
      <td>107.666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>OD</td>
      <td>0</td>
      <td>66</td>
      <td>12</td>
      <td>-1.65</td>
      <td>2.89</td>
      <td>2</td>
      <td>490</td>
      <td>162.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OS</td>
      <td>0</td>
      <td>66</td>
      <td>12</td>
      <td>-1.14</td>
      <td>3.88</td>
      <td>2</td>
      <td>495</td>
      <td>99.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OD</td>
      <td>1</td>
      <td>53</td>
      <td>24</td>
      <td>-2.90</td>
      <td>3.78</td>
      <td>2</td>
      <td>547</td>
      <td>74.666667</td>
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
      <td>...</td>
    </tr>
    <tr>
      <th>494</th>
      <td>OS</td>
      <td>0</td>
      <td>13</td>
      <td>15</td>
      <td>-2.44</td>
      <td>2.49</td>
      <td>0</td>
      <td>531</td>
      <td>109.666667</td>
    </tr>
    <tr>
      <th>495</th>
      <td>OD</td>
      <td>0</td>
      <td>55</td>
      <td>15</td>
      <td>-1.21</td>
      <td>2.17</td>
      <td>0</td>
      <td>562</td>
      <td>109.333333</td>
    </tr>
    <tr>
      <th>496</th>
      <td>OS</td>
      <td>0</td>
      <td>55</td>
      <td>16</td>
      <td>-0.84</td>
      <td>1.86</td>
      <td>0</td>
      <td>566</td>
      <td>110.333333</td>
    </tr>
    <tr>
      <th>497</th>
      <td>OD</td>
      <td>0</td>
      <td>55</td>
      <td>18</td>
      <td>-0.43</td>
      <td>1.91</td>
      <td>0</td>
      <td>545</td>
      <td>120.666667</td>
    </tr>
    <tr>
      <th>498</th>
      <td>OS</td>
      <td>0</td>
      <td>55</td>
      <td>18</td>
      <td>-0.62</td>
      <td>2.46</td>
      <td>0</td>
      <td>542</td>
      <td>111.666667</td>
    </tr>
  </tbody>
</table>
<p>499 rows × 9 columns</p>
</div>




```python
df_whole.plot.scatter(y='ocular_pressure', x='age', figsize=(7,7), c='lightblue')
plt.title("ocular pressure vs age")
df_whole.plot.scatter(y='cornea_thickness', x='age', figsize=(7,7), c='pink')
plt.title("cornea thickness vs age")

# from a plain scatter plot between ocular pressure and age, 
```




    Text(0.5, 1.0, 'cornea thickness vs age')




![png](output_2_1.png)



![png](output_2_2.png)



```python
# dropping the column titled "RL" to remove strings within this dataset to prepare for PCA analysis.
df_whole = df_whole.drop('RL', 1) 
```


```python
# checking to see if "RL" column was dropped. 
df_whole
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
      <th>glaucoma</th>
      <th>age</th>
      <th>ocular_pressure</th>
      <th>MD</th>
      <th>PSD</th>
      <th>GHT</th>
      <th>cornea_thickness</th>
      <th>RNFL4.mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>62</td>
      <td>17</td>
      <td>-0.54</td>
      <td>1.81</td>
      <td>0</td>
      <td>558</td>
      <td>103.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>62</td>
      <td>17</td>
      <td>-0.64</td>
      <td>1.38</td>
      <td>0</td>
      <td>564</td>
      <td>107.666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>66</td>
      <td>12</td>
      <td>-1.65</td>
      <td>2.89</td>
      <td>2</td>
      <td>490</td>
      <td>162.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>66</td>
      <td>12</td>
      <td>-1.14</td>
      <td>3.88</td>
      <td>2</td>
      <td>495</td>
      <td>99.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>53</td>
      <td>24</td>
      <td>-2.90</td>
      <td>3.78</td>
      <td>2</td>
      <td>547</td>
      <td>74.666667</td>
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
      <th>494</th>
      <td>0</td>
      <td>13</td>
      <td>15</td>
      <td>-2.44</td>
      <td>2.49</td>
      <td>0</td>
      <td>531</td>
      <td>109.666667</td>
    </tr>
    <tr>
      <th>495</th>
      <td>0</td>
      <td>55</td>
      <td>15</td>
      <td>-1.21</td>
      <td>2.17</td>
      <td>0</td>
      <td>562</td>
      <td>109.333333</td>
    </tr>
    <tr>
      <th>496</th>
      <td>0</td>
      <td>55</td>
      <td>16</td>
      <td>-0.84</td>
      <td>1.86</td>
      <td>0</td>
      <td>566</td>
      <td>110.333333</td>
    </tr>
    <tr>
      <th>497</th>
      <td>0</td>
      <td>55</td>
      <td>18</td>
      <td>-0.43</td>
      <td>1.91</td>
      <td>0</td>
      <td>545</td>
      <td>120.666667</td>
    </tr>
    <tr>
      <th>498</th>
      <td>0</td>
      <td>55</td>
      <td>18</td>
      <td>-0.62</td>
      <td>2.46</td>
      <td>0</td>
      <td>542</td>
      <td>111.666667</td>
    </tr>
  </tbody>
</table>
<p>499 rows × 8 columns</p>
</div>




```python
pca = PCA(n_components=8)
```


```python
pca.fit(df_whole)
```




    PCA(n_components=8)




```python
print(f"The fraction of the variation in the dataset captured by the first two components is:") 
print(f"{pca.explained_variance_ratio_[1]} and {pca.explained_variance_ratio_[2]}")
```

    The fraction of the variation in the dataset captured by the first two components is:
    0.31743713542431584 and 0.09530650602013346
    


```python
X_pca=pca.transform(df_whole)
X_pca.shape
```




    (499, 8)




```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(df_whole) ## fit the new scaler object to the model 
print('mean:', scaler.mean_)
print('variance:', scaler.var_)

X_df_scale = scaler.transform(df_whole) ## transform the data by appling the fitted model by subtracting mean and dividing by SD

pca2 = PCA(n_components=8) ## create PCA model
pca2.fit(X_df_scale) ## fit new PCA model to the transformed data from earlier
X_pca2 = pca2.transform(X_df_scale) ## transform the data to the new PCA coordinate system
```

    mean: [  0.59519038  56.84569138  20.96392786  -8.66138277   5.51208417
       1.24849699 540.30861723  82.55177021]
    variance: [2.40938791e-01 2.36940117e+02 7.29045104e+01 1.05813327e+02
     1.81335323e+01 8.64100947e-01 1.12634564e+03 6.63270255e+02]
    


```python
# plotting the raw PCA plot between PCA1 = age and PCA2 = ocular pressure
plt.scatter( X_pca[:,1], X_pca[:,2], c= "lightblue")
plt.title('First two principal components (age and ocular pressure)')
plt.xlabel('PCA1')
plt.ylabel('PCA2');
```


![png](output_10_0.png)



```python
# plotting both the scaled and raw PCA plots for PCA1 = age and PCA2 = ocular pressure

plt.figure(figsize=(12, 6)) 

## define number of rows and columns we want from this plot
nrows=1
ncols=2

plt.subplot(nrows,ncols, 1)
plt.scatter( X_pca[:,1], X_pca[:,2], c='lightblue')
plt.title('Raw Plot (ocular pressure and age)')
plt.xlabel('PCA1') ## x-axis label
plt.ylabel('PCA2') ## y-axis label


plt.subplot(nrows, ncols, 2)
plt.scatter( X_pca2[:,1], X_pca2[:,2], c='lightblue') ## generate scatter plot using the scaled data
plt.title('Scaled_Plot (ocular pressure and age)')
plt.xlabel('PCA1')
```




    Text(0.5, 0, 'PCA1')




![png](output_11_1.png)



```python
# plotting both the scaled and raw PCA plots for PCA1 = age and PCA2 = cornea thickness

plt.figure(figsize=(12, 6)) 

## define number of rows and columns we want from this plot
nrows=1
ncols=2

plt.subplot(nrows,ncols, 1)
plt.scatter( X_pca[:,1], X_pca[:,6], c='pink')
plt.title('Raw Plot (cornea thickness and age)')
plt.xlabel('PCA1') ## x-axis label
plt.ylabel('PCA2') ## y-axis label


plt.subplot(nrows, ncols, 2)
plt.scatter( X_pca2[:,1], X_pca2[:,6], c='pink') ## generate scatter plot using the scaled data
plt.title('Scaled_Plot (cornea thickness and age)')
plt.xlabel('PCA1')
```




    Text(0.5, 0, 'PCA1')




![png](output_12_1.png)

