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

When it came to training and testing data, our wrangling was largely similar. Firstly, we dropped several geographical features such as address and block. This is because these didn't add very much to our model given that we already had geographical coordinates and specific trap information.

## Feature Engineering

## Modeling

## Cost-Benefit Analysis

## Presentation
