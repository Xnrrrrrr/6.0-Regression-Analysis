It is important to know that regression models are predictions, not causal models.
We’ll use the word prediction a lot when discussing regression but keep in mind that, unless you are working with data from a controlled experiment or have time-lagged data, you cannot assume that X causes Y.
You can only infer that there is an association between these variables.
This goes back to the old adage in statistics of correlation does not mean causation. Two variables can have a strong association but there can be a host of third variables that would help you reinterpret the causal link between X and Y.
It is okay to interpolate but do not extrapolate beyond your data.
This is also known as having a restriction of range problem.
Interpolation and extrapolation are often discussed with regard to regression results. This simply means that our model allows us to draw inferences about Y within the range of our X variable (i.e., interpolate). However, we should not make interferences about relationships outside of this range (i.e., extrapolate).
An example of this would be if our X values represented a range of 0 to 20. We know what the Y values would be for this range, but we don’t know what the relationship could look like for someone who had an X value greater than 20. Your linear relationship could start to become non-linear after this point.
Extrapolation can also impact your conclusions if you find no significant effects between two variables.
For example, if you ran a regression model and found that X did not predict Y, this does not mean that there is no relationship between X and Y. It could just be that our range was restricted to only one portion of X variables which failed to tell the full story of how the two variables could be linked.
 
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/e47e622d-4e0f-4d52-902a-28724502ed65)

Importance of Range 

Graph showing there is a moderate relationship between x and y when x includes a certain range, but there is no relationship between x and y when x is restricted to a different range of the same data
This graphic highlights the importance of gathering data from a representative sample of the X values that interest you.

 

It is important to be aware of the impact of outliers on your regression line.
An outlier is a data point (or set of data points) that are significantly different from the rest of the data in your model
We are concerned about outliers because they can be very influential in our regression line
A quick way to detect outliers is to examine a scatterplot of your data
The graphic below helps to illustrate how an influential data point can change the nature of your slope, intercept, and estimates of strength.
When outliers are detected we do not automatically remove them. On the one hand, an outlier may indicate a larger issue with the sample data not being representative of the population. On the other hand, it may indicate a smaller issue like a simple data entry mistake. There are complex decisions involved in removing outliers. Therefore in this course, we will practice outlier detection but not removal.
 
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/7dcb052b-fbe5-43ec-b3e9-6a91f9965f10)

Impact of Outliers 

Outliers.png

 

Assumptions

The mathematical expression above is shorthand for our regression assumptions. You would read it as: “The errors in our regression model are independent and identically normally distributed (iid) with a mean of 0 and a common variance.” This summarizes three core assumptions of regression analysis that are described below in the order of importance. 

NOTE: 

Independence of Observations: ![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/d10bbebc-aaed-4a4a-8b89-750be7a73fcf)

This the most important assumption in regression.
While it is perfectly fine to have correlations across your variables (shown in blue below), you should not have strong correlations across your observations (shown in red below).
Each row in your dataset should represent an observation that is randomly and independently sampled.
For example, if we were supposed to examine how Barium levels (“Ba”) predict Manganese levels (“Mn”), we would violate the assumption of independence.
This is because all the data are grouped based on body parts (as shown in the variable “Part” in the second column). If we examined the relationship between Ba and Mn, we would see more similarity in the relationships within each body part as opposed to across body parts. This would violate our assumption of independence.
In addition to using logic to determine whether your data is segmented into clusters, you can also empirically test this using the Durbin-Watson test. The Durbin-Watson test evaluates the degree of overlap in our observations or cases. This test will be discussed more in the R tutorial.
Homogeneity of Variance:

![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/916ef6a5-ddf6-4cfa-86a5-6cd01530e642)


This is the second most important assumption of regression.
It states that the spread of observations (or errors) for each value of X is constant around the regression line.
You can evaluate the homogeneity of variance in your data by examining a residual vs. fitted values plot.
Normality:
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/d8a53035-b2a1-4da9-ba7d-9c95f791f992)![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/34d6a3e2-c3c5-48b5-bfea-d58366054612)


This is the least important assumption.
It states that the spread of observations (and therefore the errors) have a bell-shaped curve.
The normal distribution of these errors has a mean of 0, meaning that positive and negative error values cancel each other out.
You can evaluate normality using a Q-Q plot of your residuals or a histogram of your residuals. You can also run a Shapiro-Wilk test to get a significance test associated with the normality of your findings.
R Tutorial: Checking Assumptions in R
Independence of Observations:
This assumption will be evaluated using the Durbin-Watson test.
To run this test, you’ll first need to install and load the “car” package.
#Installs and loads the car package
install.packages("car")
library(car)
You can then run the Durbin-Watson test on your regression model using the dwt() function.
dwt(mn.fit) 
#substitute “mn.fit” with the name of your regression model
Interpreting the results:
The Durbin-Watson statistic ranges from 0 to 4.
<1	1	2	3	>3
Positively Correlated	Uncorrelated	Negatively Correlated
Values between 1 to 3 typically show that observations in our data are uncorrelated.
The closer to 2 the result is, the better our data meets the independence of observation criteria.
Values between 0 and 1 suggest that the cases in our data are positively correlated with each other.
Whereas, values between 3 and 4 suggest that the cases in our data are negatively correlated with each other.
Reporting the results:
The key estimates that we would report from the Durbin-Watson test are highlighted below.
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/6e66ad98-d1bc-4dc0-9cdf-57d606adfb20)

