
# League of Legends Early Game Advantage Analysis
Predicting match outcomes from early-game performance metrics in the 2022 LoL esports dataset.

## Introduction

League of Legends is a competitive 5v5 strategy game where early advantages such as kills or gold 
can snowball into decisive wins. In professional play, teams closely track performance metrics such as 
gold differential, XP lead, CS advantage, and kill counts to assess their standing at 
various stages of the game.

This project investigates whether **10-minute early-game stats** can predict the *final 
outcome* of a match. Using match data from the 2022 season, I perform exploratory data 
analysis, run hypothesis tests, build predictive models, and evaluate fairness across 
different team conditions.

The question I am looking at in this analysis is:
How well do early game advantages like gold, XP, CS, or kills at the 10 minute mark correlate with the outcome of the game?


## Data Cleaning and Exploratory Data Analysis

Before modeling, I cleaned the dataset and explored key early-game features.

- I restricted the data to team-level rows relevant to my question.
- I removed unused or redundant columns.
- I converted boolean columns into numerical indicators where appropriate.
- I focused on complete cases for important early-game metrics like gold, XP, and CS differentials at 10 minutes.

### Overall Distributions

The plots below show how game length and early-game stats are distributed across matches.

**Game Length (minutes)**

<iframe src="assets/eda_game_length_hist.html" width="850" height="600"></iframe>

Most games fall into a moderate length range, with relatively few extremely short or extremely long games.

**Gold Differential at 10 Minutes**

<iframe src="assets/eda_gold_hist.html" width="850" height="600"></iframe>

Gold differential is centered near zero, which means many games start fairly even. However, the right tail shows matches where teams secure substantial early leads.

**XP Differential at 10 Minutes**

<iframe src="assets/eda_xp_hist.html" width="850" height="600"></iframe>

XP differentials follow a similar pattern to gold, with most games close to even and some matches where one team pulls ahead early.

### Game Outcomes

**Win/Loss Distribution**

<iframe src="assets/eda_result_distribution.html" width="850" height="600"></iframe>

This plot shows that the classes are relatively balanced, which is helpful for modeling because the classifier does not simply predict one outcome most of the time.

### Early-Game Stats by Outcome

To better understand how early leads relate to match results, I compared stat distributions between wins and losses.

**Gold Differential by Outcome**

<iframe src="assets/eda_gold_vs_result_box.html" width="850" height="600"></iframe>

Winning teams tend to have higher gold leads at 10 minutes. The distribution for wins is shifted upward relative to losses, suggesting that early gold leads are associated with final victory.

**XP Differential by Outcome**

<iframe src="assets/eda_xp_vs_result_box.html" width="850" height="600"></iframe>

A similar pattern appears for XP: teams that win generally have higher XP at 10 minutes, reinforcing the importance of early experience advantages.

**CS Differential by Outcome**

<iframe src="assets/eda_cs_violin.html" width="850" height="600"></iframe>

CS (creep score) differential also tends to be higher for winning teams, though the separation is somewhat noisier. This still supports the idea that consistently out-farming the opponent contributes to winning.

### Relationships Between Early-Game Features

**Gold vs XP Differential (Colored by Result)**

<iframe src="assets/eda_gold_vs_xp_scatter.html" width="850" height="600"></iframe>

Gold and XP differentials are positively correlated: teams that lead in one often lead in the other. Wins tend to cluster in the upper-right region, where teams have both a gold and XP advantage. This motivates using multiple early-game stats together in a predictive model.

---


## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
