---
layout: post
title: Performance of Different Rating Systems on Predicting MLB Half-Game Results
subtitle: John F. Adamek
cover-img: /assets/img/baseball.jpg
tags: [test]
comments: true
---

This is an ongoing project I was interested in based on the current trend we see in major league baseball with starting pitchers rarely going past the 5th or 6th inning. I focused on David Smyth's (later modified by Tom M. Tango) BaseRuns formula and applied it to starting pitchers:

BaseRuns = ((A * B) / (B + C)) * D
A = Hits + BB + HBP − HR
B = ((1.4 ∗ tbe) − (0.6 ∗ H) − (3 ∗ HR) + (0.1 ∗ (BB + HBP))) ∗ 1.1
C = 3 * IP
D = HR

Where A = batter reaching base, B = advancing the runner, C = Failure to reach base, and D = Home run

All data was scraped from Fangraphs and represented only the first 5 innings of players' statistics. For the A factor, linear regressions were ran to determine the appropriate weights for the different variables. Each SP's BaseRun was backtested for the 2017-2020 seasons showing strong correlations (r's > 0.97).

This process is still being tested and adjusted. For the 2021 season, I automated my r-script each day to pull in the day's game data, compute BSR for each starting pitcher and extract game results after 5 innings. Comparing this BSR approach to Bill James's pythagorean theorem of baseball W% =  RS^2 / (RS^2 + RA^2) it was found that BSR correctly predicted the winner 53.98% of the time compared to 55.64%. 

So far for the 2022 season BSR is predicting the winner after 5 innings 60.6% of the time, James's W% is slightly better at 61.36%. I expect BSR to improve in accuracy after calculating each batters BSR contribution to the team and computing its impact on the SP's BSR [currently in process]. As always, I'm continuously testing a "cognitive factor" to the algorithm.


PDF to the original study analyze is attached.


[Final_Report.pdf](https://github.com/adamek2120/adamek2120.github.io/files/6729393/Final_Report.pdf)

