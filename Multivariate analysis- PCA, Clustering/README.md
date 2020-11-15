# Introduction
This report presents the results of multivariate analysis carried out on a self-constructed dataset.
Dataset used in this report includes 14 numerical variables which were individually downloaded from the Gap Minder website, cleaned and combined together. Data can be grouped into three categories: health, economic and environmental. Additionally, for each country two categorical variables indicating geographical information as well as income information were provided.


## 1. Data Cleaning

The steps taken to clean the data involved:
- Downloading each required variable separately. Each dataset consisted of a country name in the first column and years as subsequent columns.
- Two columns from each data set were selected, the first column and the column with the most recent year. In case when the most recent year contained many missing values, the second most recent year was selected instead.
- This resulted in obtaining 14 excel spreadsheets, each containing different variable. In order to combine them into a single spreadsheet, each dataset was moved into a common Excel spreadsheet consisting of 14 worksheets. The worksheets had been merged using Excel Power Query based on common column with country names. This method ensured that each variable is matched with a corresponding country. The same approach was taken to match each country with its corresponding income and geographical category. Once all information were merged, rows with missing values were deleted.


## 2. Analysis of dependency between variables

In order to establish which variables might be depended, correlation analysis was employed using Pearson method to check whether there is a linear relationship between variables and to look at the direction of a potential relationship.

Covariance analysis could potentially be used to check whether two variables tend to vary together, however covariance results largely depend on units in which a given variable is described. Correlation analysis was chosen over covariance as results of correlation are easy to interpret even if variables are not in the same unit.

We need to note that Pearson correlation coefficient looks only at linear relationship, while in some cases, relationship between two variables might exist but it may not be linear. However, using data transformation we can make the relationship become linear and therefore we might still be able to use Pearson correlation (Hazarika, 2013).

In the first step I looked at distribution of each variable to see whether they follow normal distribution and whether transformation of variables might be required. I noticed that some variables are highly skewed what is especially visible for variables in group Health (see Figure 1). This suggests that we might need to use log transformation to make the variables less skewed.

Firstly, correlations were investigated before data transformation. Results of the analysis for group Health are shown below (see Figure 1):

