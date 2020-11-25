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


## Interpretation of regression for medal count in 2008:


GDP is statistically significant as its p-value is less than 0.05, while Population is not statistically significant with p-value equal to 0.246750. This suggests that a change in GDP is associated with the change in Medal count for 2008 while change in Population is not associated with the change in Medal count for 2008.

The intercept’s coefficient of 5.613e+00 is the mean value of the number of medals if both Population and GDP were set to 0.
The coefficient of GDP tells us that if GDP increases by 1 unit, keeping other variables constant, number of medals in 2008 will increase by 8.435e-09.
The small value of coefficient is associated with the fact that the range of GDP values is much bigger as compared to the range of medal count.


Regression for 2012


      medal_2012=glm(Medal2012~Population + GDP, data=medal_data) 
      summary(medal_2012)     
      
      
![reg2](https://user-images.githubusercontent.com/61549398/100260278-241a1180-2f41-11eb-8dce-86513a95bf0e.png)


Interpretation of regression for medal count in 2012:

Again, Population is not statistically significant with high p-value of 0.468225, while GDP is statistically significant with p-value below 0.05. Therefore, there is no association between Population and number of medals in 2012, but there is a relationship between GDP and the number of medals in 2012.

Intercept says that mean the number of medals in 2012 is 6.076e+00 assuming Population and GDP is 0, while GDP coefficient says that 1 unit increase in GDP increases number of medals by 7.564e-03 keeping other variables constant.
 
      
## 2. How consistent are the effects of Population and GDP over time?

In both cases, p-value for Population was greater than 0.05 suggesting that there is no relationship between Population and number of medals. On the other hand, p-values for GDP were statistically significant with p-value less than 0.05 what means that there is an association between country’s GDP and number of medals.

Coefficients values in both regressions are very small due to the significant difference between the range of values for GDP and number of medals.


## 3. Using the regression for the 2012 medal count make a prediction for the results of 2016.

The values were calculated using the code below:

        predicted_2016=predict(medal_2012, medal_data)
        
        
## 4. Plot your predictions against the actual results of 2016. If the results are hard to see, use a transformation of the axes to make it these clearer. How good are the predictions? Which countries are outliers from the trend?

       plot(log10(d$predicted_2016), log10(d$Medal2016), xlab="Predicted values", ylab="Actual values", main="Predicted vs Actual values for 2016", xlim=range(0:2.5),   ylim=range(0:2.5)) abline(a=0, b=1, xlim=1)
       

![best_plot](https://user-images.githubusercontent.com/61549398/100268735-5598da00-2f4d-11eb-8733-b0b0eaac5e62.png)


Interpretation:

The graph shows predicted and actual values for the number of medals in 2016. The values have been transformed using logarithmic transformation and axes were adjusted accordingly.

The diagonal line shows data points for which predicted and actual values are equal. This indicates that if the point lies on the line, predicted value reflects actual value very well. Therefore, the further away from the line the point is, the weaker prediction our model made.

If the point is below the line, predicted value is higher than actual value, while in case when the point is above the line, predicted value is less than actual value.
We can see that there are only few points which were well predicted with many points being far from the line what suggests that our predictions are not very accurate.


Outliers:

In order to analyze outliers, boxplot function was employed. In R, boxplot classifies points as outliers based on the Interquartile range. The point is an outlier if it is less than Q1 - 1.5 * IQR or greater than Q3 + 1.5 * IQR. Based on that, three countries were classified as outliers, namely: Great Britain, India and Russian Federation.

       diff = predicted_2016-medal_data$Medal2016 
       Outliers = which(diff %in% boxplot(diff, plot=FALSE)$out)
       OutlierCountries = vector(mode = "list", length = 0) for (row in 1:nrow(medal_data)) { if(is.element(row, Outliers)){ OutlierCountries = append(OutlierCountries, c(as.character(medal_data[,1][[row]]))) } }
       
       
       
       OutlierCountries ## [[1]] ## [1] "Great Britain" ## ## [[2]] ## [1] "India" ## ## [[3]] ## [1] "Russian Federation"
       
       
  ## 5. Model Selection: Fit linear regressions models for the total medal count in 2012 using: (i) Population alone; (ii) GDP alone; (iii) Population and GDP. Select the model that minimises the Akaike Information Criterion.
  
      medal_2012_pop=glm(Medal2012~Population, data=medal_data) 
      
      medal_2012_gdp=glm(Medal2012~GDP, data=medal_data) 
      
      medal_2012_both=glm(Medal2012~GDP+Population, data=medal_data) 
      
      medal_2012_pop$aic ## [1] 618.1484 
      
      medal_2012_gdp$aic ## [1] 551.7404 
      
      medal_2012_both$aic ## [1] 553.187
      
  The model which minimizes AIC criterion is the model which uses GDP only as an explanatory variable with AIC of 551.7404. However, model which uses both GDP and Population has only slightly higher value.
  
  
 ## 6. Use cross-validation to perform a model selection between (i) Population alone; (ii) GDP alone; (iii) Population and GDP. Does your result agree with the model selected by AIC?
 
 In order to perform Cross Validation, data was divided into train and test data. For training, we used 50 data points, while for testing we used the remaining 21 datapoints.
 
      idx= sample(1:71, 50) 
      
      train_data=d[idx, ] 
      
      test_data=d[-idx, ] 
      
      formulas = c("Medal2016 ~ Population", "Medal2016 ~ GDP", "Medal2016 ~ Population + GDP") predictive_log_likelyhood=rep(NA, length(formulas)) for (i in 1:length(formulas)){ current_model=glm(formula=formulas[i], data=train_data) sigma=sqrt(summary(current_model)$dispersion) ypredict_mean=predict(current_model, test_data) predictive_log_likelyhood[i]=sum(dnorm(test_data$Medal2016, ypredict_mean, sigma, log=TRUE)) } 
      
      plot(1:length(formulas), predictive_log_likelyhood, xlab="Model Number", ylab="Log Probability")
      
      
 ![weird_plot](https://user-images.githubusercontent.com/61549398/100265999-fc2eac00-2f48-11eb-869b-f75bc2ab2cc1.png)


Interpretation:

Cross Validation results confirm that model 2 (“Medal2016 ~ GDP”) has the highest log-probability for test outputs. However, the difference between model 2 and 3 is very small. Therefore, in order to ensure that we chose the best model, the same procedure is repeated 100 times. The results confirmed that the best model is model 2.


      winner=rep(NA, 100) 
      
      for (iteration in 1:100){ idx=sample(1:71, 50) train_data=d[idx,] test_data=d[-idx,] predictive_log_likelyhood[1]=sum(dnorm(test_data$Medal2016, ypredict_mean, sigma, log=TRUE)) winner[iteration]=which.max(predictive_log_likelyhood) } 
      
      hist(winner, breaks=seq(0.5, 7.5, 1))
      
      
   ![hist](https://user-images.githubusercontent.com/61549398/100266836-6136d180-2f4a-11eb-8377-44afaeafd2d7.png)
   
   
 The results are consisted with the previous results when we used AIC criterion. At that time, the best model was the model which uses GDP only to predict number of medals. The same results were obtained using Cross Validation.
 
 ## 7. Using the three fitted models from Q1, predict the results of Rio 2016. Which predicts best? Compare this result with earlier answers.
 
 In order to determine which model predicts best, MSE is used. MSE is the mean squared error measured between predicted value and actual value. The model which predicts best is the model which minimizes the MSE.
 
We can see that the model with the lowest MSE is the model which uses GDP only, while the model which gives the worst prediction is the model which uses Population only. These results are consisted with results obtained in previous parts. According to AIC, the best model was the model with GDP only and the same applies to our Cross Validation’s result.


      library("MLmetrics")
      
      ## ## Attaching package: 'MLmetrics'
     
      predicted_2016_pop=predict(medal_2012_pop, medal_data) 
      
      predicted_2016_gdp=predict(medal_2012_gdp, medal_data) 
      
      predicted_2016_both=predict(medal_2012_both, medal_data) 
      
      
      
      MSE_pop=MSE(y_pred=predicted_2016_pop, y_true=medal_data$Medal2016) 
      
      cat("\nMSE for medal count based on Population only is ", MSE_pop) ##
      
      
      ## MSE for medal count based on Population only is 330.5405 MSE_gdp=MSE(y_pred=predicted_2016_gdp, y_true=medal_data$Medal2016) 
      
      cat("\nMSE for medal count based on GDP only is ", MSE_gdp) 
      
      ## ## MSE for medal count based on GDP only is 79.35504 MSE_both=MSE(y_pred=predicted_2016_both, y_true=medal_data$Medal2016)
      
      cat("\nMSE for medal count based on both Population and GDP is ", MSE_both) ## ## MSE for medal count based on both Population and GDP is 83.03936
