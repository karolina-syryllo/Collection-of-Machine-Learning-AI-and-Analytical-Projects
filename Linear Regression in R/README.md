# Statistical Learning - Linear Regression


## 1. Perform a linear regression to predict the medal count in 2008 and 2012 (separately, in two regressions) from Population and GDP and report your results.

To interpret the results of regression analysis, firstly p-values will be examined to check whether explanatory variables (Population and GDP) are statistically significant. Secondly, values of the estimates will be interpreted in terms of their effect on the output variable, medal count.


P-values decide whether we can reject the null hypothesis that a coefficient has no effect on output variable (is equal to zero). The results are statistically significant if p-value for corresponding coefficient is less than 0.05.

Upload data, run regression and produce an output:


      medal_data=read.csv("medal_pop.csv") 
      medal_2008=glm(Medal2008~Population + GDP, data=medal_data) 
      summary(medal_2008)
      
      
Results: 

![1reg](https://user-images.githubusercontent.com/61549398/100259928-ba016c80-2f40-11eb-983f-d03fee2d6224.png)


##Interpretation of regression for medal count in 2008:


GDP is statistically significant as its p-value is less than 0.05, while Population is not statistically significant with p-value equal to 0.246750. This suggests that a change in GDP is associated with the change in Medal count for 2008 while change in Population is not associated with the change in Medal count for 2008.

The intercept’s coefficient of 5.613e+00 is the mean value of the number of medals if both Population and GDP were set to 0.

The coefficient of GDP tells us that if GDP increases by 1 unit, keeping other variables constant, number of medals in 2008 will increase by 8.435e-09.

The small value of coefficient is associated with the fact that the range of GDP values is much bigger as compared to the range of medal count.



      
 
      
