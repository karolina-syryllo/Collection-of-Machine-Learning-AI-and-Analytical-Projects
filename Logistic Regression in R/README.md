# Introduction

The aim of this assignment is to analyse the effect of different demographical factors on Brexit Vote. The output variable is called voteBrexit, and gives a TRUE/FALSE answer to the question ‘did this electoral ward vote for Brexit?’ (i.e. did more than 50% of people vote to Leave?). Given a binomial nature of the outcome variable, the best model to for this task is a logistic regression.  

The 5 possible input variables are:
- **abc1**: proportion of individuals who are in the ABC1 social classes (middle to upper class)
- **medianIncome**: the median income of all residents
- **medianAge**: median age of residents
- **withHigherEd**: proportion of residents with any university-level education
- **notBornUK**: the proportion of residents who were born outside the UK

These are normalised so that the lowest value is zero and the highest value is one. Data is analysed using R software.

