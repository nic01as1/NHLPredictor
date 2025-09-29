
# **NHLPredictor**
## **Predicting NHL Game Outcomes**

The goal of this project is to develop a model that predicts NHL game outcomes with an edge over traditional betting sites, which often rely on human sentiment and can be biased.
### **Project Overview**
1. **Data Source**:
   - Ranking of teams : https://www.hockey-reference.com/leagues/NHL_2025.html#all_stats
   - Games of the season infos : https://fixturedownload.com/results/nhl-2024

2. **Home Advantage**:
   - Home teams historically have a higher win rate due to factors like travel, crowd support, and familiarity with the venue.
   - In this dataset, the overall home win rate is **53.6%**.

3. **Objective**:
   - Beat the baseline home win rate using machine learning models.
   - Make Money using probability arbitrages.
   - Track model performance throughout the season and make incremental improvements.
     
### **Model Evaluation**
The primary evaluation metric is the **F1 Score**, which balances precision and recall to measure model quality.
- **Baseline F1 Score** (assuming the home team always wins): **0.3891**.
  
# **V3**

## **Improvements from V2**
- Moved to a **two–model core**: **Logistic Regression** and **MLP** (neural network).
- Adopted **time-aware cross-validation (`TimeSeriesSplit`)** and **refit on Brier score** to directly optimize probability quality for betting.
- Added **per-model feature selection inside pipelines** (no manual slicing):
  - Logistic: **RFECV** (name-preserving) → `StandardScaler` → `LogisticRegression`
  - MLP: **SelectKBest(mutual_info)** → `StandardScaler` → `MLPClassifier(early_stopping=True)`
- Tested **probability calibration** (sigmoid & isotonic) and **kept only if Brier improved**.
- Implemented a **simple ensemble** of Logistic + MLP (default **50/50** averaging); optional weight tuning if it consistently lowers Brier.
- Added **team-name normalization** for Betway scraping (aliases + light fuzzy match) to prevent “unknown team” skips.


## **Training Protocol**
- **Split**: chronological (first 80% train, last 20% holdout).
- **CV**: `TimeSeriesSplit` (5–6 folds), no shuffling.
- **Scoring (multi-metric)**:
  - Primary: **Brier** (↓ better)
  - Secondary: **ROC AUC** (↑), **F1 (weighted)** (↑)
- **Refit**: best by `neg_brier_score`.

## **Core Features**
Computed from history up to (but not including) game date—same logic as V1/V2:
- **Rank Difference**
- **Last 10 Wins** (difference)
- **Home/Away Played Yesterday**
- **Home Advantage** (win-rate difference)
- **Win Streak Impact** (streak difference)
- **Rest Days Since Last Game** (home & away)
- **Opponent Strength** (difference of recent avg opponent ranks)

> Feature selection is **per-model** inside the pipelines; no manual lists required at inference.

## **Results (Holdout)**
- **Logistic (L2)** — F1: **0.6306**, ROC AUC: **0.6830**, **Brier: 0.2209**  
- **MLP** (kept calibration only when helpful) — F1: **0.6386**, ROC AUC: **0.6744**, **Brier: 0.2219**  
- **Ensemble Avg(Logistic, MLP)** — **Brier: 0.2203**, ROC AUC: **0.6809**, F1: **0.6375**

> Compared to the “always home” baseline Brier (~0.4375), V3 probabilities are **substantially better calibrated** (~0.22).

## **Odds & Betting**
- **Scraping**: Betway via Selenium + BeautifulSoup (same flow as V2).
- **Normalization**: Map sportsbook aliases (e.g., “LA Kings” → “Los Angeles Kings”) and fuzzy-match to historical team names.
- **Bet Sizing**: **Kelly Criterion** on the ensemble probability; bankroll scaling if total exposure exceeds bankroll.
- **Outputs**: save `predictions.csv` including probabilities, Kelly fractions, and final bet sizes.

## **Artifacts Saved**
- `model_Logistic_TUNED_HEAVY.joblib`
- `model_MLP_TUNED_HEAVY.joblib`
- (Optional) `model_Ensemble_Avg.joblib`
- `selected_features_tuned.json` (for audit; selectors live inside pipelines)

## **Prediction Workflow (V3)**
1. **Scrape** upcoming games + odds from Betway.  
2. **Normalize** team names to match historical data.  
3. **Build features** (the 9 numeric columns) from latest historical snapshots.  
4. **Predict** with **both models** and the **50/50 ensemble** (or tuned weight).  
5. **Compute Kelly** stakes and apply bankroll scaling.  
6. **Export** results to `predictions.csv`.

## **Future Improvements**
- Combine **2025 + 2026** seasons with **`sample_weight`** (e.g., weight **2.0** for 2026) and reuse tuned hyper-parameters.
- Add rolling **calibration plots** and **drift monitoring** (Brier/ROC over time).
- Explore added context (goalies, injuries) and/or team identity via a **`ColumnTransformer`**, then retrain with the same protocol.

