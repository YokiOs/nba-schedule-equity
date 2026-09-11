# Research Project Plan: Does the NBA Schedule Create a Measurable Competitive Disadvantage?

**Subtitle:** Quantifying the Effects of Rest, Travel, and Schedule Density on Team Performance  
**Version:** 2.0 — Undergraduate Research Revision  
**Project type:** Sports analytics research project  
**Primary audience:** Undergraduate admissions readers, sports analytics faculty, and non-technical basketball audiences  

## 1. Project Purpose

This project studies whether the NBA schedule places some teams at a measurable competitive disadvantage. Instead of treating machine learning prediction as the main goal, it asks:

> After accounting for team strength, opponent strength, and home-court advantage, do short rest, travel, and dense schedules reduce team performance—and are these burdens distributed fairly across teams?

The project follows the student's research process:

**personal observation → hypothesis → data construction → statistical test → unexpected result or failure → revision → human feedback → final interpretation**

The central contribution is an interpretable **Schedule Fatigue Index**, not the number of algorithms used.

## 2. Why This Question Matters

NBA teams do not always enter games under comparable conditions. One team may have rested for two days, while its opponent played the previous night and traveled to another city. A team may also experience several games within a short period or a long sequence of road games.

These differences may affect team performance, competitive fairness, how fans interpret results, and how leagues and teams discuss schedule design and player workload.

This is an observational sports analytics project. It measures associations and will not claim that fatigue directly causes injuries or losses.

## 3. Research Questions

### Q1. How much does rest matter?

Compare team performance under back-to-back games, one day of rest, and two or more days of rest. Estimate whether differences remain after controlling for team strength, opponent strength, season, and home-court advantage.

### Q2. Does travel compound schedule fatigue?

Among teams playing on short rest, compare:

- home → home;
- away → home;
- away → away;
- away → away with long-distance travel or a time-zone change.

Test whether travel creates an additional performance disadvantage beyond short rest alone.

### Q3. Is schedule burden fairly distributed?

For each team and season, compare:

- number and percentage of back-to-backs;
- three games in four nights;
- estimated travel distance;
- long road-trip sequences;
- average Schedule Fatigue Index;
- number of high-fatigue games.

Determine whether some teams consistently face heavier schedule burdens than the league average.

## 4. Initial Hypotheses

| ID | Hypothesis | Expected result |
|---|---|---|
| H1 | Teams perform worse in the second game of a back-to-back. | Lower win probability and point differential |
| H2 | Travel makes the back-to-back disadvantage larger. | Away-to-away is worse than home-to-home |
| H3 | Dense stretches reduce performance relative to a team's recent baseline. | Negative association for three-in-four stretches |
| H4 | Schedule burden is not distributed perfectly evenly across teams. | Meaningful team-by-season differences |

These hypotheses must be written before the main analysis. Results that contradict them should be reported rather than hidden.

## 5. Scope

### 5.1 MVP scope

- NBA regular-season games only;
- recommended period: **2014–15 through 2024–25**;
- one row per team-game for fatigue construction;
- one row per game for matchup analysis;
- team-level game and schedule data;
- arena locations and estimated travel distance;
- descriptive analysis and interpretable statistical models;
- one transparent Schedule Fatigue Index;
- one public-facing visual explorer.

Ten seasons are sufficient to compare schedule patterns while keeping data collection and cleaning manageable. A longer historical range should be added only after the MVP works.

### 5.2 Out of scope

- direct injury prediction or medical claims;
- live betting or gambling recommendations;
- playoff games;
- deep learning;
- a full game-winner prediction product;
- collecting every possible box-score feature;
- mandatory Random Forest, XGBoost, SHAP, calibration, or a fixed ROC-AUC target;
- redesigning the full NBA schedule through optimization;
- causal claims based only on observational data.

## 6. Data Requirements

### 6.1 Required game data

- game ID and date;
- season;
- home and away teams;
- final score and point differential;
- game location;
- pre-game team strength measure;
- previous game date and location for each team.

### 6.2 Derived schedule variables

- `rest_days`;
- `is_back_to_back`;
- `is_three_in_four`;
- games played during the previous seven days;
- `travel_distance_from_previous_game`;
- `time_zone_change`, if reliable;
- `consecutive_road_games`;
- opponent schedule variables for the same game.

