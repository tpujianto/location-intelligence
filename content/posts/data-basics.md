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
- relationship between variables

Effective desciption and summaries of data is the basis for analyses and sophisticated tasks like statistical modelling. Often, checking basic point estimates of a sample, like the average, median, or mode, can give a researcher the piece of mind that the data she is studying is accurate. Describing a dataset with basic statistics is also the starting point of machine learning tasks like feature engineering. This tutorial will introduce concepts for describing data and the accompanyig terminology, that will come in handy during later more challenging examples. We will again use income data from the US Census to work through examples.


### Types of variables

**tree\_id**|**diameter**|**health**|**species**
:-----:|:-----:|:-----:|:-----:
542447|14.1|good|Sophora
542667|38.7|poor|Callery pear
542718|26.2|fair|red maple

In the world of data analytics, statistics, and machine learning, when people refer to variables they typically are referring to the types of information found in columns. The words variables, columnes, or features can be used interchangeably. In the above table, a snippet of data taken from the [NYC 2015 Tree Census](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35), there are 4 different types of variables. There are 3 **categorical** variables and one **numeric** variable. Categorical variables are those that are usually names, or identifiers of things. For example, in the above table there is a column called species which are the names of trees species across NYC. There is also a column called health which, as the name suggests, categorizes trees into levels of health. Although both these columns are categorical variables, they are of different types. The health column is what is referred to as an **ordinal** categorical variable, as the different levels of health (poo, fair, good) have an inherent ordering to them. While the species column is a **nominal** categorical variable, as it is non-ordered. The diameter column is a numeric variable, specifically a **continous** numeric variable, as the numbered values can take on any number from 0-infinity \infty. A numeric column can also be a **descrete** variable if the numbers have set intervals, like a person's age in years (typcially one is referred to as 16 or 21, not 16.35 or 21.47).

Knowing the types of variables your data is will dictate the type of analysis or modelling method you use to understand it.

### Populations vs Samples

Another important concept in data and statistics is populations and samples. A population refers to **all** the members of a specific group or category, while a sample is a **subset** drawn from a larger population. For example, if I wanted to study the impact of COVID-19 on adults earning below $50k, I could study every adult earning below that threshold across the entire US, the population, or select 1,000 below that threshold from each state, a sample.

Generally, research is conducted on samples, as collecting data on an entire population is very expensive.

### Inferential vs Descriptive Statistics

Related to samples and populations is the concepts of descriptive vs inferential statistics. Descriptive statistics is what is done when a researcher chooses to describe aspects of a sample or characteristics of a population. For example, stating the average, median, or standard deviation of building heights in the US would be a component of descriptive analysis. If a researcher wants to make conclusions about a larger population from sample data, this is known as inferential statistics. A key goal of inferential statistics is to be able to generalize the results obtained from sample data with some level of confidence.
