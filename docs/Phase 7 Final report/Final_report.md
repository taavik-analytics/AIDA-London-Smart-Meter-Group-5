# Phase 7 (Final report)

## Introduction

This project report details our analysis of the London Smart Meter dataset and our efforts to develop multiple machine learning models to predict future energy consumption. The dataset contains detailed energy usage readings from residential areas across London, offering a valuable resource for forecasting.

The goal was to utilize this data to create models that predict daily energy usage, thereby helping utility providers optimize energy distribution and enhance conservation efforts. The report describes the data preprocessing, including handling of missing values and feature engineering, followed by exploratory data analysis to uncover key consumption trends.

We evaluated various predictive models such as linear regression, decision trees, and advanced deep learning methods, focusing on their accuracy using metrics like MAE and RMSE. The findings offer insights for energy management in urban settings and suggest practical applications for policymakers and planners.

This report provides a comprehensive overview of the modeling process, results, and their implications for improving energy management in cities like London.

## Phase 1

The initial phase of our project, focused on business understanding, established a comprehensive framework for forecasting energy consumption using the London Smart Meter dataset. The primary goal was to leverage this data to enhance various aspects of energy management, including optimization of supply, dynamic pricing strategies, and efficient resource allocation.

**Business Objectives**

- Energy Supply Optimization: By predicting consumption patterns, energy suppliers can enhance their production and distribution plans, thereby reducing waste and operational costs.  
- Dynamic Pricing Strategy: Implementing pricing models that adjust based on anticipated demand will help balance the grid load and offer cost savings during off-peak times.  
- Current Situation Analysis: Current challenges involved fluctuating energy demands, inefficient distribution, and a lack of targeted energy-saving recommendations. Addressing these issues through data-driven insights can significantly improve operational efficiency and customer satisfaction.

**Initial Data Analysis and Modeling Goals**

- Understand Consumption Patterns: Analyzing daily, weekly, and seasonal trends, including the impact of weather on energy usage.
- Predict Peak Demand Times: Forecasting high-demand periods to better manage grid loads.
- Identify Energy Saving Opportunities: Detecting inefficient usage patterns to provide targeted recommendations.
- Project Goals:
    - Develop models that predict energy consumption with high accuracy within a 24 to 48-hour window. These models would support dynamic pricing and help in formulating better energy policies and maintenance scheduling.

**Measurement of Success**

- Reduced energy production costs through improved demand forecasts.
- Enhanced customer satisfaction through dynamic pricing and effective communication of energy-saving tips.
- Lower carbon emissions due to optimized energy distribution.

**Technologies and Skills**

The project utilized technologies such as Pandas for data manipulation, Scikit-learn and TensorFlow for modeling, and visualization tools like Matplotlib. The team comprised of data engineers, data scientists, business analysts, and project managers, each bringing essential skills to the project, from technical expertise in data analysis to strategic business planning.

## Phase 2

For the successful implementation of our energy consumption forecasting project, selecting the right datasets from the extensive "Smart Meters in London" dataset was crucial. The datasets were chosen based on their relevance to the project's primary objective: to forecast daily electricity consumption while taking into account variables such as weather conditions and public holidays. This chapter details the datasets included in and excluded from the project, providing a rationale for each decision.

### Datasets included in the project

- daily_dataset.csv
    - Contained daily records of household energy consumptions.
    - Pivotal dataset for analyzing daily energy usage patterns and to form the backbone of the predictive modeling.
- information_households.csv
    - Provided details about the variety of households, Acorn groups and associated data.
    - This dataset was essential for demographic segmentation and understanding the socio-economic factors that influence energy usage.
- uk_bank_holidays.csv
    - Provided a list of UK bank holidays during the study period.
    - This dataset was used to adjust models for public holidays, which typically see different energy usage patterns due to changes in daily routines.
- weather_daily_darksky.csv
    - Daily weather data sourced from the DarkSky API, specific to London during the study period.
    - Allowed integration of weather variables into the models, considering the impact of weather conditions on energy consumption.

### Datasets Excluded from the Project
- daily_dataset folder
    - Contained detailed block files that are redundant given our focus on aggregated daily data from the daily_dataset.csv.
