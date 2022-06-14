# Palm oil's FFB Yield

![Palm Oil](https://cdn.mos.cms.futurecdn.net/mGdgtbdLgJSGEt9Lv9aZuJ-1200-80.jpg)
## Introduction

A team of plantation planners are concerned about the yield of oil palm trees, which seems to fluctuate. They have collected a set of data and needed help in analysing on how external factors influence fresh fruit bunch (FFB) yield. Some experts are of opinion that the flowering of oil palm tree determines the FFB yield, and are linked to the external factors.

## Problem Statement
Perform the analysis, which requires some study on the background of oil palm tree physiology.

## Descriptive Analytics

- ### Data Information
  - It has 8 numerical variable & 1 timedate variable that will be used later for descriptive analysis.
    
- ### EDA
  - **_New Features created_**
    
    ```
    # Create new features based on existing ones
    df["Total FFB"] = df["FFB_Yield"] * df["HA_Harvested"]
    df["Total FFB per Working Day"] = df["Total FFB"] / df["Working_days"]

    # Convert the date to a datetime object
    df['Date'] = pd.to_datetime(df['Date'], format='%d.%m.%Y')
    ```
    
  - **_Correlation for FFB Yield_**
 
    ```
    Total FFB                    0.971782
    Total FFB per Working Day    0.934225
    Precipitation                0.289604
    Working_days                 0.116364
    Min_Temp                     0.103830
    SoilMoisture                -0.003183
    Average_Temp                -0.005494
    Max_Temp                    -0.071201
    HA_Harvested                -0.350222
    ```
    
    Correlation value for **FFB_Yield** was plotted against other features. Features like **Total FFB** & **Total FFB per Working Day** & **HA_Harvested** was ignored to avoid multicollinearity issue. **Precipitation** was found out to be has high correlation with **FFB_Yield**.
 
  - **_Relationship between FFB Yield & Precipitation_**
 
    ![ffbvsprec](https://user-images.githubusercontent.com/63250608/173700826-ad88d990-0562-4c7f-b7e5-7f125516f7bf.png)
    
    Apparently, **FFB_Yield** moves in line with precipitation. Based on the graph above, the yield was fluctuated & seems to drop in the 1st quarter of every year (Jan-March). The water-limited yield is an important benchmark as most of the oil palm cropping systems are rain-fed. Yield reduced if rainfail less than 2000 mm/yr or more than 3500 mm/yr and/or less than 100 mm/yr ([Yield gaps in oil palm: A quantitative review of contributing factors](https://reader.elsevier.com/reader/sd/pii/S1161030116302131?token=C26C735E785F32E0207E3A09E2A3DCD8E9F5350B4670E5D23EF888EF992F21301F374C1359FD5CFA350550E9746918A0&originRegion=eu-west-1&originCreation=20220614155939)).

  - **_Correlation for Precipitation_**


    ```
    SoilMoisture                 0.552001
    Min_Temp                     0.345944
    FFB_Yield                    0.289604
    Total FFB                    0.232475
    Total FFB per Working Day    0.195985
    Working_days                 0.127897
    HA_Harvested                -0.265866
    Average_Temp                -0.369386
    Max_Temp                    -0.461117
    Name: Precipitation, dtype: float64
    ```
  
    Eventually, **Precipitation** has high correlation with **SoilMoisture** as soil water availability depends on the influx of water (rainfall, irrigation, and groundwater), the loss of water (evapotranspiration, drainage, and surface water run-off), and the previous soil water reserve. Precipitation had effects on soil moisture in about 91% in a study ([Quantifying the Effects of Climate and Vegetation
  on Soil Moisture in an Arid Area, China](https://www.mdpi.com/2073-4441/11/4/767/pdf#:~:text=soil%20moisture%20variability.-,Precipitation%20had%20effects%20on%20soil%20moisture%20in%20about%2091%25%20of,87%25%20of%20the%20study%20area.)).
    
  - **_Seasonal decomposition for FFB Yield Datapoints_**
  
    ![season](https://user-images.githubusercontent.com/63250608/173701120-3e4a6643-9609-48ac-8b5a-bce553b16ea2.png)
    
    Seasonal decompose was also executed on **FFB_Yield** datapoints to seperate seasonality & trend. We have observed that there was a sudden drop of FFB yield in 2016 due to El Nino. For the full year of 2016, Malaysia’s total palm oil production fell 13.2% to 17.32 million tonnes, from 19.96 million tonnes in 2015, due to lingering effects of the 2015 El Nino weather phenomenon which affected oil yields ([Palm oil production down 13.2% in 2016 on after-effects of El Nino](https://www.theedgemarkets.com/article/palm-oil-production-down-132-2016-after-effects-el-nino)). 
    
  - **_FFB Yield Trendline for per month from 2008 to 2018_**

     ![ffb_trendline](https://user-images.githubusercontent.com/63250608/173701329-8285ea5f-c902-4cbd-a35d-be8d7372c79b.png)
     
     Based on the graph above, obviously for from year 2008 to 2018, FFB yield is the highest in October & the lowest in February.
    
  - **_Working Days & FFB Yield Relationship_**

    ![workvsffb](https://user-images.githubusercontent.com/63250608/173701429-c065dd3e-c0ed-4a59-ae1f-ed5bfad76f22.png)

     Between working days & FFB Yield, there is a slight positive correlation. FFB need to be harvested & performed any other maintenaince on it during working days. Beside harvesting, worers also need to apply fertiliser, clear overgrowth or remove decaying fronds from the trees ([Malaysia’s palm oil yield to continue declining on labour shortage](https://www.theedgemarkets.com/article/malaysias-palm-oil-yield-continue-declining-labour-shortage)). Eventhough not necessarily, high working days do increase FFB yield.
