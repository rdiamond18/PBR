## Intro
My name is Richard Diamond and I am a sophomore at Tufts. Prep Baseball Report has given me 3 questions for a technical assessment. In this notebook, I will answer them and explain my code.

## Importing Packages


```python
!pip install pybaseball
```

    Requirement already satisfied: pybaseball in /opt/anaconda3/lib/python3.9/site-packages (2.2.5)
    Requirement already satisfied: requests>=2.18.1 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (2.27.1)
    Requirement already satisfied: numpy>=1.13.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (1.21.5)
    Requirement already satisfied: pyarrow>=1.0.1 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (11.0.0)
    Requirement already satisfied: pandas>=1.0.3 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (1.4.2)
    Requirement already satisfied: scipy>=1.4.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (1.7.3)
    Requirement already satisfied: tqdm>=4.50.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (4.64.0)
    Requirement already satisfied: matplotlib>=2.0.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (3.5.1)
    Requirement already satisfied: attrs>=20.3.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (21.4.0)
    Requirement already satisfied: beautifulsoup4>=4.4.0 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (4.11.1)
    Requirement already satisfied: lxml>=4.2.1 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (4.8.0)
    Requirement already satisfied: pygithub>=1.51 in /opt/anaconda3/lib/python3.9/site-packages (from pybaseball) (1.58.1)
    Requirement already satisfied: soupsieve>1.2 in /opt/anaconda3/lib/python3.9/site-packages (from beautifulsoup4>=4.4.0->pybaseball) (2.3.1)
    Requirement already satisfied: fonttools>=4.22.0 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (4.25.0)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (2.8.2)
    Requirement already satisfied: pillow>=6.2.0 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (9.0.1)
    Requirement already satisfied: pyparsing>=2.2.1 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (3.0.4)
    Requirement already satisfied: packaging>=20.0 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (21.3)
    Requirement already satisfied: kiwisolver>=1.0.1 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (1.3.2)
    Requirement already satisfied: cycler>=0.10 in /opt/anaconda3/lib/python3.9/site-packages (from matplotlib>=2.0.0->pybaseball) (0.11.0)
    Requirement already satisfied: pytz>=2020.1 in /opt/anaconda3/lib/python3.9/site-packages (from pandas>=1.0.3->pybaseball) (2021.3)
    Requirement already satisfied: deprecated in /opt/anaconda3/lib/python3.9/site-packages (from pygithub>=1.51->pybaseball) (1.2.13)
    Requirement already satisfied: pynacl>=1.4.0 in /opt/anaconda3/lib/python3.9/site-packages (from pygithub>=1.51->pybaseball) (1.5.0)
    Requirement already satisfied: pyjwt[crypto]>=2.4.0 in /opt/anaconda3/lib/python3.9/site-packages (from pygithub>=1.51->pybaseball) (2.6.0)
    Requirement already satisfied: cryptography>=3.4.0 in /opt/anaconda3/lib/python3.9/site-packages (from pyjwt[crypto]>=2.4.0->pygithub>=1.51->pybaseball) (3.4.8)
    Requirement already satisfied: cffi>=1.12 in /opt/anaconda3/lib/python3.9/site-packages (from cryptography>=3.4.0->pyjwt[crypto]>=2.4.0->pygithub>=1.51->pybaseball) (1.15.0)
    Requirement already satisfied: pycparser in /opt/anaconda3/lib/python3.9/site-packages (from cffi>=1.12->cryptography>=3.4.0->pyjwt[crypto]>=2.4.0->pygithub>=1.51->pybaseball) (2.21)
    Requirement already satisfied: six>=1.5 in /opt/anaconda3/lib/python3.9/site-packages (from python-dateutil>=2.7->matplotlib>=2.0.0->pybaseball) (1.16.0)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /opt/anaconda3/lib/python3.9/site-packages (from requests>=2.18.1->pybaseball) (1.26.9)
    Requirement already satisfied: idna<4,>=2.5 in /opt/anaconda3/lib/python3.9/site-packages (from requests>=2.18.1->pybaseball) (3.3)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.9/site-packages (from requests>=2.18.1->pybaseball) (2021.10.8)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /opt/anaconda3/lib/python3.9/site-packages (from requests>=2.18.1->pybaseball) (2.0.4)
    Requirement already satisfied: wrapt<2,>=1.10 in /opt/anaconda3/lib/python3.9/site-packages (from deprecated->pygithub>=1.51->pybaseball) (1.12.1)



```python
import numpy as np
import pandas as pd
```

## Question 1 - Relationship between OBP and SLG
Is there a relationship between a player's on-base percentage (OBP) and their slugging
percentage (SLG)? Create a scatter plot to show the relationship between these
variables. Does this relationship hold for all players or only certain types of hitters?
Explain your findings.

### Gather Data


