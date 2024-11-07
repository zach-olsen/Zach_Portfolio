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

---

## Other Projects
