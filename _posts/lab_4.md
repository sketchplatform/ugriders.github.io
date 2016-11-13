Cigarettes and carbon monoxide emissions
----------------------------------------

### Setting Up

``` r
library(RCurl)
library(ggplot2)
library(bitops)
```

### The data (5 points)

``` r
cigs = read.table("https://ww2.amstat.org/publications/jse/datasets/cigarettes.dat.txt")
names(cigs)<-c("brand","tar","nicotene","weight","CO")
```

``` r
summary(cigs)
```

    ##            brand         tar           nicotene          weight      
    ##  Alpine       : 1   Min.   : 1.00   Min.   :0.1300   Min.   :0.7851  
    ##  Benson&Hedges: 1   1st Qu.: 8.60   1st Qu.:0.6900   1st Qu.:0.9225  
    ##  BullDurham   : 1   Median :12.80   Median :0.9000   Median :0.9573  
    ##  CamelLights  : 1   Mean   :12.22   Mean   :0.8764   Mean   :0.9703  
    ##  Carlton      : 1   3rd Qu.:15.10   3rd Qu.:1.0200   3rd Qu.:1.0070  
    ##  Chesterfield : 1   Max.   :29.80   Max.   :2.0300   Max.   :1.1650  
    ##  (Other)      :19                                                    
    ##        CO       
    ##  Min.   : 1.50  
    ##  1st Qu.:10.00  
    ##  Median :13.00  
    ##  Mean   :12.53  
    ##  3rd Qu.:15.40  
    ##  Max.   :23.50  
    ## 

**Exercise 1** What type of plot would you use to display the relationship between CO and one of the other numerical variables? Plot this relationship using the variable tar as the predictor. Does the relationship look linear? If you knew how much tar was in a given brand of cigarettes, would you be comfortable using a linear model to predict the carbon monoxide content of that brand?

If the relationship looks linear, we can quantify the strength of the relationship with the correlation coefficient.

``` r
cor(cigs$CO, cigs$tar)
```

    ## [1] 0.9574853

``` r
fit1 = lm(cigs$tar ~ cigs$CO)
plot(cigs$tar, cigs$CO, main = "Cigarettes, carbon monoxide, and tar")
abline(fit1)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-5-1.png)

------------------------------------------------------------------------

1.  When displaying a relationship between CO and some other numerical variable we want to use a linear model to "describe a continuous response variable as a function of one or more predictor variables". To generate a linear model you should create a simple scatterplot of the two variables. Afterwards we need to create a line of best fit variable by calling the lm (linear model) function on cigarette CO and tar. Finally we can just draw a abline onto of the scatterplot to generate a simple linear model plot.

2.  For the most part, the relationship looks linear since the points nearly draw a straight line with the exception of a few outliers. Following the correlation coefficient given to us, the variables have a strong positive and linear association.

3.  Yes, If I was given the information of how much tar was in a given brand of cigarette. I would be comfortable using the linear model to predict the CO content of the brand.

------------------------------------------------------------------------

### Sum of squared residuals (10 points)

**Exercise 2** Looking at your plot from the previous exercise, describe the relationship between these two variables. Make sure to discuss the form, direction, and strength of the relationship as well as any unusual observations.

``` r
u <- "https://raw.githubusercontent.com/nickreich/stat-modeling-2015/gh-pages/assets/labs/plot_ss.R"
script <- getURL(u, ssl.verifypeer = FALSE)
eval(parse(text = script))
plot_ss(x = cigs$tar, y = cigs$CO)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-6-1.png)

    ## Click two points to make a line.
    ## Call:
    ## lm(formula = y ~ x, data = pts)
    ## 
    ## Coefficients:
    ## (Intercept)            x  
    ##       2.743        0.801  
    ## 
    ## Sum of Squares:  44.869

After running the last command above, you'll be prompted to click two points on the plot to define a line. Once you've done that, the line you specified will be shown in black and the residuals in red. Note that there are 25 residuals, one for each of the 25 observations. Recall that the residuals are the difference between the observed values and the values predicted by the line: \[ei = yi ô Ë yi\] The most common way to do linear regression is to select the line that minimizes the sum of squared residuals. The squared residuals are represented in this plot with blue dashed lines.