```python
from pybaseball import batting_stats
df = batting_stats(2022)

df.head()
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
      <th>IDfg</th>
      <th>Season</th>
      <th>Name</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15640</td>
      <td>2022</td>
      <td>Aaron Judge</td>
      <td>NYY</td>
      <td>30</td>
      <td>157</td>
      <td>570</td>
      <td>696</td>
      <td>177</td>
      <td>87</td>
      <td>...</td>
      <td>118.4</td>
      <td>246</td>
      <td>0.609</td>
      <td>404</td>
      <td>0.169</td>
      <td>0.287</td>
      <td>0.305</td>
      <td>0.706</td>
      <td>0.463</td>
      <td>11.2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>11493</td>
      <td>2022</td>
      <td>Manny Machado</td>
      <td>SDP</td>
      <td>29</td>
      <td>150</td>
      <td>578</td>
      <td>644</td>
      <td>172</td>
      <td>102</td>
      <td>...</td>
      <td>112.4</td>
      <td>219</td>
      <td>0.490</td>
      <td>447</td>
      <td>0.126</td>
      <td>0.243</td>
      <td>0.264</td>
      <td>0.447</td>
      <td>0.338</td>
      <td>6.6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>9777</td>
      <td>2022</td>
      <td>Nolan Arenado</td>
      <td>STL</td>
      <td>31</td>
      <td>148</td>
      <td>557</td>
      <td>620</td>
      <td>163</td>
      <td>90</td>
      <td>...</td>
      <td>111.4</td>
      <td>190</td>
      <td>0.389</td>
      <td>489</td>
      <td>0.155</td>
      <td>0.241</td>
      <td>0.265</td>
      <td>0.445</td>
      <td>0.339</td>
      <td>7.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9218</td>
      <td>2022</td>
      <td>Paul Goldschmidt</td>
      <td>STL</td>
      <td>34</td>
      <td>151</td>
      <td>561</td>
      <td>651</td>
      <td>178</td>
      <td>102</td>
      <td>...</td>
      <td>112.3</td>
      <td>200</td>
      <td>0.469</td>
      <td>426</td>
      <td>0.196</td>
      <td>0.295</td>
      <td>0.261</td>
      <td>0.482</td>
      <td>0.367</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5361</td>
      <td>2022</td>
      <td>Freddie Freeman</td>
      <td>LAD</td>
      <td>32</td>
      <td>159</td>
      <td>612</td>
      <td>708</td>
      <td>199</td>
      <td>129</td>
      <td>...</td>
      <td>112.3</td>
      <td>248</td>
      <td>0.480</td>
      <td>517</td>
      <td>0.122</td>
      <td>0.206</td>
      <td>0.313</td>
      <td>0.538</td>
      <td>0.403</td>
      <td>6.5</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 320 columns</p>
</div>



### Plot OBP vs. SLG


```python
import matplotlib.pyplot as plt
plt.scatter(df['OBP'], df['SLG'])
plt.title('On Base Percentage vs. Slugging Percentage')
plt.xlabel('OBP')
plt.ylabel('SLG')

