# Introduction

The aim of this assignment is to analyse the effect of different demographical factors on Brexit Vote. The output variable is called voteBrexit, and gives a TRUE/FALSE answer to the question ‘did this electoral ward vote for Brexit?’ (i.e. did more than 50% of people vote to Leave?). Given a binomial nature of the outcome variable, the best model to for this task is a logistic regression. The results from the logistic regression will be then compared with the results achieved from running a decision tree model.

The 5 possible input variables are:
- **abc1**: proportion of individuals who are in the ABC1 social classes (middle to upper class)
- **medianIncome**: the median income of all residents
- **medianAge**: median age of residents
- **withHigherEd**: proportion of residents with any university-level education
- **notBornUK**: the proportion of residents who were born outside the UK

These are normalised so that the lowest value is zero and the highest value is one. Data is analysed using R software.


## 1. Logistic regression models using all of the available inputs. 

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


## 2. Calculating the value of each coefficient estimate with a 95% confidence interval.

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

## 3. Using aic to perform a model selection and determine which factors are useful to predict the result of the vote.

A ‘greedy’ input selection procedure was used as follows: 

- selecting the best model with 1 input;
- fixing that input and selecting the best two-input model
- selecting the best three-input model containing the first two inputs you chose, etc. 

At each stage the quality of fit using aic is evaluated and stopped if this gets worse.

                  mymodel_abc=glm(formula=voteBrexit~abc1,family=binomial, data=brexit) 
                  summary(mymodel_abc)
                 
                  ## Call: ## glm(formula = voteBrexit ~ abc1, family = binomial, data = brexit) ## ## AIC: 377.54
                  
                  
                  mymodel_not_born=glm(formula=voteBrexit~notBornUK,family=binomial, data=brexit)
                  summary(mymodel_not_born)
                  
                  Call: ## glm(formula = voteBrexit ~ notBornUK, family = binomial, data = brexit) ## ## AIC: 377.8
                  
                  
                  mymodel_income=glm(formula=voteBrexit~medianIncome,family=binomial, data=brexit)
                  summary(mymodel_income)
                  
                  Call: ## glm(formula = voteBrexit ~ medianIncome, family = binomial, data = brexit) ## ## AIC: 368.44
                  
                  
                  
                  mymodel_age=glm(formula=voteBrexit~medianAge,family=binomial, data=brexit)
                  summary(mymodel_age)
                  
                  ## Call: ## glm(formula = voteBrexit ~ medianAge, family = binomial, data = brexit) ## ## AIC: 401.28      
                  
                 
                  mymodel_education=glm(formula=voteBrexit~withHigherEd,family=binomial, data=brexit)
                  summary(mymodel_education)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd, family = binomial, data = brexit)
                  ## ## AIC: 313.56
                  
                  Call: ## glm(formula = voteBrexit ~ withHigherEd, family = binomial, data = brexit)
                  
                  
                  

                  
                  
                  
                  mymodel_education_abc1=glm(formula=voteBrexit~withHigherEd+abc1,family=binomial, data=brexit)
                  summary(mymodel_education_abc1)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1, family = binomial, ## data = brexit) ## ## AIC: 286.55           
                  
              
                  mymodel_education_notBorn=glm(formula=voteBrexit~withHigherEd+notBornUK,family=binomial, data=brexit)
                  summary(mymodel_education_notBorn)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + notBornUK, family = binomial, ## data = brexit) ## ## AIC: 310.3                  
                  
                  
                  mymodel_education_income=glm(formula=voteBrexit~withHigherEd+medianIncome,family=binomial, data=brexit)
                  summary(mymodel_education_income)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + medianIncome, family = binomial, ## data = brexit) ## ## AIC: 315.53
                  
                  
                  mymodel_education_age=glm(formula=voteBrexit~withHigherEd+medianAge,family=binomial, data=brexit)
                  summary(mymodel_education_age)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + medianAge, family = binomial, ## data = brexit) ## ## AIC: 303.31
                  
                  
                  
                  
                  
                  
                  
                  
                  mymodel_education_abc1_notBorn=glm(formula=voteBrexit~withHigherEd+abc1+notBornUK,family=binomial, data=brexit)
                  summary(mymodel_education_abc1_notBorn)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1 + notBornUK, family = binomial, ## data = brexit) ## ## AIC: 285.24
                  
                  
                  
                  mymodel_education_abc1_income=glm(formula=voteBrexit~withHigherEd+abc1+medianIncome,family=binomial, data=brexit)
                  summary(mymodel_education_abc1_income)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1 + medianIncome, ## family = binomial, data = brexit) ## ## AIC: 275.93
                  
                  
                  mymodel_education_abc1_age=glm(formula=voteBrexit~withHigherEd+abc1+medianAge,family=binomial, data=brexit)
                  summary(mymodel_education_abc1_age)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1 + medianAge, family = binomial, ## data = brexit) ## ## AIC: 271.93
                  
                  
                  mymodel_education_abc1_age_notborn=glm(formula=voteBrexit~withHigherEd+abc1+medianAge+notBornUK,family=binomial, data=brexit)
                  summary(mymodel_education_abc1_age_notborn)
                  
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1 + medianAge + ## notBornUK, family = binomial, data = brexit) ##
                  ## AIC: 269.11
                  
                  
                  
                  
                  
                  
                  mymodel_education_abc1_age_income=glm(formula=voteBrexit~withHigherEd+abc1+medianAge+medianIncome,family=binomial, data=brexit)
                  summary(mymodel_education_abc1_age_income)
                  ## Call: ## glm(formula = voteBrexit ~ withHigherEd + abc1 + medianAge + ## medianIncome, family = binomial, data = brexit) ## ## AIC: 266.95
                  
                  
                  
                  
                  
                  
