# Project 2 - Ames Housing Data and Kaggle Challenge
# L. Minter
# October 22, 2021


## Problem statement
How can contractors help homeowners assess the value of a remodel and prioritize projects with high return on investment?  As a contractor specializing in remodeling we want to provide customers with information about how the propose work will affect their property values.  We are building a model to predict home prices in Ames, Iowa based on housing market data with basic home characteristics.  We are looking for a model with high predictive accuracy that can be easily interpreted in terms of common remodeling projects.  
---
## Background
Contractors typically work with homeowners to develop and bid home remodels.  One common concern during the process is the difficulty in assessing the value-added to the home as a result of the remodel.  Homeowners often look at the bid proposal as entirely out of pocket costs without considering the value-added.  Sticker shock can lead the homeowner to forgo the project completely instead of focusing on aspects that could help with resale value.  

We seek to provide a tool that will help homeowners assess the value-added by potential home remodels based on the basic information about the home and proposed plan.  We expect that this tool will help allay concerns over the cost of remodeling for some fraction of clients.  Our hope is that it will increase job to bid ratio which is frequently quite low.  
---
## Data Dictionary

|feature|type|description|
|---|---|---|
| `GasA Heat`          |int64 | Has gas forced warm air furnace|
| `Central Air`        |int64 | Has Central Air installed *{modified from original `Y`/`N` }*|
| `Electrical_SBrkr`   |int64| Has Standard Circuit Breakers & Romex |
| `insideLot`          |int64| Lot Config is an inside lot |
|`Neighborhood_CollgCr`|int64|is in the College Creek neighborhood|
|`Neighborhood_OldTown`|int64|is in the Old Town neighborhood|
|`Neighborhood_Edwards`|int64| in in the Edwards neighborhoods|
|`Neighborhood_Somerst`|int64| is in the Somerset neighborhood|
|`Neighborhood_NridgHt`|int64|is in the Northridge Heights neighborhood|
|`Neighborhood_Gilbert`|int64|is in the Gilbert neighborhood|
|`Neighborhood_Sawyer` |int64|is in the Sawyer neighborhood|
|`Bldg Type_2fmCon`    |int64 | buliding is two-family Conversion; originally built as one-family dwelling|
| `Bldg Type_Duplex'`  |int64| building is a duplex
| `Bldg Type_Twnhs`    |int64| building is a townhouse |
| `Bldg Type_TwnhsE`   |int64|building is a townhouse end-unit|
| `Sale Type_COD`      |int64|sale was Court Officer Deed/Estate|
| `Sale Type_New`      |int64|sale was for new Home just constructed |
| `Yr Sold_2007`       |int64|sold in 20017|
| `Yr Sold_2008`       |in64|sold in 2008|
| `Yr Sold_2009`       |int64|sold in 2009|
| `Yr Sold_2010`       |int64| sold in 2010|
| `Total Bath`         |int64|total number of bathrooms (half baths + full baths)|
| `livingarea*bath`    |float64| interaction term for above grade living area and total baths|

---

## Model(s)

Normalized Linear Regression: Use StandardScaler with RidgeCV and Lasso to fit normalized data and look at feature selection.  These models are less interpretable but are good at determining which features are irrelevant.  

Ordinary Multiple Linear Regression: This model is easily interpretable with the coefficients directly in value added per unit of input.  The inputs for this model were selected based on the needs of contractors (typical remodeling aspects) as well as the information from the Lasso model about which features were relevant to the overall fit.  The final model combined features related to remodels with immutable aspects of the houses that affect price (e.g. year built, neighborhood)


## Evaluating the Model
All models had good predictive performance with R2 scores hovering at 0.85.  The This means that the models describe 85% of the variation in prices.  

The RMSE of each model was roughly 33000 compared to the RMSE of 77000 for a dummy model using the mean price.  

A plot of the actual sale prices versus predicted prices shows good agreement.  The plot of the residuals shows they are normally distributed.  Overall model performance looks good.  

Across all error metrics there was no appreciable difference in the training and testing scores indicating a low variance model.  The relatively high R2 score indicates a low bias.  By the metrics we have a model with a approximately appropriate number features.  To make improvements we could work on interaction terms to reduce collinearity.  

## Interpreting the Model
Keeping all else equal, an extra half bath adds $3249 to the value of a home.
Keeping all else equal, an extra full bath adds $6932 to the value of a home.
This can be useful to customers who are considering which type of remodel to do and comparing the costs and value added for different bathroom options.

Keeping all else equal, every extra square foot of deck increases the value by about $\$9.50$.  Thinking in terms of value-added this can help ease customers minds about the price of installation.  

## Conclusions and Recommendations

We can use the above information about added-value for typical remodel jobs in promotional materials and in meetings with clients. We can test deployment of the model coefficients in the field during job bidding to see if the information is effective in increasing the jobs to bids ratio.

We should consider refining our model by introducing more recent sales information and examining more closely the effect of the 2008 housing crash on the data in question.  We could continue to work on feature selection to ensure a fully stable model with clear interpretability.  We could trim the data set to be more in line with homes that are likely to be remodeled but would need to use outside information to determine those characteristics.  

We should continue to develop the more "black box" (Lasso) model into a tool for contractors to used when suggesting potential remodel options to the client.  Having it as a tool for contractors instead of a customer-facing tool could avoid the issues with black box issues and allow for comparison of the two models before interpretation and presentation to the client.  
