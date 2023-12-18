# MAStatPlan
Planning Macular Atrophy Project Statistical Analysis

## Research question 

To understand the impact of macular atrophy involving the foveal centre point on visual acuity.

## Objective of the statistical analysis

To predict individual's visual acuity in logMAR, based on lesion area (LA) and time from baseline (TB) in a longitudinal setting. 

## Variables 

### Dependent variable 

Visual acuity in logMAR 

![image](https://github.com/hcha3232/MAStatPlan/assets/130141508/f07125a5-1028-47ef-a7aa-bb4c31fc30a9)



### Fixed effects 

Lesion area - To account for its impact on visual acuity <br><br>
![image](https://github.com/hcha3232/MAStatPlan/assets/130141508/1e350ce0-8b48-4287-8ff5-2638817c87e2)
<br>

Time from baseline - To account for the change of visual acuity over time 
  - I am confused whether I should put time as fixed effect variable, or just leave lesion area solely.

### Random effects

Individual baseline visual acuity 

## Model 

Linear mixed-effects model 

## Justification 

**Longitudinal data** - The study involves repeated measurements over time for the same subjects<br><br>
**Individual variability** - Patients are likely to have individual baseline differences and varied responses over time. This requires accounting for within-subject correlation and between-subject variation.<br><br>
**Random effects** - Random intercepts for patients are included to capture individual baseline variation in visual acuity that is not explained by the fixed effects.<br><br>
**Fixed effects** - Lesion area and time from baseline are treated as fixed effects to understand their average impact on visual acuity across the patient population.<br><br>

## Linear mixed effects model 

* Visual Acuityij = β0 + β1 × LesionAreaij + β2 × Timeij +ui + ϵij

Where

* Visual Acuityij = logMAR Visual acuity of patient i at time j
* β0 = intercept
* β1, β2 = fixed effect coefficients for lesion area and time from baseline, respectively 
* ui = random effects for each patient variations (baseline visual acuity)
* ϵij = residual error
<br>
* I am only accounting for random intercept not random slope, which could be beneficial to account for individual's varying trajectory of visual acuity 

## Sample data

| PatientID | DaysFromBaseline | LesionArea | VisualAcuity_logMAR |
| ---       | ---              | ---        | ---                 |
| 0 | 22 | 0.219 | 0.230 |
| 0 | 49 | 1.094 | 0.421 |
| 0 | 236 | 1.355 | 0.693 |
| 1 | 14 | 0.625 | 0.161 |
| 1 | 53 | 2.099 | 0.440 | 
| 1 | 89 | 2.915 | 0.658 |
| 1 | 242 | 3.319 | 0.838 |
| 2 | 65 | 2.738 | 0.482 |
| 2 | 129 | 3.634 | 0.650 |
| 2 | 301 | 3.880 | 0.905 |
`<br>
`<br>
`<br>
| 19 | 65 |0.756 | 0.210 |
| 19 | 101 | 2.199 | 0.387 |
| 19 | 168 | 2.888 | 0.569 |
| 19 | 179 | 4.364 | 0.842 |

## Fitting of the model 

```py
import statsmodels.api as sm
from statsmodels.regression.mixed_linear_model import MixedLM

# Preparing data for the mixed-effects model
X_mixed = train_data[['DaysFromBaseline', 'LesionArea']]
X_mixed = sm.add_constant(X_mixed)
y_mixed = train_data['VisualAcuity_logMAR']

# Group variable for random effects (each patient is a group)
groups = train_data['PatientID']

# Fitting the mixed-effects model
mixed_model = MixedLM(y_mixed, X_mixed, groups=groups).fit()
```

## Prediction with test data 

```py
# Preparing the test data for prediction
X_test = test_data[['DaysFromBaseline', 'LesionArea']]
X_test = sm.add_constant(X_test)

# Predicting using the mixed-effects model (fixed effects only)
test_data['PredictedVisualAcuity'] = mixed_model.predict(X_test)
```

![alt text](https://github.com/hcha3232/MAStatPlan/blob/main/Figure_1.png?raw=true)

<br>

### Limitation 




























