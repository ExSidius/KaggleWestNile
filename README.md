# Kaggle West Nile Virus Challenge
### A group project by _Beguiling West Niling_

Members include:

* Russell Brown

* Stephanie Jones

* Mukul Ram

* Brice Wolfgang

During this group project, we undertook the task of making accurate predictions for Kaggle's West Nile Virus Challenge.

The specs. of the challenge asked us to look at a combination of mosquito information, weather information, and spray information over the years, and make predictions based on them.

We were expected to refine a problem statement, wrangle the given data, engineer new and relevant features, build a capable model, create a cost-benefit analysis based on our predictions, and make a presentation on all of the former.

## Problem Statement

> Can you predict the probability of an outbreak of West Nile virus in a given location on a given date?

Our problem statement was provided to us by Kaggle, which meant we didn't have to do a lot of mulling over it.

## Data Wrangling

When it came to our spray data, wrangling was minimal. Firstly, the spray data barely overlaps with our test set, which meant we didn't think we'd be using it very much.
However, when we took a brief look at the contents of each of the columns, we discovered that the 'Time' column contained plenty of missing values, and so we dropped it entirely.

For weather data, there were two primary problems to deal with. The first was that there was a lot of missing data and it was stored as either NaN or 'M'. This meant we had to parse through all the 'M's and set them to NaN. The second problem to deal with was that columns that contained numeric data were often stored as strings. So instead of storing 10.0, we'd be storing '10.0'. We had to manually dive into these and fix them so that they were usable.

When it came to training and testing data, our wrangling was largely similar. We dropped several geographical features such as address and block. This is because these didn't add very much to our model given that we already had geographical coordinates and specific trap information.

The initial plan was to dummify Trap and Mosquito species information. At some point we decided to switch and factorize this data instead (in order to make the columns more manageable). The idea was that given that we were using XGBoost - a tree based method - the two were virtually the same. However, for the species, we decided to switch back to dummified data. The reasoning behind this was - 

* We want to provide the model as much information as possible _without_ creating additional interaction terms (this is how one optimizes XGBoost performance).
* Pipiens/Restuans indicates presence of both, and therefore similarity to Pipiens as well as Restuans (two species). We wanted to make it clear that the combined column possessed this similarity information.
* We wanted to mark the Unspecified Culex column within the testing data as being akin to Pipiens/Restuans, since those were the most common.

_These changes resulted in marked improvements to our model._

## Feature Engineering

Given that Number of Mosquitos had a high correlation with West Nile Virus, and our test data doesn't contain it, we attempted to find a way to compute an approximate version of it. This is when we discovered that leakage was an extremely useful feature.

1. Every time an instance hits 50 mosquitos, it's split into another row that contains almost identical information (except potentially Number of Mosquitos). This held true for both training and test data. Therefore, simply by computing the number of duplicate rows, we were able to determine an approximate way to determine how many mosquitos there are, and therefore whether there is West Nile Virus.

2. We also factorized the trap.

3. We converted the date to an ordinal form and got rid of 'Year' (since it conveys virtually the same information). 'Week' information, however, was useful - presumably because it reflects seasonal patterns in mosquito breeding.

4. We calculated 'Days Since a Trap was Last Checked'. This was extremely useful given that if a trap hadn't been checked for a long time, it would naturally have many more mosquitos despite not actually indicating the presence of West Nile Virus.

5. We also engineered a 'Spray Factor' that was based on the distance from a centroid and time since spraying. Unfortunately, this only affected the model mildly since we have virtually no spray data for our test set.

6. We dummified weather codes as well, but these (for the bulk of them) proved to have very little feature importance. Only in cases of thunderstorm and similar such phenomenon was there a noticable effect.

7. We created a length of day feature - this proved integral to the model.

8. We aggregated weather data across the two stations to get a more accurate representation of the weather conditions.

9. The most computationally tricky features we engineered were time lagged data that had been operated on.
Our function could compute, for example, the maximum temperature over the last 14 days, or the average humidity over the last 3 days.

10. We also weighed each Trap manually - we created a count showing how often each Trap shows up, and how often each Trap shows up by year.

## Modeling

Our final model was an XGBoost model with a learning rate of 0.2.

Our best model gave us a public AUC-ROC score of *0.76892* and a private score of _0.76117_.

Our second-best model (which must have been a tad overfit) gave us a public AUC-ROC score of *0.77068* and a private score of _0.75741_.

For the sake of curiousity, we also weighted our model (took all probabilities in the year 2012 [which saw a mosquito breakout] and multiply them by 3 [and rounding down to 0.99 if probabilities exceeded 1]). We didn't wish to make this our official model because it made use of external data, which was prohbhited by the competition and defeats common sense. This gave us a public AUC-ROC score of *0.80424* and a private score of _0.79563_.

## Cost-Benefit Analysis

We also conducted a rigorous cost-benefit analysis to go with this project.

## Presentation

[You can find the accompanying presentation here.](goo.gl/BEpydd)