- halfhourly_dataset folder
    - Provided half-hourly energy consumption data which, while detailed, is unnecessary for our daily consumption forecasting model.
- hhblock_dataset folder
    - Included highly granular transposed daily data for each household, which is more detailed than needed for our current analysis scope.
- acorn_details.csv
    - Offered in-depth demographic and psychographic profiles that are not required for the initial phase of our forecasting model.
- weather_hourly_darksky.csv
    - The hourly weather data, while comprehensive, provided more granularity than required for daily consumption predictions.

### Data exploration

Phase 2 of our project involved a meticulous data preparation and exploratory analysis process using selected datasets from the "Smart Meters in London" collection. This phase was crucial for understanding the underlying patterns in energy consumption and preparing the data for predictive modeling.

**Data Loading and Initial Analysis**

We began by loading the necessary datasets, which included daily energy consumption records, household information, daily weather data, and UK bank holidays. For each dataset, we conducted an initial statistical analysis to understand the structure and quality of the data. This involved checking for the number of rows, duplicates, data types, and possible NaN-values. Descriptive statistics were also generated for each dataset to provide insights into the distribution of the data.

**Weather Data Processing**

Given the hourly measurements in the weather dataset, we calculated daily averages for each weather parameter to align with our daily consumption data. This simplification was essential for integrating weather variables effectively into our predictive models.

**Data Integration**

We merged the weather averages and the daily energy consumption dataset using the 'date' column as a common identifier. Additionally, we enhanced the dataset by marking UK bank holidays, providing context for anomalies in energy usage on those dates. The household dataset was incorporated using the 'LCLid' column, ensuring that each entry represented a day's measurement for a specific household.

**Exploratory Data Analysis**

Using visual tools like heatmaps, we explored correlations between variables, identifying significant relationships and potential redundancies. For example, we found a strong negative correlation between energy consumption and temperature, and a similar pattern between cloud cover and temperature.

**Visualization of Energy Consumption Trends**

We created visualizations to examine energy consumption trends over time, focusing on annual and monthly patterns. This analysis highlighted seasonal variations in energy usage and the impact of specific holidays on consumption patterns. Scatter plots were also employed to identify and assess outliers in the dataset, which informed decisions on data cleansing.

## Phase 3

Phase 3 of the project involved extensive data preparation activities to ensure that the datasets were optimally configured for the predictive modeling phase. This stage was crucial for enhancing the accuracy and effectiveness of the models developed later in the project.

**Data Cleaning and Transformation**

The phase commenced with the reading of the 'combined_data.csv' file, a dataset compiled from earlier phases. Key steps included:

- **Removal of Unnecessary Columns**: Columns that did not contribute to predictive modeling or contained redundant information were removed to streamline the dataset.
- **Transformation of Categorical Data**: We converted all string-type columns to numeric to facilitate their use in machine learning algorithms. This included mapping unique values in 'Acorn' and 'Acorn_grouped' columns to numeric values and transforming the 'Type' column into a binary 'isHoliday' column.
- **Handling NaN Values and Outliers**: Rows containing NaN values were removed, and outlier values identified in the scatter plots from the previous phase were dropped to maintain the integrity of the dataset.

**Feature Engineering**

Significant feature engineering efforts were undertaken to enhance model performance:

- **Date Handling**: Initial attempts to convert the 'date' column into a cyclical format were unsuccessful, leading us to use time-series data as the index instead. This adjustment helped better capture the temporal patterns in energy consumption.
- **Target Variable Adjustment**: The 'energy_sum' column was initially used as the target variable for predictions. However, due to its complexity, we switched to 'avg_energy', reducing data dimensionality and making the dataset more manageable.
- **One-Hot Encoding**: We applied one-hot encoding to the 'acorn' and 'acorn_grouped' columns to ensure categorical data would not be misinterpreted by the models. Later, these columns were discarded after summing up energy consumptions across different acorn groups, which made socio-economic information redundant.

**Data Splitting and Scaling**

The data was split into train and test sets to prevent data leakage. This was followed by scaling using StandardScaler() fitted only on the training set. Notably, an experiment without scaling provided better performance, leading us to adopt non-scaled data in some models.

**Saving Data**

All datasets prepared and used during this phase were saved to .csv files for transparency and ease of access during the modeling phase.

