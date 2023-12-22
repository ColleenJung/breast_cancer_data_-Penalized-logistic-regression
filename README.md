# breast_cancer_data and Penalized logistic regression
#logistic regression with Lasso penalty # elastic-net penalty #Classification error #Area under the ROC curve

## TCGA BRCA Data
- This data is obtained from Kaggle(https://www.kaggle.com/datasets/samdemharter/brca-multiomics-tcga/data). The data was processed from the TCGA (The Cancer Genome Atlas) breast cancer gene dataset. 
- We take 604 gene expressions from this data, and the goal is to model the outcome 'vital.status'. 
- This outcome is quite unbalanced. The data file can be downloaded from our course website.
<img width="481" alt="Screenshot 2023-12-21 at 4 53 10 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/a29d0016-a272-41c8-8e36-fa51e1086cb3">
<img width="928" alt="Screenshot 2023-12-22 at 12 45 39 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/4ea2e30f-e064-44da-9977-0eb57fcbe84c">

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
<img width="914" alt="Screenshot 2023-12-22 at 12 46 04 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/dcde6886-4597-456d-a90a-85eb1de5b554">
<img width="777" alt="Screenshot 2023-12-22 at 12 46 21 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/607f0697-af81-46db-a1bd-2ffd8728bc0a">

2. How many variables are selected based on this choice of λλ? Report the top three variables (excluding intercept) that has the largest absolute estimated coefficients.

<img width="922" alt="Screenshot 2023-12-22 at 12 46 35 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/afa0dcd3-9f35-4627-970d-9f72003de619">


The largest parameters are rs_APOB, rs_PIK3C2G and rs_ANO3. However, this is highly unstable because 1) the Lasso solution may not perform well in data with high colinearity, and 2) the data is very unbalanced, and there could be overfitting in certain folds.


3. Using this λ value and the fitted βˆ, what is the training data classification error if we use 0.5 as the cut-off value of predicted probability? Produce a confusion table of the training data. Do you think 0.5 is a good cut-off point for this question? Explain why.

<img width="910" alt="Screenshot 2023-12-22 at 12 46 55 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/22f02904-ef12-4e11-b948-a12035f7a66f">

In this case, 0.5 would not be a good cut-off point because the data is highly unbalanced. Very few observations are predicted to 1.

## Question 2
Using the same Lasso model you obtained previously, answer the following questions. Again, you can just use the training data (in-sample) prediction for this question.
<img width="925" alt="Screenshot 2023-12-22 at 12 47 13 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/018b600b-1930-4b01-b0fd-0b9f0c25bf90">

1. What is the sensitivity and specificity of this if we use 0.2 as the cut-off point? Choose a new cut-off value that would give a higher sensitivity, and report the confusion table and sensitivity associated with this new cut-off value.

<img width="929" alt="Screenshot 2023-12-22 at 12 48 46 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/d0445986-d73e-46fe-95b8-0a6e21263feb">

2. Produce the ROC curve plot associated with your logistic regression and report the AUC.
<img width="938" alt="Screenshot 2023-12-22 at 12 49 08 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/e5b44cb6-bdc7-4965-858e-64d51cb0a2f4">
<img width="922" alt="Screenshot 2023-12-22 at 12 49 18 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/a7dcb5b8-a8a2-494d-ae63-db31b68b5ac5">

## Question 3
Our previous model maybe unstable since the Lasso penalty is vulnerable to colinearity. Let’s consider using the elastic-net penalty. For this question, you need to consider a grid of α values seq(0, 1, 0.2). Hence both Lasso and Ridge regressions are included in the choice. Our goal is to select the model with the best combination of α and λ values. You can do this by first getting the best λ (lambda.min) for each fixed αα, and then compare their cross-validation error across different α. Complete the following:

<img width="912" alt="Screenshot 2023-12-22 at 12 49 37 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/b679a25e-a709-4bf3-b205-2062c00b86bd">

1. What are the best α and λ values?

Our best αα value is 0. We can evaluate this model:

3. Evaluate the best model by providing the ROC curve plot
<img width="996" alt="Screenshot 2023-12-22 at 12 50 38 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/2da143c4-3e3a-4a80-9b49-9ebce42b370b">

  
5. Report the AUC

<img width="930" alt="Screenshot 2023-12-22 at 12 50 47 PM" src="https://github.com/ColleenJung/breast_cancer_data_-Penalized-logistic-regression/assets/119357849/b9db392b-e80b-47eb-8d7a-a2415c669766">

The Ridge regression performs the best. This is possibly because high colinearity.
