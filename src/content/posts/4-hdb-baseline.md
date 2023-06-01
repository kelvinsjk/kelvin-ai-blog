---
title: "Baseline 1 for HDB Resale Price Predictor"
description: "Baseline for HDB Resale Price Predictor via linear regression in a spreadsheet"
date: 2023-05-19T12:00:00Z
image: "/images/posts/04.jpg"
categories: ["baseline", "hdb prices"]
authors: ["Kelvin Soh"]
tags: ["linear regression", "spreadsheet"]
draft: false
---

To get a first baseline result for the HDB resale price predictor project,
we will be running a (multiple) linear regression in a spreadsheet.

We got a standard error in the region $17,000 to $19,000,
which is already pretty decent in my opinion considering the variations
due to data not considered (we have omitted the block and street data,
and do not have indications on the state of the apartment like the renovations done).
AI will have quite a hurdle to overcome to beat these results, though I'm intrigued by
its ability to parse the definitely non-linear block and street data.

## Link to data

Access the data used (and the results) via this
[Google Sheets link.](https://docs.google.com/spreadsheets/d/1vqtDy8BJrevJ6LVWbF1QDtFkg6vJlZ-ztkSYSFvbgY4/edit?usp=sharing)
The Data was retrieved from the [HDB Resale Flat Prices Service](https://services2.hdb.gov.sg/webapp/BB33RTIS/)
on 2023 May 9, for 3 room apartments in Woodlands Town for the last 12 months (all other fields are left empty).

## Independent variables

The data provided by HDB are as follows (taking out the dependent variable, resale price)

- Block
- Street Name
- Storey
- Floor Area / Flat Model
- Lease Commence Date
- Remaining Lease
- Resale Registration Date

### Variables not used in this model

We will not use the **block** and **street** data for this baseline model as those will clearly be non-linear in nature.

The **lease commence date** and _remaining lease_ are essentially the same data point. The remaining lease data is
more detailed (including months in addition to years) while the lease commence date only provides the year. We have decided
to use only the remaining lease data simply because it's requires less cleanup, and I do not expect the extra detail to make
a huge difference. This could be investigated at a later time.

The **Flat Model** is more of a categorical data, and highly correlated with the _floor area_. For this first baseline,
we have decided to omit the flat model type.

## Data clean-up

The **storey** data provided is a range (e.g. "10-12") so we cleaned it up by taking the middle number,
and will be labeled `floor` for future considerations.

The **floor area** is parsed out as a number from the "floor area/flat model" data field and will be labeled
`area` for future considerations.

The **lease commence date** is already a number (the year) and is left untouched and will be labeled `year`.

Finally, the **resale registration date** is parsed as is (a date) and labeled `date`.

## Linear regression and results

We then use the `LINEST()` function to perform multiple linear regression on
the dependent variable of `price` to the independent variables `area`, `year`,
`date` and `floor`.

When used on the entire data set, we obtained an `r2` value of 0.89 and a standard error of $17654.

While a linear regression will not run into too much overfitting issues,
this is not the same with machine learning.
We will need a training set and validation set once we start using AI.

Moreover, the
[Deep Learning for Coders with fastai & PyTorch](https://course.fast.ai/Resources/book.html)
book recommends taking out later dates as the validation set for time-series data. Thankfully,
the data from HDB is initially sorted by date using the latest.

We thus arbitrarily take out the first 60 rows from our model in order for a fair comparison
between a linear regression model and machine learning models.

Re-running `LINEST()` again on this reduced data set gave us similar results as the first:
an `r2` value of 0.89 and a standard error of $17493. The standard error for the validation set,
is slightly higher at $18987.

> The "score to beat" at the moment is a standard error of **$18987** based on
> a validation set of the most recent 60 transactions.

## Concluding thoughts

I'm surprised a simple linear model using only 4 dependent variables
gives us a rather strong result. As such, I do not expect using AI will improve
our predictions much.

However, I am interested in the use of non-linear models, especially for the
block and street level, and will see it as a win if AI can incorporate these data
and improving the predictions by even just a smidge.

## The cover image

Generated by [Stable Diffusion Online](https://stablediffusionweb.com/).
I used the following prompts, and got something I like at the 4th attempt.

Note to self: do not key "child robot" again ever. Having a
picture of a child/infant with features slightly deformed to look more
like a robot is pretty creepy. When I feel like living life dangerously
(maybe in the next post?), perhaps I can try "robot child" to get
what I was thinking of initially.

1. Baseline prediction
2. Building blocks
3. Child robot playing with building blocks
4. Kitten playing with building blocks