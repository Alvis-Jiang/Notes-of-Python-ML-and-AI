
# Machine learning and data use cases

## Machine learning principles

### 1. Supervised ML data structure

![[1713663116801.png]]
![[1713663162883.png]]

### 2. Unsupervised Learning 

![[1713663272800.png]]

### Job roles, tools and technologies
![[1713663673678.png]]

# Prediction vs. inference dilemma
#### Inference is less accurate than prediction models

## Inference (causal) models
- answer the the "why? " questions
- Optimizes for model interpretability v.s. performance
- Models try to detect patterns in observed data and draw causal conclusion 
![[1713664879425.png]]


![[1713664911656 1.png]]


## Prediction models
### Types: 

- Classification - target variable is categorical
![[1713665381059.png]]

- Regression - target variable is continuous
![[1713665431745.png]]
## Model Training

![[1713742556978.png]]
### Model performance measurement
#### - Accuracy -- classification
#### - Error -- regression

**Precision** measures how many of the predictions of that class turned out to be true.
**Recall** measures how many of the total observations of that class the prediction was able to capture (or **recall**).
Regression **error** calculates how far away the prediction is from the observed number.

### Machine learning risks
#### - Low precision
#### - Low recall
#### - Large error

#### A/B testing:
![[1713743739211.png]]
### What if tests don't work

Get more data - business has to be involved
Build causal models to understand drivers
Run qualitative research (surveys etc.)
Change the scope of the problem: Narrow, Widen, Different question

## Machine learning mistakes
### Mistakes:
#### - Machine learning first
#### - Target variable definition
#### - Late testing, no impact
#### - Not enough data
#### - Feature selection

