# Graduation Early Warning

This project tests whether a school can identify, at the end of 9th grade, which students are likely to finish high school short of the 24 credits a diploma requires.

It compares five approaches, from a one-line rule a counselor could apply by hand to Random Forest and XGBoost. **The simple rule ranked students as well as any model tested**, and the more complex models did worse because they overfit.

## Key findings

- **A credits rule matches the best model.** Flagging students who earned fewer than 6 credits in 9th grade (the pace needed to reach 24) scored an AUC of 0.82, tied with logistic regression (0.81).
- **Random Forest and XGBoost overfit.** Both nearly memorized their training students (AUC 1.00 and 0.97) but scored only 0.75 and 0.76 on new cohorts.
- **The models found the real drivers.** Because the data is simulated, the true relationships are known, and logistic regression recovered them: course performance matters most, attendance least.

| Model | AUC | Recall | Precision |
|---|---|---|---|
| Credits rule (< 6 credits in 9th grade) | 0.82 | 0.63 | 0.77 |
| Logistic regression | 0.81 | 0.79 | 0.74 |
| XGBoost | 0.76 | 0.78 | 0.67 |
| Random Forest | 0.75 | 0.79 | 0.69 |
| Baseline (flag everyone) | 0.50 | 1.00 | 0.53 |

*Tested on the three most recent cohorts (119 students, 63 at risk). Recall and precision refer to at-risk students.*

## Approach

- **Synthetic data, real structure.** The original analysis used FERPA-protected records from an alternative high school. Notebook 01 simulates 450 students whose files match the real exports, including their duplicates, inconsistent labels, and leakage traps.
- **Early-warning timing.** Every feature is something a school knows at the end of 9th grade: GPA, pass rate, credits, attendance, and 8th-grade state test results.
- **Realistic testing.** Models train on older cohorts and are tested on newer ones, the way a district would use them. All preprocessing is fit on training data only.

## Notebooks

1. [**Generate synthetic data**](notebooks/01_generate_synthetic_data.ipynb): simulated district exports with known effects and planted data problems
2. [**Clean and explore**](notebooks/02_eda_and_cleaning.ipynb): cleaning, decoding the exports, and building one row per student
3. [**Model**](notebooks/03_model.ipynb): five models compared, then checked against the simulation's answer key

## Limitations

The outcome is earning 24+ credits, not graduation itself. The test set is small, and results describe simulated students, not a real school. Natural next steps are cross-validation across cohorts and tuning the flagging cutoff to counselor capacity.

## Running it

```
pip install -r requirements.txt
```

Run the notebooks in order from `notebooks/`. The synthetic data is already included. Real district data is never committed.
