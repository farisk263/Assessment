# Palm oil's FFB Yield

![Palm Oil]](https://cdn.mos.cms.futurecdn.net/mGdgtbdLgJSGEt9Lv9aZuJ-1200-80.jpg)
## Introduction

A team of plantation planners are concerned about the yield of oil palm trees, which seems to fluctuate. They have collected a set of data and needed help in analysing on how external factors influence fresh fruit bunch (FFB) yield. Some experts are of opinion that the flowering of oil palm tree determines the FFB yield, and are linked to the external factors.

## Problem Statement
Perform the analysis, which requires some study on the background of oil palm tree physiology.

## Descriptive Analytics

- ### Data Information
  - It has 9 variable that will be used later for descriptive analysis.
    
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
    
   