### 6.3 Performance outcomes

Primary outcomes:

- win/loss;
- point differential.

Optional secondary outcomes, used only if consistently available:

- offensive rating;
- defensive rating;
- effective field-goal percentage;
- turnover rate.

### 6.4 Data quality rules

- Every game must have a unique ID.
- Schedule variables must use only information known before tip-off.
- Current-game statistics cannot predict that same game.
- Team names, relocations, and arena locations must be standardized.
- Missing travel or time-zone information must be documented rather than silently estimated.
- At least 30 games must be manually checked against the source.

## 7. Analysis Design

### 7.1 Descriptive exploration

Create simple comparisons before building any model:

- win rate by rest category;
- average point differential by rest category;
- back-to-back performance by travel pattern;
- team-by-season schedule burden distribution;
- trends across seasons.

Every chart must answer one research question. Decorative dashboards and unrelated basketball statistics are excluded.

### 7.2 Control for obvious confounders

Raw win rates are not sufficient because weaker teams or road teams may have different schedules. Build interpretable models using:

- pre-game team-strength difference;
- opponent-strength difference;
- home-court indicator;
- rest category;
- travel distance;
- dense-schedule indicator;
- season controls.

Recommended models:

1. Logistic Regression for win probability.
2. Linear Regression for point differential.

Report effect sizes and uncertainty. For example:

> Holding team strength and home court constant, a traveling back-to-back was associated with an estimated X-percentage-point change in win probability.

### 7.3 Interaction analysis

Test only these pre-declared interactions:

- back-to-back × travel distance;
- back-to-back × home/away status;
- schedule density × travel distance.

Do not search through dozens of interactions simply to find a statistically significant result.

### 7.4 Robustness checks

- repeat the analysis with point differential instead of win/loss;
- compare recent team strength with season-to-date strength;
- cap unusually large travel values;
- run selected season or home/away subgroup analyses;
- check whether a small number of teams dominate the result.

### 7.5 Optional machine-learning extension

A single tree-based model may be added after the research analysis is complete. Its purpose is to test whether nonlinear relationships materially change the conclusion—not to maximize leaderboard performance.

Compare:

- baseline: team strength + home court;
- research model: baseline + schedule-fatigue variables.

The key question is whether schedule variables add meaningful out-of-time value. There is no required ROC-AUC target.

## 8. Schedule Fatigue Index

### 8.1 Purpose

The Schedule Fatigue Index converts several schedule conditions into a clear 0–100 indicator for a non-technical audience. It represents **schedule burden**, not medical fatigue, injury risk, or a player's physical condition.

### 8.2 MVP components

| Component | Example measurement |
|---|---|
| Short rest | Back-to-back or number of rest days |
| Schedule density | Games during the previous four/seven days |
| Travel burden | Distance since the previous game |
| Road burden | Consecutive road games |

### 8.3 Construction

1. Convert each component to a league-relative percentile or normalized score.
2. Begin with equal weights so the index is transparent.
3. Show each component's contribution for every high-burden game.
4. Ask basketball participants whether the components and weights match their experience.
5. If weights are revised, preserve and explain both versions.
6. Test whether conclusions remain similar under alternative reasonable weights.

The index should not be optimized solely to predict wins, because that would make it less interpretable as an independent schedule measure.

## 9. Human Feedback and Model Revision

After preliminary results, present them to **2–4 people with genuine sports experience**, such as a basketball coach, school-team player, rowing coach, physical-education teacher, or long-term basketball player.

Ask:

1. Does the estimated back-to-back effect match your experience?
2. Which part of the fatigue index seems unrealistic or incomplete?
3. What basketball factor could explain the result besides fatigue?
4. Which visualization is easiest or hardest to understand?

Keep short notes with permission. The final report must state what feedback was received, what changed, and what did not change and why. This is research feedback, not scientific validation by interview.

## 10. Public-Facing Visual Product

Create a simple **NBA Schedule Fatigue Explorer** that a non-technical reader can understand in a few minutes.

### Required views

1. **Team/season summary:** average index, back-to-back count, dense stretches, travel burden, league percentile and ranking.
2. **Game explanation:** both teams' scores, rest days, recent games, travel, and why the game was high burden.
3. **League fairness comparison:** burden distribution, highest/lowest teams, and limitations displayed near the result.

