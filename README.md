# Who Gets Justice? Unsolved Homicides in the United States, 1980-2014

CA2 CRISP-DM Analytics Project for DBMS71008 Data Analytics.

**Student:** Isabelle Lynch (T00264026)

## About the project

In the United States roughly one homicide in three is never cleared. This project looks at which homicide victims are least likely to see their case solved, and how much of that gap remains once we take into account how and where the killing happened (weapon, state, agency type, year).

The project follows the CRISP-DM process: business understanding, data understanding, data preparation, modelling, evaluation and deployment. A logistic regression is used as the main diagnostic tool to measure adjusted gaps between victim groups, with a random forest and gradient boosting as checks that the conclusions hold for more flexible models. The models are **not** intended to score or prioritise real cases.

## Dataset

- **Name:** Homicide Reports, 1980-2014
- **Source:** Murder Accountability Project (MAP), on Kaggle: https://www.kaggle.com/datasets/murderaccountability/homicide-reports
- **Original data:** FBI Uniform Crime Reporting programme, Supplementary Homicide Reports (SHR), plus records MAP obtained through Freedom of Information requests
- **Size:** 638,454 victim records, 24 columns

The data are released by the Murder Accountability Project for public use and are based on US federal government data. All credit for the data goes to the Murder Accountability Project and the FBI.

The dataset is not included in this repository because of its size. Download it from Kaggle (see below).

## Repository structure

```
WhoGetsJustice-DataAnalytics-2026/
├── data/
│ ├── database.csv
│ └── homicides_prepared.csv
├── notebook/
│ └── who-gets-justice.ipynb
├── requirements.txt
└── README.md
```

## Main findings

- **Leakage:** the offender fields (perpetrator sex, age, race, relationship) are only filled in when a case is solved. A model using just two of them reached 99.96% accuracy, so all offender fields were removed.
- **Overall:** about 30% of homicides in the data are unsolved, and this share barely changed between 1980 and 2014 even though the number of homicides fell.
- **Models:** gradient boosting reached a ROC AUC of 0.740, the random forest 0.736 and the logistic regression 0.696. All clearly beat the majority-class baseline at finding unsolved cases. Results were stable when training on 1980-2009 and testing on 2010-2014.
- **Who gets justice:** cases with Black victims were unsolved 7.4 percentage points more often than cases with White victims. After adjusting for weapon, state, agency type, year and other case details, the gap was still 6.3 points in the logistic regression and 4.8 points in gradient boosting.
- **The gap depends on the weapon:** for firearm killings the adjusted Black-White gap was 9.1 points, but for other weapons it was only 1.1 points.
- **Fairness:** the model's errors are not spread evenly between groups, which is one reason it should not be used to score individual cases.

The full details, charts and discussion are in the notebook.

## Limitations

- Reporting to the SHR is voluntary, so some agencies and years are missing.
- "Solved" means the agency reported an offender to the FBI, not that someone was arrested or convicted.
- The data have no information on witnesses, forensic evidence or detective workload, which are known to matter a lot for clearance.
- The gaps are adjusted associations, not causal effects.

## Responsible use

The results are meant for aggregate, oversight-level use (for example, deciding where to audit cold cases). They should not be used to make decisions about individual cases.

## References

The full reference list is at the end of the notebook. Main sources:

- Campedelli, G. M. (2024). Homicides involving Black victims are less likely to be cleared in the United States. *Criminology*, 62(1), 90-128.
- Wellford, C. and Cronin, J. (1999). *An Analysis of Variables Affecting the Clearance of Homicides: A Multisite Study*.
- Puckett, J. L. and Lundman, R. J. (2003). Factors affecting homicide clearances. *Journal of Research in Crime and Delinquency*, 40(2), 171-193.
- Murder Accountability Project. Homicide Reports, 1980-2014. Kaggle.
