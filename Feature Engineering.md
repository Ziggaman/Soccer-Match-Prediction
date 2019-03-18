# Functions for Feature Engineering

##### Team Form Feature Creation: this function looks up the result column of the teams previous ten matches (home or away) and sums up the 'result' column. The sum of the results provide a measure of the teams 'form' coming into the game.

###### Home Team Form Function

    def Form_Home_Team (df):
    
      ''' Returns the sum of the 'Result' column for the last ten matches for the home team'''

      team = df['home_team_api_id']
      date = df['date']
      team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
      last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
      score = last_matches['Result'].sum()
      return score

###### Away Team Form Function
    
    def Form_Away_Team (df):

      ''' Returns the sum of the 'Result' column for the last ten matches for the home team'''

      team = df['away_team_api_id']
      date = df['date']
      team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
      last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
      score = last_matches['Result'].sum()
      return score
      
      

##### Attacking Form Feature Creation 

###### Goals scored Function: This function takes a dateframe and a team as paremeter and returns the total number of goals scored by that team. This function is called in the goal scoring form function.

    def goals_scored_calc(df, team):

        '''Returns goals scored by both teams in previous 10 matches'''

        home_goals = int(df['home_team_goal'][df['home_team_api_id'] == team].sum())
        away_goals = int(df['away_team_goal'][df['away_team_api_id'] == team].sum())
        total_goals = home_goals + away_goals
        return total_goals
        
###### This function returns the total number of goals scored by a team in its last ten matches

    def Home_Team_Goal_Scoring_Form (df):

        ''' Returns the goals scored by the home team in prevous 10 matches'''

        team = df['home_team_api_id']
        date = df['date']
        team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
        last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
        return goals_scored_calc(last_matches,team)

    def Away_Team_Goal_Scoring_Form (df):

        ''' Returns the goals scored by the away team in prevous 10 matches'''

        team = df['away_team_api_id']
        date = df['date']
        team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
        last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
        return goals_scored_calc(last_matches,team)


##### Defensive Form Feature Creation

###### Goals conceded Function: This function takes a dateframe and a team as paremeter and returns the total number of goals conceded by that team. This function is called in the goal conceded form function.

    def goals_conceded_calc(df, team):

        '''Returns goals conceded by both teams in previous 10 matches'''

        home_goals_conceded = int(df['away_team_goal'][df['home_team_api_id'] == team].sum())
        away_goals_conceded = int(df['home_team_goal'][df['away_team_api_id'] == team].sum())
        total_goals_conceded = home_goals_conceded + away_goals_conceded
        return total_goals_conceded
        
###### This function returns the total number of goals conceded by a team in its last ten matches.

    def Home_Team_Defensive_Form (df):

        ''' Returns the goals conceded by the home team in prevous 10 matches'''

        team = df['home_team_api_id']
        date = df['date']
        team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
        last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
        return goals_conceded_calc(last_matches,team)

    def Away_Team_Defensive_Form (df):

        ''' Returns the goals conceded by the away team in prevous 10 matches'''

        team = df['away_team_api_id']
        date = df['date']
        team_matches = Match[(Match['home_team_api_id'] == team) | (Match['away_team_api_id'] == team)]
        last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:10,:]
        return goals_conceded_calc(last_matches,team)
        
##### Head to Head Feature Creation 

###### This function takes a sum of the result column for the previous 4 matches played between the two teams. The sum of the result column is a feature engineered as a metric to quantify the head-to-head record of the teams coming into a particular game.
    
    def Head_to_Head (df):

        ''' Returns the sum of the 'Result' column for the previous four mathes played between the two teams'''

        team1 = df['home_team_api_id']
        team2 = df['away_team_api_id']
        date = df['date']
        team_matches = Match[((Match['home_team_api_id'] == team1) & (Match['away_team_api_id'] == team2))|\
         ((Match['home_team_api_id'] == team2) & (Match['away_team_api_id'] == team1))]
        last_matches = team_matches[team_matches['date'] < date ].sort_values(by = 'date', ascending = False).iloc[0:4,:]
        score = last_matches['Result'].sum()
        return score
    


