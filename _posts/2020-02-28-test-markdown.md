---
layout: post
title: Performance of Different Rating Systems on Predicting MLB Half-Game Results
subtitle: John F. Adamek
tags: [test]
comments: true
---

# 1   Introduction

Major League Baseball (MLB) provides a large amount of statistics relating to team and individual performance. Statisticians have gravitated towards analyzing these statistics in what has become to be known as 'Sabermetrics'. As a result, sabermetrician's have developed numerous rating systems over the years with the intent of explaining and predicting future performance. Bill James 'GameScore' is one system used to gauge a pitchers game performance. David Smyth's 'BaseRuns' (modified by Tom M. Tango) is another system thats been used to estimate the number of runs a team should have. Typically, these systems are analyzed on full-game data. Due to the shift in how baseball is played today with starting pitchers rarely going past 5 or 6 innings, these systems will be used with half-game (5-inning) data to predict half-game results.  


## 1.1    Research Questions

In this study, I will be comparing how the systems above perform in predicting the results between two opposing starting pitchers during the first 5-innings. First I will examine the relationship between a starting pitchers BaseRuns (BSR) for 5-inning to their actual 5-inning Runs allowed (R) and Earned Runs allowed (ER) as well as BaseRuns per 5-inning average (BSRA) to actual Earned Run average (ERA)

# 2   Methods


## 2.1    Data Preparation

The data was prepared through a multiple stages. Data downloaded from the Fangraphs website (e.g., 2020 data: https://www.fangraphs.com/leaders/splits-leaderboards?splitArr=42,44,45,46,47,48&splitArrPitch=&position=P&autoPt=true&splitTeams=false&statType=player&statgroup=1&startDate=2020-3-1&endDate=2020-11-1&players=&filter=&groupBy=season&sort=-1,1) was imported into R and joined so that the IP from the Advanced variables was added to the standard variables by the pitchers name. 

\begin{table}[h]
    \centering
    \caption{Description of Individual Statistics Used}
    \begin{tabular}{l c}
    \hline
    Name & Description (per 5IP) \\
    \hline
    Pitcher & The name of the pitcher \\
    Season & Baseball Season \\
    IP & Innings pitched \\
    TBF & Total Batters Faced \\
    ERA & Earned run average\\
    H & Hits given up\\
    R & Runs given up\\
    ER & Earned Runs given up\\
    HR & Home Runs given up\\
    OBP & On-Base Percentage\\
    \hline
    \end{tabular}
    \label{tab:my_label}
\end{table}

These variables were further prepared for analyzing Baseruns by computing new variables:

\begin{table}[h]
    \centering
    \caption{Variables Prepared for BaseRuns Formular}
    \begin{tabular}{l c c}
    \hline
    New Variable & Computation & Purpose  \\
    \hline
    Abr &= OBP*TBF & Provides a continuous variable for number times someone reached base \\
    H123 &= H - HR & Provide the amount of hits given up minus home runs \\
    tbe &= ((1.12 * H) + (4 * HR)) & This provides part of the 'B' factor\\
    \hline
    \end{tabular}
    \label{tab:my_label}
\end{table}

## 2.2    Variables for Analysis

For the BaseRuns formula, data was prepared by running linear regressions to calculate weights to be used in the formula. The BaseRuns formula for pitchers:

$$BaseRuns = \frac{A * B}{B + C} *D$$

$A = Hits + BB + HBP - HR$

$B = ((1.4 * tbe) - (0.6*H) - (3*HR) + (0.1*(BB+HBP))) * 1.1$

$C = 3 * IP$

$D = HR$


*Where A = batter reaching base, B = advancing the runner, C = Failure to reach base, and D = Home run.*

For the A formula in the equation, linear regressions were ran to determine the appropriate weights for the hits, BB, and HBP variables. This was  done by regressing those three variables on a continuous variable representing a batter reaching base. This continuous variable (Abr) was calculated by multiplying the pitchers opponents on-base percentage by total batters faced. Additionally, HR's was removed from hits in order to stay true to the formula(H123 = Hits - HR) (Table 2). The regression formula was therefore be Abr ~ H123 + BB + HBP. The coefficients for each variable was then used to calculate factor A: $$A = \beta_1*H123 + \beta_2*BB + \beta_3*HBP - \beta_4*HR$$ Final BaseRuns variables that were used for analysis are provided in Table 3.

\begin{table}[h]
    \centering
    \caption{BaseRuns 5-inning Variables}
    \begin{tabular}{l c}
    \hline
    Variables & Formula  \\
    \hline
    BSR &= ((A*B) / (B + C)) + HR \\
    BSRperGame &= BSR / G \\
    BSRAvg &= (BSR/IP)*5)\\
    \hline
    \end{tabular}
    \label{tab:my_label}
\end{table}

# 3   Results

Descriptive were done for the main variables of interest separated by each of the four seasons and bivariate correlations were ran for Baseruns on actual runs allowed and actual earned runs allowed:

## 3.1    Relationship of Starting Pitchers BaseRuns vs Actual Statistics 

Baseruns is a metric used to determine how many runs a starting pitcher SHOULD give up. A pitchers actual runs statistic is a summary of all runs they had given up whether it was their fault (i.e., fielding error) or not. Therefore another statistic, earned runs, is commonly used to summarize the amount of runs a pitcher gave up which they had control of. When running a linear regression with the actual statistic (R or ER) as the dependent variable and BSR as the independent, we found that there was a significant linear relationship between BaseRuns and both Runs and earned runs (adjusted R-squared range: 0.83 to 0.95). Its important to note that the 2020 baseball season consisted on a pandemic shortened 60-game season which is expected to explain the lower adjusted R-squared for the data. 

When baseruns per 5 innings (BSRA) was compared to ERA we again found a significant linear relationship with adjusted R-square ranging from 0.81 - 0.82).




[Final_Report.pdf](https://github.com/adamek2120/adamek2120.github.io/files/6729393/Final_Report.pdf)

