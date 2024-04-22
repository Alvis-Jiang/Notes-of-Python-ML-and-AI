
# Machine learning and data use cases

## Machine learning principles

### 1. Supervised ML data structure

![1713663116801](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/b02ddbe4-b840-4537-ad24-54b993797ae4)

![1713663162883](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/d45371d1-a7be-4b4f-912e-65bfe6d3ee9f)


### 2. Unsupervised Learning 

![1713663272800](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/4f316a64-5584-4e3a-b1fe-807cbaa63601)


### Job roles, tools and technologies
![1713663673678](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/6138716c-6115-42e7-87ee-1d5f6ff8f649)


# Prediction vs. inference dilemma
#### Inference is less accurate than prediction models

## Inference (causal) models
- answer the the "why? " questions
- Optimizes for model interpretability v.s. performance
- Models try to detect patterns in observed data and draw causal conclusion 
![1713664879425](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/d0893f6c-623d-42ab-8786-6945e360b77b)


![1713664911656 1](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/fd43fc77-9ec6-4680-8a47-eabd10b175d5)




## Prediction models
### Types: 

- Classification - target variable is categorical
![1713665381059](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/961ba65f-7668-4747-81a0-e79b17df8075)


- Regression - target variable is continuous
![1713665431745](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/0b73a09d-c996-44ae-8e3b-fe6dea56516f)

## Model Training

![1713742556978](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/f0052471-5609-4bfd-aff1-51fab1f2fea0)

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
![1713743739211](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/454b05fb-8cc6-484a-9bd3-b9e5e002a341)

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

