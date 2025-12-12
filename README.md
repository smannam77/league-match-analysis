
# League of Legends Early Game Advantage Analysis
Predicting match outcomes from early game performance metrics in the 2022 LoL esports dataset.

## **Introduction**

League of Legends is a very popular 5v5 strategy game with where early game advantages such as kills or gold can snowball into potential wins. In competitive games, teams closely track performance metrics such as gold differential, XP lead, CS advantage, and kill counts to assess their standing at 
various stages of the game.

This project investigates whether **10 minute early game stats** can predict the *final 
outcome* of a game. Using match data from the 2022 season, I perform exploratory data 
analysis, run hypothesis tests, build predictive models, and evaluate fairness across 
different team conditions.

The question I am looking at in this analysis is:\
**How well do early game advantages like gold, XP, CS, or kills at the 10 minute mark correlate with the outcome of the game?**


## Data Cleaning and Exploratory Data Analysis

Before modeling, I cleaned the dataset and explored key early game features.

- I restricted the data to team level rows relevant to my question.
- I removed unused or redundant columns.
- I converted boolean columns into numerical indicators where appropriate.
- I focused on complete cases for important early game metrics like gold, XP, and CS differentials at 10 minutes.

### Overall Distributions

The plots below show how game length and early game stats are distributed across matches.

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

### Early Game Stats by Outcome

To better understand how early leads relate to match results, I compared stat distributions between wins and losses.

**Gold Differential by Outcome**

<iframe src="assets/eda_gold_vs_result_box.html" width="850" height="600"></iframe>

Winning teams tend to have higher gold leads at 10 minutes. The distribution for wins is shifted upward relative to losses, suggesting that early gold leads are associated with final victory.

**XP Differential by Outcome**

<iframe src="assets/eda_xp_vs_result_box.html" width="850" height="600"></iframe>

A similar pattern appears for XP: teams that win generally have higher XP at 10 minutes, reinforcing the importance of early experience advantages.

**CS Differential by Outcome**

<iframe src="assets/eda_cs_violin.html" width="850" height="600"></iframe>

CS (creep score) differential also tends to be higher for winning teams, though the separation is somewhat noisier. This still supports the idea that consistently outfarming the opponent contributes to winning.

### Relationships Between Early Game Features

**Gold vs XP Differential**

<iframe src="assets/eda_gold_vs_xp_scatter.html" width="850" height="600"></iframe>

Gold and XP differentials are positively correlated: teams that lead in one often lead in the other. Wins tend to cluster in the upper ight region, where teams have both a gold and XP advantage. This motivates using multiple early game stats together in a predictive model.




## **Assessment of Missingness**

I examined missing values in the dataset to understand whether they were likely to be random or systematic.

- I identified columns with non trivial amounts of missing data and focused on those most relevant to early game stats.
- I considered whether certain variables could be **Not Missing At Random (NMAR)** based on how the data is generated. For example, fields that might only be recorded under specific conditions.
- To assess whether missingness in a particular column depended on another variable, I ran permutation tests:
  - In one case, I found evidence that missingness *does* depend on another column, suggesting **Missing At Random (MAR)** behavior rather than completely independent missingness.
  - In another case, the permutation test supported the idea that missingness is independent of the chosen variable.

These results informed my decision to either filter out the rows with missing values or avoid using certain columns in  modeling, depending on how strongly the missingness seemed to be tied to other features.


## **Hypothesis Testing**

I  tested whether having an early gold or XP lead at 10 minutes is associated with a higher chance of winning a game. 

### Gold Lead vs No Gold Lead

I split teams into two groups:

- Teams with a **positive gold differential** at 10 minutes.
- Teams with a **gold deficit or even gold** at 10 minutes.

I used the difference in win rates between these two groups as the test statistic and ran the permutation test.

<iframe src="assets/hypothesis_gold_perm.html" width="850" height="600"></iframe>

The observed difference in win rates lies far in the tail of the null distribution, indicating that such a large gap is very unlikely to occur by random chance. This provides strong evidence that having an early gold lead is associated with a higher probability of winning a game.

### XP Lead vs No XP Lead

I repeated a similar analysis using XP differential at 10 minutes.

<iframe src="assets/hypothesis_xp_perm.html" width="850" height="600"></iframe>

As I said before, the observed difference in win rates between teams with an XP lead and those without is unlikely under the null distribution, suggesting that early XP advantages are also predictive of eventual win.

## **Framing a Prediction Problem**

Based on the EDA and hypothesis tests, I framed a supervised learning problem:

- **Prediction task:** Predict whether a team will **win or lose** the match.
- **Response variable:** A binary indicator of whether the team won.
- **Features:** Early-game statistics available at 10 minutes, including:
  - Gold differential
  - XP differential
  - CS differential
  - Kills at 10 minutes and related engineered features
- **Timing constraint:** All features are restricted to information that would be available at the 10-minute mark, mimicking an in-game prediction case.
- **Primary evaluation metric:** Accuracy, since the classes are relatively balanced and accuracy is easy to interpret. I also examined other metrics such as precision, recall, and F1 for additional context.

## **Baseline Model**

As a baseline, I fit a simple **logistic regression model** using a small set of core features like gold and XP differentials at 10 minutes. I used a scikit-learn pipeline to handle preprocessing and model fitting in a clean, efficent, and modular way.

The baseline model achieved pretty reasonable accuracy and provided interpretable coefficients showing how increases in gold and XP differential affect the log odds of winning. However, there was still room to improve performance by incorporating more features and non-linear relationships.


## Final Model

## Fairness Analysis
