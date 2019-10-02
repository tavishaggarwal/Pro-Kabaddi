# Pro Kabaddi League Hackathon

In this hackathon, our task is to use our coding and analytics skills to predict various outcomes at the end of Pro Kabaddi league tournament.

## Data Preparation Cleaning and Tasks Solution Planning

### Scraping Data:

We are using traditional technique to scrap out the data from the (https://www.prokabaddi.com/ ) website. We analyzed the website and found out that the websites are using microservices as a middleware. Therefore we have leveraged this to scrap out the data.
We have gone further and found out the micro services URLs to get team and player statistics.

### Creating dataset containing details of the teams and players:

We iterate over all the teams and player data to extract season wise data for all the teams and players.

We are also getting details of the matches played in each season. For getting this information we are using all the matches played in **Pro kabaddi** till date across all the seasons.

We are also generating data for the successful raids and tackles and total raids and tackles for the players using the above data.

We then generate overall Stats of the **Pro kabaddi** seasons.

### Data Cleaning and Preparation:

For finding the task results, first we are selecting only the columns that are needed for predicting the task results and dropping all other columns.

We are then updating row values to align to the format.

We are then removing data for the non **Pro Kabaddi** League Challenges.

Lastly we are filling missing values for field *Season* with *Overall* as they depict Overall Results.

There are rows which don't have a team name and position name. We are not removing these rows since we will be checking if these players are present in Season 7. As the target goal is to predict the results on Season 7.

 

#### Data files:
1. playerData_needed.csv : Contains player related data of all seasons.

2. teamStats_needed.csv : Contains team related data of all seasons.

3. matchStats_needed.csv : Contains the stats for the individual matches played between teams across various tournaments from all seasons.

4. totalPointData_needed.csv : Contains total points related data of all seasons.

#### Code files:
1. pro_kabaddi_hackathon_Collect_Data.ipynb : Code to collect data from pro kabaddi sites.

2. pro_kabaddi_hackathon_Predict.ipynb : code to process data and predict task results.

### Solution Planning:

For all tasks, we are training separate *Linear Regression models* per task using *data from season 1 to 6* for training. And to predict the results we are using season 7 data. 

Also we are veryfying the perdiction with the latest matches happened in Season 7.(not part of the dataset)

So, for all tasks we are using data from *Season 1 to 6* for training the models and *Season 7* to predict the results.

For every task, we are generating a derived column as a target variable which depicts the season wise ranking for the player or team in predicting the results.

Additionally, we have added weight to the rank (no of matched played/total matches) such that results are not biased for any team or player.

#### For Tasks 6 and 7:

**Step 1**. We first filter out the task specific player data and then analyze them based on various columns to see which ones are to be used for finding final score based task results.

**Step 2**. We then find a derived feature column obtained by formulating a weighted linear combination of columns relevant for those tasks after analyzing the relevance and correlation of various columns.

For **Task 6**, we find “weighted_success_raid_percent” derived feature column and use it along with columns [ ‘series_name’, ’'player_name’, ‘total_raids’, ‘unsuccess_raids’, ‘empty_raid’, ‘super_raids’, ‘success_raid_percent’].

For **Task 7**, we find “weighted_success_tackle_percent” derived feature column and use it along with columns [ ‘series_name’, ’'player_name’, 'tackle_points', 'tackle_success_rate', 'total_tackles', 'success_tackles', 'unsuccesful_tackles', 'super_tackles'].

These two derived feature columns show current form of players for particular tasks by giving season-wise weightage to the various score columns of the players of that task and gets used in the final ranking score calculation.

**Step 3**. These derived feature columns along with other relevant columns are then used in a *Linear Regression model* to find final ranking scores which act as task specific ranking points for the players.

## Tasks and Results

### Task 1: Predict the winner of the tournament

Result: Dabang Delhi K.C.

Method Adopted:
1. We did EDA to see historical team level data available. 
2. We created a derived column weighted rank at each season level, which acts as a target variable in regression.
3. We checked for the correlated column using heatmap.
4. We did data cleaning like converting column to numeric.
5. We fitted a Linear Regression Model on data from Season 1 to Season 6.
6. And verified the results on Season 7 available data.


### Task 2: Predict the top team in the points table after the completion of the league matches

Result: Dabang Delhi K.C.

Method Adopted:
1. To predict the winner of the tournament we generated two seprate datasets depecting total points in past seasons and number of matches won by the teams at crucial level (qualifiers, playoffs, semis and Finals matches) in past seasons.
2. Similar to task 2, we fitted Linear Regression Model on data from Season 1 to Season 6. And verified the results on Season 7 available data.


### Task 3: Predict the team with the highest points for successful raids

Result: Dabang Delhi K.C.

Method Adopted:
1. After doing EDA, we generated weighted rank column which depicts weighted ranks of teams accross various seasons.
2. After cleaning up the dataset, we fitted Linear Regression Model on data from Season 1 to Season 6. And verified the results on Season 7 available data.

### Task 4: Predict the team with the highest points for successful tackles

Result: Jaipur Pink Panthers

Method Adopted: We adobted same technique as that of task 3.

 

### Task 5: Predict the team with the highest super-performance total

Result: Bengal Warriors

Method Adopted:
1. For this task we need a column stating no of all outs conceded by the team, which was missing in the datset which we are having.
2. Therefore we generated a calculated column from the match level data which we were having for each season. Where, all outs conceded by the teams are same as all outs other team has done.
3. Based on this we calculated Super Performance Total for the teams accross various seasons.
4. After generating required columns we followed same technique as stated in task 3.

 

### Task 6: Predict the player with the highest SUCCESSFUL RAID percentage

Result: Pardeep Narwal

Method Adopted:
1. We selected the player raid columns required for the prediciton.
2. We calculated the column `weighted_success_tackle_percent` which depicts the successfull tackles percetange over the no of matched played. This is done to avoid outliers in the prediction.
3. After doing EDA, we generated rank of players accross seasons. This column is used as a trget variable in Regression algorithm.
4. To further refine `weighted_success_tackle_percent`, we updated this column with the player form in the match where weitage is given to each season.
4. Sesaon 1, Season 2, Season 3 and Season 4: Giving 5 percent weightage, Season 5: Giving 10 percent weightage, Season 6: Giving 20 percent weightage and Season 7: Giving 50 percent weightage. Giving highest weitage to cuurent season because it depicts the current form of the player.
5. After calculating derived features, we fitted Linear Regression Model on data from Season 1 to Season 6. And verified the results on Season 7 available data.

 

### Task 7: Predict the player with the highest SUCCESSFUL TACKLE percentage

Result: Fazel Atrachali

Method Adopted: We adobted same technique as that of task 6.