------------------------------------------------------------------------

Describe the relationship between the two cigarette variables:

-   Form: Linear
-   Direction: Positive
-   Strength: Strong
-   Unusual Observations: Outlier to the right

------------------------------------------------------------------------

**Exercise 3** Try running the above command again, this time with a line that is not a good fit. What happens to the squared residuals? Compare the sum of squares (given in the R output) of this poorly fit line to the first line you fit. Are you surprised at these results?

------------------------------------------------------------------------

The sum of squared residuals increased.

-   Good Fit: 56.724
-   Bad Fit: 568.671

Honestly, I am not surprised at the results because the sum of squared is defined as "the sum of squared differences of each observation from the overall mean." In other words, a good fit should have points closer to the line of best fit while a bad fit has points that deviate from the line.

------------------------------------------------------------------------

**Exercise 4** Run this code several more times trying to minimize the sum of squares each time. What is the smallest sum of squares you can obtain? How does it compare to your neighbors? Compared to the first line you drew, what adjustments did you make to reduce the RSS?

------------------------------------------------------------------------

Minimizing the sum of squares:

-   Original Sum of Squares: 56.724
-   Smaller Sum of Squares: 46.677

First, I had to enlarge the plot so I can see better and improve my accuracy. Second, I did my best to select two points to draw a linear line that symmetrically goes through all the points.

------------------------------------------------------------------------

### The linear model (10 points)

It is rather cumbersome to try to get the correct least squares line, i.e. the line that minimizes the sum of squared residuals, through trial and error. Instead we can use the lm function in R to fit the linear model (a.k.a. regression line).

``` r
m1 <- lm(CO ~ tar, data = cigs)
```

The first argument in the function lm is a formula that takes the form y~ x. Here it can be read that we want to make a linear model of CO as a function of tar. The second argument specifies that R should look in the cigs data frame to find the CO and tar variables. The output of lm is an object that contains all of the information we need about the linear model that was just fit. We can access this information using the summary function.

``` r
summary(m1)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ tar, data = cigs)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.1124 -0.7167 -0.3754  1.0091  2.5450 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  2.74328    0.67521   4.063 0.000481 ***
    ## tar          0.80098    0.05032  15.918 6.55e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.397 on 23 degrees of freedom
    ## Multiple R-squared:  0.9168, Adjusted R-squared:  0.9132 
    ## F-statistic: 253.4 on 1 and 23 DF,  p-value: 6.552e-14

Let's consider this output piece by piece. First, the formula used to describe the model is shown at the top. After the formula you find the five-number summary of the residuals. "The Coefficients" table shown next is key; its first column displays the linear model's y-intercept and the coefficient of tar. With this table, we can write down the least squares regression line for the linear model: \[Ë y = 2.74328 + 0.80098 * tar\] One last piece of information we will discuss from the summary output is the Multiple R-squared, or more simply, R2. The R2 value represents the proportion of variability in the response variable that is explained by the explanatory variable. For this model, 91.68% of the variability in carbon monoxide content is explained by the amount of tar in the cigarette.

**Exercise 5** Calculate Ëb 0 and Ëb1 from the model of CO as a function of tar by hand (i.e. using arithmetic/linear algebra and R as your calculator). Confirm that they match up with the values from the fitted model using lm.

``` r
plot(cigs$tar, cigs$CO, main = "Cigarettes, carbon monoxide, and tar")
abline(m1)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
Using math to make sure that the values match up with the linear model equation given to us

y = 2.74328 + 0.80098 * tar 

y = bo + b1 * x

mean(y) = bo + b1 * mean(x)

b1 = cor(y,x) * SDy / SDx