## Phase 4
In phase 4, our objective was to refine our predictive modeling approach to accurately forecast the average daily electricity consumption of households using a combination of weather and temporal variables.

**Refinement of Preprocessed Data for Modeling**

During this phase, we further refined our preprocessed data to ensure compatibility with machine learning and deep learning models. We identified the most correlated features for our prediction task and executed the train-test split, where the test data represents the last month of the time series.

**Modeling**

Our modeling phase involved implementing various algorithms to develop a robust predictive model:

- **LSTM (Long Short-Term Memory)**
- **XGBoost**
- **GradientBoostingRegressor**
- **RandomForestRegressor**
- **LinearRegression**
- **Support Vector Machine (SVM)**
- **KNN Regressor**
  
We applied these algorithms to our preprocessed data and rigorously evaluated their performance metrics, including RMSE (Root Mean Squared Error), MAE (Mean Absolute Error), and R² (Coefficient of Determination). The results were visualized to gain insights into the effectiveness of each algorithm in predicting electricity consumption accurately.

## Phase 5

In Phase 5 of our project, we conducted a comprehensive evaluation of various predictive models to assess their effectiveness in forecasting daily energy consumption. The algorithms tested included LSTM (Long Short-Term Memory networks), XGBoost, Gradient Boosting Regressor, Random Forest Regressor, Linear Regression, SVM (Support Vector Machine), and KNN (K-nearest Neighbors). This phase was crucial for determining the optimal approach for our predictive needs and setting the stage for future enhancements.

**Model Performance Analysis**

Our analysis revealed that while all the algorithms provided reasonably good predictions, they generally tended to underpredict energy consumption compared to actual figures. This consistent underprediction highlighted potential areas for improvement in both data handling and model configuration.

**Detailed Model Performance**

- **LSTM (Long Short-Term Memory)**: Best results among all models, albeit sometimes predicting slightly lower values than the actual figures, likely due to its conservative approach to avoid large errors.
- **XGBoost**: Produced second-best results, offering predictions very close to the target values without typically exceeding them.
- **Gradient Boosting Regressor (GBR) and Random Forest Regressor (RFR)**: Both models, based on decision tree structures, performed well with consistent but slightly overestimated energy consumption predictions.
- **Linear Regression (LR)**: Showed good results; however, it might be sensitive to the linearity assumptions and tended to slightly underestimate energy consumption.
- **Support Vector Machine (SVM)**: Delivered consistent but not top-tier results compared to other models, typically underestimating energy consumption.
**K-Nearest Neighbors (KNN)**: Had the most variable performance, often overestimating energy consumption considerably more than other models.

**Exploring Solutions and Future Enhancements**

To enhance the accuracy and reliability of our models, we identified several strategies to explore in future work:

- **Feature Engineering**: We considered creating new features from existing data, such as calculating average energy consumption based on time variables like the day of the week or hour, and incorporating these as new inputs to the models.
- **Hyperparameter Optimization**: Testing different hyperparameter settings across models to find the most effective configurations was identified as a key area for potential improvement.
- **Ensemble Modeling**: Combining multiple models into a single ensemble approach was suggested as a way to leverage the strengths of individual models and potentially improve overall prediction accuracy.
- **Advanced Neural Network Models**: Investigating more complex neural network architectures, such as CNNs (Convolutional Neural Networks) and RNNs (Recurrent Neural Networks), was planned to capture deeper patterns and relationships within the data that simpler models might miss.

## Phase 6

Phase 6 was designed to be the final step in our project, involving the deployment of the predictive model as a web service on a cloud platform, accessible via an API. Although we did not execute this phase due to time constraints, the following outlines our planned approach for transitioning from a developmental framework to a fully functional service

**Deployment Strategy**

The model was intended to be deployed as a web service, providing an API for easy access and interaction. This setup was aimed at enabling real-time energy consumption forecasting and ensuring that stakeholders could seamlessly integrate our model into their operations.

**Monitoring System**

We planned to implement a robust monitoring system to track the model's performance continuously. Key performance metrics such as Mean Squared Error (MSE), Mean Absolute Error (MAE), and R^2 Score were to be used to assess accuracy and predictive power. Automated alerts were also planned to notify the team of any significant performance drops, allowing for quick identification and resolution of issues.

