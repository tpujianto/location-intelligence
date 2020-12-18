---
title: "Data Basics"
draft: false
---

TO DO:
- Introduction
- types of variables
- population vs sample
- descriptive vs inferential
- distributions
- Indepentant and Dependant variable

Effective description and summaries of data is the basis for analyses and sophisticated tasks like statistical modelling. Often, checking basic point estimates of a sample, like the average, median, or mode, can give a researcher the piece of mind that the data she is studying is accurate. Describing a dataset with basic statistics is also the starting point of machine learning tasks like feature engineering. This tutorial will introduce concepts for describing data and the accompanying terminology, that will come in handy during later more challenging examples. We will again use income data from the US Census to work through examples.


### Types of variables

**tree\_id**|**diameter**|**health**|**species**
:-----:|:-----:|:-----:|:-----:
542447|14.1|good|Sophora
542667|38.7|poor|Callery pear
542718|26.2|fair|red maple

In the world of data analytics, statistics, and machine learning, when people refer to variables they typically are referring to the types of information found in columns. The words variables, columns, or features can be used interchangeably. In the above table, a snippet of data taken from the [NYC 2015 Tree Census](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35), there are 4 different types of variables. There are 3 **categorical** variables and one **numeric** variable. Categorical variables are those that are usually names, or identifiers of things. For example, in the above table there is a column called species which are the names of trees species across NYC. There is also a column called health which, as the name suggests, categorizes trees into levels of health. Although both these columns are categorical variables, they are of different types. The health column is what is referred to as an **ordinal** categorical variable, as the different levels of health (poo, fair, good) have an inherent ordering to them. While the species column is a **nominal** categorical variable, as it is non-ordered. The diameter column is a numeric variable, specifically a **continous** numeric variable, as the numbered values can take on any number from 0-infinity \infty. A numeric column can also be a **descrete** variable if the numbers have set intervals, like a person's age in years (typically one is referred to as 16 or 21, not 16.35 or 21.47).

Knowing the types of variables your data is will dictate the type of analysis or modelling method you use to understand it.

### Populations vs Samples

Another important concept in data and statistics is populations and samples. A population refers to **all** the members of a specific group or category, while a sample is a **subset** drawn from a larger population. For example, if I wanted to study the impact of COVID-19 on adults earning below $50k, I could study every adult earning below that threshold across the entire US, the population, or select 1,000 below that threshold from each state, a sample.

Generally, research is conducted on samples, as collecting data on an entire population is very expensive.

### Inferential vs Descriptive Statistics

Related to samples and populations is the concepts of descriptive vs inferential statistics. Descriptive statistics is what is done when a researcher chooses to describe aspects of a sample or characteristics of a population. For example, stating the average, median, or standard deviation of building heights in the US would be a component of descriptive analysis. If a researcher wants to make conclusions about a larger population from sample data, this is known as inferential statistics. A key goal of inferential statistics is to be able to generalize the results obtained from sample data with some level of confidence.

### Distributions

One of the most fundamental concepts in data and analytics is that of distributions. Distributions are simply a collection of data for a specific variable. To illustrate, we can look at median household income as a variable, and look at the distribution of that in NYC. One way to summarize median household income in NYC might be to give the average, ___ or the median ___. While these are useful statistics, they do not give information about the variation of the variable. This is where distributions are useful. The below plot is a histogram of median household income at census blockgroup level in the city. The X axis represents intervals of household income, while the Y axis is the number of blockgroups with incomes within that frequency.




What this plot tells us is that are centered around _____ . From this we can also. There are many ways to plot the distribution of variables, but histograms are one of the most popular. Plotting variables to see the distribution is important as it is a succinct visual way to see the *shape* of data. There are a collection of statistical terms for describing the shape of data. For example, the above distribution of income, is normally distributed. Normal distributions are one of the most common (other names include gaussian or bell curve). Distributions can also tell us the probability of values in a variable will be a certain value. For example, the above distribution is centered on ___ the average, and it can tell us that values within 2 standard deviations of the average are 95%.....

There could be a set of tutorials devoted to distributions which is out of scope of this course. But I encourage you to read up on distributions further if curious. Some good sources:

- fdsjkhfjksdajjkadsh
- fdskfhfdklhsdkn
- fjhjfkHJKSD


### Independence, dependance and correlation between variables

Dependant variables are variables that have some relationship to each other based on some law or mathematical function. For example, if we take income as an example variable again, it is well known that income, for a number of reasons, is related to education. There are a number of ways to study relationships between variables: correlation and regression for continuous numeric variables, chi-squared for categorical variables, mutual information, to name just a few. Knowing whether variables are related is often one of the first steps in analysis, and is crucial for statistical modelling and machine learning. But its equally important to have some intuition about the properties of things like correlation or mutual information in order to avoid relying on spurios correlation or incorrect insights. The old adage: "correlation does not imply causation" holds true. Below we'll discuss the properties of correlation and things to watch out for.

Correlation, specifically the [Pearson correlation coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient), measures the linear relationship between two variables. It can take values anywhere from +1.0 to -1.0. A value of 1.0 means that two variables are perfectly positively correlated, meaning as one value rises by amount *N*, the other will do the same. A value of -1.0 implies perfect negative correlation, meaning they move perfectly in opposite direction, as one value increases by *N* amount the other decreases.

Example of positive and negative linear correlation.

Correlation can sometimes imply that one variable relates to another, but it can also just be a [random coincidence](https://en.wikipedia.org/wiki/Spurious_relationship#Examples) that two things are related. There can also be situations where two things are in fact related to each other but the correlation is 0. Detecting these situations is done by setting up rigorous analysis.

example of relation but no correlation.
