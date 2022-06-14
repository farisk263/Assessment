# Formulation with different level of additives

![Oil](https://upload.wikimedia.org/wikipedia/commons/6/68/SIGAUS_aceite.jpg)
## Introduction

A customer informed their consultant that they have developed several formulations of petrol that gives different characteristics of burning pattern. The formulations are obtaining by adding varying levels of additives that, for example, prevent engine knocking, gum prevention, stability in storage, and etc. However, a third party certification organisation would like to verify if the formulations are significantly different, and request for both physical and statistical proof. Since the formulations are confidential information, they are not named in the dataset.

## Problem Statement
Please assist the consultant in the area of statistical analysis by doing this:

A) A descriptive analysis of the additives (columns named as “a” to “i”), which must include summaries of findings (parametric/non-parametric). Correlation and ANOVA, if applicable, is a must.

B) A graphical analysis of the additives, including a distribution study.

C) A clustering test of your choice (unsupervised learning), to determine the distinctive number of formulations present in the dataset.

## Descriptive Analytics

- ### Data Information
  - It has 9 additives/variable that will be used later for descriptive analysis.
    
- ### EDA
  - **_Distribution of the additives ?_**
 
    ![dist_additives](https://user-images.githubusercontent.com/63250608/173683738-731986b3-6c0c-4b4d-bf66-30d22a0f54ad.png)

  - **_Boxplot for the additives._**
   
    ![box_additives](https://user-images.githubusercontent.com/63250608/173684012-f9c33aa4-4330-40a8-937e-52b5024d603f.png)
    
  - **_QQplot used to check distribution normality._**
  
    ![qqplot](https://user-images.githubusercontent.com/63250608/173684200-b4c28396-bea8-4e27-b3c9-c0ae2aa3e25a.png)
    
  - **_Hypothesis Testing using Kruskal-Wallis between additives_**

    From above graphs we can find out that Additive C has bimodal & most of the additives are not in normal distribution. Due to this, I have decided to perform non parametric test like **Kruskal-Wallis** test to find out if there is any significant difference between the additives or not. So the null hypothesis built on the current case is, all additives are the same while the alternative hypothesis is the additives have significant difference. 
  
    `KruskalResult(statistic=1707.6383280751495, pvalue=0.0)`
    
    As the P value is below significance level, which is 0.05, the null hypothesis is rejected. The test shows that the additives are indeed difference with each other. 
    
  - **_Correlation between two highly correlated additives_**

    ![heatmap](https://user-images.githubusercontent.com/63250608/173684797-e70e96d0-3ab2-46ac-9f5a-a523d5222cb9.png)
    
    Seems like Additive A & G is highly positively linear with correlation coefficient of value 0.81. Quantifying a relationship between two variables using the correlation coefficient only tells half the story, because it measures the strength of a relationship in samples only. If we obtained a different sample, we would obtain different correlation coefficient values, and therefore potentially different conclusions. 

The alternative hypothesis is always what we are trying to prove, in our case, we try to prove that there is a significant correlation between additive A and G in the population (i.e. ρ ≠ 0).

The null hypothesis is the hypothesis that we are trying to provide evidence against, in our case, we try to provide evidence againt the hypothesis that there is not a significant linear correlation between additive A and G in the population (i.e. ρ = 0)

**Mann-Whitney U** Test is performed in hypothesis testing procedure.

    `MannwhitneyuResult(statistic=0.0, pvalue=6.41324618896848e-72)`
    
    Since the p value of the Mann-Whitney U test is below the significance level, 0.05, we reject the null hypothesis in favor of the alternative. We conclude that the correlation is statically significant at population level.
    
  - **_Find out the busiest month for hotels_**

   
    
  - **_Find out the daily price relationship with length of stay_**
    
    ![length_stay_over_price](https://user-images.githubusercontent.com/63250608/165355842-bf26de5b-1b74-4a2b-b9cf-3f758691a130.png)

    Average daily price for city hotel spiked at 24 days length of stay. Most of the time, resort hotel daily room price can be considered much cheaper than city hotel daily rate. This may possible due to high land value in the city & high land rental cost which cause the daily room rate to be much expensive. Higher length of stay for resort hotel means cheaper price while city hotel's does not have the same effect.
    

## Model Development
  - Correlation against target variable was first been calculated prior model training:

    ![correlation](https://user-images.githubusercontent.com/63250608/165358574-dd342176-8d06-4d0f-87e7-56cfc6c46c49.png)

    ```
    lead_time                         0.293123
    total_of_special_requests         0.234658
    required_car_parking_spaces       0.195498
    booking_changes                   0.144381
    previous_cancellations            0.110133
    is_repeated_guest                 0.084793
    agent                             0.083114
    adults                            0.060017
    previous_bookings_not_canceled    0.057358
    days_in_waiting_list              0.054186
    adr                               0.047557
    babies                            0.032491
    stays_in_week_nights              0.024765
    company                           0.020642
    arrival_date_year                 0.016660
    arrival_date_week_number          0.008148
    arrival_date_day_of_month         0.006130
    children                          0.005048
    stays_in_weekend_nights           0.001791
    Name: is_canceled, dtype: float64
    ```
    
    We have identified the 5 important numerical features which is highly correlated with booking cancellation status. They are lead time, special requests, car parking spaces required, booking changes & number of previous cancellation.
    
  - Multiple machine learning classification models were tested against the dataset, eg: Random Forest, Logistic regression, XGBoost & Decision Tree.
    
  - Model Evaluation:
    
    ```
    F1 Score values for each models:
    DecisionTree model: 0.8215
    RandomForest model: 0.8623
    LogisticRegression model: 0.7811
    XGBoost model: 0.8412
    ```
  
  - Model Debugging using ELI5:

    | Feature | Weight | Std |
    | --- | --- | --- |
    | lead_time | 0.143413 | 0.014738 |
    | deposit_type_Non Refund |	0.132847 |	0.110055 |
    |	adr |	0.095444 | 0.004118 |
    |	deposit_type_No Deposit |	0.086032 |	0.106844 |
    |	arrival_date_day_of_month |	0.069602 |	0.002306 |
    |	arrival_date_week_number |	0.054540 |	0.002246 |
    |	total_of_special_requests |	0.050384 |	0.014383 |
    |	agent |	0.043701 |	0.007353 |
    |	stays_in_week_nights |	0.041340 |	0.002036 |
    |	previous_cancellations |	0.038880 |	0.013721 |
    
    From the table above, we can find out that the top 3 are the most important features that affect the prediction of the model, which is lead time, deposit type and ADR. The lead time feature bear the heaviest weight in determining either the booking got cancelled or not.
    
    ![leadtime_prediction_over_cancellation](https://user-images.githubusercontent.com/63250608/165361342-e1893c89-8a00-4ddb-aa95-16786c9418c3.png)

    The vertical line indicates the 365 days, one whole year. As it is clearly can be seen that the model inidicates that the bookings rarely got cancelled if the lead time is below 365 days. While most of the bookings got cancelled after the lead time reached more than 1 year.
    
## Conclusion
 
  - Repeated guests tend to not cancel their bookings.
  - Room price per night at both hotels are higher than other months during summer season (June - September).
  - Family guests tend to stay longer than individuals / couple at both hotels.
  - Popular length of stay for both hotels would be from 1 to 4 days. Surprisingly, choice of 7 days of stay is also well liked for Resort Hotel's guests as well.
  - The number of guests cancel their booking is higher than non-cancelled guests when the lead time is more than 50 days.
  - The number of booking is much higher in summer season (June - September) than other months. On top of that, bookings in winter season (December - March) is the least.
  - The trained model predict the possibility guests cancelled the bookings is soaring once the lead time is more than 1 whole year (365 days)