Key estimates in this summary include:
the level of autocorrelation (0.65),
the Durbin-Watson statistic (0.68), and
the p-value (<.001)
The null hypothesis is that the observations are uncorrelated, whereas the alternative hypothesis is that the observations are correlated.
Given that the results are statistically significant (p < .05), we reject the null hypothesis and conclude that there is evidence of autocorrelation in the data.
The results can be reported in the following way:
“The Durbin-Watson test indicates that there is a strong positive correlation among our observations (ρ = 0.65, DW = 0.68, p < .001), therefore it is not safe to assume our observations are independent of each other and we should not be running or interpreting a regression model for this data.”
Homogeneity of Variance:
This assumption is evaluated by plotting the residuals against the fitted/predicted values.
R is capable of running a number of diagnostics on our linear object ("mn.fit").
We will use the plot() function along with the “which” argument to test for homogeneity of variance.
In the syntax below, the argument “which = 1” specifies that we would like to see only the first set of diagnostics on our linear model, which is a plot of residuals against our fitted/predicted values.
#Runs a residual plot on our regression model
plot(mnfit, which = 1)
Interpreting the results:
The residual plot is shown below:
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/f9afbcda-19f5-4559-bc35-eb84ade6792d)


Given that the errors are more or less equally distributed around the zero line, we can conclude that there is homogeneity in the variance of our residuals around our predicted values of Y. The images below show potential problems with these plots.
Examples of problems with your plot:

For these plots, you want to see that as you move along the x-axis the spread of the scores on the y-axis should be more or less the same.

A funnel-shaped plot indicates that the spread of errors increases when we predict higher values of Y. Note that the funnel can also go in the opposite direction and its meaning would be reversed.
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/8f50c251-2eed-47a7-bcfc-85bf75539c79)


A U-shaped plot indicates that the spread of errors increases when we predict values in the middle of Y. This would also suggest that a quadratic model would fit the data better than a linear model.
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/925baef6-3e0d-4c03-8ff1-f14759a578ed)


Normality:
We’ll discuss 3 ways to evaluate normality:

Create a Q-Q plot of your residuals.
We will once again use the plot() function along with the “which” argument to test for normality.
In the syntax below, the argument "which = 2" specifies that we would like to see only the second set of diagnostics on our linear model, which is a Normal Q-Q plot
#Runs a normality plot
plot(mnfit, which = 2)
Interpreting the results
The Q-Q plot is shown below. It compares the distribution of our residuals to what we would theoretically expect them to be if our data was normal. Ideally, these data should be closely related along a straight line. The graph below helps to support the normality of our residuals.
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/db736076-a17d-421b-b633-9832258c11b4)![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/699a7e0f-3972-47e8-8e5a-e989ab374562)![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/5de26fb5-20a3-46a3-8114-f5d5cc1c3a9f)



Below are some of the common problems you’ll find when examining normality, along with their corresponding shapes on the Q-Q plot.
![image](https://github.com/Xnrrrrrr/6.0-Regression-Analysis/assets/133546385/e3de5b85-bcd1-4f31-8246-b11b6f59c818)

These represent (in order of appearance from left to right) distributions that are positively skewed, negatively skewed, and have fatter tails than the normal distribution (i.e., S-shaped).
Create a histogram of your residuals.
This can be done by storing the residuals in the data frame and using ggplot2 to create a histogram
#Stores raw residuals
data$res = resid(mn.fit)

#Creates a plot of the residuals
resid.plot = ggplot(data, aes(res)) 
resid.plot + geom_histogram()
Note: You’ll need to replace data with the name of your data frame and mn.fit with your regression model
Run a Shapiro-Wilk test on your residuals
The most objective way to evaluate normality is using the Shapiro Wilk test. This can be done using the following syntax.
#Provides an objective measure of normality
shapiro.test(data$res)
Note: You’ll need to replace data with the name of your data frame and res with the name of your residuals
Note that the Shapiro Wilk test is being used here to evaluate the normality of the residuals as opposed to the variables, which is a key assumption of regression
Interpreting the results:
When you run a Shapiro-Wilk test, you should report the W and the p-value.
Interpreting the results can be tricky because the null hypothesis is that the data is normally distributed and the alternative hypothesis is that the data is not normally distributed.
This means that a p-value less than .05 indicates that the data is not normally distributed.
Therefore unlike other significance tests you actually want your test of normality to be non-significant.
Detecting Outliers:
A fairly fast way of detecting if you have outliers and where they are, is to first standardize your residuals. You can do this using the following syntax.
#Stores standardized residuals
data$std.r = rstandard(mn.fit)
Note: You’ll need to replace data with the name of your data frame and mn.fit with your regression model
Then, inspect the newly stored standardized residuals in your data frame by clicking it in the Global Environment and sorting the standardized residuals. You can plot them using a histogram.
You can also visually inspect your data for outliers by creating a scatterplot of the relationship between your IV and DV.
You can do this using the following syntax.
#Loads the ggplot2 library needed to create the graph
library(ggplot2)

#Creates an object storing the relationship between X and Y
scatter <- ggplot(data, aes(X, Y))

#Displays the scatterplot between X and Y
scatter + geom_point() + geom_smooth(method = "lm", colour = "Red")+ labs(x = "X", y = "Y")
Note: You’ll need to replace data with the name of your data frame and X and Y with your variable names

