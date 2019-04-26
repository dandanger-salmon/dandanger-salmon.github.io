---
layout:     post
title:      decay problem
subtitle:   dacay problem
date:       2019-04-21
author:     ZYC
header-img: img/post-bg-miui6.jpg
catalog: true
comments: true
tags:
    - ocean
    - NWP
    - test
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# Ocean Modelling for Beginners:decay problem

## 1 显示时间导数向前迭代 explicit scheme

离散方程为:  <br/>
$$\frac{c^{n+1}-c^{n}}{\triangle t}=\kappa c^{n}$$ <br/>
调整为: <br/>
$$ c^{n+1}=(1-\triangle t \cdot \kappa)c^{n} $$   <br/>
为了保证数值解稳定，需要满足条件： <br/>
$$ \triangle t < \frac{1}{\kappa} $$ <br/>

## 2 隐式时间导数向前迭代 implicit scheme

离散方程为： <br/>
$$ \frac{c^{n+1}-c^{n}}{\triangle t}=\kappa c^{n+1} $$ <br/>
调整为： <br/>
$$ c^{n+1}=(1+\triangle t \cdot \kappa)c^{n} $$ <br/>
此时，数值解稳定。

## 3 混合形式 hybrid scheme

离散方程为： <br/>
$$ \frac{c^{n+1}-c^{n}}{\triangle t}=-\alpha \cdot \kappa \cdot c^{n+1}-(1-\alpha )\cdot \kappa \cdot c^{n} $$ <br/>
调整为： <br/>
$$ c^{n+1}=\frac{1-(1- \alpha) \cdot \triangle t \cdot \kappa}{1+\alpha \cdot \triangle t \cdot \kappa}c^{n} $$ <br/>
当 $\alpha=1$ 时是隐式； <br/> <br/> 当 $\alpha=0$ 时是显示； <br/><br/> 当 $\alpha=0.5$ 时是半隐式。 <br/><br/>


```python
import numpy as np
import math
import pandas as pd
import matplotlib.pyplot as plt
czero = 100
c=czero
kappa = 0.0001
dt = 3600

facmod1 = 1.0-dt*kappa
if facmod1<=0 :
    print("STABITY CRITERION ALERT: REDUCE TIME STEP")
    exit()   
facmod2 = 1.0/(1.+dt*kappa)
ntot = 24.0*3600./dt

cnmode1=np.empty(int(ntot))
cnmode1[:]=c

cnmode2=np.empty(int(ntot))
cnmode2[:]=c

cture=np.empty(int(ntot))
cture[:]=czero

fac025=(1-(1-0.25)*dt*kappa)/(1+0.25*dt*kappa)
cna025=np.empty(int(ntot))
cna025[:]=c

fac050=(1-(1-0.50)*dt*kappa)/(1+0.50*dt*kappa)
cna050=np.empty(int(ntot))
cna050[:]=c

fac075=(1-(1-0.75)*dt*kappa)/(1+0.75*dt*kappa)
cna075=np.empty(int(ntot))
cna075[:]=c

for i in (range(int(ntot)-1)):
    cnmode1[(i+1)]=cnmode1[i]*facmod1
    cnmode2[(i+1)]=cnmode2[i]*facmod2
    cna025[(i+1)]=cna025[i]*fac025
    cna050[(i+1)]=cna050[i]*fac050
    cna075[(i+1)]=cna075[i]*fac075
    cture[i+1]=czero*math.exp(-kappa*(i+1)*dt)
plotframe=pd.DataFrame({'true':cture,'ex':cnmode1,'im':cnmode2,'a025':cna025,'a050':cna050,'a075':cna075})
```


