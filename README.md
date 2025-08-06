# Greenhouse Gas Emissions: Analysis & Forecasting

This project analyzes the Our World in Data (OWID) CO₂ and greenhouse gas emissions dataset to extract insights and build forecasting models. The primary goal was to explore the historical trends of greenhouse gases (CO₂, CH₄, N₂O) and their relationship with temperature, population, and GDP, and to apply time series forecasting models.
This is my first time series project, and the primary focus was on learning the foundational concepts and building end-to-end pipelines for exploratory analysis and forecasting. This project only explores univariant time series analysis.

## About the data
The project uses the dataset “CO₂ and Greenhouse Gas Emissions” sourced from the websites [World Bank Group Data 360](https://data360.worldbank.org/en/dataset/OWID_CB) ([GitHub Page](https://github.com/owid/co2-data)). The dataset captures an extensive list of datapoints, however for the scope of the project we focus on the following:
|Columns from the dataset|	Analysis - Technology|
|------------------------|------------------------|
|Year, temperature change due to CO₂, CH₄ and N₂O|	Times Series Forecasting - Python|
|Year, Population, GDP, temperature change due to CO₂, CH₄ and N₂O|	Exploratory analysis – Power BI|

The original dataset cover almost all the countries but for the project analysis was conducted on the following countries for the years 1900-2023:
China, India, Italy, France, Turkey, United Kingdom and United States

## Exploratory Data Analysis (Power BI)
PowerBI served as a powerful tool in quickly exploring the dataset and provided two-fold analysis results:
|Analysis type| Analysis details|
|-------------|-----------------|
|Relational analysis| (1) Explore the influence of GDP and pupolation on temperature changes; (2) Analyze correlations and trends|
|Decision-making support| (1) Calculate contribution % to identify which country to focus on; (2) Prioritize focus based the contributions; (3) Encourages deeper analysis of countries with lower emissions|

### Calculating contribution %
Added the [GISTEMP v4 Annual temperature anomalies](https://data.giss.nasa.gov/gistemp/) to the existing OWID data to obtain the contribution %. 
<img width="714" height="426" alt="image" src="https://github.com/user-attachments/assets/47bef4ae-aacd-4691-96c9-a9dbd96c0a9d" />

The OWID temperature change columns made it easy, so the formulae listed below sufficed:
```
GHG contribution % = OWID(temperature_change_from_ghg per year)/ GISTEMP(annual_temperature per year) * 100

CO₂ contribution % = OWID(temperature_change_from_CO₂ per year)/ GISTEMP(annual_temperature per year) * 100

CH₄ contribution % = OWID(temperature_change_from_CH₄ per year)/ GISTEMP(annual_temperature per year) * 100

N₂O contribution % = OWID(temperature_change_from_N₂O per year)/ GISTEMP(annual_temperature per year) * 100
```

### Key Insights:
#### Relational analysis
- Overall GHG emission has a positive correlation with GDP and population.
- No strong correlation among the GHG gases except that they increased with time. 
- The GHG emission patterns plateaued for random durations indicating the complexity of the data without a seasonal pattern.
  <img width="137" height="138" alt="image" src="https://github.com/user-attachments/assets/6bf71815-eb91-41cf-895e-312302fd19a8" />

#### Decision-making support
- US, China and India top the charts for GHG emissions while occupying the same position for GDP growth.
- Population growth of US was much slower than China and India, indicating GDP was the major contributor for US.
- China and India seem to follow a similar trend; GDP and population grew exponetially.
- India's CO₂ and CH₄ contributions are almost similar indicating they are forced to focus on reducing emissions for both the gases.
  <img width="601" height="338" alt="image" src="https://github.com/user-attachments/assets/d3fac7fa-081e-4937-bbde-400387fde02a" />
- UK and Italy managed to reduce their CH₄ emission, which is commendable.
  <img width="629" height="336" alt="image" src="https://github.com/user-attachments/assets/c8f5330c-9899-48ae-8535-f22c4748f773" />
Please find the insights gaianed from each page in the pdf file: [PowerBI_Analysis_Comments.pdf](https://github.com/dasenath/Emission-Time-Series-Forecasting/blob/main/eda-powerbi/PowerBI_Analysis_Comments.pdf)

## Forecasting (Python)
Time series forecasting was done in Python using several models to understand and predict greenhouse gas emissions.

|Models| Calculated Metrics | Residual analysis|
|------|---------|------------------|
|ARIMA(p,d,q)| MAE, MAPE, AIC, BIC| Ljung-Box Test and Jarque-Bera Test|
|Prophet| MAE, MAPE| Ljung-Box Test and Jarque-Bera Test|
|Random Forest| MAE, MAPE| Ljung-Box Test and Jarque-Bera Test|
|LSTM| MAE, MAPE| Ljung-Box Test and Jarque-Bera Test|

### Choosing the best model
Seperate models were built for each country and the train-test split was 1900-2010 and 2011-2023 respectively. The following rules were used to select the best model:
    1. Top 3 models with least MAE was selected
    2. If the models selected were ARIMA then compared the AIC values (this was not a large model)
    3. Using the Ljung-Box and Jarque-Bera Tests the confidence of the model was labelled as high, medium and low
```
IF Ljung-Box >= 0.05 AND Jarque-Bera >= 005 THEN "low"
IF Ljung-Box >= 0.05 AND Jarque-Bera < 005 THEN "medium"
IF Ljung-Box < 0.05 AND Jarque-Bera >= 005 THEN "medium"
IF Ljung-Box < 0.05 AND Jarque-Bera < 005 THEN "high"
```

### Model metrics
The table below displays the model metrics for the three countries Chine, India and US.
<img width="1566" height="399" alt="image" src="https://github.com/user-attachments/assets/cf812183-5624-4da4-b29e-8ecbe22afec8" />


## Challenges faced and overcame
- The Ljung-Box and Jarque-Dera test results from SARIMAX report and the functions from sklearn and scipy were different. So checking the units is key when comparing.
- Identifying the right data sources especially the secondary sources,  GISTEMP v4, is crucial.


## Improvements & Next Steps
- Hyperparameterize Prophet, Random Forest and LSTM.
- Explore other Machine learning (XGBoost, SVM) and Deep learning (Transformers, CNN, GRU) models.
- Perform multi-variant analysis especially with GDP and population (strong correlation was observed).
