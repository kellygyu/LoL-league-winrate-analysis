# League of Legends - Banned Champions Performance & Win Rate Analysis


## Project Overview
The following project was completed for DSC80 at UCSD. This project aims to investigate the difference in winrate and performance of heavily banned champions vs other champions within league of legends tournaments.

**Author**: Kelly Yu

## Jargon Definitions
Throughout this project, I will be using a lot of jargon particular to League of Legends. I have included a short description of what each word means below for better understanding. 

| Jargon | Definition |
|: ----------- :|: ----------- :|
| Champion | characters you play as in game |
| Meta | describes whether a champion is popular |
| KDA | calculated metric that describes a player's performance in game base off kills, deaths, and assists|

## Background
League of Legends is a game purely focused on game understanding, champion mastery, macro/micro gameplay, and teamwork. There are currently over 152 champions as of 2022, each with their own skillsets and playstyles. In professional matches, a team's chance of winning is not only heavily dependent on the champions they select, but also the champions they ban from the game, which reduces their enemy's ability to play such champions. Banning certain champions could heavily increase a team's winrate multiple ways, for example, banning strong champions could reduce a team's chance of losing lane due to having to play against a "broken" champion. 

With each season, we tend to see the same champions being banned in profession league matches, as generally these champions are known to be part of a meta, or just have great utility for teamfights overall. Therefore, in this project, we will explore whether the top most banned champions truly have a higher win rate and performance distribution compared to the rest of the champion pool. 

### Dataset Explaination
The dataset we will be using will be from Oracle's Elixir. The dataset itself contains League Esports data from 2022 and 2023, however we will be filtering out data from 2023 to only focus on competitive that occured in 2022. The overall dataset has a total of 123 columns. After filtering and cleaning, our final dataset for hypothesis tests has a total of 107257 rows, and 11 columns. For Missing Data investigations, we will be using subsets of the original dataframe, at 121010 rows and 4 columns.

**Column Focus/Descriptions**

For our analysis, we will only focus on the columns defined below. All other columns are not as relevant for our needs and will be discarded.

| Column Name | Description |
|: ----------- :|: ----------- :|
| gameid | id corresponding the played match |
| league | the tournament name |
| result | True if team win False if loss |
| year | year of tournament |
| position | the player's role in game |
| champion | the champion name that a player plays as |
| kills | number of opponents a player has slain |
| deaths | number of times a player has died |
| assists | number of times a player assisted in killing an opponent |
| ban1 ... ban5| the champion banned within a match|
| goldat10 | the gold a player/team has 10 minutes into the game |

Additionally, we will use two calculated columns we defined.

| Column Name | Description |
|: ----------- :|: ----------- :|
| kda | [(kills + assists)/deaths] which is a performance metric |
| meta | true if a champion is heavily banned false if not |


## Data Cleaning & EDA

### Data Cleaning

As our dataset contains a lot of extra columns, we will first drop all the columns that won't be used for our analysis. Additionally, the 2022 dataset we downloaded contains some matches from 2023 preseason, hence we will also drop any match data with the "year" 2023. After removing these matches, we have a total of 12101 rows. Our next steps will be to create new columns to measure performance and determine whether a champion is heavily banned or not. We define these columns to be "meta" and "kda". 

**Defining `"Meta"`**

As defined in our jargon, meta refers to the popularity status of a champion. Here, we will use the term `"meta"` as a reference to whether a champion is popularly banned. As there are 152 champions in League of Legends, if we take all champions as equal/fair, each champion would techinally have about a 1/152 chance of getting banned. As there can be up to 10 bans per game, the chance of a certain champion being banned within one game is 1 - (151/152) x (150/151) x ... (142/143), which is 5/76 or about 6.6% chance of getting banned. 

Hence, using this probability as a baseline, let's define a champion to be `"meta"` (banned a lot) if their ban rate is larger than 15%, about double the fair chance of a champion being banned. 15% is high for a champion if they are being banned in at least 15% of professional tourament matches given there are 152 other champions that could also be banned. Using this definition, we will create the `"meta"` column


**Defining `"kda"`**

KDA, also known as Kills, Deaths and Assists, is a common metric used to quantify a player's performance in most competitive games. The formula that is universally used to calculate this metric is `("Kills" + "Assists")/"Deaths"`. Having a KDA higher than 1 usually means that a player had a positive impact in game and to their team. Having a KDA between 0 and 1 usually indicates poor performance, meaning that a player has died more times than they have helped, or killed enemies. This indicates negative impact to a team. Using this definition, we will create the `"kda"` column.

After some additional filtering conducted after univariate & bivariate analysis, our final dataframe `"cldf"`, a short term for cleaned_df, is created.

**Here is the head of our `"cldf"` dataframe**

| gameid                | league   |   year | result   | position   | champion   |   kills |   deaths |   assists | meta   |     kda |
|:----------------------|:---------|-------:|:---------|:-----------|:-----------|--------:|---------:|----------:|:-------|--------:|
| ESPORTSTMNT01_2690210 | LCKC     |   2022 | False    | top        | Renekton   |       2 |        3 |         2 | False  | 1.33333 |
| ESPORTSTMNT01_2690210 | LCKC     |   2022 | False    | jng        | Xin Zhao   |       2 |        5 |         6 | False  | 1.6     |
| ESPORTSTMNT01_2690210 | LCKC     |   2022 | False    | mid        | LeBlanc    |       2 |        2 |         3 | True   | 2.5     |
| ESPORTSTMNT01_2690210 | LCKC     |   2022 | False    | bot        | Samira     |       2 |        4 |         2 | False  | 1       |
| ESPORTSTMNT01_2690210 | LCKC     |   2022 | False    | sup        | Leona      |       1 |        5 |         6 | False  | 1.4     |

<iframe src="charts/kills_dist.html" width=800 height=600 frameBorder=0></iframe>
