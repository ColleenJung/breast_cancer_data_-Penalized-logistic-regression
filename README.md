# breast_cancer_data and Penalized logistic regression
#logistic regression with Lasso penalty # elastic-net penalty #Classification error #Area under the ROC curve

## TCGA BRCA Data
- This data is obtained from Kaggle(https://www.kaggle.com/datasets/samdemharter/brca-multiomics-tcga/data). The data was processed from the TCGA (The Cancer Genome Atlas) breast cancer gene dataset. 
- We take 604 gene expressions from this data, and the goal is to model the outcome 'vital.status'. 
- This outcome is quite unbalanced. The data file can be downloaded from our course website.
<img width="481" alt="Screenshot 2023-12-21 at 4 53 10 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/a29d0016-a272-41c8-8e36-fa51e1086cb3">

### We are going to perform two models:
1. A logistic regression with Lasso penalty
2. A logistic regression with elastic-net penalty

### Evaluating using two different criteria:
1. Classification error
2. Area under the ROC curve
In all questions will use the cv.glmnet() function to obtain the best tuning parameter λ (and α when necessary).

## What is λ(lambda)?
- To get the best possible model, GLM and GAM need to find the optimal values of the regularization parameters 𝛼 and 𝜆
- When performing regularization, penalties are introduced to the model building process to avoid overfitting, to reduce variance of the prediction error, and to handle correlated predictors.
- The two most common penalized models are ridge regression and LASSO (least absolute shrinkage and selection operator).
- The elastic net combines both penalties. These types of penalties are described in greater detail in the Regularization section in GLM for more information.
- The larger lambda is, the more the coefficients are shrunk toward zero (and each other).
- When the value is 0, regularization is disabled, and ordinary generalized liner models are fit. 
- This option also works closely with the alpha parameter, which controls the distribution between the ℓ1 (LASSO) and ℓ2 (ridge regression) penalties.

The following table describes the type of penalized model that results based on the values specified for the lambda and alpha options.

<img width="605" alt="Screenshot 2023-12-21 at 9 39 31 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/5d9872b1-8927-4eca-8c81-84f4166fac21">
 
## Question 1
Our first task is to perform the logistic regression with Lasso penalty and obtain a sparse model. We will use the classification error as the criteria in the cross-validation to select the best model. Our collaborator that provides this data is interested in knowing which gene(s) are important for predicting 'vital.status'. 

1. What is the best λλ value (lambda.min) for your logistic regression?
2. How many variables are selected based on this choice of λλ? Report the top three variables (excluding intercept) that has the largest absolute estimated coefficients.
3. Using this λ value and the fitted βˆ, what is the training data classification error if we use 0.5 as the cut-off value of predicted probability? Produce a confusion table of the training data. Do you think 0.5 is a good cut-off point for this question? Explain why.

## Question 2
Using the same Lasso model you obtained previously, answer the following questions. Again, you can just use the training data (in-sample) prediction for this question.

1. What is the sensitivity and specificity of this if we use 0.2 as the cut-off point? Choose a new cut-off value that would give a higher sensitivity, and report the confusion table and sensitivity associated with this new cut-off value.
2. Produce the ROC curve plot associated with your logistic regression and report the AUC.

## Question 3
Our previous model maybe unstable since the Lasso penalty is vulnerable to colinearity. Let’s consider using the elastic-net penalty. For this question, you need to consider a grid of α values seq(0, 1, 0.2). Hence both Lasso and Ridge regressions are included in the choice. Our goal is to select the model with the best combination of α and λ values. You can do this by first getting the best λ (lambda.min) for each fixed αα, and then compare their cross-validation error across different α. Complete the following:

1. What are the best α and λ values?
2. Evaluate the best model by providing the ROC curve plot
3. Report the AUC


