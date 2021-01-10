# Introduction

The aim of this assignment is to analyse the effect of different demographical factors on Brexit Vote. The output variable is called voteBrexit, and gives a TRUE/FALSE answer to the question ‘did this electoral ward vote for Brexit?’ (i.e. did more than 50% of people vote to Leave?). Given a binomial nature of the outcome variable, the best model to for this task is a logistic regression.  

The 5 possible input variables are:
- **abc1**: proportion of individuals who are in the ABC1 social classes (middle to upper class)
- **medianIncome**: the median income of all residents
- **medianAge**: median age of residents
- **withHigherEd**: proportion of residents with any university-level education
- **notBornUK**: the proportion of residents who were born outside the UK

These are normalised so that the lowest value is zero and the highest value is one. Data is analysed using R software.


# 1. Logistic regression models using all of the available inputs. 

Uploading data and running the model.

      brexit=read.csv("brexit(2).csv") 
      mymodel=glm(formula=voteBrexit~abc1+notBornUK+medianIncome+medianAge+withHigherEd,family=binomial, data=brexit) 
      summary(mymodel)


![logist](https://user-images.githubusercontent.com/61549398/104131523-be59db80-536e-11eb-9cad-6e4586f85ab8.png)


**Interpretation:**

The outcome variable answers the question whether a person voted for Brexit or not. The True outcome is in case when people voted to leave while False if they voted to remain.
This is important when interpreting the magnitude of coefficients. If the coefficient is negative it means that an increase in a given variables is not associated with an increase in the probability of voting for Leave, while positive sign of coefficient is associated with the Leave vote.

**ab1**- The sign of the coefficient is positive therefore an increase in ab1 is associated with an increase in the probability of voting for Leave. For every unit increase in ab1, the log odds of Brexit Vote increases by 17.5780. 

**notBornUK** – The sign of the coefficient is again positive therefore the fact that a person was not Born in UK is associated with the Leave vote. For every unit increase in notBornUK, the log odds of Brexit Vote increases by 5.6861. 

**medianIncome**- The sign of the coefficient is negative therefore increase in median income is associated with a decrease in the probability of Leave vote. In other words, people with higher income voted for Remain. For every unit increase in medianIncome, the log odds of Brexit Vote decreases by 6.3857.

**medianAge**- The sign of the coefficient is positive therefore increase in median age is associated with an increase in the probability of Leave vote. For every unit increase in medianAge, the log odds of Brexit Vote increases by 5.9209. 

**withHigherEd**- The sign of the coefficient is negative therefore increase in higher education is not associated with an increase in the probability for the Leave vote. For every unit increase in withHigherEd, the log odds of Brexit Vote decreases by 26.7443. 


# 2. Calculating the value of each coefficient estimate with a 95% confidence interval.

The code below was used to calculate the value of the confidence interval for the first variable abc1. Confidence intervals for other variables have been calculated in the same way and the value for all confidence intervals is presented below.

                  zc=qnorm(0.975) 
                  estimate_abc1=summary(mymodel)$coefficients[2,1] 
                  standard_error_abc1=summary(mymodel)$coefficients[2,2] 
                  CI_min_abc1=estimate_abc1-zc*standard_error_abc1 
                  CI_max_abc1=estimate_abc1+zc*standard_error_abc1 
                  
                  print(paste("Abc1 Min: ", CI_min_abc1)) 
                  ## [1] "Abc1 Min: 11.8717241841725" 
                  print(paste("Abc1 Max: ", CI_max_abc1)) 
                  ## [1] "Abc1 Max: 23.2842718890574"
                  
![print](https://user-images.githubusercontent.com/61549398/104131528-c0bc3580-536e-11eb-80a9-771e40649b1f.png)

In order to decide which variable has the strongest effect, I created a copy of my current dataset and standardized the values of explanatory variables using the function scale( ) in R before running the model. This allowed me to directly compare estimates of the coefficients as now they use the same scale. The coefficient with the greatest absolute value is the one which has the strongest effect.

The results are as follows:

                  summary(mymodel2)
                  
 ![log2](https://user-images.githubusercontent.com/61549398/104131649-9d45ba80-536f-11eb-8bc6-555170f8d398.png)
 
 From the above output we can say that the variable which has the strongest effect is withHigherEd because it has the highest absolute value. This is followed by variable called abc1.
 
On the other hand, the variable with the weakest effect is medianIncome.

# 3. Using aic to perform a model selection and determine which factors are useful to predict the result of the vote.

A ‘greedy’ input selection procedure was used as follows: 

- selecting the best model with 1 input;
- fixing that input and selecting the best two-input model
- selecting the best three-input model containing the first two inputs you chose, etc. 

At each stage the quality of fit using aic is evaluated and stopped if this gets worse.

                  mymodel_abc=glm(formula=voteBrexit~abc1,family=binomial, data=brexit) summary(mymodel_abc)
                  
                  ## Call: ## glm(formula = voteBrexit ~ abc1, family = binomial, data = brexit) ## ## AIC: 377.54
                  
                  



