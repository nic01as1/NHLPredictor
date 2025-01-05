NHLPredictor
Predicting NHL Game Outcomes
The goal of this project is to develop a model that predicts NHL game outcomes with an edge over traditional betting sites, which often rely on human sentiment and can be biased.

Future Improvements
There are several areas for optimization in this project:

Incorporating betting odds into the model.
Adjusting the weighting of features or adding/removing values.
Automating data scraping from sports websites.
Creating a user-friendly interface.
Project Overview
Data Source:

The project starts with data extracted from an Excel file containing historic NHL games from the 2024-2025 season.
Additional data manipulation is currently performed in Excel but could be automated in Python in the future.
Home Advantage:

Home teams historically have a higher win rate due to factors like travel, crowd support, and familiarity with the venue.
In this dataset, the overall home win rate is 53.6%.
Objective:

Beat the baseline home win rate using machine learning models.
Track model performance throughout the season and make incremental improvements.
Model Evaluation
The primary evaluation metric is the F1 Score, which balances precision and recall to measure model quality.

Baseline F1 Score (assuming the home team always wins): 0.37.
Model Development
Feature Engineering:

Home/Visitor Win Rate Over Last 10 Games: F1 = 0.51.
Season Win Rate for Home and Away Teams: F1 = 0.5096.
Home Team is Stronger (Ranking Comparison): F1 = 0.5226.
Encoding Teams:

Teams were one-hot encoded (32 teams) and combined with other features.
Decision Tree Model: F1 = 0.5654.
Random Forest Model: F1 = 0.5823.
Model Optimization:

Merging team encodings with additional features improved performance:
Decision Tree Model: F1 = 0.5032.
Random Forest Model (Optimized): F1 = 0.6381.
Final Model:

The optimized Random Forest model achieves an F1 score of 0.6381, indicating "decent" performance in predicting game outcomes.
Results
Improvement in F1:
Baseline F1 Score ≈ 0.37.
Final Model F1 Score = 0.63.
Absolute Improvement: 26%.
Relative Improvement in Accuracy:
Assuming accuracy aligns closely with F1, the model likely predicts ~63% of games correctly, compared to the 53.6% baseline.
Approximate Accuracy Improvement: 9.4%.
Prediction Workflow
The final code takes two inputs:

Home team.
Away team.
The model predicts the outcome of the game using the trained Random Forest model.

Acknowledgments
Shout out to ChatGPT and some indians dudes on the internet for assisting with model development and code implementation!

