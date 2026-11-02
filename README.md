# Healthcare Loyalty Program Feasibility

MIS 5480 Data Wrangling Project, Iowa State University

Can a retail style loyalty rewards program encourage healthier behavior and preventive care? This team project looked at the question from the data side, using two public Kaggle datasets, and from the legal and ethical side.

## Data

Both datasets are public on Kaggle. The raw files are not included here. Download them and place them next to the notebook.

* Medical insurance costs. 1,338 rows with age, sex, BMI, children, smoker status, region and charges.
* Electronic sales, Sep 2023 to Sep 2024. 20,000 rows with customer age, loyalty membership, product, price, quantity and add on spending.

The two datasets are unrelated records, not the same people. They stand in for the two sides of a loyalty program: healthcare cost drivers and retail membership behavior.

## What I did

1. Cleaned both datasets. Standardized column names, converted yes and no fields to binary flags, and checked for missing values. Neither dataset had any in the columns used.
2. Explored spending and charge distributions and correlations.
3. Built a linear regression to predict insurance charges.
4. Built a logistic regression to predict loyalty membership.
5. Designed a six table relational database for a loyalty program (members, healthcare claims, retail transactions, products, loyalty accounts and recommendation events). The diagram is in the report.
6. Built a rule based recommender that suggests incentives by spending level and age group. Examples are in the report.
7. Reviewed the legal and compliance side, including HIPAA and the Anti Kickback Statute.

Cursor and ChatGPT were used for coding support. All outputs were checked by hand.

## Results

**Insurance charges (linear regression).** Test set RMSE is 5,796.77 and R squared is 0.78.
* Smoking is the strongest driver of charges. Its correlation with charges is 0.79, against 0.30 for age and 0.20 for BMI.
* Smokers pay an average of 32,050 against 8,434 for non smokers, about 3.8 times as much.

**Loyalty membership (logistic regression).** The model could not predict membership.
* Members are 21.7 percent of rows.
* The model predicts that no one is a member. It scores 0.779 accuracy, but its F1 score is 0.0 and its cross validated F1 is 0 on every fold. Always guessing non member would score 0.783.
* A balanced version reaches an F1 of 0.31 and a ROC AUC of 0.49, which is no better than chance.
* Members and non members spend almost the same. Average spend is 3,137 for members and 3,192 for non members.
* The correlation between loyalty and every other column is close to zero.

What this means: in this dataset, loyalty status does not separate customers by spending, price, quantity, add on spending or age. A real program would need stronger signals, such as visit frequency, claim history or how the customer enrolled.

## Note on the original report

The original written report described the loyalty model as having an F1 score of about 0.78 and described loyalty members as spending less with more stable behavior. When I reran the analysis for this repository, those figures did not hold. The 0.78 is the accuracy, not the F1. The numbers above come from the notebook in this repository and can be reproduced.

## Limitations

* The two datasets describe different people, so no individual level link between healthcare and retail behavior can be tested.
* The loyalty field is a simple yes or no flag with no information about how or why people joined.
* The recommender is rule based and was not tested against real outcomes.

## Files

* healthcare_loyalty_analysis.ipynb reruns the cleaning, both models and the checks above.
* The report PDF is the full written project, including the database design and the legal review.

## Tools

Python, pandas, scikit learn, seaborn, Jupyter Notebook
