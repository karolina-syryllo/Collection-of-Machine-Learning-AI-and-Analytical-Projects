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