bo = y - b1* x
```

``` r
#b1
slope = cor(cigs$CO, cigs$tar) * (sd(cigs$CO) / sd(cigs$tar))
slope
```

    ## [1] 0.800976

``` r
#bo
intercept = mean(cigs$CO) - slope * mean(cigs$tar)
intercept
```

    ## [1] 2.743278

**Exercise 6** What does the slope tell us in the context of the relationship between the amount of carbon monoxide emitted into the environment and the amount of tar in the cigarette?

------------------------------------------------------------------------

First of all, we know that our slope equal to "0.80098" this tells us that there is a positive relationship between carbon monoxide emitted into the environment and the amount of tar in the cigarette. The question is what is the context behind "0.80098," essentially this is the amount of CO emmisions (in mg) that enter into the atmosphere per (mg) amount of tar contained in a cigarette. In other words, the more tar that is put into a cigarette means the more CO that enters the atmosphere.

------------------------------------------------------------------------

**Exercise 7** Fit a new model m2 that uses weight to predict CO. Using the estimates from the R output, write the equation of the regression line. How much of the variability in CO emission is explained by the weight of the cigarette? Which model, m1 or m2, would you trust more to predict CO emission? Explain.

``` r
m2 = lm(CO ~ weight, data = cigs)
plot(cigs$weight, cigs$CO, main = "Cigarettes, carbon monoxide, and weight")
abline(m2)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
summary(m2)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ weight, data = cigs)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -6.524 -2.533  0.622  2.842  7.268 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)  -11.795      9.722  -1.213   0.2373  
    ## weight        25.068      9.980   2.512   0.0195 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 4.289 on 23 degrees of freedom
    ## Multiple R-squared:  0.2153, Adjusted R-squared:  0.1811 
    ## F-statistic: 6.309 on 1 and 23 DF,  p-value: 0.01948

``` r
cor(cigs$weight, cigs$CO)
```

    ## [1] 0.4639592

------------------------------------------------------------------------

I would definitely use model "m1" because it has a much high correlation and lower residual standard error compared to model "m2". The amount of variablity in the CO emission that is associated with weight is determine by R squared which is \[R^2 = Explained Variation / Total Variation.\] In this scenario, R squared is much lower telling us that the variance is not greatly explained by the weight variable.

------------------------------------------------------------------------

### Prediction and prediction errors (5 points)

Let's create a scatterplot of CO versus tar with the least squares line laid on top.

``` r
qplot(tar, CO, data=cigs)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-13-1.png)

``` r
ggplot(cigs, aes(tar, CO)) + geom_point() + geom_smooth(method="lm")
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-13-2.png)

``` r
ggplot(cigs, aes(tar, CO)) + geom_point() + geom_smooth(method="lm", se=FALSE)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-13-3.png)

**Exercise 8** If you saw the least squares regression line and not the actual data, how much CO (mg) would you predict to be emitted from a cigarette with 15 mg of tar? Is this an overestimate or an underestimate, and by how much? In other words, what is the residual for this prediction?

``` r
## Residual result of 15mg from CO emission from 15mg of tar
15 - (m1$coefficients[[2]]*15+m1$coefficients[[1]])
```

    ## [1] 0.2420829

``` r
## Residual result of 14mg from CO emission from 15mg of tar
14 - (m1$coefficients[[2]]*15+m1$coefficients[[1]])
```

    ## [1] -0.7579171

------------------------------------------------------------------------

If I just saw the least squares regression line, I can make a guess there will be 15mg of CO emitted from 15mg of tar. Although this would be an overestimate of the slope and underestimate of the intercept. However if I made the guess that there will be 14mg of CO emitted from 15mg of tar. This would be an underestimate of the slope and overestimate of the intercept.

The residual for this prediction is ".24" for 15mg and "-.75" for 14mg of CO emission.

------------------------------------------------------------------------

### Model diagnostics (10 points)

To assess whether the linear model is reliable, we need to check for (1) linearity, (2) nearly normal residuals, and (3) constant variability.

1.  Linearity: You already checked if the relationship between CO content and amount of tar is linear using a scatterplot. We should also verify this condition with a plot of the residuals vs. tar.

``` r
qplot(tar, m1$residuals, data=cigs) + geom_hline(yintercept=0, linetype=3)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-15-1.png)

**Exercise 9** Is there any apparent pattern in the residuals plot? What does this indicate about the linearity of the relationship between CO content and tar?

