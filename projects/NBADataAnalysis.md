- [Home](README.md)
- [Projects](projects.md)
- [About](about.md)

## NBA Statistics Data Analysis

#### [Project Link](../notebooks/NBADataAnalysis.ipynb)
---
In the first part of this project, the data from [nba.com](https://www.nba.com) was scraped for the years from 2014-2023 for the regular season and playoffs. In this part, I will analyze the data to show some pretty cool trends over the years, statistic changes between regular season and playoffs, among other things.

#### [Link to Part 1](NBAStatsScraping.md)

---

### Imports and Initialization
```python
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
pd.set_option('display.max_columns', None)

data = pd.read_excel('nba_player_data.xlsx')
```
Here, [pandas](https://pandas.pydata.org/) as ```pd```, [plotly.express](https://plotly.com/python/plotly-express/) as ```px```, and [plotly.graph_objects](https://plotly.com/python/graph-objects/) as ```go``` are our imported libraries. We also need to configure the display settings to show all columns of a DataFrame. Finally, and most importantly, the data is gathered using pandas ```read_excel``` function.

---

### Sample of the Data
```python
data.sample(10)
```

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
      <th>6709</th>
      <td>2023-24</td>
      <td>Regular%20Season</td>
      <td>1627759</td>
      <td>22</td>
      <td>Jaylen Brown</td>
      <td>1610612738</td>
      <td>BOS</td>
      <td>70</td>
      <td>2343</td>
      <td>627</td>
      <td>1256</td>
      <td>0.499</td>
      <td>145</td>
      <td>410</td>
      <td>0.354</td>
      <td>211</td>
      <td>300</td>
      <td>0.703</td>
      <td>84</td>
      <td>303</td>
      <td>387</td>
      <td>249</td>
      <td>83</td>
      <td>37</td>
      <td>166</td>
      <td>185</td>
      <td>1610</td>
      <td>1482</td>
      <td>1.50</td>
      <td>0.50</td>
    </tr>
    <tr>
      <th>5831</th>
      <td>2021-22</td>
      <td>Playoffs</td>
      <td>1629651</td>
      <td>116</td>
      <td>Nic Claxton</td>
      <td>1610612751</td>
      <td>BKN</td>
      <td>4</td>
      <td>98</td>
      <td>19</td>
      <td>24</td>
      <td>0.792</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>4</td>
      <td>22</td>
      <td>0.182</td>
      <td>7</td>
      <td>18</td>
      <td>25</td>
      <td>6</td>
      <td>5</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>42</td>
      <td>62</td>
      <td>3.00</td>
      <td>2.50</td>
    </tr>
    <tr>
      <th>823</th>
      <td>2015-16</td>
      <td>Regular%20Season</td>
      <td>202685</td>
      <td>124</td>
      <td>Jonas Valančiūnas</td>
      <td>1610612761</td>
      <td>TOR</td>
      <td>60</td>
      <td>1557</td>
      <td>303</td>
      <td>536</td>
      <td>0.565</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>162</td>
      <td>213</td>
      <td>0.761</td>
      <td>184</td>
      <td>363</td>
      <td>547</td>
      <td>42</td>
      <td>25</td>
      <td>80</td>
      <td>85</td>
      <td>158</td>
      <td>768</td>
      <td>1093</td>
      <td>0.49</td>
      <td>0.29</td>
    </tr>
    <tr>
      <th>4288</th>
      <td>2019-20</td>
      <td>Playoffs</td>
      <td>203118</td>
      <td>174</td>
      <td>Mike Scott</td>
      <td>1610612755</td>
      <td>PHI</td>
      <td>4</td>
      <td>20</td>
      <td>1</td>
      <td>5</td>
      <td>0.200</td>
      <td>0</td>
      <td>2</td>
      <td>0.000</td>
      <td>4</td>
      <td>4</td>
      <td>1.000</td>
      <td>2</td>
      <td>6</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>11</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>5156</th>
      <td>2021-22</td>
      <td>Regular%20Season</td>
      <td>202699</td>
      <td>48</td>
      <td>Tobias Harris</td>
      <td>1610612755</td>
      <td>PHI</td>
      <td>73</td>
      <td>2543</td>
      <td>493</td>
      <td>1022</td>
      <td>0.482</td>
      <td>101</td>
      <td>275</td>
      <td>0.367</td>
      <td>170</td>
      <td>202</td>
      <td>0.842</td>
      <td>77</td>
      <td>419</td>
      <td>496</td>
      <td>252</td>
      <td>47</td>
      <td>43</td>
      <td>116</td>
      <td>164</td>
      <td>1257</td>
      <td>1418</td>
      <td>2.17</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>4071</th>
      <td>2019-20</td>
      <td>Regular%20Season</td>
      <td>1629648</td>
      <td>486</td>
      <td>Jordan Bone</td>
      <td>1610612765</td>
      <td>DET</td>
      <td>10</td>
      <td>53</td>
      <td>5</td>
      <td>20</td>
      <td>0.250</td>
      <td>2</td>
      <td>10</td>
      <td>0.200</td>
      <td>0</td>
      <td>0</td>
      <td>0.000</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>12</td>
      <td>8</td>
      <td>4.00</td>
      <td>0.50</td>
    </tr>
    <tr>
      <th>4213</th>
      <td>2019-20</td>
      <td>Playoffs</td>
      <td>203085</td>
      <td>101</td>
      <td>Austin Rivers</td>
      <td>1610612745</td>
      <td>HOU</td>
      <td>12</td>
      <td>211</td>
      <td>19</td>
      <td>61</td>
      <td>0.311</td>
      <td>9</td>
      <td>35</td>
      <td>0.257</td>
      <td>10</td>
      <td>13</td>
      <td>0.769</td>
      <td>2</td>
      <td>28</td>
      <td>30</td>
      <td>16</td>
      <td>7</td>
      <td>1</td>
      <td>8</td>
      <td>22</td>
      <td>57</td>
      <td>58</td>
      <td>2.00</td>
      <td>0.88</td>
    </tr>
    <tr>
      <th>2133</th>
      <td>2017-18</td>
      <td>Regular%20Season</td>
      <td>201569</td>
      <td>42</td>
      <td>Eric Gordon</td>
      <td>1610612745</td>
      <td>HOU</td>
      <td>69</td>
      <td>2154</td>
      <td>415</td>
      <td>970</td>
      <td>0.428</td>
      <td>218</td>
      <td>608</td>
      <td>0.359</td>
      <td>195</td>
      <td>241</td>
      <td>0.809</td>
      <td>25</td>
      <td>145</td>
      <td>170</td>
      <td>154</td>
      <td>44</td>
      <td>27</td>
      <td>130</td>
      <td>116</td>
      <td>1243</td>
      <td>907</td>
      <td>1.19</td>
      <td>0.34</td>
    </tr>
    <tr>
      <th>4310</th>
      <td>2019-20</td>
      <td>Playoffs</td>
      <td>201960</td>
      <td>196</td>
      <td>DeMarre Carroll</td>
      <td>1610612745</td>
      <td>HOU</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>2</td>
      <td>0.500</td>
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
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>0.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>5366</th>
      <td>2021-22</td>
      <td>Regular%20Season</td>
      <td>202397</td>
      <td>258</td>
      <td>Ish Smith</td>
      <td>1610612764</td>
      <td>WAS</td>
      <td>65</td>
      <td>1126</td>
      <td>185</td>
      <td>430</td>
      <td>0.430</td>
      <td>25</td>
      <td>67</td>
      <td>0.373</td>
      <td>15</td>
      <td>24</td>
      <td>0.625</td>
      <td>25</td>
      <td>115</td>
      <td>140</td>
      <td>244</td>
      <td>47</td>
      <td>23</td>
      <td>78</td>
      <td>78</td>
      <td>410</td>
      <td>532</td>
      <td>3.13</td>
      <td>0.60</td>
    </tr>
  </tbody>
</table>
</div>

---

### Data Cleaning & Analysis Preparation
#### The following tasks clean up the data and get rid of any imperfections:

```python
data.drop(columns=['RANK','EFF'], inplace=True)
```
These statistics are irrelevant to what I will be doing. ```'RANK'``` is to determine the player's rank in the specific statistic that you search on [nba.com](https://www.nba.com). ```'EFF'``` is a statistic that is calculated using other statistics with the formula: ```EFF = (PTS + REB + AST + STL + BLK − Missed FG − Missed FT - TO) / GP```.

```python
data['season_start_year'] = data['Year'].str[:4].astype(int)
```
We add in a new header ```'season_start_year'``` that changes the ```'Year'``` from format "2015-16" to "2015" by taking the first four characters of the string.

```python
data['Season_type'].replace('Regular%20Season', 'RS', inplace=True)
```
We change the header ```'Season_type'``` if it is labeled ```'Regular%20Season'``` (as seen in the sample of data above), we want to change it to ```'RS'```.

```python
rs_df = data[data['Season_type']=='RS']
playoffs_df = data[data['Season_type']=='Playoffs']
```
Here, we create two different dataFrames, ```rs_df``` for data during the regular season, and ```playoffs_df``` for data during the playoffs.

```python
data.columns
```
Finally, we can see all of our columns/headers:
    
    Index(['Year', 'Season_type', 'PLAYER_ID', 'PLAYER', 'TEAM_ID', 'TEAM', 'GP',
           'MIN', 'FGM', 'FGA', 'FG_PCT', 'FG3M', 'FG3A', 'FG3_PCT', 'FTM', 'FTA',
           'FT_PCT', 'OREB', 'DREB', 'REB', 'AST', 'STL', 'BLK', 'TOV', 'PF',
           'PTS', 'AST_TOV', 'STL_TOV', 'season_start_year'],
          dtype='object')

---

### Which Player Stats are Correlated with Each Other?

```python
total_cols = ['MIN', 'FGM', 'FGA', 'FG3M', 'FG3A', 'FTM', 'FTA',
              'OREB', 'DREB', 'REB', 'AST', 'STL', 'BLK', 'TOV', 'PF', 'PTS']
```

To start off, we want to limit the columns/headers we are looking at.

```python
data_per_min = data.groupby(['PLAYER', 'PLAYER_ID', 'Year'])[total_cols].sum().reset_index()
for col in data_per_min.columns[4:]:
    data_per_min[col] = data_per_min[col]/data_per_min['MIN']

data_per_min['FG%'] = data_per_min['FGM']/data_per_min['FGA']
data_per_min['3PT%'] = data_per_min['FG3M']/data_per_min['FG3A']
data_per_min['FT%'] = data_per_min['FTM']/data_per_min['FTA']
data_per_min['FG3A%'] = data_per_min['FG3A']/data_per_min['FGA']
data_per_min['PTS/FGA'] = data_per_min['PTS']/data_per_min['FGA']
data_per_min['FG3m/FGM'] = data_per_min['FG3M']/data_per_min['FGM']
data_per_min['FTA/FGA'] = data_per_min['FTA']/data_per_min['FGA']
data_per_min['TRU%'] = 0.5*data_per_min['PTS']/(data_per_min['FGA']+0.475*data_per_min['FTA'])
data_per_min['AST_TOV'] = data_per_min['AST']/data_per_min['TOV']

data_per_min = data_per_min[data_per_min['MIN']>=50]
data_per_min.drop(columns='PLAYER_ID', inplace=True)

fig = px.imshow(data_per_min.corr())

fig.show()
```  
  In this cell, I process NBA player data to calculate a set of per-minute statistics, shooting efficiencies, and performance ratios. Also, it filters out players with low playing time, and then creates a correlation heatmap to show relationships between these metrics.  
  First, we want to group the data by ```'PLAYER', 'PLAYER_ID', 'Year'```. It is important that ```'PLAYER_ID'``` is included in case there is two players with the same name.  
  Then, we get the sum of all player data based on the columns in ```total_cols```. We then iterate through the ```data_per_min``` columns (excluding the first 4 columns which are unnecessary: ```PLAYER, PLAYER_ID, Year, MIN```), and average all the stats based on a "per-minute" basis.  
  We then create all new headers ```'FG%', '3PT%', 'FT%', 'FG3A%', 'PTS/FGA', 'FG3m/FGM', 'FTA/FGA', 'TRU%', 'AST_TOV'```. Most of these are straightforward, but I want to discuss ```'TRU%'```. True shooting percentage is calculated: Points / (2 * (Field Goals Attempted + 0.44 * Free Throws Attempted)). It is a more accurate way to calculate a player's shooting than looking at field goal percentage, free throw percentage, and three-point field goal percentage separately.
  It is crucial that we don't have any outliers. For example, we don't want to include a player who scored 3 points, but only played 1 minute. This would skew our data, so we ensure that the players played at least 50 minutes. We can also drop the ```'PLAYER_ID'``` column to simplify the final display.  
  Finally, we print out a correlation matrix, demonstrated with a heatchart:
![alt text](../assets/img/NBADataAnalysis_11_1.png?raw=true)
For a little more information, the higher the number on the heatchart, the higher the correlation. When the number on the heatchart is positive, there is a positive correlation between the two statistics, and vice versa, when the number on the heatchart is negative, there is a negative correlation between the two statistics. Analyzing this chart, we see that the diagonal from the top left to the bottom right is all 1, because it is correlating a statistic to itself. I want to look at a couple of specific statistic correlations that I find interesting.

#### PTS/Min vs. FGM/Min
Let's start with an easy one to wrap our head around this heatchart.
![alt text](../assets/img/PTS_FGM.png?raw=true)
  This heatchart shows the correlation between points per minute and field goals made per minute. The correlation, represented by ```color``` is 0.958389. This number shows how positive a correlation these two statistics have. When we take a step back and think about it, it makes sense that these stats are positively correlated. In order for a player to score a lot of points, he can get points in three ways: 2-point field goals, 3-point field goals, and free throws, which means that 2 out of the 3 ways a player can score is by making field goals (FGM).  
  We can find the coefficient of determination to quantify how much one stat can be explained by a relationship with another stat. The formula is ```r^2```where ```r``` is out correlation coefficient: 0.958389. We find ```r^2``` to be roughly 0.9185. This means that 91.85% of the variation in points scored per minute can be explained by the number of field goals made per minute.  

#### REB/Min vs. AST/Min
Now, let's take a look at two statistics that are not generally associated with the same player: rebounds and assists.
![alt_text](../assets/img/REB_AST.png?raw=true)
For those who aren't as familiar with the game of basketball, an assist is given to a player when the pass they made to another player results in a made field goal (within 3 dribbles of the pass). A rebound is tracked for a player when they grab the ball after a missed field goal from their team or the opposing team. Typically, guards are the facilitators of the offense, meaning they get more assists, while centers and power forwards are the anchors of the team, and stand closer to the basket, resulting in more rebounds. Guards are typically shorter players, with centers and power forwards typically being the tallest players on the court. With these two statistics being at opposite spectrums of the game, it would make sense that the correlation between the two would be extremely negative. While the correlation is still negative, it is not as strong of a negative relationship as we would've presumed: -0.2855277.  
  We find the coefficient of determination to be ```-0.2855277^2``` which is roughly 0.0815, which means that 8.15% of the variation in assists per minute can be explained by the number of rebounds per minute.  
  This correlation is extremely interesting because it shows how the game has changed over the years. In prior years to this dataset, a majority of players who recorded high assist numbers never recorded high rebound numbers. They were seen as two completely different sides of the game that relied heavily on player height. However, in recent years, this belief of the game has fallen off. Players like LeBron James, Russell Westbrook, James Harden, and Nikola Jokic have popularized doing it all: recording above average rebound numbers, while also recording above average assist numbers.  
  This correlation was not as negative as I originally predicted because of the rise of "positionless" basketball. The idea is that there is no reason that a player should be limited to a position based on their height, but it more focuses on a player's individual skillset. If a player is extremely tall, but they are a great passer, why should you feel the need as a coach to restrict them to being a center, and not handle the ball and run the offense if that's one of their strengths? If we were to look at this exact statistic for the years from 2004-2013 (the previous decade to this dataset), I can guarantee the correlation would be closer to -1 than for this decade.

#### TRU%/Min vs. PTS/FGA/Min
Here is another easier one to comprehend: true shooting percentage per minute vs. points per field goal attempt per minute
![alt text](../assets/img/TRU_PTSFGA.png?raw=true)
True shooting percentage, defined above is an advanced statistic that measures a player's efficiency. The formula is: True shooting = Points / (2 * (Field Goals Attempted + 0.44 * Free Throws Attempted)). Points per field goal attempt is calculated simply by dividing points from field goal attempts. We see the correlation between the two statistics is extremely positive at 0.8985978. This makes sense since points is in the numerator of the true shooting percentage formula, and field goal attempts are in the denominator. So when we get more points than field goal attempts, the ```PTS/FGA``` statistic increases, and the ```TRU%``` statistic would increase since the numerator is increasing, and the denominator is decreasing.  
With a correlation of 0.8985978 between true shooting percentage and points per field goal attempt, we find the coefficient of determination to be ```0.8985978^2``` which is roughly 0.8075. This means that 80.75% of the variation in true shooting percentage per minute can be explained by points per field goal attempt per minute.

#### PF/Min vs. BLK/Min
Another correlation between statistics that interests me and I want to take a look at is personal fouls per minute and blocks per minute.
![alt text](../assets/img/PF_BLK.png?raw=true)
A personal foul is a violation of the NBA rules that happens when a player makes illegal contact with an opponent. There are many different types of personal fouls, like charging fouls, off-ball fouls, reaching foul, blocking foul, etc. Majority of personal fouls are committed by centers and power forwards, which are normally the tallest players on the court at any given time. Personal fouls are committed more by these players since they are typically closer to the basket, where majority of personal fouls are committed, sometimes during a field goal attempt. Another huge part in defensive schemes in basketball is a "block". A block is when a player swats the ball out of the air when their opponent shoots the ball. Blocks are associated with taller players, typically centers and power forwards, since they are closer to the basket, on average, than any other position in basketball.  
  We expect this relationship to be positive, and we can see that we are correct, with a correlation of 0.420338. By squaring the correlation of 0.420338, we find the coefficient of determination to be 0.1767, meaning that 17.67% of the variation in personal fouls per minute can be explained by blocks per minute.
  
#### FG3A/Min vs. FG%/Min
Finally, I want to take a lok at the correlation between 3-point field goals attempted per minute and field goal percentage per minute
![alt text](../assets/img/FG3A_FGP.png?raw=true)
3-point field goals have been extremely popular in the NBA recently. With the rise of 3-point connoisseur Stephen Curry, this decade shows the rise of the love for the 3-point line. 3-point field goal attempts per game have increased over the last decade. While 3-point shots do reward more points than a regular field goal shot (2 points), they are further away, and therefore much more difficult shots. Before true shooting percentage, field goal percentage was the best way to quantify a player's efficiency. This statistic is simply field goals made divided by field goal attempts.  
  We find that the correlation between field goal 3-point attempts per minute and field goal percentage per minute is -0.5246738. When we square this correlation, we find the coefficient of determination to be roughly 0.2753, meaning that 27.53% of the variation in field goal 3-point attempts per minute can be explained by field goal percentage per minute.  
  The correlation between the two is negative, which makes sense. 3-point shots are a great deal more difficult than regular field goals, so when a player attempts more 3-pointers, they will miss at a greater rate than when shooting regular field goals, resulting in a lower field goal percentage. 
  
### How are Minutes Distributed?
Next, let's take a look at the percent of players that have played a certain amount of minutes in the playoffs in a particular year. We construct a histogram with 20-minute bars. The percent of players is on the y-axis and the total minutes played during a playoffs run is on the x-axis:  
![alt text](../assets/img/NBADataAnalysis_13_0.png?raw=true)


You can see that majority of players haven't eclipsed more than 200 points in a playoffs run, but there are a lot of variables that could impact that. For example, the NBA playoffs is structured in a 7-game series method. 16 teams make the playoffs, so a team can at most play 4 series, meaning a maximum of 28 games. Some players could be on bad teams that only play 1 series, and possibly only 4 games. There is one player who topped everyone else, and it was Jayson Tatum in 2021, when he played 983 minutes. In that playoffs run, he led his Boston Celtics to the championship, where they lost to the Golden State Warriors. He averaged 41.0 minutes per game and played 24 games. 

The difference between the regular season and playoffs is obvious. Playoff basketball is simply more competitive for a reason: it's the playoffs. Every offensive possession becomes harder to score because of the intensified defense for the playoffs, however that's not the only difference between regular season and playoff basketball: the number of minutes played by players also changes. In the playoffs, star players tend to play mroe minutes because their team needs to put the best players on the court. Meanwhile, some players play less minutes (or none at all) because their team's star players are playing more. Here is a histogram showing the difference between regular season and playoff minutes.
![alt text] (..assets/img/playoff_vs_rs_mins.png?raw=true)