![left](https://user-images.githubusercontent.com/61549398/99194749-73aa5180-2779-11eb-8df1-38930b667b1d.png)
![right](https://user-images.githubusercontent.com/61549398/99194750-7442e800-2779-11eb-9c5e-ac1279311fc4.png)


Left- Figure 1: Health indicators before data transformation 

Right- Figure 2: Health indicators after applying logarithmic transformation

The plots above show scatterplots for each variable pair as well as their corresponding correlation coefficient. Values close to 1 indicate strong positive correlation, while values close to -1 indicate strong negative correlation. On the other hand, values close to 0 suggest there is no correlation between variables.

We can see that before data transformation (see Figure 1) there appear to be a linear positive relationship between variables: health1 and health2, while there is no visible relationship among other variables. However, after logarithmic transformations (see Figure 2) we can notice a strong positive correlation between variables health1- health2 as well as strong negative correlation between health1-health4. We can also notice negative correlation between variables health2-health4 however in this case correlation is not that strong with coefficient being equal to -0.653.

Hypothesis test was performed to check whether correlations between variables health1-health2, health1-health4 and health2-health4 are statistically significant with the following results:

![pearson](https://user-images.githubusercontent.com/61549398/99194814-d00d7100-2779-11eb-8596-6e79f3c0291f.png)


Test results confirm that there is statistically significant relationship between those variables. P-value for each of the tests is very small therefore we can reject null hypothesis that correlation between those variables is equal to 0.

The same analysis can performed on other groups of variables. Unfortunately, due to size of dataset (14 dimensions) it was not possible to plot all correlations for all variables as each plot was very small and impossible to read. Therefore, in the next part we will perform dimensionality reduction.

## 3. Reducing dimension of dataset

**3.1 Brief introduction to dimensionality reduction and PCA**

Dimensionality reduction can be regarded as either feature elimination or feature extraction. Feature elimination occurs when we eliminate a variable or a set of variables from our dataset. Although dimension of our dataset is in fact reduced, we loose information held by dropped variables such as trends and patterns (Brems, 2017).

On the other hand, during feature extraction we do not loose any information as we create a set of new independent variables which are linear combination of all old variables (Brems, 2017).

PCA belongs to the feature extraction methods. In PCA data is projected geometrically into principal components which are linear combinations of old variables. It also worth to add that PCA can be conducted on covariance as well as on correlation matrix. For experimental purposes I conducted PCA using both methods however in this report only results for running PCA on correlation matrix are presented.

**3.2 Results of PCA**

**3.2.1 Interpretation of Loadings**

By doing PCA we obtained 14 principal components, each explaining a certain percentage of total variation (Hayden, 2018). Loadings’ values obtained using princomp function in R, show how much each variable contribute to a given component. Larger values suggest stronger relationship between a given variable and component. Results presented in the graph below (see Figure 3) show variable contribution for first principal component. Top six variables contributing to PC1 with the highest values are:

![pca1](https://user-images.githubusercontent.com/61549398/99195096-89207b00-277b-11eb-9c41-6635d51d200d.png)

Figure 3: Percentage of contribution of each variable to PC1.

We can see that variables econ1 and health1 are the variables which contribute most to principal component 1. Similarly, we could check contribution for each of our 14 components.

In the Loading table we can also see the sign of each variable contributing to a given component. Positive sign indicates that a variable positively affects our component, while negative sign means it affect the component negatively. 


**3.2.2 Components’ Importance interpretation**

Using summary() function, we can see how many components are adequate to capture most of the variance. The function outputs the proportion of variance explained by each component as well as cumulative variance. The results are presented in report using a scree plot (see Figure 4). By looking at summary() function’ results, we could notice that first 8 principal components capture more than 91% of total variance, therefore we might reduce dimension of dataset to 8 components.

![screeplot](https://user-images.githubusercontent.com/61549398/99195265-9be77f80-277c-11eb-9043-fc40b93f75e1.png)

Figure 4: Scree plot showing percentage of variance explained by Each component.

We can see that first component captures the majority of total variance as compared to other components what is a desired result of PCA. All other components capture significantly less variance. We can conclude that we might need 8 components to capture the variance.

It is worth to add that when PCA was run on covariance matrix instead, the first component captured 99% of total variance. This happens because each variable has a different scale, however while doing PCA on correlation matrix we standardize the values to have a mean 0 and standard deviation of 1.

**3.2.3 Interpretation of Biplot**


![biplot](https://user-images.githubusercontent.com/61549398/99195317-e23cde80-277c-11eb-8c30-1671e29f397e.png)


Figure 5: Biplot representing first two principal components.

We already looked at loadings to investigate correlation between a given variable and each component. The loadings for the first two components can be displayed using biplot along with observations. Red arrows (see Figure 5) show loadings of PC1 and PC2. In this case, we can see that variables econ1 and health1 strongly influence PC1 what confirms our results from Figure 3 where we showed that health1 and econ1 are the top 2 variable which contribute most to PC1.
From the above plot, we can also identify which variables are correlated with each other by looking at the angle between them. Vectors of variables which are close together are positively correlated with each other (have a small angle), while if the angle between them is close to 180 degrees they are negative correlated. If the angle is close to 45 degrees then there is no correlation (Linh Ngo, 2018). For example, we can see that health3 and env1 are positively correlated, while env1 and econ2 are negatively correlated. There is however no relationship between econ7 and env3.

## 4. Patterns in data

**4.1 Clustering**

In order to find patterns in data, clustering method will be used. In clustering we aim to group observations together by their similarity. In other words, we want to check whether natural groups or segments exist. Ideally, we want similar observations to be grouped together (to belong to the same cluster) while observations from different groups should be dissimilar. In order to measure similarity between observations, we measure Euclidean distance between them, what is the most popular method for measuring the distance. (Provost, Fawcett, 2013)

Partitioning clustering method called k-means will be used with pre-determined number of 3 clusters, which is the most popular unsupervised learning method. However, the method will be conducted using our new variables (principal components) what addressed the issue of high dimensionality. The results of analysis will be again shown using first 2 principal components. The same type of analysis can be done using more than 2 principal components.

In order to fully understand patterns in data, we first need to decode our variables to see if we can logically interpret principal components. We can do this by looking at loadings as previously. We could see that Econ1, econ3, econ5, env1,2,3, health3, health4 were positively correlated to PC1. These variables correspond to the following indicators: GNI per capita, corruption index, % export, number of cell phones per person, electricity, CO2 emissions, DTP3 immunization level and health spending. While it is negatively correlated with Econ2-population, econ4-income growth, health1-newborn mortality, health2-babies per woman. It is therefore reasonable conclude that PC1 describes country’s development level or income status as usually developed (high income) countries are characterised by high GNI per capita, high number of cell phones, high co2 emission, high vaccination level and high health spending, while developing countries or low income countries are associated with high number of children per woman and high mortality rate among children.

Results of the analysis are shown on the graph below (see Figure 6). We can see that countries within each cluster show similarities in terms of income level. Countries assigned to the blue cluster (and therefore countries which have high score on PC1) such as Luxemburg, Qatar, Arab Emirates, United States, Austria etc are the ones which belong to high income countries. On the other hand, countries from the yellow cluster such as Nigeria, Ghana, Angola, Pakistan, all belong to lower middle class.

![clusterig](https://user-images.githubusercontent.com/61549398/99195398-48c1fc80-277d-11eb-8da0-6f46425f9780.png)

Figure 6: Cluster plot showing 3 clusters.


**4.2 Discriminant Analysis**

We have already seen that countries can be naturally grouped by its income level. However, in order to further exploit differences between High-Income and Low-income countries, Linear discriminant analysis was conducted to see which factors contribute most to the separation.

LDA can answer questions such as whether two groups are different and which variables make them different. That in turn may allow us to classify new observations based on those variables. LDA aims to project data into a new feature space to maximize classes separability (Lopes, 2019). In LDA we try to develop a discriminant function which is the linear combination of our variables.

In order to perform the analysis, two new datasets were created, one containing only observations from High Income group and the other one containing observations from Low Income group.

Standardized coefficients of discriminant function are displayed in the table below:

![table](https://user-images.githubusercontent.com/61549398/99195436-7e66e580-277d-11eb-909e-ec98fbde0f31.png)





We can see that many coefficients are 0 or close to 0 what suggests that their corresponding variables are not important in classifying countries into Low/High income group. However, there is one variable with significantly higher value as compared to other coefficients and corresponds to variable named: health2 with coefficient value of -6.422. In out dataset that variable indicates number of babies per woman. Therefore, the variable which contributes the most to separation of those 2 groups is number of children per woman. In fact, if we grouped our dataset according to the number of children in descending order, we could see that the first 14 countries belong to Low Income or Lower Middle Income. On the other hand, if countries were grouped in ascending order relative to health2 variable, the majority of the first 14 countries belong to High Income group. In order to check whether algorithm classify countries correctly, we can also take out 1 country from the dataset and check whether correct group is assigned.
References Brems, M., 2017. A One-Stop Shop For Principal Component Analysis. [online] Medium. Available at: <https://towardsdatascience.com/a-one-stop-shop-for-principal-component-analysis-5582fb7e0a9c> [Accessed 1 May 2020]. Lever, J., Krzywinski, M. and Altman, N., 2017. Principal Component Analysis. [online] Nature Methods. Available at: <https://www.nature.com/articles/nmeth.4346> [Accessed 28 April 2020]. Hazarika, N. and Hazarika, N., 2013. Correlation And Data Transformations -. [online] Majestic Blog. Available at: <https://blog.majestic.com/case-studies/correlation-data-transformations/> [Accessed 26 April 2020]. Hayden, L., 2018. PCA Analysis In R. [online] DataCamp Community. Available at: <https://www.datacamp.com/community/tutorials/pca-analysis-r> [Accessed 4 May 2020]. Ngo, L., 2018. How To Read PCA Biplots And Scree Plots - Bioturing's Blog. [online] BioTuring's Blog. Available at: <https://blog.bioturing.com/2018/06/18/how-to-read-pca-biplots-and-scree-plots/> [Accessed 25 April 2020]. Lopes, M., 2017. Is LDA A Dimensionality Reduction Technique Or A Classifier Algorithm?. [online] Medium. Available at: <https://towardsdatascience.com/is-lda-a-dimensionality-reduction-technique-or-a-classifier-algorithm-eeed4de9953a> [Accessed 24 April 2020]. Fawcett, T. and Provost, F., 2013. Data Science For Business. 1st ed. O'Reilly, p.21.
