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
    
  - **_Correlation between two highly correlated additives_**

    ![heatmap](https://user-images.githubusercontent.com/63250608/173684797-e70e96d0-3ab2-46ac-9f5a-a523d5222cb9.png)
    
    Seems like Additive A & G has high value of correlation coefficient with value of 0.81. While Additive A & E has low value of correlation coefficient with value of -0.54. This show that A & G are positively correlated while A & E are negatively correlated. Since Additive A & G are two different additives, with assumption of these two are independent variables, this can cause multicollinearity which can cause instability to machine learning model, if we plan to create one.
    
## Model Development
  - Standard Scaler was used & decompose it into 2 principal components as part of dimensionality reduction procedure.

    ![pca](https://user-images.githubusercontent.com/63250608/173685550-70c30316-b6a0-4af6-837d-5669c4b894a1.png)

    ![pca_variance](https://user-images.githubusercontent.com/63250608/173685752-364e4c75-1633-4737-9d56-f6af79a39f19.PNG)

    Principal component 1 & 2 was plotted into a graph. Principal component 1 & 2 were composed of 0.27% & 0.22% of variance from the original dataset. The total variance covered by both Principal Components were 0.5068%
    
  - Unsupervised learning was used to perform clustering

    ![elbow](https://user-images.githubusercontent.com/63250608/173686081-f25f3564-227a-4946-ad4e-2ac7131f1107.png)

    Elbow method was used to find out the optimal cluster to used in K Means clustering algorithm. WCSS is the sum of squared distance between each point and the centroid in a cluster. As the number of clusters increases, the WCSS value will start to decrease.
    
  - K-Means:
    
    ![kmeans](https://user-images.githubusercontent.com/63250608/173686241-c413692f-d19c-4f8d-ab58-6fe066345986.png)

    Clustering the principal components using K Means algorithm which been plotted into 3 clusters. K Means is really senstive to outliers as it considers it as one of the cluster as well. In graph above, K Means predicted there are 3 formulation existed based on the created principal components.
  
  - DBSCAN:

    ![DBSCAN](https://user-images.githubusercontent.com/63250608/173686436-c81fe950-283a-454b-80fc-b623826da6cc.png)
    
    DBSCAN algorithm was tested as well with the principal components. DBSCAN is more robust with outliers as it excluded them from any cluster. It detects there are 4 difference clusters / formulation exist in the dataset instead. In graph above, cluster -1 is consider as outliers.
    
   
