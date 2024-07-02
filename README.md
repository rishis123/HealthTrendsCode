Code for internship with Healthtrends. Hospitalization trend Analysis primarily with Prophet, ARIMA, and Poisson Regression till date (7/2/24).

General Findings:

- Prophet is prone to Overfitting (well-documented)
- At the cost of speed in running and finding optimal parameters for ARIMA (AIC test automates ACF and PACF plot -- see NYS model), it is far better in predictive accuracy. TBD how well it generalizes to timeframes larger than a week.
- For ARIMA, a lag of one is usually difference, so d=1 is usually a good rule of thumb for data that is not immediately stationary over time (as determined by adfuller)
- Poisson Regression's requirement of mean = variance is quite unrealistic, making Negative Binomial regression a more interesting option.
- SIMLR is interesting (compartmental modelling from epidemiology), but the requirements for rate of flow between susceptible, infected, and removed can be difficult without appropriate domain knowledge. Even then, for something like COVID where the
- value has changed drastically, it is quite difficult to discern.
- Synthetic data through Gretel (probably using transformer models) is able to extend a small dataset for use in training ML models -- good as a POC. 
- Deep Learning is of interest, although it very computationally intensive, and the rule of thumb in ML is "if a simple model works well,don't overcomplicate things."