------------------------------------------------------------------------

There is a slight incidication of a pattern displayed in the residual plot. Essentially the residual plot takes the line of best fit and turns it around to have a horizontal display. So it tells you the amount a residual error or variablity that each point has on the central line which is the observed minus the predicted values. The ideal plot will have the residuals groupped around the center with points all around both sides, but here we have an outlier and a slight pattern which means we can do better in the linearity between CO content and tar.

------------------------------------------------------------------------

1.  Nearly normal residuals: To check this condition, we can look at a histogram

``` r
qplot(m1$residuals)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](lab_4_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
##qplot(m1$residuals, binwidth =1)
```

or a normal probability plot of the residuals. Recall that any code following a \# is intended to be a comment that helps understand the code but is ignored by R.

``` r
qqnorm(m1$residuals)
qqline(m1$residuals) # adds diagonal line to the normal prob plot
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-17-1.png)

**Exercise 10** Based on the histogram and the normal probability plot, does the nearly normal residuals condition appear to be met?

------------------------------------------------------------------------

Yes, the nearly normal residual condition appears to be statisfied since:

1.  The histogram is nearly symmetrical
2.  The regression model is nearly linear
3.  There is no outlier or high source of variablity

------------------------------------------------------------------------

1.  Constant variability:

**Exercise 11** Based on the plot in (1), does the constant variability condition appear to be met?

------------------------------------------------------------------------

No, the constant variability condition is not satisfied in plot (1). The issue is that the plot has some sort of identifiable pattern where the points form a type of drift or wave. It also contains an outlier that can impactfully disrupt the data.

------------------------------------------------------------------------

### Comparing linear models with other variables (10 points)

**Exercise 12** Produce a scatterplot of CO and nicotene and fit a linear model. At a glance, does there seem to be a linear relationship?

``` r
plot(cigs$nicotene, cigs$CO, main = "Cigarettes, carbon monoxide, and nicotene")
m3 = lm(CO ~ nicotene, data = cigs)
abline(m3)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-18-1.png)

------------------------------------------------------------------------

Yes, at first glance there is a postive linear relationship between CO and nicotene amounts.

------------------------------------------------------------------------

**Exercise 13** How does this relationship compare to the relationship between CO and tar? Use the R2 values from the two model summaries to compare. Does nicotene seem to predict CO better than tar? How can you tell?

``` r
par(mfrow=c(1,2))
plot(cigs$nicotene, cigs$CO, main = "Cigarettes, carbon monoxide, and nicotene", col = "blue")
abline(m3)
plot(cigs$tar, cigs$CO, main = "Cigarettes, carbon monoxide, and tar", col = "darkgreen")
abline(m1)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-19-1.png)

``` r
summary(m3)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ nicotene, data = cigs)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.3273 -1.2228  0.2304  1.2700  3.9357 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   1.6647     0.9936   1.675    0.107    
    ## nicotene     12.3954     1.0542  11.759 3.31e-11 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.828 on 23 degrees of freedom
    ## Multiple R-squared:  0.8574, Adjusted R-squared:  0.8512 
    ## F-statistic: 138.3 on 1 and 23 DF,  p-value: 3.312e-11

