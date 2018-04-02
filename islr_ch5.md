---
title: ISLR Chapter 5
author: Aaron Shaffer
date: '2018-03-30'
slug: islr_ch5
categories:
  - ISLR
  - R
tags:
  - homework
summary: "ISLR Ch5, Exercises #3, #5"
output: 
  html_document: 
    keep_md: yes
---

ISLR Ch 5: #3, #5

## 3 We now review k-fold cross-validation.

$(a)$ Explain how k-fold cross-validation is implemented.

k-fold cross-validation is implemented by chosing a number of folds and then splitting the data into k nearly evenly partitioned 

$(b)$ What are the advantages and disadvantages of k-fold cross-validation relative to:

> i. The validation set approach?

> Having more than one set of training data allows for a better model.  If there is a low amount of a certain population represented in a dataset than a single validation set might not do a good job representing how that low variable represented by the low population predicts the outcomes.  The trade for this is time complexity.  Since the validation set approach only uses one validation set it is much faster than k-fold cross-validation.

> ii. LOOCV?

> Time complexity, k-fold is much faster than LOOCV and its accurate enough.  Disadvantage is that it might be less accurate than LOOCV.

## 5. In Chapter 4, we used logistic regression to predict the probability of `default` using `income` and `balance` on the `Default` data set. We will now estimate the test error of this logistic regression model using the validation set approach. Do not forget to set a random seed before beginning your analysis.

$(a)$ Fit a logistic regression model that uses income and balance to predict default.




```r
library(ISLR)
set.seed(1)
summary(glm(default ~ income + balance, data = Default, family = "binomial"))$coefficient %>% pander
```


-----------------------------------------------------------------
     &nbsp;        Estimate    Std. Error   z value    Pr(>|z|)  
----------------- ----------- ------------ --------- ------------
 **(Intercept)**    -11.54       0.4348     -26.54    2.958e-155 

   **income**      2.081e-05   4.985e-06     4.174    2.991e-05  

   **balance**     0.005647    0.0002274     24.84    3.638e-136 
-----------------------------------------------------------------

$(b)$ Using the validation set approach, estimate the test error of this model. In order to do this, you must perform the following steps:

> i. Split the sample set into a training set and a validation set.


```r
N <- nrow(Default)
train <- base::sample(N, N / 2)
```
> ii. Fit a multiple logistic regression model using only the training observations.


```r
train.model <- glm(default ~ income + balance, data = Default, family = "binomial", subset = train)
summary(train.model) %>% pander
```


----------------------------------------------------------------
     &nbsp;        Estimate    Std. Error   z value   Pr(>|z|)  
----------------- ----------- ------------ --------- -----------
 **(Intercept)**    -12.08       0.6658     -18.15    1.334e-73 

   **income**      1.858e-05   7.573e-06     2.454     0.01414  

   **balance**     0.006053    0.0003467     17.46    3.047e-68 
----------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- -----------------------------
   Null deviance:     1457.0  on 4999  degrees of 
                                freedom           

 Residual deviance:   734.4  on 4997  degrees of  
                                freedom           
-------------------- -----------------------------
> iii. Obtain a prediction of default status for each individual in the validation set by computing the posterior probability of default for that individual, and classifying the individual to the default category if the posterior probability is greater than 0.5.


```r
preds <- ifelse(predict(train.model, newdata = Default[-train,], type = "response") > .5, "Yes", "No")
```

> iv. Compute the validation set error, which is the fraction of the observations in the validation set that are misclassified.


```r
mean(preds != Default$default)
```

```
## [1] 0.0477
```
> The validation set error for this data is 4.77%

$(c)$ Repeat the process in $(b)$ three times, using three different splits of the observations into a training set and a validation set. Comment on the results obtained.