We can see that with only one input variable, the model which minimizes AIC is the one which uses withHigherEd variable only, with AIC of 313.56. Therefore it will be used in stage 2 in which I will add one more additional explanatory variable.

The stage 2 revealed that abc1 led to the lowest AIC score of 286.55 when combined with withHigherEd, therefore for stage 3 variables called withHigherEd and abc1 will be used.
                  
The stage 3 revealed that medianAge gave the lowest AIC when combined with withHigherEd and abc1, leading to lowest AIC score which we obtained so far of 271.93                  
Stage 4 revealed that combination of the previous three variables (withHigherEd, abc1, medianAge) plus medianIncome gave the lowest AIC score of 266.95. However this value is still higher than in case of using the original model with all five explanatory variables. Therefore, we can conclude that the best model is the model which uses all five explanatory variables giving the AIC score of 259.39.                 
                  
                  
                  
 ## 4. Running a decision tree classification model. 
 
 

                  #install.packages("rpart") 
                  library("rpart")
                  
                  mytree=rpart(voteBrexit~ abc1+notBornUK+medianIncome+medianAge+withHigherEd, data=brexit, method="class")
                  
                  #install.packages("rpart.plot") 
                  library("rpart.plot")
                  
                  prp(mytree)
                  
![tree](https://user-images.githubusercontent.com/61549398/104133179-c9fecf80-5379-11eb-9377-139fb93f9560.png)

In decision tree models, each branch tests the outcome variable whether it outputs TRUE or FALSE. In our case, TRUE means that they voted for Brexit while FALSE means they did not.

The model tells us that the most informative feature is withHigherEd variable hence the first decision is based on that. If the value of variable withHigherEd is greater than 0.47, then this is associated with the Remain vote. If it’s less than 0.47, then it looks at another variable notBornUK to make a decision. If its value is greater than 0.43, then this is associated with the Remain vote, while if it is less ,then it looks at withHigherEd variable again. If in this case, the value of withHigherEd is lower than 0.31, this is associated with the vote of Leave, while if it is greater, then it looks at abc1 variable. If it is less than 0.41, then it is associated with the Remain vote while if it is greater than 0.41 it is associated with Leave vote.

## 5. Comparing decision tree model and your logistic regression model. 


For logistic regression model, we already established which variables give the strongest and weakest effect in part 1. This was done by checking the absolute value of coefficients after running the regression on standardized values of explanatory variables.

The features’ importance is represented below, with the most important feature being on the top.

- withHigherEd
- abc1
- medianAge
- notBornUK
- median Income

Decision tree model gives different priority to our features. Although both models agree that the most important factor is variable withHigherEd, the importance of remaining features is different for those two models.

In decision tree, the second most important factor is notBornUK, while according to logistic regression this is the second least important factor. Similarly, for logistic regression the third most important factor is medianAge, while for decision tree this variable is not even taken into account.

Both models attach slightly different level of importance for explanatory variables and also the direction of the effect is slightly different for the models. We can therefore say that both models explain the referendum vote differently.

In terms of logistic regression, an increase in higher education and median income decreases the probability of voting for Leave. However an increase in all of the remaining factors is associated with an increase in the probability for voting for Leave.

For decision tree, the greater proportion of people with higher education is associated with the Remain Vote, and the greater proportion of people not born in UK is also associated with the vote of Remain. On the other hand, the greater proportion of people in the middle to upper class is associated with the Leave vote.

We can therefore conclude that increase in proportion of people with higher education is associated with increase in the probability of the Remain Vote for both models and increase in the proportion of people in middle to upper class is associated with the Leave vote in both models. However, in decision tree, the higher proportion of people not born in UK is associated with the vote for Remain while in logistic regression this is associated with the vote for Leave.

The model I would choose for explaining the results for a newspaper article is the decision tree model. Most of the readers probably do come from non-statistical background what makes them harder to understand the results of logistic regression. With decision tree interpretation is very easy even for people who do not have statistical knowledge.
