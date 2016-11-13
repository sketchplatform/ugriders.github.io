#### **Data exploration**

``` r
library("Hmisc")
getHdata(FEV)
library(ggplot2)
library(easyGgplot2)
```

**Exercise 1** *Before looking at the data, what types of relationships do you expect to see between each of these variables and FEV? Justify your answers briefly.*

Before looking at the data, I can expect these types of relationships with the FEV (Forced Expiatory Volume) variable:

1.  **Age vs. Fev:** positive linear relationship with weak association
2.  **Height vs. Fev:** positive linear relationship with strong association
3.  **Sex vs. Fev:** positive linear relationship with very weak association
4.  **Smoke vs. Fev:** positive linear relationship with strong association

The associations are determined by my own estimates of which variables that I believe would become a good predictor for FEV.

**Exercise 2** *Think about the different measures variables that you have at your disposal. Hypothesize at least two possible interactions, and come up with a plausible justification about why such an interaction might exist.*

The two interactions that I would make is that "Height and Smoke could have a true relationship based on the amount of FEV" and "Age and Height may also have a true relationship due to the amount FEV." I find it plausible that the interactions among these variables may exist because an individual's FEV (Forced Expiatory Volume) is dependent on how healthy the lungs are as well as the size of the lungs itself. So it is safe for me to say that depending on how tall or big a person is could identify the size of their lung capacity. Whether you're a smoker or nonsmoker as well as young or old may have an effect on the overall health of your lung's condition.

**Exercise 3** *Generate a few simple plots of the data to evaluate the possible bi-variate relationships that exist. Based on these plots, do you think that linear relationships will be sufficient to explain the data?*

Recall (from lecture 7) that when you have a large sample size relative to the number of possible covariates (which we do), one justifiable way to build a model is to:

-   Include key covariates of interest.
-   Include covariates needed because they might be con-founders.
-   Include covariates that your colleagues/reviewers/collaborators will demand be included for face validity.
-   Test a reasonable/limited number of interaction terms if it seems appropriate.

No, it will not be sufficient enough to explain the data. From what we can tell by using a linear relationship is the association with a single variable, but if we apply multiple linear regression models we can get reduced error and have a better interpretation of the real life effects on FEV. While we may be able to collect data on a single regression, it may not be accurate enough to determine a person's overall lung health.

    ## [1] 0.756459

![](lab_5_files/figure-markdown_github/unnamed-chunk-3-1.png)

    ## [1] 0.868135

**Exercise 4** *Before fitting any models, identify somewhere between 2 and 6 multiple linear regression models that you think follow the rules above, i.e. write down what each model is, and which covariates and/or interactions it includes. Fit each model. Compare the coefficients from each of the models as well as the adjusted R^2 values. For the 2-3 models with the best adjusted R2 values, look at residual plots. Do you see any worrisome patterns that suggest the model assumptions aren't met? If so, do something to try to improve it.*

Best adjusted R^2 Values 1. lm(fev~age + smoke + height, data = FEV): 2. erwre 3. werwerwe

**Exercise 5** *Based on your work above, pick one model that represents what you think is the "best" model for this data. Justify your choice, and interpret what you think are the most important or interesting relationships with the covariates and FEV. Illustrate one of these relationships with a figure.*

The best model for this data would be "would be thre"

**Exercise 6** *Based on your thinking about this dataset and correlates of lung health more generally, if you were to collect similar data like this in a new study, what additional variables would you be interested in collecting and why?*

The types of variables that I would add are lung disease, weight, environment, fitness level, and lung (oxygen or vital) capacity:

-   **Lung diseases** - There are different people with different types of lung diseases, knowing whether a person has a medical history with lung disease is important.
-   **Weight** - We have height, might as well add weight since both diet and size relate to you overall lung health.
-   **Environment** - This is a interesting variable because it could potentially mean several different factors. This could be CO2 or pollution in a given area that may determine your overall health or worse nearby chemicals that may directly damage your lungs.
-   **Fitness level** - This is an appropriate variable because your level of fitness can help identify the level of health or lung capacity.
-   **Lung cpacity** - We have the amount of FEV, but why not the total amount for your lung capacity or how much can an individual inhale?

**Exercise 7** *Ensure that your entire report is reproducible. That means that the instructors should be able to run your .Rmd file and have it generate the same PDF that you handed in.*