The MVP can use Streamlit. User accounts, live data, prediction APIs, and complex deployment infrastructure are unnecessary.

## 11. Final Deliverables

### A. Research report: 8–12 pages

1. Personal motivation and research question
2. Initial hypotheses
3. Data and schedule-variable construction
4. Descriptive results
5. Controlled statistical analysis
6. Unexpected findings and failed approaches
7. Human feedback and revisions
8. Schedule Fatigue Index
9. Fairness interpretation
10. Limitations and future research

### B. Reproducible GitHub repository

```text
nba-schedule-fatigue/
├── README.md
├── requirements.txt
├── data/
│   ├── README.md
│   └── processed/
├── notebooks/
│   ├── 01_data_quality.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   └── 03_statistical_analysis.ipynb
├── src/
│   ├── build_dataset.py
│   ├── build_schedule_features.py
│   └── build_fatigue_index.py
├── app/
│   └── streamlit_app.py
├── figures/
└── report/
```

### C. NBA Schedule Fatigue Explorer

A clear visual product for admissions readers, coaches, athletes, and fans. Traffic or viewer-count claims are not a success criterion.

### D. Research journal

Keep a short dated log of why the topic was chosen, early assumptions, data problems, failed variables or models, surprising results, feedback, and subsequent changes. This provides evidence of genuine intellectual ownership for applications and interviews.

## 12. Success Criteria

The project succeeds when:

- all three research questions receive evidence-based answers;
- schedule features contain no future-data leakage;
- the analysis controls for team strength and home court;
- the index is transparent and explainable component by component;
- results are interpreted rather than reduced to model scores;
- limitations and alternative explanations are stated honestly;
- human feedback produces a documented review or revision;
- another person can reproduce the core figures;
- a non-technical reader can understand the explorer without reading code.

Success is **not** defined by using many algorithms, reaching a fixed ROC-AUC, generating thousands of lines of code, or attracting a particular number of viewers.

## 13. Risks and Limitations

| Risk | Response |
|---|---|
| Strong and weak teams may have different schedules | Control for pre-game team and opponent strength |
| Travel distance is only a proxy for actual burden | State assumptions and test alternative definitions |
| Calendar rest is not physical recovery | Use “schedule burden” and avoid medical claims |
| Historical arena or time-zone data may be incomplete | Document missingness and simplify if necessary |
| NBA schedule policies change across seasons | Include season controls and show season-level results |
| Observational associations may be mistaken for causation | Use cautious language and discuss alternatives |
| Index weights may be subjective | Start transparently, collect feedback, and test sensitivity |

## 14. Implementation Plan

### Phase 1 — Question and pilot data (Week 1)

- Write a 150–250 word personal motivation statement.
- Freeze the three questions and four hypotheses.
- Collect one season as a pilot.
- Manually validate dates, locations, rest days, and travel for 30 games.

**Decision gate:** Continue only if rest and location variables can be constructed reliably.

### Phase 2 — Full dataset (Weeks 2–3)

- Expand to 2014–15 through 2024–25.
- Build rest, back-to-back, density, travel, and road-trip variables.
- Create quality tests and a data dictionary.

### Phase 3 — Analysis (Weeks 4–5)

- Produce descriptive comparisons.
- Fit the two interpretable models.
- Test the pre-declared interactions.
- Run focused robustness checks.
- Record unexpected and failed results immediately.

### Phase 4 — Index and feedback (Week 6)

- Build the first transparent Schedule Fatigue Index.
- Create preliminary charts.
- Conduct 2–4 feedback conversations.
- Revise the index or presentation only when justified.

### Phase 5 — Communication (Weeks 7–8)

- Build the Streamlit explorer.
- Write the 8–12 page report.
- Clean the repository and reproducibility instructions.
- Ask one technical and one non-technical reader to test the materials.

## 15. Immediate Next Actions

Before adding another model or feature:

1. Write why this question personally interests the student.
2. Confirm the three research questions and four hypotheses.
3. Obtain one season of schedule and game data.
4. Create the team-game table.
5. Calculate rest days and back-to-back status.
6. Manually verify 30 games.
7. Produce two charts: performance by rest category and back-to-back performance by travel pattern.
8. Decide whether the question is answerable before expanding the dataset.

Only after these steps should the project add ten seasons, regression controls, the index, interviews, or the explorer.
