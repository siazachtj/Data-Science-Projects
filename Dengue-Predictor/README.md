# Dengue Prediction using Time Series Models

## 1. Problem Statement

The National Environment Agency (NEA) of Singapore has tasked our team to predict dengue cases in Singapore. Based on her own intuition, the director of NEA thought that weather data (such as rainfall, temperature etc.) and google search trends about dengue should help our dengue prediction, and so required us to explore using these data for dengue prediction. Finally, to help determine if our developed model is useful, the NEA director also tasked our team to come up with a cost-benefit analysis of 2 scenarios: (1) status quo, and (2) if NEA adopts our model.

Our objective: develop a dengue prediction model with a Mean Absolute Percentage Error (MAPE) score of <= 0.2, and a cost-benefit analysis of using our developed model for NEA.

## 2. Methodology

1. Conduct exploratory data analysis (EDA) on Singapore's dengue, weather and google search trends

2. Develop time-series models to predict Singapore's dengue cases and compare the effectiveness of different parameters such as seasonality, exogenous features

3. Conduct a cost-benefit analysis to show whether there is value is using our developed model to predict dengue cases in Singapore

## 3. Conclusion

Our objectives were achieved: (1) we developed a SARIMA model that does a 12-week dengue case prediction with a MAPE score of 0.08, and (2) our cost-benefit analysis showed that the use of our model can help optimize the allocation of NEA resources in terms of time and money.

1. Findings

    1. Model: it was found that weather, google search trends and our self-developed COVID-19 circuit break phases features were ineffective in improving model performance. Ultimately, the shortening of training data used (to better model the new dominant strain of dengue in Singapore) and shortening of prediction range (from 1 year to 12 weeks) improved the model performance to within and beyond the required MAPE score.
    
    2. Cost-benefit analysis: We evaluate the impact of our 12-week dengue cases prediction model on the National Environment Agency's (NEA) dengue inspection process. The model has a Mean Absolute Percentage Error (MAPE) of 0.8, indicating that it can accurately predict dengue cases with a high degree of accuracy.

        Based on sources from [NEA](https://www.nea.gov.sg/media/news) and government websites, NEA conducts an average of 988,000 dengue inspections in a year, with an average of 82,333 inspections per month. The average time taken for each inspection is estimated at 3 minutes, translating to 4116 manhours per month. The manpower hourly rate is estimated at \\$12, resulting in an estimated total cost for inspections of $49,400 per month.

        Our dengue cases prediction model can potentially reduce the number of dengue inspections required by NEA. For example, in our case, as the model predicts an average of 19.31% drop in cases for 12 weeks in advance, this could result in a savings of \\$9,538 in monetary value or 795 manhours.

        These savings can be used in a number of ways, such as reducing monetary costs or reallocating human resources used for NEA dengue inspections to other ongoing campaigns. Additionally, the reduced workload could allow for the stand-down of volunteers from town councils to focus on other social improvements in their respective towns.

        If the model predicts an increase in dengue cases, the NEA may need to allocate additional resources towards dengue inspections in order to mitigate the potential impact of the outbreak. This could include increasing the number of inspections, hiring additional staff, or deploying additional volunteers. In this scenario, the cost benefit analysis would shift, as the benefits of the dengue cases prediction model would no longer be solely focused on cost savings. Instead, the primary benefit would be the ability to respond to an outbreak more effectively, thereby reducing its overall impact on public health.

        For example, if the model predicts an increase in cases several weeks in advance, the NEA could proactively increase the number of inspections and take other preventative measures to help contain the outbreak. This could include increasing public awareness efforts, increasing vector control activities, and working with local healthcare providers to ensure that they are prepared to handle an increase in cases.

        In conclusion, our 12-week dengue cases prediction model offers significant benefits to the NEA's dengue inspection process. By providing accurate predictions of decreasing dengue cases, the model can help to reduce the number of inspections required and ultimately save both time and money, and while a prediction of increasing cases may initially result in additional costs for the NEA, the benefits of the dengue cases prediction model can still be substantial. By providing more accurate and timely predictions of dengue outbreaks, the model can help to mitigate the impact of the outbreak. This highlights the importance of using data-driven models in the public health sector, as they can help to improve overall health outcomes and make the best use of limited resources.
<br>        
        


|               | Baseline                          | Improved 1                                                                                    | Improved 2                                                       | Improved 3                           | Improved 4                                           | Improved 5                                         |
|---------------|-----------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------|--------------------------------------|------------------------------------------------------|----------------------------------------------------|
| Model         | ARIMA                             | ARIMAX                                                                                        | ARIMAX                                                           | ARIMA                                | SARIMA                                               | SARIMA                                             |
| Training data | - 2013 - 2021 monthly dengue data | - 2013 - 2021 monthly dengue data<br>- Weather<br>- Google trends<br>- Circuit breaker phases | - 2013 - 2022 Sep weekly dengue data<br>- Circuit breaker phases | - 2013 - 2022 Sep weekly dengue data | - 2013 - 2022 Sep weekly dengue data<br>- Season = 4 | - 2021-2022 Sep weekly dengue data<br>- Season = 3 |
| Testing data  | year of 2022                      | year of 2022                                                                                  | year of 2022                                                     | year of 2022                         | Oct - Dec 2022 (12 weeks)                            | Oct - Dec 2022 (12 weeks)                          |
| MAPE score    | 0.47                              | 0.36                                                                                          | 0.80                                                             | 0.47                                 | 0.12                                                 | 0.08                                               |

2. Areas for improvement

    1. Improve prediction range from 12-weeks to a longer time period: a model capable of longer prediction ranges will be useful to help NEA better plan for its annual budget, instead of having to raise out-of-cycle budget requests to MOF (in the case when dengue peak is predicted in the next quarter). The team may explore other exogenous features such as the infectivity of different dengue strains, levels of cleaning and maintenance activities (some years were affected by COVID-19), etc., as well as other time-series models.

    2. Enhance and develop a spatial-temporal model: while our model can predict dengue cases quite accurately, it does a nation-wide prediction which may not be useful for NEA ground teams to identify local dengue hotspots within Singapore (e.g. our model cannot tell which town will the predicted dengue cases appear in Singapore). A further enhancement to the model to predict both dengue cases and their occurrence location will be even more useful for NEA awareness campaigns, ground inspections, installation of gravitraps etc. to be effective.
