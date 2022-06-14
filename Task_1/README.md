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
  - It has about 31 features & 1 target variable that will be used later for model training to predict either the guest would cancel the booking or not. 
  - The dataset consists of 20 numerical data & 12 categorical data, including the target variable.
  - The target variable would be `is_canceled` column which contain integer value of 1 and 0. 1 indicates guests will cancel the booking while 0 is not.
    
- ### EDA
  - **_How many repeated guests decided to cancel booking ?_**
 
    ![booking repeated_guests](https://user-images.githubusercontent.com/63250608/165352175-e00135e9-3f53-4d1a-a80a-3df7b796ba3b.png)
    
    ```
    Percentage of repeated guests who:- 
    Cancelled: 14.6471%
    Not Cancelled: 85.3529%
    ```
    
    We can pretty much understand that, mostly of the repeated guests tend to not cancel their bookings. About 85% of the repeated guests decided to proceed with their bookings and stayed at the respective hotels. Only ~15% of them cancelled the bookings because of any other unplanned events.
    
  - **_Identified how much guests paid for a night & how the price fluctuated over the year._**

    ```
    Average cost per person for each hotel per night for at average of all room types:- 
    Resort Hotel: 47.49 €
    City Hotel: 59.27 €
    ```
    
    Above are the average room costs per night regardless of the meal & room types. It only covers the actual guests who really come & stay at the hotels. It does not include guests who cancelled their booking. Price is in EUR as the hotels are operated in Portugal.
    
    ![average_room_price](https://user-images.githubusercontent.com/63250608/165353847-410128ef-0d94-45c1-834f-50306f342b86.png)
    
    The graph explained that during summer season (June - September), the price per night at Resort Hotel spiked as people tend to go to beaches. The demand spiked & triggered the price to jack up even more during that time. Moreover, the price at City Hotel peaked in May & September. The vertical line shown the max price for the respective hotels in a year.
    
  - **_Finding the difference of average total nights stayed between guests who has family & no family._**
  
    ![night_stay_family](https://user-images.githubusercontent.com/63250608/165354592-a6326459-709b-4539-a20b-046c9f3b6e49.png)
    
    Based on the graph above, we can understand that guests which came as family, tend to go for longer stay compared to individual / couple. This is totally understandable as family tends to go for longer vacation compared to individual, who some of them potentially just went for short business trip. We also can see that guests favored to stay longer at resort hotel than city hotel.
    
  - **_How long people stay at the hotels ?_**

    ![length_stay](https://user-images.githubusercontent.com/63250608/165354819-fed2d86a-dcb1-42f7-baf8-427e6e9a8705.png)
    
    Based on the graph above, both resort & city hotel, most of the guests stayed from 1 - 4 nights. However, 7 nights stay can be well liked by the resort hotel's guests as well. Minority of resort hotel's guests also liked to stay in until 14 nights stays.
    
  - **_Number of bookings got canceled due to high lead time_**

    ![lead_time_over_cancellation](https://user-images.githubusercontent.com/63250608/165355236-ef378083-b120-4bc0-85e6-697d81b0ea57.png)

    It shows that,number of people cancelling their booking is higher than not when the lead time is higher than 50 days (cancellation rate is higher at this point).
    
  - **_Find out the busiest month for hotels_**

    ![booking_trend](https://user-images.githubusercontent.com/63250608/165355548-06693be7-ee0b-4be2-80cf-c12d89801fa9.png)

    From graph above, both hotels are pretty much occupied in summer (June - September) due to holiday season. Bookings are pretty much less during winter season (December - March).
    
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

