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

## 1.2    Data Source

```{r include=FALSE}
setwd('C:/Users/jadam/Desktop/Courses/Spring 2021/EPSY590/Final Project')
fg1 <- read_csv('fg1.csv')
fg2 <- read_csv('fg2.csv')
fg1a <- colnames(fg1)
fg2a <- colnames(fg2)

```

Individual statistics will be extracted from the publicly available website Fangraphs. By use of the websites advanced filtering tools, starting pitcher data from the years 2017, 2018, 2019, & 2020 will be exported into a comma-separated values (CSV) file. This file will contain only the data for the first, second, third, fourth, and fifth innings over the course of the year. The statistics from the file will include the standard variables: `r fg1a` and Advanced variables `r fg2a`.