```python
plotframe
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
      <th>true</th>
      <th>ex</th>
      <th>im</th>
      <th>a025</th>
      <th>a050</th>
      <th>a075</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>69.767633</td>
      <td>64.000000</td>
      <td>73.529412</td>
      <td>66.972477</td>
      <td>69.491525</td>
      <td>71.653543</td>
    </tr>
    <tr>
      <th>2</th>
      <td>48.675226</td>
      <td>40.960000</td>
      <td>54.065744</td>
      <td>44.853127</td>
      <td>48.290721</td>
      <td>51.342303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33.959553</td>
      <td>26.214400</td>
      <td>39.754223</td>
      <td>30.039250</td>
      <td>33.557959</td>
      <td>36.788579</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23.692776</td>
      <td>16.777216</td>
      <td>29.231047</td>
      <td>20.118030</td>
      <td>23.319937</td>
      <td>26.360320</td>
    </tr>
    <tr>
      <th>5</th>
      <td>16.529889</td>
      <td>10.737418</td>
      <td>21.493417</td>
      <td>13.473543</td>
      <td>16.205380</td>
      <td>18.888104</td>
    </tr>
    <tr>
      <th>6</th>
      <td>11.532512</td>
      <td>6.871948</td>
      <td>15.803983</td>
      <td>9.023565</td>
      <td>11.261366</td>
      <td>13.533996</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8.045961</td>
      <td>4.398047</td>
      <td>11.620576</td>
      <td>6.043305</td>
      <td>7.825695</td>
      <td>9.697587</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5.613476</td>
      <td>2.814750</td>
      <td>8.544541</td>
      <td>4.047351</td>
      <td>5.438195</td>
      <td>6.948665</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3.916390</td>
      <td>1.801440</td>
      <td>6.282751</td>
      <td>2.710611</td>
      <td>3.779085</td>
      <td>4.978965</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2.732372</td>
      <td>1.152922</td>
      <td>4.619670</td>
      <td>1.815364</td>
      <td>2.626143</td>
      <td>3.567605</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1.906311</td>
      <td>0.737870</td>
      <td>3.396816</td>
      <td>1.215794</td>
      <td>1.824947</td>
      <td>2.556315</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1.329988</td>
      <td>0.472237</td>
      <td>2.497659</td>
      <td>0.814247</td>
      <td>1.268184</td>
      <td>1.831690</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.927901</td>
      <td>0.302231</td>
      <td>1.836514</td>
      <td>0.545322</td>
      <td>0.881280</td>
      <td>1.312471</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.647375</td>
      <td>0.193428</td>
      <td>1.350378</td>
      <td>0.365215</td>
      <td>0.612415</td>
      <td>0.940432</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.451658</td>
      <td>0.123794</td>
      <td>0.992925</td>
      <td>0.244594</td>
      <td>0.425577</td>
      <td>0.673853</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.315111</td>
      <td>0.079228</td>
      <td>0.730092</td>
      <td>0.163811</td>
      <td>0.295740</td>
      <td>0.482839</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0.219846</td>
      <td>0.050706</td>
      <td>0.536832</td>
      <td>0.109708</td>
      <td>0.205514</td>
      <td>0.345972</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.153381</td>
      <td>0.032452</td>
      <td>0.394730</td>
      <td>0.073474</td>
      <td>0.142815</td>
      <td>0.247901</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.107010</td>
      <td>0.020769</td>
      <td>0.290242</td>
      <td>0.049207</td>
      <td>0.099244</td>
      <td>0.177630</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.074659</td>
      <td>0.013292</td>
      <td>0.213413</td>
      <td>0.032955</td>
      <td>0.068966</td>
      <td>0.127278</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.052088</td>
      <td>0.008507</td>
      <td>0.156922</td>
      <td>0.022071</td>
      <td>0.047926</td>
      <td>0.091199</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.036340</td>
      <td>0.005445</td>
      <td>0.115384</td>
      <td>0.014782</td>
      <td>0.033304</td>
      <td>0.065347</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.025354</td>
      <td>0.003484</td>
      <td>0.084841</td>
      <td>0.009900</td>
      <td>0.023144</td>
      <td>0.046824</td>
    </tr>
  </tbody>
</table>
</div>




```python
plotframe[0:10].plot()
plt.show()
```


![image](_posts/output_9_0.png)