**Maintenance Procedures**

An automated pipeline was envisioned to manage data collection, model retraining, and deployment updates efficiently. This would have minimized human intervention, maintained the relevance and accuracy of the model, and ensured efficient operation over time.

### Assesment of the Implementation Plan

**Objective Achievement**

The project's objectives were fully met through the initial phases, with a variety of models developed to optimize prediction accuracy. Although Phase 6 was not implemented, the groundwork laid in earlier phases confirmed the viability and effectiveness of our approach.

**Methodology**

The methodologies and techniques chosen were proven effective in earlier stages. Our strategy of employing multiple models to leverage the strengths of each demonstrated significant potential. Had Phase 6 been implemented, it would likely have further validated these methods.

**Challenges and Solutions**

During the project, we faced challenges such as data interpretation and the integration of cyclical date information. Solutions such as transforming dates into sine and cosine components were identified and would have been fully integrated in this phase to ensure accurate model processing.

**Teamwork**

The project team was highly collaborative and effective, with clear communication and well-defined roles ensuring a productive workflow. This strong teamwork was anticipated to continue into the deployment phase, ensuring a smooth transition to operational status.

**Learnings**

The importance of rigorous data preparation was a key takeaway from the project. Our extensive work in earlier phases highlighted the complex requirements of preparing data for predictive modeling, a lesson that would have significantly influenced our deployment strategies.

## Conclusions

**Phase 1**

The selection process was guided by the project's need to balance detail with manageability, ensuring that the datasets included would provide sufficient information without overwhelming the analysis process. This careful selection allowed us to focus on constructing a robust model that forecasts daily energy consumption efficiently, taking into account significant variables such as weather conditions and holiday impacts. This focused dataset approach layed a solid foundation for our subsequent analysis and modeling phases, setting the stage for meaningful insights and actionable forecasts.

**Phase 2**

This phase was instrumental in setting the stage for model development. By refining our datasets and gaining a deeper understanding of the factors influencing energy consumption, we were better equipped to develop accurate predictive models. The insights gained from the exploratory analysis not only validated our approach but also highlighted the complexities of energy consumption patterns, which were critical in the subsequent modeling phase.

**Phase 3**

Phase 3 was pivotal in preparing the data for subsequent predictive modeling. The thorough cleaning, transformation, and feature engineering processes ensured that the datasets were not only clean and well-structured but also aligned with the needs of the modeling techniques to be applied in the next phase. This preparation set a solid foundation for achieving robust and accurate model performance.

**Phase 4**

Phase 4 was pivotal in enhancing our project's predictive capabilities. Through meticulous data refinement and comprehensive modeling, we were able to significantly advance our understanding and application of various predictive techniques. The rigorous evaluation of model performance not only highlighted the strengths and weaknesses of each algorithm but also set the stage for future improvements.

The insights gained from this phase are invaluable, guiding us toward more nuanced model adjustments and the potential integration of additional data sources or features. As we move forward, the foundation laid in this phase will be instrumental in achieving even greater accuracy and reliability in our predictive models, ultimately aiding in more effective energy management and planning.

**Phase 5**

Phase 5 was a pivotal moment in our project, bringing to light the strengths and limitations of our current predictive models and paving the way for targeted improvements. The insights gained not only enhance our understanding of the models' dynamics but also chart a clear course for future enhancements that will refine our forecasting abilities. With a strategic plan in place for advancing our models, we are well-positioned to develop more accurate and reliable tools for energy consumption forecasting, ultimately leading to more effective energy management and planning strategies.

**Phase 6**

Although Phase 6 was not executed due to time constraints, the planning and preparatory work completed provide a comprehensive blueprint for future deployment. This phase was designed to ensure that the predictive model would remain a valuable and effective tool in energy management, driving informed decision-making through advanced analytics.

**Summary**

Throughout the project's six phases, we systematically advanced from selecting and preparing datasets to developing and evaluating predictive models for forecasting daily energy consumption. Each phase built upon the insights and groundwork of the previous one.

Together, these phases underscored a continuous progression toward developing reliable predictive tools for energy management, emphasizing systematic evaluation, and strategic planning for ongoing improvements and deployment readiness.
