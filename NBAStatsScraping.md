- [Home](README.md)
- [Projects](projects.md)
- [About](./about.md)

## NBA Statistics Data Scraping

#### [Project Link](../notebooks/NBAStatsScraping.ipynb)
---
This part of the project is to simply scrape the data from [nba.com](https://www.nba.com) for all player stats from 2014-2023 in the regular season and the playoffs. In the next part of this project, I will analyze this data in this part.
#### [Link to Part 2](NBADataAnalysis.md)

---

### Imports
```python
import pandas as pd
import requests # data scripting
pd.set_option('display.max_columns', None) # so we can see all columns in a wide DataFrame
import time
import numpy as np
```

To start off any good project, it is important to import necessary libraries. This project imports [pandas](https://pandas.pydata.org/), [requests](https://pypi.org/project/requests/), [time](https://docs.python.org/3/library/time.html), and [numpy](https://numpy.org/) libraries. For more information, feel free to click their names to be redirected to their respective websites.

---

### Fetching NBA Data via API
```python
test_url = 'https://stats.nba.com/stats/leagueLeaders?LeagueID=00&PerMode=Totals&Scope=S&Season=2023-24&SeasonType=Regular%20Season&StatCategory=PTS'
r = requests.get(url=test_url).json()
table_headers = r['resultSet']['headers']
print(table_headers)
```

Now, I need to get access to the data via an API URL. An API URL is an address that allows others to access data within an API (Application Programming Interface), which allows software programs to communicate and share data. Once we find that URL, we send a GET request to the URL and parses the JSON response. We then extract the headers from ```r```, and print them to the screen.

Output:

    ['PLAYER_ID',
     'RANK',
     'PLAYER',
     'TEAM_ID',
     'TEAM',
     'GP',
     'MIN',
     'FGM',
     'FGA',
     'FG_PCT',
     'FG3M',
     'FG3A',
     'FG3_PCT',
     'FTM',
     'FTA',
     'FT_PCT',
     'OREB',
     'DREB',
     'REB',
     'AST',
     'STL',
     'BLK',
     'TOV',
     'PF',
     'PTS',
     'EFF',
     'AST_TOV',
     'STL_TOV']

---

### Adding Extra Columns
```python
df_cols = ['Year', 'Season_type'] + table_headers
pd.DataFrame(columns=df_cols)
```

Add in the headers so that we can determine the ```year``` and ```season_type``` (regular season or playoffs). Using pandas neat ```DataFrame``` data structure, let's see what our headers will look like:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Season_type</th>
      <th>PLAYER_ID</th>
      <th>RANK</th>
      <th>PLAYER</th>
      <th>TEAM_ID</th>
      <th>TEAM</th>
      <th>GP</th>
      <th>MIN</th>
      <th>FGM</th>
      <th>FGA</th>
      <th>FG_PCT</th>
      <th>FG3M</th>
      <th>FG3A</th>
      <th>FG3_PCT</th>
      <th>FTM</th>
      <th>FTA</th>
      <th>FT_PCT</th>
      <th>OREB</th>
      <th>DREB</th>
      <th>REB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>EFF</th>
      <th>AST_TOV</th>
      <th>STL_TOV</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>

---

### Get API Request Headers
```python
headers = {
    'accept': '*/*',
    'accept-encoding': 'gzip, deflate, br, zstd',
    'accept-language': 'en-US,en;q=0.9',
    'connection': 'keep-alive',
    'host': 'stats.nba.com',
    'origin': 'https://www.nba.com',
    'referer': 'https://www.nba.com/',
    'sec-ch-ua':
    '"Google Chrome";v="129", "Not=A?Brand";v="8", "Chromium";v="129"',
    'sec-ch-ua-mobile': '?1',
    'sec-ch-ua-platform': '"Android"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-site',
    'user-agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/129.0.0.0 Mobile Safari/537.36'
}
```
This dictionary looks extremely confusing, but you don't need to know all of this information to complete this task. This is called a request/response header. It provides additional info to the server when making the API call. Once you locate the API call on [NBA.com](https://www.nba.com), find the request/response header section, and merely copy it into python and format correctly into a dictionary.

---

### Gathering the Data
```python
df = pd.DataFrame(columns=df_cols)
season_types = ['Regular%20Season', 'Playoffs']
years = ['2014-15', '2015-16', '2016-17', '2017-18', '2018-19', '2019-20', '2020-21', '2021-22', '2022-23', '2023-24']

begin_loop = time.time()

for y in years:
    for s in season_types:
        api_url = 'https://stats.nba.com/stats/leagueLeaders?LeagueID=00&PerMode=Totals&Scope=S&Season='+y+'&SeasonType='+s+'&StatCategory=PTS'
        r = requests.get(url=api_url, headers=headers).json()
        temp_df1 = pd.DataFrame(r['resultSet']['rowSet'], columns=table_headers)
        temp_df2 = pd.DataFrame({'Year':[y for i in range(len(temp_df1))],
                                 'Season_type':[s for i in range(len(temp_df1))]})
        temp_df3 = pd.concat([temp_df2,temp_df1], axis=1)
        df = pd.concat([df, temp_df3], axis=0)
        print(f'Finished scraping data for the {y} {s}.')
        lag = np.random.uniform(low=5,high=20)
        print(f'...waiting {round(lag,1)} seconds')
        time.sleep(lag)
        
print(f'Process completed! Total run time: {round((time.time()-begin_loop)/60,2)}')
df.to_excel('nba_player_data.xlsx', index=False)
```
This is the main part of the project: where I actually gather all the data that was pulled via API requests. Initially, I need to define the season_types and years that should be included in the data with lists ```season_types, years```. First, manipulate the ```api_url``` to populate with data from year ```y``` and season_type ```s```. Then request the information, and put said information into a DataFrame ```temp_df1```. It's important that our year and season_type as columns in our DataFrame, so they get added into ```temp_df2``` and then, concatenate both DataFrames into ```temp_df3```. Once we get this information, finally add it to our main DataFrame ```df``` and continue to add to ```df``` for all iterations. There is also a time element that plays into account. API requests are sometimes done by bots, so to combat that, companies have bot detectors in order to limit other's access to their data and check for bots. To not appear "botty", a ```lag``` is introduced to wait to make the next request. I also print out information as reassurance during iterations to ensure we have gained successful access to the data, as well as a timestamp to show how long the process took:

    Finished scraping data for the 2014-15 Regular%20Season.
    ...waiting 12.8 seconds
    Finished scraping data for the 2014-15 Playoffs.
    ...waiting 9.9 seconds
    Finished scraping data for the 2015-16 Regular%20Season.
    ...waiting 10.7 seconds
    Finished scraping data for the 2015-16 Playoffs.
    ...waiting 5.3 seconds
    Finished scraping data for the 2016-17 Regular%20Season.
    ...waiting 18.7 seconds
    Finished scraping data for the 2016-17 Playoffs.
    ...waiting 10.4 seconds
    Finished scraping data for the 2017-18 Regular%20Season.
    ...waiting 14.2 seconds
    Finished scraping data for the 2017-18 Playoffs.
    ...waiting 6.2 seconds
    Finished scraping data for the 2018-19 Regular%20Season.
    ...waiting 13.8 seconds
    Finished scraping data for the 2018-19 Playoffs.
    ...waiting 11.4 seconds
    Finished scraping data for the 2019-20 Regular%20Season.
    ...waiting 17.1 seconds
    Finished scraping data for the 2019-20 Playoffs.
    ...waiting 18.6 seconds
    Finished scraping data for the 2020-21 Regular%20Season.
    ...waiting 16.1 seconds
    Finished scraping data for the 2020-21 Playoffs.
    ...waiting 14.2 seconds
    Finished scraping data for the 2021-22 Regular%20Season.
    ...waiting 12.5 seconds
    Finished scraping data for the 2021-22 Playoffs.
    ...waiting 11.1 seconds
    Finished scraping data for the 2022-23 Regular%20Season.
    ...waiting 11.5 seconds
    Finished scraping data for the 2022-23 Playoffs.
    ...waiting 12.1 seconds
    Finished scraping data for the 2023-24 Regular%20Season.
    ...waiting 13.8 seconds
    Finished scraping data for the 2023-24 Playoffs.
    ...waiting 6.6 seconds
    Process completed! Total run time: 4.39

---

### Final Product: Example of ```df``` DataFrame

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Season_type</th>
      <th>PLAYER_ID</th>
      <th>RANK</th>
      <th>PLAYER</th>
      <th>TEAM_ID</th>
      <th>TEAM</th>
      <th>GP</th>
      <th>MIN</th>
      <th>FGM</th>
      <th>FGA</th>
      <th>FG_PCT</th>
      <th>FG3M</th>
      <th>FG3A</th>
      <th>FG3_PCT</th>
      <th>FTM</th>
      <th>FTA</th>
      <th>FT_PCT</th>
      <th>OREB</th>
      <th>DREB</th>
      <th>REB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>EFF</th>
      <th>AST_TOV</th>
      <th>STL_TOV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-15</td>
      <td>Regular%20Season</td>
      <td>201935</td>
      <td>1</td>
      <td>James Harden</td>
      <td>1610612745</td>
      <td>HOU</td>
      <td>81</td>
      <td>2981</td>
      <td>647</td>
      <td>1470</td>
      <td>0.440</td>
      <td>208</td>
      <td>555</td>
      <td>0.375</td>
      <td>715</td>
      <td>824</td>
      <td>0.868</td>
      <td>75</td>
      <td>384</td>
      <td>459</td>
      <td>565</td>
      <td>154</td>
      <td>60</td>
      <td>321</td>
      <td>208</td>
      <td>2217</td>
      <td>2202</td>
      <td>1.76</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-15</td>
      <td>Regular%20Season</td>
      <td>201939</td>
      <td>2</td>
      <td>Stephen Curry</td>
      <td>1610612744</td>
      <td>GSW</td>
      <td>80</td>
      <td>2613</td>
      <td>653</td>
      <td>1341</td>
      <td>0.487</td>
      <td>286</td>
      <td>646</td>
      <td>0.443</td>
      <td>308</td>
      <td>337</td>
      <td>0.914</td>
      <td>56</td>
      <td>285</td>
      <td>341</td>
      <td>619</td>
      <td>163</td>
      <td>16</td>
      <td>249</td>
      <td>158</td>
      <td>1900</td>
      <td>2073</td>
      <td>2.49</td>
      <td>0.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-15</td>
      <td>Regular%20Season</td>
      <td>201566</td>
      <td>3</td>
      <td>Russell Westbrook</td>
      <td>1610612760</td>
      <td>OKC</td>
      <td>67</td>
      <td>2302</td>
      <td>627</td>
      <td>1471</td>
      <td>0.426</td>
      <td>86</td>
      <td>288</td>
      <td>0.299</td>
      <td>546</td>
      <td>654</td>
      <td>0.835</td>
      <td>124</td>
      <td>364</td>
      <td>488</td>
      <td>574</td>
      <td>140</td>
      <td>14</td>
      <td>293</td>
      <td>184</td>
      <td>1886</td>
      <td>1857</td>
      <td>1.96</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2014-15</td>
      <td>Regular%20Season</td>
      <td>2544</td>
      <td>4</td>
      <td>LeBron James</td>
      <td>1610612739</td>
      <td>CLE</td>
      <td>69</td>
      <td>2493</td>
      <td>624</td>
      <td>1279</td>
      <td>0.488</td>
      <td>120</td>
      <td>339</td>
      <td>0.354</td>
      <td>375</td>
      <td>528</td>
      <td>0.710</td>
      <td>51</td>
      <td>365</td>
      <td>416</td>
      <td>511</td>
      <td>109</td>
      <td>49</td>
      <td>272</td>
      <td>135</td>
      <td>1743</td>
      <td>1748</td>
      <td>1.88</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-15</td>
      <td>Regular%20Season</td>
      <td>203081</td>
      <td>5</td>
      <td>Damian Lillard</td>
      <td>1610612757</td>
      <td>POR</td>
      <td>82</td>
      <td>2925</td>
      <td>590</td>
      <td>1360</td>
      <td>0.434</td>
      <td>196</td>
      <td>572</td>
      <td>0.343</td>
      <td>344</td>
      <td>398</td>
      <td>0.864</td>
      <td>49</td>
      <td>329</td>
      <td>378</td>
      <td>507</td>
      <td>97</td>
      <td>21</td>
      <td>222</td>
      <td>164</td>
      <td>1720</td>
      <td>1677</td>
      <td>2.28</td>
      <td>0.44</td>
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
      <th>209</th>
      <td>2023-24</td>
      <td>Playoffs</td>
      <td>1641765</td>
      <td>198</td>
      <td>Olivier-Maxence Prosper</td>
      <td>1610612742</td>
      <td>DAL</td>
      <td>3</td>
      <td>9</td>
      <td>0</td>
      <td>2</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>210</th>
      <td>2023-24</td>
      <td>Playoffs</td>
      <td>1631115</td>
      <td>198</td>
      <td>Orlando Robinson</td>
      <td>1610612748</td>
      <td>MIA</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>0.000</td>
      <td>0</td>
      <td>1</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>211</th>
      <td>2023-24</td>
      <td>Playoffs</td>
      <td>203933</td>
      <td>198</td>
      <td>T.J. Warren</td>
      <td>1610612750</td>
      <td>MIN</td>
      <td>3</td>
      <td>11</td>
      <td>0</td>
      <td>2</td>
      <td>0.000</td>
      <td>0</td>
      <td>1</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>212</th>
      <td>2023-24</td>
      <td>Playoffs</td>
      <td>201152</td>
      <td>198</td>
      <td>Thaddeus Young</td>
      <td>1610612756</td>
      <td>PHX</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>213</th>
      <td>2023-24</td>
      <td>Playoffs</td>
      <td>203648</td>
      <td>198</td>
      <td>Thanasis Antetokounmpo</td>
      <td>1610612749</td>
      <td>MIL</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
<p>7473 rows × 30 columns</p>
</div>


---
