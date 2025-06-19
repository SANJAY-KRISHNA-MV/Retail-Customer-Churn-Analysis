Phase 4: Modeling & Evaluation
Phase Overview
This phase focused on building, training, and rigorously evaluating machine learning models to predict customer churn. It encompassed preparing the feature-engineered data, splitting it into training and testing sets, training multiple classification algorithms, assessing their performance using a range of metrics, performing hyperparameter tuning on the best candidate model (Gradient Boosting), and identifying key features influencing churn.

1. Data Preparation for Modeling
The customer_churn_features.csv dataset, generated in Phase 3, was loaded. This dataset contains customer-level features (RFM, Tenure, encoded Country) and the binary is_churned target variable.

Feature-Target Split: The dataset was divided into features (X) and the target variable (y). The Customer ID column was excluded from features as it is an identifier, not a predictive attribute.

Train-Test Split: The data was split into training (75%) and testing (25%) sets. To ensure that the proportion of churned and non-churned customers was maintained in both sets, stratify=y was used during the train_test_split. A random_state was set for reproducibility.

2. Model Selection and Initial Training
Three distinct machine learning classification algorithms were chosen for initial evaluation:

Logistic Regression: A linear model, serving as a robust baseline due to its interpretability and simplicity. A StandardScaler was applied within a Pipeline for this model, as linear models are sensitive to feature scaling.

Random Forest Classifier: An ensemble tree-based model known for its high performance, robustness to outliers, and ability to capture non-linear relationships. Tree-based models generally do not require feature scaling.

Gradient Boosting Classifier: Another powerful ensemble tree-based model, often delivering state-of-the-art performance by sequentially building trees and correcting errors of preceding trees.

Each model was trained on the X_train and y_train sets.

3. Model Evaluation Metrics
After initial training, each model's performance was evaluated on the X_test set using several key metrics relevant to churn prediction:

Accuracy: Overall proportion of correct predictions.

Precision: The proportion of correctly predicted churners out of all instances predicted as churners. Important for minimizing false positives.

Recall (Sensitivity): The proportion of correctly predicted churners out of all actual churners. Crucial for identifying as many true churners as possible.

F1-Score: The harmonic mean of Precision and Recall, providing a balanced measure, especially useful for imbalanced datasets.

ROC-AUC (Receiver Operating Characteristic - Area Under the Curve): Measures the model's ability to distinguish between churned and non-churned customers across all possible classification thresholds. A higher value indicates better discrimination. ROC curves were plotted for visual assessment.

Confusion Matrix: Provides a detailed breakdown of true positives, true negatives, false positives, and false negatives.

Initial ROC-AUC Scores (Out-of-the-box):

Logistic Regression: 0.7949

Random Forest: 0.7818

Gradient Boosting: 0.8120

Based on these initial runs, the Gradient Boosting Classifier demonstrated the highest ROC-AUC score, indicating strong discriminatory power even before hyperparameter tuning. This led to the decision to select Gradient Boosting as the primary model for further optimization.

For this project, Recall and ROC-AUC were prioritized to ensure the model effectively identifies a high proportion of actual churners and provides excellent overall discriminative ability, which is vital for proactive retention strategies.

4. Hyperparameter Tuning (Gradient Boosting)
Hyperparameter tuning was performed on the Gradient Boosting Classifier using GridSearchCV. The goal was to find the optimal combination of parameters (n_estimators, learning_rate, max_depth, subsample) that maximize the roc_auc score through cross-validation on the training set.

The GridSearchCV successfully identified the optimal parameters for the Gradient Boosting model.

Best Tuned Gradient Boosting Model Performance on Test Set:

Accuracy: 0.7517

Precision: 0.7536

Recall: 0.8342

F1-Score: 0.7919

ROC-AUC: 0.8121

Confusion Matrix:

True Negatives (Correctly predicted non-churners): 369

False Positives (Predicted churners, but are non-churners): 204

False Negatives (Predicted non-churners, but are actual churners): 124

True Positives (Correctly predicted churners): 624

These results indicate that the tuned Gradient Boosting model performs well in identifying actual churners (high recall) while maintaining good precision and overall discriminative power (ROC-AUC).

5. Feature Importance Analysis
The feature_importances_ attribute of the best-tuned Gradient Boosting model was utilized to understand the relative contribution of each feature to the model's predictions.

Features were ranked by importance, providing insights into which aspects of customer behavior are most strongly associated with churn.

A bar plot visualized the top 15 most important features.

This analysis is crucial for deriving actionable business insights and translating model findings into practical retention strategies. For instance, high importance of Recency would suggest targeting customers who haven't purchased recently.

6. Model Saving
The best-performing model, the Tuned Gradient Boosting Classifier, was saved using joblib to the models/ directory as best_churn_model.joblib. This serialized model can be easily loaded for future predictions or deployment.

This concludes Phase 4, resulting in a robust, evaluated churn prediction model and key insights into churn drivers.