# My Projects

Here are some of the projects I've worked on:

## NBA Statistics Data Analysis

Brief intro

Imports
```python
import pandas as pd
import requests # data scripting
pd.set_option('display.max_columns', None) # so we can see all columns in a wide DataFrame
import time
import numpy as np
```

Request url api call
```python
test_url = 'https://stats.nba.com/stats/leagueLeaders?LeagueID=00&PerMode=Totals&Scope=S&Season=2023-24&SeasonType=Regular%20Season&StatCategory=PTS'
r = requests.get(url=test_url).json()
table_headers = r['resultSet']['headers']
```
Example of headers
```python
print(table_headers)
```
Which outputs:

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

Add in columns and show all columns using pandas library
```python
df_cols = ['Year', 'Season_type'] + table_headers
pd.DataFrame(columns=df_cols)
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

## Other Projects