``` r
summary(m1)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ tar, data = cigs)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.1124 -0.7167 -0.3754  1.0091  2.5450 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  2.74328    0.67521   4.063 0.000481 ***
    ## tar          0.80098    0.05032  15.918 6.55e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.397 on 23 degrees of freedom
    ## Multiple R-squared:  0.9168, Adjusted R-squared:  0.9132 
    ## F-statistic: 253.4 on 1 and 23 DF,  p-value: 6.552e-14

``` r
cor(cigs$tar, cigs$CO)
```

    ## [1] 0.9574853

``` r
cor(cigs$nicotene, cigs$CO)
```

    ## [1] 0.9259473

------------------------------------------------------------------------

Despite looking nearly identical at first glance, tar would perform better when predicting the accurate amount of CO emission compared to tar. Since tar has a greater correlation compared to nicotene and it has a better R squared. The R squared describes the measure of differences. Since R square is a better tool of measurement for predictions, we see that tar overall has less variability compared to nicotene.

------------------------------------------------------------------------

**Exercise 14** Which variable best predicts CO out of the three in this data set? Support your conclusion using the graphical and numerical methods we've discussed.

``` r
par(mfrow=c(1,3))
plot(cigs$nicotene, cigs$CO, main = "Cigarettes, carbon monoxide, and nicotene", col = "blue")
abline(m3)
plot(cigs$tar, cigs$CO, main = "Cigarettes, carbon monoxide, and tar", col = "darkgreen")
abline(fit1)
plot(cigs$weight, cigs$CO, main = "Cigarettes, carbon monoxide, and weight", col = "orangered2")
abline(m2)
```

![](lab_4_files/figure-markdown_github/unnamed-chunk-20-1.png)

``` r
summary(m3)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ nicotene, data = cigs)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.3273 -1.2228  0.2304  1.2700  3.9357 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   1.6647     0.9936   1.675    0.107    
    ## nicotene     12.3954     1.0542  11.759 3.31e-11 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.828 on 23 degrees of freedom
    ## Multiple R-squared:  0.8574, Adjusted R-squared:  0.8512 
    ## F-statistic: 138.3 on 1 and 23 DF,  p-value: 3.312e-11

``` r
summary(fit1)
```

    ## 
    ## Call:
    ## lm(formula = cigs$tar ~ cigs$CO)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.9309 -1.4093  0.0667  0.7414  5.0257 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -2.12325    0.96074   -2.21   0.0373 *  
    ## cigs$CO      1.14458    0.07191   15.92 6.55e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.67 on 23 degrees of freedom
    ## Multiple R-squared:  0.9168, Adjusted R-squared:  0.9132 
    ## F-statistic: 253.4 on 1 and 23 DF,  p-value: 6.552e-14

``` r
summary(m2)
```

    ## 
    ## Call:
    ## lm(formula = CO ~ weight, data = cigs)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -6.524 -2.533  0.622  2.842  7.268 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)  -11.795      9.722  -1.213   0.2373  
    ## weight        25.068      9.980   2.512   0.0195 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 4.289 on 23 degrees of freedom
    ## Multiple R-squared:  0.2153, Adjusted R-squared:  0.1811 
    ## F-statistic: 6.309 on 1 and 23 DF,  p-value: 0.01948

``` r
cor(cigs$tar, cigs$CO)
```

    ## [1] 0.9574853

``` r
cor(cigs$nicotene, cigs$CO)
```

    ## [1] 0.9259473

``` r
cor(cigs$weight, cigs$CO)
```

    ## [1] 0.4639592

------------------------------------------------------------------------

Again, all bets are on tar precisely because it has the highest r square among the three variables.

------------------------------------------------------------------------

**Exercise 15** Check the model diagnostics for the regression model with the variable you decided was the best predictor for CO content.

``` r
plot(cigs$tar, cigs$CO, main = "Cigarettes, carbon monoxide, and tar", col = "darkgreen")
abline(m1)
ggplot(cigs, aes(tar, CO)) + geom_point() + geom_smooth(method="lm")
qplot(tar, m1$residuals, data=cigs) + geom_hline(yintercept=0, linetype=2, col = "olivegreen")
qplot(m1$residuals)
qqnorm(m1$residuals)
qqline(m1$residuals) 
summary(m1)

#Tar by far is the best predictor among the three variables. No need to rerun all the graphs
```

``` r
plot(cigs$nicotene, cigs$CO, main = "Cigarettes, carbon monoxide, and nicotene", col = "blue")
abline(m3)
ggplot(cigs, aes(nicotene, CO)) + geom_point() + geom_smooth(method="lm")
qplot(nicotene, m3$residuals, data=cigs) + geom_hline(yintercept=0, linetype=2, col = "navy")
qplot(m3$residuals)
qqnorm(m3$residuals)
qqline(m3$residuals) 

#Despite having nearly identical model diagnostics as tar, the histogram breaks the rule of constant linearity because it is not symmetric. 
```

<h1>
<center>
~FIN
</center>
</h1>