# **V2**

## **Improvements from V1**
- Incorporated **Kelly Criterion** for betting size  
- Adjusted the **model data selection** (Permutation and Data Importance)  
- Added **scraping for data inputs** (Selenium and BeautifulSoup)  
- Simplified the **model optimization process**  
- Added **explicative variables**  
- **Easier input process** for user interaction
  
## **Future Improvements**
- **Neural Network & Logistic Regression Models** for enhanced prediction accuracy.  
- **Automated Scraping of Odds & Games from Betway** for real-time data updates.  
- **Live Model Updates & Performance Tracking** with automated retraining and accuracy monitoring.
  
---

## **Data Scraping Sources**
### **Extracted Data:**
- **Ranking of teams:** [Hockey-Reference](https://www.hockey-reference.com/leagues/NHL_2025.html#all_stats) *(via Selenium)*  
- **Season game results:** [Fixture Download](https://fixturedownload.com/results/nhl-2024) *(via BeautifulSoup)*  

---

## **Data Cleaning & Formatting**
- Removed **unplayed games** by dropping rows with missing scores  
- Converted **date, scores, and win status** into proper formats  
- Sorted data **chronologically** for consistency  

---

## **Feature Engineering**
- **Last 10 Wins:** Tracks team performance in recent matches  
- **Played Yesterday:** Identifies back-to-back games  
- **Win Rate(At Home or Away):** Computes historical win percentage for home & away teams  
- **Overall Win Streak:** Tracks consecutive wins/losses  
- **Opponent Strength:** Measures the average ranking of past opponents in the last 10 days
- **Rest Days:** Determines time since last game  

---

## **Finding the Best Numerical Features**
- Selected key **performance metrics** as features  
- Tuned **Random Forest & Decision Tree** using **GridSearchCV**  
- Identified top features via **model-based & permutation importance**  
- Ranked models by **F1-score** and saved results for comparison  

 **Base Numerical Features F1 Score:** `0.4925`  

---

## **Encoding Team Data for Model Training**
- **Label Encoding** for home and away teams  
- **One-Hot Encoding** for categorical team data  
- Combined **encoded team features** with numerical features  

---

## **Optimizing Random Forest for Final Model with All Data**
 **Final Model F1 Score:** `0.5868`  

---

## **Match Prediction & Betting Strategy**
- **Team Selection & Encoding:** User selects home & away teams
- **Feature Extraction:** Retrieves and averages historical team stats  
- **Model Prediction:** Constructs input vector & predicts **win probabilities**  
- **Kelly Criterion Betting Strategy:** User inputs odds, and the model **calculates optimal bet size**  
- **Final Recommendation:** Displays **win probabilities, betting advice, and Kelly Criterion values**  

---

## **Prediction Workflow**
The final model requires **four user inputs**:  
1. **Home Team**  
2. **Away Team**  
3. **Home Team Odds** *(from Betway)*  
4. **Away Team Odds** *(from Betway)*  

The model predicts the **outcome** of the game using the trained **Random Forest model** and provides the **optimal bet percentage** based on the **Kelly Criterion**.
  
---
# **V1**
### **Future Improvements**
- Incorporate betting odds into the model.
- Adjust the weighting of features or add/remove values.
- Automate data scraping from sports websites.
- Create a user-friendly interface.



---

### **Model Development**
1. **Feature Engineering**:
   - **Home/Visitor Win Rate Over Last 10 Games**: F1 = **0.51**.
   - **Season Win Rate for Home and Away Teams**: F1 = **0.5096**.
   - **Home Team is Stronger (Ranking Comparison)**: F1 = **0.5226**.

2. **Encoding Teams**:
   - Teams were one-hot encoded (32 teams).
   - **Decision Tree Model**: F1 = **0.5654**.
   - **Random Forest Model**: F1 = **0.5823**.

3. **Model Optimization**:
   - Merging team encodings with additional features improved performance:
     - **Decision Tree Model**: F1 = **0.5032**.
     - **Random Forest Model (Optimized)**: F1 = **0.6381**.

4. **Final Model**:
   - The optimized Random Forest model achieves an F1 score of **0.6381**, indicating "decent" performance in predicting game outcomes.

---

### **Results**
- **Improvement in F1**:
  - Baseline F1 Score ≈ **0.37**.
  - Final Model F1 Score = **0.63**.
  - Absolute Improvement: **26%**.

- **Relative Improvement in Accuracy**:
  - Assuming accuracy aligns closely with F1, the model likely predicts ~63% of games correctly, compared to the **53.6% baseline**.
  - Approximate Accuracy Improvement: **9.4%**.

---

### **Prediction Workflow**
The final code takes two inputs:
1. Home team.
2. Away team.

The model predicts the outcome of the game using the trained Random Forest model.

---

### **Acknowledgments**
Shout out to ChatGPT and all the indians guys on youtube for assisting with model development and code implementation!