#create equation for trendline
z = np.polyfit(df['OBP'], df['SLG'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(df['OBP'], p(df['OBP']))

#find r^2
cc = np.corrcoef(df['OBP'], df['SLG'])
print('Correlation Coefficient -', round(cc[0][1],2))
```

    Correlation Coefficient - 0.58



    
![png](output_8_1.png)
    


The correlation coefficient value between these two factors is roughly 0.584. This indicates a relationship between the two factors, which makes sense, as guys who get a high number of base hits will have those hits contribute to both their OBP and SLG. Next, I wanted to take a look at whether this relationship holds for different types of batters.

### Taking a look at the relationship for free-swingers
One difference between OBP and slugging is that OBP factors in walks, while SLG does not. I wanted to try to filter for players who had a more free-swinging approach and observe that relationship. I chose to do this and not filter by OBP because this would bias my data towards better players. I wanted to include all types of players to get a more accurate representation of the sample.


```python
#find the median swing rate
median_swingrate = df['Swing%'].median()
print(median_swingrate)
```

    0.48050000000000004



```python
#Keep data for above average swing rate players
df2 = df[df['Swing%'] > .48]
df2
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
      <th>IDfg</th>
      <th>Season</th>
      <th>Name</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>11493</td>
      <td>2022</td>
      <td>Manny Machado</td>
      <td>SDP</td>
      <td>29</td>
      <td>150</td>
      <td>578</td>
      <td>644</td>
      <td>172</td>
      <td>102</td>
      <td>...</td>
      <td>112.4</td>
      <td>219</td>
      <td>0.490</td>
      <td>447</td>
      <td>0.126</td>
      <td>0.243</td>
      <td>0.264</td>
      <td>0.447</td>
      <td>0.338</td>
      <td>6.6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>9777</td>
      <td>2022</td>
      <td>Nolan Arenado</td>
      <td>STL</td>
      <td>31</td>
      <td>148</td>
      <td>557</td>
      <td>620</td>
      <td>163</td>
      <td>90</td>
      <td>...</td>
      <td>111.4</td>
      <td>190</td>
      <td>0.389</td>
      <td>489</td>
      <td>0.155</td>
      <td>0.241</td>
      <td>0.265</td>
      <td>0.445</td>
      <td>0.339</td>
      <td>7.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5361</td>
      <td>2022</td>
      <td>Freddie Freeman</td>
      <td>LAD</td>
      <td>32</td>
      <td>159</td>
      <td>612</td>
      <td>708</td>
      <td>199</td>
      <td>129</td>
      <td>...</td>
      <td>112.3</td>
      <td>248</td>
      <td>0.480</td>
      <td>517</td>
      <td>0.122</td>
      <td>0.206</td>
      <td>0.313</td>
      <td>0.538</td>
      <td>0.403</td>
      <td>6.5</td>
    </tr>
    <tr>
      <th>26</th>
      <td>11739</td>
      <td>2022</td>
      <td>J.T. Realmuto</td>
      <td>PHI</td>
      <td>31</td>
      <td>139</td>
      <td>504</td>
      <td>562</td>
      <td>139</td>
      <td>86</td>
      <td>...</td>
      <td>110.4</td>
      <td>182</td>
      <td>0.467</td>
      <td>390</td>
      <td>0.155</td>
      <td>0.269</td>
      <td>0.275</td>
      <td>0.463</td>
      <td>0.351</td>
      <td>6.6</td>
    </tr>
    <tr>
      <th>53</th>
      <td>18314</td>
      <td>2022</td>
      <td>Dansby Swanson</td>
      <td>ATL</td>
      <td>28</td>
      <td>162</td>
      <td>640</td>
      <td>696</td>
      <td>177</td>
      <td>119</td>
      <td>...</td>
      <td>109.6</td>
      <td>213</td>
      <td>0.461</td>
      <td>462</td>
      <td>0.149</td>
      <td>0.288</td>
      <td>0.257</td>
      <td>0.461</td>
      <td>0.337</td>
      <td>4.9</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
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
      <th>118</th>
      <td>11342</td>
      <td>2022</td>
      <td>Jesus Aguilar</td>
      <td>- - -</td>
      <td>32</td>
      <td>129</td>
      <td>464</td>
      <td>507</td>
      <td>109</td>
      <td>74</td>
      <td>...</td>
      <td>108.1</td>
      <td>126</td>
      <td>0.354</td>
      <td>356</td>
      <td>0.173</td>
      <td>0.297</td>
      <td>0.240</td>
      <td>0.393</td>
      <td>0.298</td>
      <td>-0.6</td>
    </tr>
    <tr>
      <th>112</th>
      <td>10324</td>
      <td>2022</td>
      <td>Marcell Ozuna</td>
      <td>ATL</td>
      <td>31</td>
      <td>124</td>
      <td>470</td>
      <td>507</td>
      <td>106</td>
      <td>64</td>
      <td>...</td>
      <td>113.9</td>
      <td>154</td>
      <td>0.438</td>
      <td>352</td>
      <td>0.156</td>
      <td>0.289</td>
      <td>0.256</td>
      <td>0.478</td>
      <td>0.337</td>
      <td>-0.7</td>
    </tr>
    <tr>
      <th>109</th>
      <td>11737</td>
      <td>2022</td>
      <td>Nick Castellanos</td>
      <td>PHI</td>
      <td>30</td>
      <td>136</td>
      <td>524</td>
      <td>558</td>
      <td>138</td>
      <td>98</td>
      <td>...</td>
      <td>110.1</td>
      <td>137</td>
      <td>0.346</td>
      <td>396</td>
      <td>0.110</td>
      <td>0.280</td>
      <td>0.250</td>
      <td>0.395</td>
      <td>0.302</td>
      <td>-0.3</td>
    </tr>
    <tr>
      <th>116</th>
      <td>2434</td>
      <td>2022</td>
      <td>Nelson Cruz</td>
      <td>WSN</td>
      <td>41</td>
      <td>124</td>
      <td>448</td>
      <td>507</td>
      <td>105</td>
      <td>79</td>
      <td>...</td>
      <td>113.8</td>
      <td>153</td>
      <td>0.457</td>
      <td>335</td>
      <td>0.127</td>
      <td>0.277</td>
      <td>0.241</td>
      <td>0.399</td>
      <td>0.320</td>
      <td>-0.9</td>
    </tr>
    <tr>
      <th>121</th>
      <td>19198</td>
      <td>2022</td>
      <td>Yuli Gurriel</td>
      <td>HOU</td>
      <td>38</td>
      <td>146</td>
      <td>545</td>
      <td>584</td>
      <td>132</td>
      <td>84</td>
      <td>...</td>
      <td>109.6</td>
      <td>168</td>
      <td>0.354</td>
      <td>475</td>
      <td>0.187</td>
      <td>0.249</td>
      <td>0.240</td>
      <td>0.319</td>
      <td>0.271</td>
      <td>-0.3</td>
    </tr>
  </tbody>
</table>
<p>65 rows × 320 columns</p>
</div>




```python
#plot new data
plt.scatter(df2['OBP'], df2['SLG'], color = 'red')
plt.title('On Base Percentage vs. Slugging Percentage - High Swing Rate')
plt.xlabel('OBP')
plt.ylabel('SLG')

#create equation for trendline
z = np.polyfit(df2['OBP'], df2['SLG'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(df2['OBP'], p(df2['OBP']))

#find r^2
cc = np.corrcoef(df2['OBP'], df2['SLG'])
print('Correlation Coefficient -', round(cc[0][1],2))
```

    Correlation Coefficient - 0.68



    
![png](output_13_1.png)
    


The above plot and correlation coefficient value show that the relationship is stronger for players who swing more. This is likely because of the fact that these players walk less, and thus their OBP is more tied to the amount of hits they get. Next, I wanted to try and understand the relationship for players who swing less than average.


```python
#Keep data for below average swing rate players
df3 = df[df['Swing%'] < .48]
df3.head()
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
      <th>IDfg</th>
      <th>Season</th>
      <th>Name</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15640</td>
      <td>2022</td>
      <td>Aaron Judge</td>
      <td>NYY</td>
      <td>30</td>
      <td>157</td>
      <td>570</td>
      <td>696</td>
      <td>177</td>
      <td>87</td>
      <td>...</td>
      <td>118.4</td>
      <td>246</td>
      <td>0.609</td>
      <td>404</td>
      <td>0.169</td>
      <td>0.287</td>
      <td>0.305</td>
      <td>0.706</td>
      <td>0.463</td>
      <td>11.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9218</td>
      <td>2022</td>
      <td>Paul Goldschmidt</td>
      <td>STL</td>
      <td>34</td>
      <td>151</td>
      <td>561</td>
      <td>651</td>
      <td>178</td>
      <td>102</td>
      <td>...</td>
      <td>112.3</td>
      <td>200</td>
      <td>0.469</td>
      <td>426</td>
      <td>0.196</td>
      <td>0.295</td>
      <td>0.261</td>
      <td>0.482</td>
      <td>0.367</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>12916</td>
      <td>2022</td>
      <td>Francisco Lindor</td>
      <td>NYM</td>
      <td>28</td>
      <td>161</td>
      <td>630</td>
      <td>706</td>
      <td>170</td>
      <td>114</td>
      <td>...</td>
      <td>110.7</td>
      <td>207</td>
      <td>0.411</td>
      <td>504</td>
      <td>0.154</td>
      <td>0.254</td>
      <td>0.254</td>
      <td>0.427</td>
      <td>0.331</td>
      <td>6.3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19556</td>
      <td>2022</td>
      <td>Yordan Alvarez</td>
      <td>HOU</td>
      <td>25</td>
      <td>135</td>
      <td>470</td>
      <td>561</td>
      <td>144</td>
      <td>76</td>
      <td>...</td>
      <td>117.4</td>
      <td>222</td>
      <td>0.598</td>
      <td>371</td>
      <td>0.165</td>
      <td>0.256</td>
      <td>0.329</td>
      <td>0.672</td>
      <td>0.462</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5417</td>
      <td>2022</td>
      <td>Jose Altuve</td>
      <td>HOU</td>
      <td>32</td>
      <td>141</td>
      <td>527</td>
      <td>604</td>
      <td>158</td>
      <td>91</td>
      <td>...</td>
      <td>109.8</td>
      <td>130</td>
      <td>0.295</td>
      <td>441</td>
      <td>0.173</td>
      <td>0.240</td>
      <td>0.269</td>
      <td>0.440</td>
      <td>0.354</td>
      <td>6.4</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 320 columns</p>
</div>




```python
#plot new data
plt.scatter(df3['OBP'], df3['SLG'], color = 'green')
plt.title('On Base Percentage vs. Slugging Percentage - Low Swing Rate')
plt.xlabel('OBP')
plt.ylabel('SLG')

#create equation for trendline
z = np.polyfit(df3['OBP'], df3['SLG'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(df3['OBP'], p(df3['OBP']))

#find r^2
cc = np.corrcoef(df3['OBP'], df3['SLG'])
print('Correlation Coefficient -', round(cc[0][1], 2))
```

    Correlation Coefficient - 0.6



    
![png](output_16_1.png)
    


This relationship between OBP and SLG for players who swing less appears to be a little weaker than that of free-swinging players. This makes sense, because these players are more patient, leading them to be more likely to walk and have their walks contribute to OBP, but not SLG.

## Question 2 - Which team had the best offense?
Which team had the best offense in the 2022 MLB season? To answer this question, you
should consider statistics such as runs scored, batting average, on-base percentage,
and slugging percentage. How does the performance of the top team compare to that of
the average team?


```python
from pybaseball import team_batting
batting = team_batting(2022)
batting
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
      <th>teamIDfg</th>
      <th>Season</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>2B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22</td>
      <td>2022</td>
      <td>LAD</td>
      <td>29</td>
      <td>2326</td>
      <td>5526</td>
      <td>6247</td>
      <td>1418</td>
      <td>850</td>
      <td>325</td>
      <td>...</td>
      <td>112.5</td>
      <td>1760</td>
      <td>0.418</td>
      <td>4210</td>
      <td>0.161</td>
      <td>0.268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>36.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>2022</td>
      <td>TOR</td>
      <td>28</td>
      <td>2445</td>
      <td>5555</td>
      <td>6158</td>
      <td>1464</td>
      <td>945</td>
      <td>307</td>
      <td>...</td>
      <td>118.4</td>
      <td>1920</td>
      <td>0.440</td>
      <td>4361</td>
      <td>0.161</td>
      <td>0.267</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16</td>
      <td>2022</td>
      <td>ATL</td>
      <td>29</td>
      <td>2259</td>
      <td>5509</td>
      <td>6082</td>
      <td>1394</td>
      <td>842</td>
      <td>298</td>
      <td>...</td>
      <td>116.8</td>
      <td>1756</td>
      <td>0.434</td>
      <td>4048</td>
      <td>0.148</td>
      <td>0.278</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>2022</td>
      <td>NYY</td>
      <td>29</td>
      <td>2342</td>
      <td>5422</td>
      <td>6172</td>
      <td>1308</td>
      <td>821</td>
      <td>225</td>
      <td>...</td>
      <td>119.8</td>
      <td>1705</td>
      <td>0.417</td>
      <td>4091</td>
      <td>0.177</td>
      <td>0.287</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>35.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28</td>
      <td>2022</td>
      <td>STL</td>
      <td>28</td>
      <td>2355</td>
      <td>5496</td>
      <td>6165</td>
      <td>1386</td>
      <td>878</td>
      <td>290</td>
      <td>...</td>
      <td>114.4</td>
      <td>1611</td>
      <td>0.373</td>
      <td>4322</td>
      <td>0.168</td>
      <td>0.272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>33.8</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>2022</td>
      <td>NYM</td>
      <td>29</td>
      <td>2340</td>
      <td>5489</td>
      <td>6176</td>
      <td>1422</td>
      <td>952</td>
      <td>272</td>
      <td>...</td>
      <td>116.5</td>
      <td>1603</td>
      <td>0.370</td>
      <td>4337</td>
      <td>0.162</td>
      <td>0.265</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>21</td>
      <td>2022</td>
      <td>HOU</td>
      <td>29</td>
      <td>2279</td>
      <td>5409</td>
      <td>6054</td>
      <td>1341</td>
      <td>830</td>
      <td>284</td>
      <td>...</td>
      <td>117.4</td>
      <td>1666</td>
      <td>0.389</td>
      <td>4287</td>
      <td>0.156</td>
      <td>0.258</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>26</td>
      <td>2022</td>
      <td>PHI</td>
      <td>28</td>
      <td>2327</td>
      <td>5496</td>
      <td>6077</td>
      <td>1392</td>
      <td>903</td>
      <td>255</td>
      <td>...</td>
      <td>114.8</td>
      <td>1714</td>
      <td>0.410</td>
      <td>4184</td>
      <td>0.163</td>
      <td>0.275</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>2022</td>
      <td>BOS</td>
      <td>29</td>
      <td>2379</td>
      <td>5539</td>
      <td>6144</td>
      <td>1427</td>
      <td>908</td>
      <td>352</td>
      <td>...</td>
      <td>117.9</td>
      <td>1724</td>
      <td>0.408</td>
      <td>4230</td>
      <td>0.160</td>
      <td>0.277</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>23</td>
      <td>2022</td>
      <td>MIL</td>
      <td>29</td>
      <td>2363</td>
      <td>5417</td>
      <td>6122</td>
      <td>1271</td>
      <td>784</td>
      <td>251</td>
      <td>...</td>
      <td>117.2</td>
      <td>1586</td>
      <td>0.396</td>
      <td>4001</td>
      <td>0.173</td>
      <td>0.280</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>8</td>
      <td>2022</td>
      <td>MIN</td>
      <td>28</td>
      <td>2422</td>
      <td>5476</td>
      <td>6113</td>
      <td>1356</td>
      <td>891</td>
      <td>269</td>
      <td>...</td>
      <td>115.1</td>
      <td>1714</td>
      <td>0.410</td>
      <td>4180</td>
      <td>0.165</td>
      <td>0.272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.7</td>
    </tr>
    <tr>
      <th>11</th>
      <td>19</td>
      <td>2022</td>
      <td>COL</td>
      <td>28</td>
      <td>2265</td>
      <td>5540</td>
      <td>6105</td>
      <td>1408</td>
      <td>945</td>
      <td>280</td>
      <td>...</td>
      <td>115.5</td>
      <td>1563</td>
      <td>0.367</td>
      <td>4261</td>
      <td>0.169</td>
      <td>0.275</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>30</td>
      <td>2022</td>
      <td>SFG</td>
      <td>29</td>
      <td>2552</td>
      <td>5392</td>
      <td>6117</td>
      <td>1261</td>
      <td>805</td>
      <td>255</td>
      <td>...</td>
      <td>114.3</td>
      <td>1496</td>
      <td>0.375</td>
      <td>3989</td>
      <td>0.173</td>
      <td>0.283</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>13</th>
      <td>11</td>
      <td>2022</td>
      <td>SEA</td>
      <td>28</td>
      <td>2383</td>
      <td>5375</td>
      <td>6117</td>
      <td>1236</td>
      <td>791</td>
      <td>229</td>
      <td>...</td>
      <td>117.2</td>
      <td>1473</td>
      <td>0.365</td>
      <td>4035</td>
      <td>0.164</td>
      <td>0.270</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.3</td>
    </tr>
    <tr>
      <th>14</th>
      <td>29</td>
      <td>2022</td>
      <td>SDP</td>
      <td>28</td>
      <td>2323</td>
      <td>5468</td>
      <td>6175</td>
      <td>1317</td>
      <td>871</td>
      <td>275</td>
      <td>...</td>
      <td>115.2</td>
      <td>1579</td>
      <td>0.375</td>
      <td>4209</td>
      <td>0.174</td>
      <td>0.274</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.5</td>
    </tr>
    <tr>
      <th>15</th>
      <td>17</td>
      <td>2022</td>
      <td>CHC</td>
      <td>28</td>
      <td>2388</td>
      <td>5425</td>
      <td>6072</td>
      <td>1293</td>
      <td>838</td>
      <td>265</td>
      <td>...</td>
      <td>116.2</td>
      <td>1494</td>
      <td>0.370</td>
      <td>4033</td>
      <td>0.165</td>
      <td>0.284</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>16.7</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4</td>
      <td>2022</td>
      <td>CHW</td>
      <td>29</td>
      <td>2371</td>
      <td>5611</td>
      <td>6123</td>
      <td>1435</td>
      <td>1005</td>
      <td>272</td>
      <td>...</td>
      <td>117.8</td>
      <td>1783</td>
      <td>0.406</td>
      <td>4393</td>
      <td>0.156</td>
      <td>0.273</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>5</td>
      <td>2022</td>
      <td>CLE</td>
      <td>26</td>
      <td>2338</td>
      <td>5558</td>
      <td>6163</td>
      <td>1410</td>
      <td>979</td>
      <td>273</td>
      <td>...</td>
      <td>115.8</td>
      <td>1489</td>
      <td>0.330</td>
      <td>4510</td>
      <td>0.176</td>
      <td>0.268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.2</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2</td>
      <td>2022</td>
      <td>BAL</td>
      <td>27</td>
      <td>2359</td>
      <td>5429</td>
      <td>6049</td>
      <td>1281</td>
      <td>810</td>
      <td>275</td>
      <td>...</td>
      <td>112.0</td>
      <td>1590</td>
      <td>0.388</td>
      <td>4100</td>
      <td>0.154</td>
      <td>0.272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>13</td>
      <td>2022</td>
      <td>TEX</td>
      <td>28</td>
      <td>2365</td>
      <td>5478</td>
      <td>6029</td>
      <td>1308</td>
      <td>866</td>
      <td>224</td>
      <td>...</td>
      <td>113.0</td>
      <td>1601</td>
      <td>0.392</td>
      <td>4080</td>
      <td>0.143</td>
      <td>0.274</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.9</td>
    </tr>
    <tr>
      <th>20</th>
      <td>15</td>
      <td>2022</td>
      <td>ARI</td>
      <td>28</td>
      <td>2407</td>
      <td>5351</td>
      <td>6027</td>
      <td>1232</td>
      <td>773</td>
      <td>262</td>
      <td>...</td>
      <td>115.0</td>
      <td>1458</td>
      <td>0.356</td>
      <td>4095</td>
      <td>0.176</td>
      <td>0.278</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>24</td>
      <td>2022</td>
      <td>WSN</td>
      <td>29</td>
      <td>2347</td>
      <td>5434</td>
      <td>5998</td>
      <td>1351</td>
      <td>943</td>
      <td>252</td>
      <td>...</td>
      <td>115.0</td>
      <td>1509</td>
      <td>0.353</td>
      <td>4275</td>
      <td>0.161</td>
      <td>0.268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.6</td>
    </tr>
    <tr>
      <th>22</th>
      <td>12</td>
      <td>2022</td>
      <td>TBR</td>
      <td>28</td>
      <td>2403</td>
      <td>5412</td>
      <td>6008</td>
      <td>1294</td>
      <td>842</td>
      <td>296</td>
      <td>...</td>
      <td>115.0</td>
      <td>1556</td>
      <td>0.384</td>
      <td>4056</td>
      <td>0.160</td>
      <td>0.278</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.5</td>
    </tr>
    <tr>
      <th>23</th>
      <td>7</td>
      <td>2022</td>
      <td>KCR</td>
      <td>27</td>
      <td>2372</td>
      <td>5437</td>
      <td>6010</td>
      <td>1327</td>
      <td>904</td>
      <td>247</td>
      <td>...</td>
      <td>113.7</td>
      <td>1582</td>
      <td>0.375</td>
      <td>4215</td>
      <td>0.151</td>
      <td>0.265</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.9</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1</td>
      <td>2022</td>
      <td>LAA</td>
      <td>28</td>
      <td>2327</td>
      <td>5423</td>
      <td>5977</td>
      <td>1265</td>
      <td>825</td>
      <td>219</td>
      <td>...</td>
      <td>119.1</td>
      <td>1400</td>
      <td>0.356</td>
      <td>3935</td>
      <td>0.164</td>
      <td>0.282</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.7</td>
    </tr>
    <tr>
      <th>25</th>
      <td>18</td>
      <td>2022</td>
      <td>CIN</td>
      <td>28</td>
      <td>2406</td>
      <td>5380</td>
      <td>5978</td>
      <td>1264</td>
      <td>855</td>
      <td>235</td>
      <td>...</td>
      <td>113.6</td>
      <td>1381</td>
      <td>0.345</td>
      <td>4004</td>
      <td>0.169</td>
      <td>0.282</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.8</td>
    </tr>
    <tr>
      <th>26</th>
      <td>20</td>
      <td>2022</td>
      <td>MIA</td>
      <td>28</td>
      <td>2384</td>
      <td>5395</td>
      <td>5949</td>
      <td>1241</td>
      <td>829</td>
      <td>248</td>
      <td>...</td>
      <td>117.6</td>
      <td>1486</td>
      <td>0.370</td>
      <td>4014</td>
      <td>0.163</td>
      <td>0.282</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>27</th>
      <td>27</td>
      <td>2022</td>
      <td>PIT</td>
      <td>27</td>
      <td>2340</td>
      <td>5331</td>
      <td>5912</td>
      <td>1186</td>
      <td>778</td>
      <td>221</td>
      <td>...</td>
      <td>122.4</td>
      <td>1442</td>
      <td>0.371</td>
      <td>3885</td>
      <td>0.182</td>
      <td>0.293</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.3</td>
    </tr>
    <tr>
      <th>28</th>
      <td>6</td>
      <td>2022</td>
      <td>DET</td>
      <td>27</td>
      <td>2350</td>
      <td>5378</td>
      <td>5870</td>
      <td>1240</td>
      <td>868</td>
      <td>235</td>
      <td>...</td>
      <td>112.1</td>
      <td>1456</td>
      <td>0.362</td>
      <td>4019</td>
      <td>0.152</td>
      <td>0.277</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.6</td>
    </tr>
    <tr>
      <th>29</th>
      <td>10</td>
      <td>2022</td>
      <td>OAK</td>
      <td>28</td>
      <td>2394</td>
      <td>5314</td>
      <td>5863</td>
      <td>1147</td>
      <td>746</td>
      <td>249</td>
      <td>...</td>
      <td>114.0</td>
      <td>1357</td>
      <td>0.341</td>
      <td>3982</td>
      <td>0.159</td>
      <td>0.281</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.7</td>
    </tr>
  </tbody>
</table>
<p>30 rows × 319 columns</p>
</div>



When trying to answer the question of who had the best offense, the naive solution would be to count who scored the most runs. We can answer this question with a simple sort.


```python
runs = batting.sort_values(by = 'R', ascending = False)
runs.head()
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
      <th>teamIDfg</th>
      <th>Season</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>2B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22</td>
      <td>2022</td>
      <td>LAD</td>
      <td>29</td>
      <td>2326</td>
      <td>5526</td>
      <td>6247</td>
      <td>1418</td>
      <td>850</td>
      <td>325</td>
      <td>...</td>
      <td>112.5</td>
      <td>1760</td>
      <td>0.418</td>
      <td>4210</td>
      <td>0.161</td>
      <td>0.268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>36.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>2022</td>
      <td>NYY</td>
      <td>29</td>
      <td>2342</td>
      <td>5422</td>
      <td>6172</td>
      <td>1308</td>
      <td>821</td>
      <td>225</td>
      <td>...</td>
      <td>119.8</td>
      <td>1705</td>
      <td>0.417</td>
      <td>4091</td>
      <td>0.177</td>
      <td>0.287</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>35.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16</td>
      <td>2022</td>
      <td>ATL</td>
      <td>29</td>
      <td>2259</td>
      <td>5509</td>
      <td>6082</td>
      <td>1394</td>
      <td>842</td>
      <td>298</td>
      <td>...</td>
      <td>116.8</td>
      <td>1756</td>
      <td>0.434</td>
      <td>4048</td>
      <td>0.148</td>
      <td>0.278</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>2022</td>
      <td>TOR</td>
      <td>28</td>
      <td>2445</td>
      <td>5555</td>
      <td>6158</td>
      <td>1464</td>
      <td>945</td>
      <td>307</td>
      <td>...</td>
      <td>118.4</td>
      <td>1920</td>
      <td>0.440</td>
      <td>4361</td>
      <td>0.161</td>
      <td>0.267</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28</td>
      <td>2022</td>
      <td>STL</td>
      <td>28</td>
      <td>2355</td>
      <td>5496</td>
      <td>6165</td>
      <td>1386</td>
      <td>878</td>
      <td>290</td>
      <td>...</td>
      <td>114.4</td>
      <td>1611</td>
      <td>0.373</td>
      <td>4322</td>
      <td>0.168</td>
      <td>0.272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>33.8</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 319 columns</p>
</div>



By runs, the top offense in 2022 was the Dodgers, followed by the Yankees and Braves. However, this ignores park factors. So we can instead use wRC+, which normalizes for park factors.


```python
wrc = batting.sort_values(by = 'wRC+', ascending = False)
wrc.head()
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
      <th>teamIDfg</th>
      <th>Season</th>
      <th>Team</th>
      <th>Age</th>
      <th>G</th>
      <th>AB</th>
      <th>PA</th>
      <th>H</th>
      <th>1B</th>
      <th>2B</th>
      <th>...</th>
      <th>maxEV</th>
      <th>HardHit</th>
      <th>HardHit%</th>
      <th>Events</th>
      <th>CStr%</th>
      <th>CSW%</th>
      <th>xBA</th>
      <th>xSLG</th>
      <th>xwOBA</th>
      <th>L-WAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22</td>
      <td>2022</td>
      <td>LAD</td>
      <td>29</td>
      <td>2326</td>
      <td>5526</td>
      <td>6247</td>
      <td>1418</td>
      <td>850</td>
      <td>325</td>
      <td>...</td>
      <td>112.5</td>
      <td>1760</td>
      <td>0.418</td>
      <td>4210</td>
      <td>0.161</td>
      <td>0.268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>36.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>2022</td>
      <td>TOR</td>
      <td>28</td>
      <td>2445</td>
      <td>5555</td>
      <td>6158</td>
      <td>1464</td>
      <td>945</td>
      <td>307</td>
      <td>...</td>
      <td>118.4</td>
      <td>1920</td>
      <td>0.440</td>
      <td>4361</td>
      <td>0.161</td>
      <td>0.267</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25</td>
      <td>2022</td>
      <td>NYM</td>
      <td>29</td>
      <td>2340</td>
      <td>5489</td>
      <td>6176</td>
      <td>1422</td>
      <td>952</td>
      <td>272</td>
      <td>...</td>
      <td>116.5</td>
      <td>1603</td>
      <td>0.370</td>
      <td>4337</td>
      <td>0.162</td>
      <td>0.265</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>2022</td>
      <td>NYY</td>
      <td>29</td>
      <td>2342</td>
      <td>5422</td>
      <td>6172</td>
      <td>1308</td>
      <td>821</td>
      <td>225</td>
      <td>...</td>
      <td>119.8</td>
      <td>1705</td>
      <td>0.417</td>
      <td>4091</td>
      <td>0.177</td>
      <td>0.287</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>35.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28</td>
      <td>2022</td>
      <td>STL</td>
      <td>28</td>
      <td>2355</td>
      <td>5496</td>
      <td>6165</td>
      <td>1386</td>
      <td>878</td>
      <td>290</td>
      <td>...</td>
      <td>114.4</td>
      <td>1611</td>
      <td>0.373</td>
      <td>4322</td>
      <td>0.168</td>
      <td>0.272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>33.8</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 319 columns</p>
</div>



The Dodgers still have the top offense, but the number 2 and number 3 spots are now occupied by the Blue Jays and the Mets. I wanted to determine how much better the Dodgers were than all other teams.


```python
print(wrc.iloc[0]['wRC+'])
```

    119


Since the dodgers had a wRC+ of 119, they were 19% better than an average offense. Lastly, I wanted to confirm that the Dodgers were the best offensive team by looking at where they ranked in more traditional counting stats.


```python
newDF = wrc.set_index('Team')

#filter for traditonal stats
newDF = newDF[['AVG', 'OBP', 'SLG', 'RBI', 'R']]

#get ranks of statistics
newDF = newDF.rank(ascending = False)

#show df
newDF.head()
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
      <th>AVG</th>
      <th>OBP</th>
      <th>SLG</th>
      <th>RBI</th>
      <th>R</th>
    </tr>
    <tr>
      <th>Team</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LAD</th>
      <td>4.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>TOR</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>NYM</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>8.0</td>
      <td>6.0</td>
      <td>5.5</td>
    </tr>
    <tr>
      <th>NYY</th>
      <td>15.5</td>
      <td>4.5</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>STL</th>
      <td>10.0</td>
      <td>4.5</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>5.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Only consider teams in the top 10 of wRC+ to avoid cluttering visualization
top10 = newDF.head(10)

import seaborn as sns
#create a heatmap to visualize
ax = plt.subplots(figsize=(16, 12))
ax = sns.heatmap(top10, square = True, cmap = sns.light_palette("seagreen"))

```


    
![png](output_28_0.png)
    


Lighter colors in this plot indicate a higher rank. Because of this, we see that the only two teams in the top tier of each category are the Dodgers and Blue Jays. Because of this, and the edge the Dodgers have in wRC+, I feel confident in saying that the Dodgers had the overall best offense in MLB in 2022.

## Question 3 - Defensive Shifts
How do defensive shifts affect a team's defensive performance? To answer this
question, you should analyze the relationship between the number of shifts a team
employs and their defensive statistics, such as fielding percentage and defensive runs
saved. Is there a correlation between the number of shifts and defensive performance?
Does this vary by team or by position?


```python
from pybaseball import team_fielding
#grab fielding stats from 2022 season, will take about 5 mins to gather
fielding = team_fielding(2022)
fielding.head()
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
      <th>teamIDfg</th>
      <th>Season</th>
      <th>Team</th>
      <th>G</th>
      <th>GS</th>
      <th>Inn</th>
      <th>PO</th>
      <th>A</th>
      <th>E</th>
      <th>FE</th>
      <th>...</th>
      <th>60-90%</th>
      <th># 60-90%</th>
      <th>90-100%</th>
      <th># 90-100%</th>
      <th>rSZ</th>
      <th>rCERA</th>
      <th>rTS</th>
      <th>FRM</th>
      <th>OAA</th>
      <th>RAA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9</td>
      <td>2022</td>
      <td>Yankees</td>
      <td>2179</td>
      <td>1458</td>
      <td>13065.0</td>
      <td>4355</td>
      <td>1473</td>
      <td>74</td>
      <td>43</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20</td>
      <td>1</td>
      <td>25</td>
      <td>24.5</td>
      <td>21</td>
      <td>16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15</td>
      <td>2022</td>
      <td>Diamondbacks</td>
      <td>2208</td>
      <td>1458</td>
      <td>12870.0</td>
      <td>4290</td>
      <td>1370</td>
      <td>86</td>
      <td>46</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-15</td>
      <td>3</td>
      <td>33</td>
      <td>-6.4</td>
      <td>44</td>
      <td>38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>2022</td>
      <td>Cardinals</td>
      <td>2192</td>
      <td>1458</td>
      <td>12921.0</td>
      <td>4307</td>
      <td>1701</td>
      <td>66</td>
      <td>30</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-8</td>
      <td>3</td>
      <td>11</td>
      <td>-3.4</td>
      <td>24</td>
      <td>18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>2022</td>
      <td>Astros</td>
      <td>2104</td>
      <td>1458</td>
      <td>13008.0</td>
      <td>4336</td>
      <td>1290</td>
      <td>72</td>
      <td>36</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-1</td>
      <td>0</td>
      <td>44</td>
      <td>-2.9</td>
      <td>31</td>
      <td>26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2022</td>
      <td>Guardians</td>
      <td>2134</td>
      <td>1458</td>
      <td>13104.0</td>
      <td>4368</td>
      <td>1444</td>
      <td>97</td>
      <td>41</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3</td>
      <td>2</td>
      <td>22</td>
      <td>3.7</td>
      <td>19</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 61 columns</p>
</div>



I couldn't find how to quickly get statcast shift data from pybaseball, so I grabbed statcast shift frequencies by myself.


```python
#gather statcast data
statcast2022 = pd.read_csv('/Users/richarddiamond/Desktop/personalProjects/statcast2022.csv')

#merge dataframes
fielding = pd.merge(fielding, statcast2022, on = 'Team')
```

Now, let's take a look at how traditional fielding stats correlate with statcast shift percentage.


```python
#plot new data
plt.scatter(fielding['%'], fielding['FP'], color = 'gray')
plt.title('Shift% vs. Fielding%')
plt.xlabel('Shift%')
plt.ylabel('Fielding%')

#create equation for trendline
z = np.polyfit(fielding['%'], fielding['FP'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(fielding['%'], p(fielding['%']))

#find r^2
cc = np.corrcoef(fielding['%'], fielding['FP'])
print('Correlation Coefficient -', round(cc[0][1], 2))
```

    Correlation Coefficient - 0.18



    
![png](output_35_1.png)
    


This relationship appears to be very weak, and the data seems to have a low r-squared as well. Now, let's take a look at how other traditional stats are affected by shift percentage.


```python
#plot new data
plt.scatter(fielding['%'], fielding['DRS'], color = 'orange')
plt.title('Shift% vs. DRS')
plt.xlabel('Shift%')
plt.ylabel('DRS')

#create equation for trendline
z = np.polyfit(fielding['%'], fielding['DRS'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(fielding['%'], p(fielding['%']))

#find r^2
cc = np.corrcoef(fielding['%'], fielding['DRS'])
print('Correlation Coefficient -', round(cc[0][1], 2))
```

    Correlation Coefficient - 0.15



    
![png](output_37_1.png)
    


Perhaps we can say there exists some polynomial fit here, but overall, the relationship looks weak once again. I also wanted to take a look at a statcast metric, Outs Abover Average.


```python
#plot new data
plt.scatter(fielding['%'], fielding['OAA'], color = 'Purple')
plt.title('Shift% vs. OAA')
plt.xlabel('Shift%')
plt.ylabel('DRS')

#create equation for trendline
z = np.polyfit(fielding['%'], fielding['OAA'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(fielding['%'], p(fielding['%']))

#find r^2
cc = np.corrcoef(fielding['%'], fielding['OAA'])
print('Correlation Coefficient -', round(cc[0][1], 2))
```

    Correlation Coefficient - 0.03



    
![png](output_39_1.png)
    


Once again, no relationship here. Lastly, I wanted to take a look at UZR to see if any important defensive metric has a relationship with shift %.


```python
#plot new data
plt.scatter(fielding['%'], fielding['UZR'], color = 'black')
plt.title('Shift% vs. UZR')
plt.xlabel('Shift%')
plt.ylabel('DRS')

#create equation for trendline
z = np.polyfit(fielding['%'], fielding['UZR'] , 1)
p = np.poly1d(z)

#plot trendline
plt.plot(fielding['%'], p(fielding['%']))

#find r^2
cc = np.corrcoef(fielding['%'], fielding['UZR'])
print('Correlation Coefficient -', round(cc[0][1], 2))
```

    Correlation Coefficient - -0.36



    
![png](output_41_1.png)
    


This is the strongest relationship we've observed, and it is an inverse one. This could be because UZR takes into account the 'difficulty' of a play based on how far a defender ranges. The shift obviously changes defensive positioning and thus makes certain plays that would be easy with a normal defensive alignment difficult. Because of this, teams that shift often are likely to concede hits to vacated holes in the infield that appear to be easy plays, thus hurting team UZR.

## References
Thanks to pybaseball and Baseball Savant for the data used for this notebook.
