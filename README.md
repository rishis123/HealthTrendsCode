Code for internship with Healthtrends. Hospitalization trend Analysis primarily with Prophet, ARIMA, and Poisson Regression till date (7/2/24).


Updates as of 7/25:

- A general point: the number one focus of any data analytics, ML, or AI-focused firm should be sourcing data, whether to train an LLM or a simple linear regression.
- There seem to be only a few ways to do this: one is to buy the data (which for one requires someone to have collected it all along and the ability to do so without fear of HIPPAA or other privacy laws), a solution that is generally untenable for burgeoning startups in this space.
- The other possibility is to use a proxy data source or type that can reasonably mock our target's trends -- this is the path I've been embarking upon, but the core assumption that we are mimicking the data's trends has been unproven of yet. We seek to demonstrate it now with the Nova Scotia dataset and public health datasets in the region, allowing us to potentially take the POC to VCs to get the seed funding to purchase data, or to begin operations directly with unions given a compelling accuracy level.
- The path forward seems with ARIMA, given Prophet and Negative Binomial's respective problems (the latter has some poorly-documented bugs), and XGBOOST's less promising R^2 for the time being.



General Findings:

- Prophet is prone to Overfitting (well-documented)
- At the cost of speed in running and finding optimal parameters for ARIMA (AIC test automates ACF and PACF plot -- see NYS model), it is far better in predictive accuracy. TBD how well it generalizes to timeframes larger than a week. EDIT: Generalizes well up to a month, after which it falls below 70% R^2. Considering longer-term adds further weight to ensemble model or additional features.
- ARIMA speed -- 35 minutes for one county. Across USA or even just NYS will exceed 12 hour runtime of Colab.
- For ARIMA, a lag of one is usually difference, so d=1 is usually a good rule of thumb for data that is not immediately stationary over time (as determined by adfuller)
- Poisson Regression's requirement of mean = variance is quite unrealistic, making Negative Binomial regression a more interesting option.
- SIMLR is interesting (compartmental modelling from epidemiology), but the requirements for rate of flow between susceptible, infected, and removed can be difficult without appropriate domain knowledge. Even then, for something like COVID where the value has changed drastically, it is quite difficult to discern.
- Synthetic data through Gretel (probably using transformer models) is able to extend a small dataset for use in training ML models -- good as a POC. 
- Deep Learning is of interest, although it very computationally intensive, and the rule of thumb in ML is "if a simple model works well,don't overcomplicate things."


Things to Look at/Next Steps: 

- XGBoost, RNN for trend analysis. Predicting future tokens in classification scenario may replicate predicting future values.
- Accessing data with anonymized employees rather than as a time-series allows for promise in SIMLR, where one can probabilistically determine the likely state of an
- employee, and when transitions will occur between Sick, Infected, and Recovered.
- NYS contains a lot of interesting datasets -- after evaluating data quality, it is worthwhile to parse using domain knowledge and consider which of features will be best additions.
- Look into deployment with GCP.
- When using the model over the course of multiple-weeks, shoult the model query the most recent data, clean it up (whole pipeline, well-documented with hospitalization prediction by covid wastewater from CDC), and then retrain the model? It seems best practice for time-series models is to retrain, given how smaller lags are generally better predictors. However, given the involved runtime, is this feasible? Maybe create a CRON scheduled job to automate that process, so that during the remainder of the week, can just call the API endpoint and instantaneously return the alreayd-generated row.
