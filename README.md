# t20-cricket-dashboard
📊 Project Overview
This project is an interactive Power BI dashboard analyzing the top-performing players in T20 cricket. The dashboard ranks players based on key metrics such as runs, wickets, strike rate, and economy rate, providing a comprehensive view of player performance for cricket enthusiasts and analysts.

🖼️ Dashboard Snapshots
(Insert screenshots of the dashboard here)

Overall Player Performance

Batting Metrics

Bowling Metrics

Interactive Filters

🚀 Features

Comprehensive Analysis: Identify the top 11 players based on batting, bowling, and fielding metrics.

Dynamic Filtering: Drill down data by teams, roles, or specific match scenarios.

Advanced Metrics: Custom calculations for strike rate, boundary percentages, and economy rates.

Interactive Visuals: Heatmaps, charts, and comparison visuals to explore data trends.

🔧 Tools & Technologies

python coding-

```python
import pandas as pd
import json

with open ('/content/t20_wc_match_results.json') as f:
    data = json.load(f)

df_match=pd.DataFrame(data[0]['matchSummary'])
df_match.head()



df_match.shape


df_match.rename(columns={'scorecard': 'match_id'}, inplace=True)
df_match.head()

match_ids_dict = {}

for index, row in df_match.iterrows():
    key1 = row['team1'] + ' Vs ' + row['team2']
    key2 = row['team2'] + ' Vs ' + row['team1']
    match_ids_dict[key1] = row['match_id']
    match_ids_dict[key2] = row['match_id']

match_ids_dict

df_match.to_csv('/content/t20_wc_match_results.json', index = False)

## batting summary

with open('/content/t20_wc_batting_summary.json') as f:
    data = json.load(f)
    all_records = []
    for rec in data:
        all_records.extend(rec['battingSummary'])

df_batting = pd.DataFrame(all_records)
df_batting.head(11)

df_batting['out/not_out'] = df_batting.dismissal.apply(lambda x: "out" if len(x)>0 else "not_out")
df_batting.head(11)

df_batting.drop(columns=['dismissal'],inplace=True)
df_batting.head(10)

df_batting['match_id'] = df_batting['match'].map(match_ids_dict)
df_batting.head()

from google.colab import sheets
sheet = sheets.InteractiveSheet(df=df_batting)

df_batting['batsmanName'] = df_batting['batsmanName'].apply(lambda x: x.replace('â€', ''))
df_batting['batsmanName'] = df_batting['batsmanName'].apply(lambda x: x.replace('\xa0', ''))
df_batting.head(40)

df_batting.shape

df_batting.to_csv('batting information .csv', index = False)

# Bowling Sumarry

with open('/content/t20_wc_bowling_summary.json') as f:
    data = json.load(f)
    all_records = []
    for rec in data:
        all_records.extend(rec['bowlingSummary'])
all_records[:2]

df_bowling = pd.DataFrame(all_records)
print(df_bowling.shape)
df_bowling.head()

df_bowling['match_id'] = df_bowling['match'].map(match_ids_dict)
df_bowling.head()

df_bowling.to_csv('bowling information.csv', index = False)

# Process Player info

with open('/content/t20_wc_player_info.json') as f:
    data = json.load(f)

df_players = pd.DataFrame(data)

print(df_players.shape)
df_players.head(10)

df_players['name'] = df_players['name'].apply(lambda x: x.replace('â€', ''))
df_players['name'] = df_players['name'].apply(lambda x: x.replace('†', ''))
df_players['name'] = df_players['name'].apply(lambda x: x.replace('\xa0', ''))
df_players.head(10)

df_players[df_players['team'] == 'India']

df_players.to_csv('player info.csv', index = False)

```

Power BI Desktop: For creating the interactive dashboard.
https://colab.research.google.com/drive/1vtWxHnuJsB8D00vp_aSSzAqbbhq_UhYF?usp=sharing

DAX Formulas: Used for custom measures and KPIs.

Data Source: Historical T20 cricket data curated for analysis.

📈 Key Insights

Top Performers: Highlights players who contribute most significantly with the bat or ball.

Boundary Analysis: Displays the impact of fours and sixes on match outcomes.

Bowling Efficiency: Insights into economy rate and strike rate of bowlers.

Team Comparisons: Compare player performances across teams and match types.

💡 How to Use

Download the .pbix file from this repository.

Open the file in Power BI Desktop.

Interact with the dashboard using slicers and visuals to explore insights.

🎯 Future Enhancements

Integrating real-time data via APIs for live updates.


Adding more player statistics, such as fielding metrics (catches, run-outs).

Expanding to include historical data for trend analysis.



  
