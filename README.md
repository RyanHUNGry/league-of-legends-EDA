# League of Legends Exploratory Data Analysis
### Creator: Ryan Hung
---
## Introduction:
### Question summary:
This exploratory data analysis employs **[Oracle Elixir's](https://oracleselixir.com/tools/downloads)** dataset consisting of player and team data from the 2022 League of Legends competitive season. 

The dragon is one of the most pivotal objectives in the game as it provides powerful buffs to the team that claims its souls. Thus, having solid macro and map control to ensure dragon kills is crucial for high level competitive play, where teams can use any sliver of advantage they can get. 

This exploratory data analysis focuses on how the value of dragons have shifted by split - is there a variation in dragon kills by split? It will also focus on analyzing the overall impact dragons have in a competitive match.

### Data summary:
The dataset consists of 149232 rows and 123 columns. The first 10 rows consists of player rows - 5 players from each team. The 11th and 12th columns consists of team rows. Thus, the first 12 rows make up the data for a match. The same pattern follows for the remaining rows.

Relevant columns that we address in our processing and analysis include:
1. `split` - The season split the match was played in
2. `side` - The side of map assigned to a team
3. `gamelength` - Length of game in minutes
4. `result` - The result of the match for a team
5. `firstdragon` - Whether or not a team secured the first dragon kill
6. `dragons` - The total number of dragons killed
7. `infernals`, `mountains`, `clouds`, `chemtechs`, `hextechs`, `elders` - The total number of dragons of each type killed
8. `date` - The date that the match was played on
9. `position` - The lane position of a player or simply team for team rows
10. `teamname` - The name of the team

---
## Cleaning and EDA:
### Data cleaning:
1. First, our analysis focuses on team performance with respect to dragons so we filtered out all rows that consisted of player data
    - For this, we queried all rows that had the value "team" in the `position` column - this is based on the data generating process that produced this dataset
2. `gamelength` was originally represented as seconds but League of Legends game duration is measured in minutes by convention so we scaled this feature
3. `date` was converted into a `pd.Timestamp` object
4. Many columns that were specific to player row data were removed because we could gain zero information from these columns as we are dealing with team data
    - For this, we went through each column and dropped all columns that were completely missing - again, based on data generating process creating variables that were specific only to players
5. Many columns such as `result` and `firstdragon`, to name a few, could be represented as `Boolean` types

Performing the data cleaning steps above helped streamline our analyses by shrinking our dataframe to consist only of relevant information needed. Here is the head of the cleaned dataframe:

| gameid                | datacompleteness   | url                                         | league   |   year | split   | playoffs   | date                |   game |   patch | side   | teamname                      | teamid                                  | ban1     | ban2         | ban3         | ban4     | ban5    |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills | firstblood   |   team kpm |   ckpm | firstdragon   |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders | firstherald   |   heralds |   opp_heralds | firstbaron   |   barons |   opp_barons | firsttower   |   towers |   opp_towers | firstmidtower   | firsttothreetowers   |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   goldspent |        gspd |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |     cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |
|:----------------------|:-------------------|:--------------------------------------------|:---------|-------:|:--------|:-----------|:--------------------|-------:|--------:|:-------|:------------------------------|:----------------------------------------|:---------|:-------------|:-------------|:---------|:--------|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|:-------------|-----------:|-------:|:--------------|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|-------------------------:|---------:|-------------:|:--------------|----------:|--------------:|:-------------|---------:|-------------:|:-------------|---------:|-------------:|:----------------|:---------------------|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------:|------------:|--------------:|---------------:|------------------------:|--------------------------:|---------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 | Blue   | Fredit BRION Challengers      | oe:team:68911b3329146587617ab2973106e23 | Karma    | Caitlyn      | Syndra       | Thresh   | Lulu    |      28.55   | False    |       9 |       19 |        19 |           9 |           19 |             0 |             0 |             0 |            0 | True         |     0.3152 | 0.9807 | False         |         1 |             3 |                 1 |                     3 |           0 |           0 |        0 |        0 |           0 |          1 |                      nan |        0 |            0 | True          |         2 |             0 | False        |        0 |            0 | True         |        3 |            6 | True            | True                 |              5 |                  0 |            0 |                1 |               56560 | 1981.09 |                3537.2  |                    2364.73 |            74 | 2.5919 |            51 | 1.7863 |                   33 |           197 | 6.9002 |       47070 |        28222 |      988.511 |       44570 | -0.0283123  |           680 |            160 |                     nan |                       nan |  29.4221 |      16218 |    18213 |      322 |          14695 |        18076 |          330 |           1523 |          137 |           -8 |           3 |             5 |            0 |               0 |                 0 |                3 |      24806 |    28001 |      487 |          24699 |        29618 |          510 |            107 |        -1617 |          -23 |           5 |            10 |            6 |               6 |                18 |                5 |
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 | Red    | Nongshim RedForce Challengers | oe:team:d2dc3681437e2beb2bb4742477108ff | Lee Sin  | Twisted Fate | Zoe          | Nautilus | Rell    |      28.55   | True     |      19 |        9 |        62 |          19 |            9 |             6 |             0 |             0 |            0 | False        |     0.6655 | 0.9807 | True          |         3 |             1 |                 3 |                     1 |           2 |           1 |        0 |        0 |           0 |          0 |                      nan |        0 |            0 | False         |         0 |             2 | False        |        0 |            0 | False        |        6 |            3 | False           | False                |              0 |                  5 |            1 |                0 |               79912 | 2799.02 |                3009.67 |                    2872.33 |            93 | 3.2574 |            51 | 1.7863 |                   45 |           205 | 7.1804 |       52617 |        33769 |     1182.8   |       45850 |  0.0283123  |           792 |            184 |                     nan |                       nan |  34.1856 |      14695 |    18076 |      330 |          16218 |        18213 |          322 |          -1523 |         -137 |            8 |           0 |             0 |            3 |               3 |                 5 |                0 |      24699 |    29618 |      510 |          24806 |        28001 |          487 |           -107 |         1617 |           23 |           6 |            18 |            5 |               5 |                10 |                6 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCK CL   |   2022 | Spring  | False      | 2022-01-10 08:38:24 |      1 |   12.01 | Blue   | T1 Challengers                | oe:team:6dcacec00a6ba7576c5ab7f30c995cd | Sona     | Jarvan IV    | Caitlyn      | Lulu     | Lucian  |      35.2333 | False    |       3 |       16 |         7 |           3 |           16 |             0 |             0 |             0 |            0 | False        |     0.0851 | 0.5393 | False         |         1 |             4 |                 1 |                     4 |           0 |           1 |        0 |        0 |           0 |          0 |                      nan |        0 |            0 | True          |         1 |             1 | False        |        0 |            2 | False        |        3 |           11 | False           | False                |              2 |                  3 |            0 |                2 |               59579 | 1690.98 |                2984.02 |                    3109.61 |           119 | 3.3775 |            55 | 1.561  |                   47 |           277 | 7.8619 |       57629 |        34688 |      984.522 |       53945 | -0.207137   |           994 |            215 |                     nan |                       nan |  34.3141 |      14939 |    17462 |      317 |          16558 |        19048 |          344 |          -1619 |        -1586 |          -27 |           1 |             1 |            3 |               3 |                 3 |                1 |      23522 |    28848 |      533 |          25285 |        29754 |          555 |          -1763 |         -906 |          -22 |           1 |             1 |            3 |               3 |                 3 |                1 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCK CL   |   2022 | Spring  | False      | 2022-01-10 08:38:24 |      1 |   12.01 | Red    | Liiv SANDBOX Challengers      | oe:team:5380cdbc2ad2b8082624f48f99f6672 | LeBlanc  | Yuumi        | Twisted Fate | Karma    | Alistar |      35.2333 | True     |      16 |        3 |        39 |          16 |            3 |             1 |             0 |             0 |            0 | True         |     0.4541 | 0.5393 | True          |         4 |             1 |                 4 |                     1 |           0 |           2 |        1 |        0 |           0 |          1 |                      nan |        0 |            0 | False         |         1 |             1 | True         |        2 |            0 | True         |       11 |            3 | True            | True                 |              3 |                  2 |            2 |                0 |               74855 | 2124.55 |                2745.72 |                    2868.42 |           129 | 3.6613 |            70 | 1.9868 |                   65 |           346 | 9.8202 |       71004 |        48063 |     1364.13  |       66410 |  0.207137   |          1013 |            244 |                     nan |                       nan |  35.6764 |      16558 |    19048 |      344 |          14939 |        17462 |          317 |           1619 |         1586 |           27 |           3 |             3 |            1 |               1 |                 1 |                3 |      25285 |    29754 |      555 |          23522 |        28848 |          533 |           1763 |          906 |           22 |           3 |             3 |            1 |               1 |                 1 |                3 |
| 8401-8401_game_1      | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 | Spring  | False      | 2022-01-10 09:24:26 |      1 |   12.01 | Blue   | Oh My God                     | oe:team:f4c4528c6981e104a11ea7548630c23 | Renekton | Lee Sin      | Caitlyn      | Jayce    | Camille |      22.75   | True     |      13 |        6 |        35 |          13 |            6 |           nan |           nan |           nan |          nan | False        |     0.5714 | 0.8352 | False         |         2 |             1 |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                        2 |      nan |          nan | False         |       nan |           nan | False        |        1 |            0 | False        |        8 |            3 | False           | False                |            nan |                nan |            1 |                0 |               40086 | 1762.02 |                2263.25 |                     nan    |            79 | 3.4725 |            33 | 1.4505 |                   32 |           162 | 7.1209 |       45468 |        30167 |     1326.02  |       36908 | -0.00586225 |           nan |            172 |                      98 |                        18 | nan      |        nan |      nan |      nan |            nan |          nan |          nan |            nan |          nan |          nan |         nan |           nan |          nan |             nan |               nan |              nan |        nan |      nan |      nan |            nan |          nan |          nan |            nan |          nan |          nan |         nan |           nan |          nan |             nan |               nan |              nan |

### Univariate Analysis:
To better understand the importance of the dragon objective, we can graph the number of dragons killed by all teams in 2022. It seems that teams generally kill 2 dragons the most frequently.
<iframe src="assets/fig_univariate_1.html" width=800 height=600 frameBorder=0>s</iframe>

### Bivariate Analysis:
To answer our main question, let's see how the value of `dragons` has shifted over time. Specifically, how has the mean number of dragons killed varied by split over the 2022 competitive season? In the bar plot below, we can see that the Fall split had the lowest mean number of dragons killed by 1.827.
<iframe src="assets/fig_bivariate_1.html" width=800 height=600 frameBorder=0>s</iframe>

### Interesting Aggregates:

We wanted to see if getting the first dragon increased a team's chance of winning. The columns `False` and `True` denote whether or not a team has killed the first dragon. The index `result` denote whether or not a team has won the game. The pivot table shown below is actually a concatenation of two pivot table, one for each team `side`. The proportions inside shown the conditional probability **P(Winning | First Dragon)**.

| result   |    False |     True | team   |
|:---------|---------:|---------:|:-------|
| False    | 0.52774  | 0.377525 | blue   |
| True     | 0.47226  | 0.622475 | blue   |
| False    | 0.596116 | 0.454819 | red    |
| True     | 0.403884 | 0.545181 | red    |

The trend in this table is that killing first dragon always increases the chances of winning for both team colors - and it also means that not killing the first dragon increases the chances of losing for both team colors. To better visualize this, we provide a bar plot.
<iframe src="assets/aggregate.html" width=800 height=600 frameBorder=0>s</iframe>

---
## Assessment of Missingness:
### NMAR Analysis:
We can argue that `teamname` is NMAR. This is because the missingness of `teamname` is dependent on the value of `teamname` itself. The likely reason for this missingness can be attributed to the fact that perhaps new teams simply do not have an established name, and so the missingness of their name depends on them not having a name. To make `teamname` MAR, we can perhaps add a new column of data called `teamage` which shows how long a team has been together. Thus, if `teamname` is missing, the distribution of `teamage` would be less than that of `teamage` if `teamname` exists.

### Missingness Dependency:
##### Dependent testing
The elder dragon only spawns after one team has killed all four elemental dragons. Thus, the elder dragon typically only appears in games that are longer. Because of this assumption, we can check if `elders` is MAR dependent on `gamelength` using a permutation test.

Null: The distribution of `gamelength` is the same when `elders` is missing and when `elders` is not missing

Alternative: The distribution of `gamelength` is different when `elders` is missing and when `elders` is not missing

Test statistic: Absolute difference of group means

Significance level: 0.05

<iframe src="assets/MAR.html" width=800 height=600 frameBorder=0>s</iframe>
<iframe src="assets/MAR_ts.html" width=800 height=600 frameBorder=0>s</iframe>

Our p-value is 0.0 so we reject the null. Since we reject the null hypothesis that the two distributions are similar, we show statistical evidence that `elder` is, potentially, MAR dependent on `gamelength`. However, this isn't an absolute conclusion! 

##### Not dependent testing

Now, we can perform another permutation test on `elders` missingness and `side` because we can follow the reasoning that `elders` is not MAR dependent on `side`. This is because, in our data, every pair of rows consists of a match between two teams. These two teams are separated into a blue and red side and so the distribution of `side` will always be 50/50 regardless of whether `elders` is missing. However, we can still run a permutation test to have stronger statistical evidence for this logic.

Null: The distribution of `side` is the same when `elders` is missing and when `elders` is not missing

Alternative: The distribution of `side` is different when `elders` is missing and when `elders` is not missing

Test statistic: Total variation distance

Significance level: 0.05

<iframe src="assets/NOT_MAR.html" width=800 height=600 frameBorder=0>s</iframe>
<iframe src="assets/NOT_MAR_TS.html" width=800 height=600 frameBorder=0>s</iframe>

Our p-value is 1.0 so we fail to reject the null. Since we fail to reject the null hypothesis that the two distributions are similar, we have statistical evidence that `elder`, potentially, isn't MAR dependent on `side`. However, this conclusion isn't absolute! 

---
## Hypothesis testing:
### Answering the question:
Our specific question was: how have dragons been valued over time? We can gain more insight into this question by looking at the mean number of `dragons` killed per split, and seeing if any variations occurred. Per our bivariate analysis above, We can see that the Fall split had the lowest mean number of dragons killed by 1.827.

<iframe src="assets/fig_bivariate_1.html" width=800 height=600 frameBorder=0>s</iframe>

Let's conduct a hypothesis test to see if the mean number of `dragons` killed during the Fall split was lower than the other averages due to lower chance. That is, `dragons` and `split` aren't related - and thus the importance of dragons is potentially not related to the split.

Null hypothesis: `dragons` and `split` are not related - the low average number of dragons killed during the Fall is due to random chance

Alternative hypothesis: `dragons` and `split` are related - the low average number of dragons killed during the Fall is not due to random chance

Test statistic: Average `dragons` killed

Significance level: 0.05

<iframe src="assets/hypoth.html" width=800 height=600 frameBorder=0>s</iframe>

Since we reject the null hypothesis that the low mean value of `dragons` for the Fall `split` was due to random chance, we show significant statistical evidence that `dragons` and `split` are potentially related. This potential connection could show that team's value dragons differently in a certain `split`. The variation could also be caused by many confounding factors such as meta shifts, game durations, buffs/nerfs, and map updates.