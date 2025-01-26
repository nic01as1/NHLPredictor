
# **NHLPredictor**
## **Predicting NHL Game Outcomes**

The goal of this project is to develop a model that predicts NHL game outcomes with an edge over traditional betting sites, which often rely on human sentiment and can be biased.
### **Project Overview**
1. **Data Source**:
   - Ranking of teams : https://www.hockey-reference.com/leagues/NHL_2025.html#all_stats
   - Games of the season infos : https://fixturedownload.com/results/nhl-202

2. **Home Advantage**:
   - Home teams historically have a higher win rate due to factors like travel, crowd support, and familiarity with the venue.
   - In this dataset, the overall home win rate is **53.6%**.

3. **Objective**:
   - Beat the baseline home win rate using machine learning models.
   - Make Money using probability arbitrages
   - Track model performance throughout the season and make incremental improvements.
     
### **Model Evaluation**
The primary evaluation metric is the **F1 Score**, which balances precision and recall to measure model quality.
- **Baseline F1 Score** (assuming the home team always wins): **0.3891**.

# **V2**

## **Improvements from V1**
- Incorporated **Kelly Criterion** for betting size  
- Adjusted the **model data selection** (Permutation and Data Importance)  
- Added **scraping for data inputs** (Selenium and BeautifulSoup)  
- Simplified the **model optimization process**  
- Added **explicative variables**  
- **Easier input process** for user interaction  

---

## **Data Scraping Sources**
### **Extracted Data:**
- **Ranking of teams:** [Hockey-Reference](https://www.hockey-reference.com/leagues/NHL_2025.html#all_stats) *(via Selenium)*  
- **Season game results:** [Fixture Download](https://fixturedownload.com/results/nhl-202) *(via BeautifulSoup)*  

---

## **Data Cleaning & Formatting**
- Removed **unplayed games** by dropping rows with missing scores  
- Converted **date, scores, and win status** into proper formats  
- Sorted data **chronologically** for consistency  

---

## **Feature Engineering**
- **Last 10 Wins:** Tracks team performance in recent matches  
- **Played Yesterday:** Identifies back-to-back games  
- **Win Rate:** Computes historical win percentage for home & away teams  
- **Overall Win Streak:** Tracks consecutive wins/losses  
- **Opponent Strength:** Measures the average ranking of past opponents  
- **Rest Days:** Determines time since last game  

---

## **Finding the Best Numerical Features**
- Selected key **performance metrics** as features  
- Tuned **Random Forest & Decision Tree** using **GridSearchCV**  
- Identified top features via **model-based & permutation importance**  
- Ranked models by **F1-score** and saved results for comparison  

📌 **Base Numerical Features F1 Score:** `0.4925`  

---

## **Encoding Team Data for Model Training**
- **Label Encoding** for home and away teams  
- **One-Hot Encoding** for categorical team data  
- Combined **encoded team features** with numerical features  

---

## **Optimizing Random Forest for Final Model with All Data**
📌 **Final Model F1 Score:** `0.5868`  

---

## **Match Prediction & Betting Strategy**
- **Team Selection & Encoding:** User selects home & away teams (numerical + one-hot encoding)  
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
Shout out to ChatGPT and some indians guys on youtubefor assisting with model development and code implementation!





V2 Changes
Changed the binary variable (Higher rank) for the difference in ranks between the 2 teams (Increased the F1 by 0.0061)
