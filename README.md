# MAStatPlan Overview
Planning Macular Atrophy Project Statistical Analysis

## Research question 

To understand the impact of macular atrophy involving the foveal centre point on visual acuity.

## Objective of the statistical analysis

To predict individual's visual acuity in logMAR, based on lesion area (LA), baseline lesion area (BLA), and time from baseline (T) in a longitudinal setting. 

## Variables 

### Dependent variable 

Visual acuity in logMAR (most between 0 and 1) 

![image](https://github.com/hcha3232/MAStatPlan/assets/130141508/f07125a5-1028-47ef-a7aa-bb4c31fc30a9)



### Fixed effects 

1. Lesion area - To account for its impact on visual acuity <br><br>
![image](https://github.com/hcha3232/MAStatPlan/assets/130141508/1e350ce0-8b48-4287-8ff5-2638817c87e2)
<br>

2. Baseline Area
<br>

3. Time from baseline - To account for the change of visual acuity over time 
<br>

Not sure whether I should put time and baseline lesion area as fixed effect variable, or just leave lesion area solely for ease of analysis. 

### Random effects

Individual baseline visual acuity 

## Model 

Linear mixed-effects model 

## Justification 

**Longitudinal data** - The study involves repeated measurements over time for the same subjects<br><br>
**Individual variability** - Patients are likely to have individual baseline differences in their visual acuities. This requires accounting for within-subject correlation and between-subject variation.<br><br>
**Random effects** - Random intercepts for patients are included to capture individual baseline variation in visual acuity that is not explained by the fixed effects.<br><br>
**Fixed effects** - Lesion area, baseline lesion area, and time from baseline are treated as fixed effects to understand their average impact on visual acuity across the patient population.<br><br>

## Linear mixed effects model 

* Visual Acuityij = β0 + β1 × LesionAreaij + β2 × BaselineAreaij + β3 × Timeij + ui + ϵij

Where

* Visual Acuityij = logMAR Visual acuity of patient i at time j
* β0 = intercept
* β1, β2, β3 = fixed effect coefficients for lesion area, baseline area and time from baseline, respectively 
* ui = random effects for each patient variations (baseline visual acuity)
* ϵij = residual error
<br>

## Sample data

- DaysFromBaseline / 100 for pseudo-standardisation
  <br>
  
| PatientID | DaysFromBaseline/100 | LesionArea | BaselineLesionArea | VisualAcuity_logMAR |
| ---       | ---              | ---        | ---                 | ---                |
| 0 | 0.0 | 1.607 | 1.607 | 0.127 |
| 0 | 0.03 | 2.379 | 1.607 | 0.239 
| 0 | 1.57 | 3.732 | 1.607 | 0.268
| 1 | 0.0 | 0.625 | 0.625 | 0.161 |
| 1 | 0.53 | 2.099 | 0.625 | 0.440 | 
| 1 | 0.89 | 2.915 | 0.625 | 0.658 |
| 1 | 2.42 | 3.319 | 0.625 | 0.838 |
| 2 | 0.0 | 2.738 | 2.738 | 0.482 |
| 2 | 1.29 | 3.634 | 2.738 | 0.650 |
| 2 | 3.01 | 3.880 | 2.738 | 0.905 |
`<br>
`<br>
`<br>
| 49 | 0.0 | 0.756 | 0.756 | 0.210 |
| 49 | 1.01 | 2.199 | 0.756 | 0.387 |
| 49 | 1.68 | 2.888 | 0.756 | 0.569 |
| 49 | 2.79 | 4.364 | 0.756 | 0.842 |

## Fitting of the model 
In python - Possible in R using lme4 package as well
<br>
```py
import statsmodels.api as sm
from statsmodels.regression.mixed_linear_model import MixedLM

# Preparing data for the mixed-effects model
X_mixed = train_data[['DaysFromBaseline', 'LesionArea','BaselineLesionArea']]
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
X_test = test_data[['DaysFromBaseline', 'LesionArea', 'BaselineArea']]
X_test = sm.add_constant(X_test)

# Predicting using the mixed-effects model (fixed effects only)
test_data['PredictedVisualAcuity'] = mixed_model.predict(X_test)
```

![alt text](https://github.com/hcha3232/MAStatPlan/blob/main/Figure_2.png?raw=true)

<br>

### Limitation 




























