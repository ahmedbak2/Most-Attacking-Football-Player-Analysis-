# Most-Attacking-Football-Player-Analysis-
This project analyses the offensive performance of 500+ football players using SQL and Power BI. The goal is to identify the most impactful players across positions   forwards, midfielders, and defenders   by evaluating advanced per-90 metrics such as goals, assists, expected goals (xG), expected assists (xA), key passes, and progressive actions.


Project Introduction

This project explores offensive performance analysis in football using player data from the 2025–2026 season across Europe’s top leagues.

As a football fan and data analyst, I wanted to quantify attacking performance beyond traditional stats like goals and assists — by creating a normalized offensive index that combines expected goals (xG), assists, key passes, and progressive actions.

The project combines SQL for data cleaning and metric computation with Power BI for interactive visualization, helping identify the most offensive players per position (Forward, Midfielder, Defender).

📊 Dataset Information

Source: FBref
 – a leading football statistics platform.
Collected on: September 27, 2025
Season: 2025–2026

Credit: All original data belongs to FBref.

📋 Columns (original dataset highlights)

The dataset contained detailed player-level stats across multiple statistical tables, including:

Basic Info: Player, Nation, Pos, Squad, Comp, Age, Born

Performance: Gls, Ast, xG, xA, KP, PrgC, PrgP, Tkl, Int, Clr, Err

Playing Time: MP, Starts, Min, 90s

Possession & Passing: Touches, Cmp%, PPA, Carries, Recov, Dis

❓ Problems to Solve

Who are the most offensive players in each position (FW, MF, DF)?

How do traditional stats (goals/assists) compare to expected performance (xG/xA)?

Which players are most progressive in terms of passes and carries?

Can a composite “Offensive Index” provide a fair comparison across positions?

 Data Cleaning Process

Due to the raw data complexity from FBref (over 300+ columns across different sections), the dataset required restructuring into separate SQL tables:

 SQL Structure

Players – player-level information (name, nationality, position, team, competition).

PlayerStats – numeric performance data.

Teams – unique team and league mappings.

Key Cleaning Steps:

Created base tables with relevant columns using CREATE TABLE and INSERT INTO statements.

Filtered out players with fewer than 200 minutes played to ensure a reliable sample size.

Normalized player names and team references for consistent joins.

Used window functions to handle normalization (per 90 metrics and min–max scaling).

Renamed and standardized columns to align with Power BI field names.

 Challenges:

Early-season data meant small sample sizes, affecting per-90 reliability.

Positional categorization inconsistencies (FW/MF, MF/FW, etc.) required manual validation to fairly classify players.

 SQL Analysis Process

The main analysis was built through a SQL View called Offensive_Stats, created using CTEs (WITH per90 AS, norm AS).

Key Calculations:

Per 90 Metrics:

CASE WHEN ps.Mins > 0 THEN ps.Goals * 90 / ps.Mins ELSE 0 END AS Goals_per_90


Normalization by Position (min–max scaling):

(Goals_per_90 - MIN(Goals_per_90) OVER(PARTITION BY Pos)) /
(MAX(Goals_per_90) OVER(PARTITION BY Pos) - MIN(Goals_per_90) OVER(PARTITION BY Pos))


Composite Score (Offensive_Total):
Sum of all normalized offensive metrics.

Ranking Players by Position:

DENSE_RANK() OVER(PARTITION BY Pos ORDER BY Offensive_Total DESC)

 Power BI Dashboard
 Dashboard Features

Dynamic Player Cards:
Highlight the most offensive Forward, Midfielder, and Defender using DAX measures and slicers.

Offensive Summary Metrics:
Each card shows:

Goals/90 • Assists/90 • xG/90 • xA/90 • Key Passes/90

Visual Components:

Scatter Plot: Goals vs. Assists (bubble size = Offensive Total)

Bar Chart: Expected vs. Actual Contributions

Radar Chart: Offensive profiles across metrics

Rank Distribution Chart: Offensive rank by position

Slicers: Filter by position, team, or league

 Dashboard Takeaways

Clear performance outliers emerge when combining actual and expected metrics.

Progressive carries and passes highlight creative playmakers that raw goals miss.

Normalized rankings provide fairer comparisons between defenders, midfielders, and attackers.

 Skills Demonstrated

SQL: Table design, joins, CTEs, window functions, ranking, normalization

Power BI: Data modeling, DAX measures, interactive visuals, slicers

Data Cleaning: Handling inconsistent categorical data and incomplete samples

Data Analysis: Building composite indices and deriving meaningful comparisons

 Conclusion

This project demonstrates how SQL and Power BI can work together to transform raw football data into actionable insights.

Identified the most offensively impactful players by position

Showed that expected metrics (xG/xA) often align closely with output over larger samples

Created a repeatable structure for ongoing football data analysis

 Future Improvements

Add positional sub-grouping (e.g., LW, CM, RB) for deeper tactical analysis

Integrate defensive stats for a balanced player rating

Automate FBref scraping for real-time seasonal updates
