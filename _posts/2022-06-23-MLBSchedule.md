---
layout: post
title: Automating and Extracting MLB schedule, linueup, and SP each day
subtitle: John F. Adamek
cover-img: /assets/img/baseball.jpg
tags: [test]
comments: true
---

## Introduction

This was a fun data cleanning project I did in R which I then automated each morning allowing me to view it once I got on the PC. I had spent a lot of time working with Bill Petti's baseballr package and processing data to be used as part of a number of algorithms I've created. These were all for 'fun' purposes (I am a die hard baseball fan) and I have come back to this code when seeking to run new predictive models or look at different statistical techniques. Here I am simply going over one way to clean the data from baseballr to extract today's MLB schedule, the probable pitchers, and starting lineups. Then you can do whatever you wish with the data! More information on the baseballr package documentation can be found below:

[baseballr](https://billpetti.github.io/baseballr/)

Library

```{r}
library(baseballr)
```

Today's date. You can do this for any other date in the past but the below will use today's date

```{r}
# DATE
date <- Sys.Date()
#date <- "2022-06-23"
```

## Pre Processing 

**Functions**

Next we need to create some functions of our own to make life a whole lot easier. The first function is to pull in the **game ID** for all of today's games.

```{r}
# Today's Game ID
games <- function(date){
  get_game_pks_mlb(date, level_ids = 1)
}
```

We then create a function to capture todays **starting pitcher**.

```{r}
# Today's Starting Pitcher
## Away SP
away_sp <- function(df) {
  output <- vector("numeric", length(df))
  for (i in seq_along(df)) {
    output[i] <- get_probables_mlb(df[[i]])[1,3]
  }
  output
}
## Home SP
home_sp <- function(df) {
  output <- vector("numeric", length(df))
  for (i in seq_along(df)) {
    output[i] <- get_probables_mlb(df[[i]])[2,3]
  }
  output
}
```

Lastly, we create a function to capture today's **starting lineup**

```{r}
## Today's Lineup
lineup <- function(df) {
  output <- list()
  for (i in seq_along(df)) {
    output[[i]] <- get_batting_orders(df[[i]])
  }
  output
}
```

## Today's Games

Now we're ready to use these functions and find out what games are played today!

```{r}
# Todays Games
##Pulling in Today's Game Information
var_keep <- c("game_pk","gameType", "officialDate", "dayNight", "scheduledInnings",
              "seriesGameNumber","status.detailedState","teams.away.score",
              "teams.away.leagueRecord.pct", "teams.away.team.id",
              "teams.away.team.name","teams.home.score", "teams.home.leagueRecord.pct",
              "teams.home.team.id", "teams.home.team.name", "venue.id", "venue.name")
todays_games <- games(date)
todays_games <- todays_games[, var_keep]

head(todays_games)
```

## Today's Schedule

The data is pretty messy so we need to do a bit of cleaning. Below we begin to pull data in

```{r}
todays_schedule <- data.frame(
  Date = todays_games$officialDate,
  GameID = todays_games$game_pk,
  HomeTeam = todays_games$teams.home.team.name,
  AwayTeam = todays_games$teams.away.team.name,
  HomeSP = unlist(home_sp(todays_games$game_pk)),
  AwaySP = unlist(away_sp(todays_games$game_pk)),
  HomeScore = todays_games$teams.home.score,
  AwayScore = todays_games$teams.away.score
)

# Today's Schedul
kable(todays_schedule[,c(1,3:6)])
```


## Today's Lineup's

We do a bit more cleaning...


```{r}
todays_lineup <- lineup(todays_games$game_pk)

# Today's lineup
kable(todays_lineup[1])

```

DONE! This will give you today's schedule & starting pitchers (todays_schedule) as well as the starting lineup for each game (todays_lineup). 