```r
for(i in c(1:3)) {
  train <- base::sample(N, N / 2)
  train.model <- glm(default ~ income + balance, data = Default, 
                     family = "binomial", subset = train)
  preds <- ifelse(predict(train.model, newdata = Default[-train,], 
                          type = "response") > .5, "Yes", "No")
  cat(summary(train.model) %>% pander)
  cat(sprintf("> The validation set error of split:%d of this dataset is %.2f%%\n\n",i,mean(preds != Default$default) * 100))
}
```


----------------------------------------------------------------
     &nbsp;        Estimate    Std. Error   z value   Pr(>|z|)  
----------------- ----------- ------------ --------- -----------
 **(Intercept)**    -11.36       0.5982     -18.99    2.177e-80 

   **income**      2.264e-05   6.947e-06     3.259    0.001116  

   **balance**      0.00553    0.0003142     17.6     2.476e-69 
----------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- -----------------------------
   Null deviance:     1483.8  on 4999  degrees of 
                                freedom           

 Residual deviance:   829.1  on 4997  degrees of  
                                freedom           
-------------------- -----------------------------

> The validation set error of split:1 of this dataset is 4.85%


----------------------------------------------------------------
     &nbsp;        Estimate    Std. Error   z value   Pr(>|z|)  
----------------- ----------- ------------ --------- -----------
 **(Intercept)**    -12.37       0.6649     -18.61    2.918e-77 

   **income**      2.566e-05   7.175e-06     3.577    0.0003479 

   **balance**     0.006042    0.0003452     17.5     1.375e-68 
----------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- -----------------------------
   Null deviance:     1463.7  on 4999  degrees of 
                                freedom           

 Residual deviance:   739.5  on 4997  degrees of  
                                freedom           
-------------------- -----------------------------

> The validation set error of split:2 of this dataset is 4.77%


----------------------------------------------------------------
     &nbsp;        Estimate    Std. Error   z value   Pr(>|z|)  
----------------- ----------- ------------ --------- -----------
 **(Intercept)**    -11.42       0.6128     -18.63    1.896e-77 

   **income**      2.383e-05   7.184e-06     3.317    0.0009084 

   **balance**     0.005471    0.0003123     17.52    1.029e-68 
----------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- -----------------------------
   Null deviance:     1436.7  on 4999  degrees of 
                                freedom           

 Residual deviance:   780.8  on 4997  degrees of  
                                freedom           
-------------------- -----------------------------

> The validation set error of split:3 of this dataset is 4.39%

> All three new splits of the data had an error in the ~4.5% region the estimates and all of other values from the linear models are also all very close to eachother.  This shows that splitting the data in half produces variable results all very close to eachother.

$(d)$ Now consider a logistic regression model that predicts the probability of default using income , balance , and a dummy variable for student . Estimate the test error for this model using the validation set approach. Comment on whether or not including a dummy variable for student leads to a reduction in the test error rate.


```r
train <- base::sample(N, N / 2)
train.model <- glm(default ~ income + balance + student, data = Default, family = "binomial", subset = train)
preds <- ifelse(predict(train.model, newdata = Default[-train,], type = "response") > .5, "Yes", "No")
summary(train.model) %>% pander
```


----------------------------------------------------------------
     &nbsp;         Estimate    Std. Error   z value   Pr(>|z|) 
----------------- ------------ ------------ --------- ----------
 **(Intercept)**     -10.3        0.6694     -15.38    2.24e-53 

   **income**      -3.958e-06   1.147e-05    -0.3452    0.7299  

   **balance**       0.0056      0.000321     17.45    3.57e-68 

 **studentYes**     -0.8668       0.3276     -2.646    0.008155 
----------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- -----------------------------
   Null deviance:     1463.7  on 4999  degrees of 
                                freedom           

 Residual deviance:   810.7  on 4996  degrees of  
                                freedom           
-------------------- -----------------------------

> The validation set error for this data is 4.83%

> Adding the `student` dummy variable to the logistic regression model didn't change the error by any signifigant amount.  The test error rate is within the range of previous test error rates without the `student` variable 
