---
title: "Assignments"
date: 2021-01-17T20:27:23-05:00
draft: false
---

## Final Assignment
For the final project you are required to collate all of your semesters research into a single, visually compelling narrative. The keyword here is narrative – you should be telling a story with data! Projects can be in the format of a research paper or presentation, but either way they must be heavily illustrative. Including diagrams to convey process, logic, and ideas, alongside charts and maps to reveal results of your analysis. Think about ways to make your line of questioning compelling to a potential reader. Why is this relevant? What surprised? Also, try to focus your project around a single idea.

These are the minimum requirements, feel free to add/rename sections:

- Compelling Title
- Abstract / Overview — *what is the project about? What are you trying to uncover?*
- Methodology — *what data sources are you using? How are you approaching the analysis? Are you using any statistical techniques (regression, clustering, similarity metrics)?*
- Results — *what did you discover? What is interesting about the results? What did you learn?*
- Interactive maps and charts
- Code (pushed to GitHub repo)

You can submit your work as a PDF presentation, but it is encouraged you submit your work as a Reveal.js presentation so any interactive content (maps or charts) can exist on the web. Images, charts, and maps should be submitted separately for archiving purposes and the GSAPP Abstract.

---

Resources:

- [Envisioning Information](http://okhaos.com/tufte.pdf), Edward Tufte
- [NYT The True Color of US Elections](https://www.nytimes.com/interactive/2020/09/02/upshot/america-political-spectrum.html)
- [Forensic Architecture](https://forensic-architecture.org/investigation/nso-groups-breach-of-private-data-with-fleming-a-covid-19-contact-tracing-software), Analysis of the misuse of COVID-19 contact tracing software
- [NYT Single Family Zoning](https://www.nytimes.com/interactive/2019/06/18/upshot/cities-across-america-question-single-family-zoning.html)
- [Making data mean more through storytelling](https://www.youtube.com/watch?v=6xsvGYIxJok), TED talk

---
---
---
---
-----

## Week-5:
### Map data | Refine your research topic
This week's assignment will be two fold – first, you will be tasked with creating a choropleth map of a single variable within your topic area. You can choose to create a static map using GeoPandas or an interactive map using Folium.

Questions to consider when creating your map:

- What is the distribution of the data (minimum / maximum values)? And will this look interesting in map form? For example, if the variable is skewed towards a particular value it may look weird as a choropleth.
- Does the color palette correctly highlight interesting values in the data?
- What is the area you want to cover?

The second aspect of this week's assignment will be to refine your research question and start thinking about a narrative. You should start thinking like a a capital R researcher and structure your project/presentation like a research paper, including:

- Abstract / Overview: Describe your entire project in one paragraph
- Background  / Context: what is the context behind the research? Why is this interesting?
- Method: What techniques will you use to complete the study?
- Results / Findings: What did you discover?

Of course at this stage you will have nothing to say about results and findings, or even a method. But it will be good practice to start structuring your project and presentation in this format.

---

Resources:

- [Mapping data](https://www.placedata.net/posts/mapping/) with Python

---
---
---
---
-----

## Week-4:
### Data analysis with Pandas
This week's assignment expands on the use of Pandas, requiring you to do a descriptive analysis of data within your chosen research area. By next week's class, you are to find an interesting dataset(s) and ask a series of probing questions to surface basic insights. The output of the analysis is not important at this stage, what is important is getting comfortable with data analysis + Pandas + Python. However, you should summarize your findings in a Reveal.js presentation.

Questions to ask yourself when choosing your dataset:

- How does this relate to my topic?
- Is this data trustworthy? Is it meaningful?
- Is it at the right granularity?
- Is the dataset structured in a useful format?

With your chosen dataset, you are to pick two or more variables and depending on the *data type* of the variables answer the following questions:

- If the data type is numeric: surface the median, minimum, maximum, and standard deviation of the variable.
- If the data type is categorical: count the frequency of the various values and surface the mode.
- Pick two numeric variables and surface the relationship (if any) between them by analysing the correlation.


Once you have gathered the above statistics – present the information in Reveal.js


For those looking to advance quickly:
- Visualise the above statistics using simple charts in Pandas.

---

Resources:
-

---
---
---
---
-----


## Week-3:
### Data manipulation with Pandas

#### Purpose
The goal of this assignment is to get you coding in Python and familiar with the Pandas library. You are tasked with locating a dataset on your chosen research topic and load the data in Python using Pandas. Once the data is loaded into Python you must describe the data using basic statistics such as averages, medians, summing, etc. For example, if your topic is NYC housing, then you could download the building data from the NYC Urban Planning department (PLUTO), load it into Pandas and find out the average building height in NYC or the total square footage in Manhattan.

There are many interesting datasets online and I encourage you to search the internet to locate something interesting. Most research is only as good as the available data.

---

Resources:
- You may find the datasets linked in the [resource](https://www.placedata.net/resources/) section of this site useful.
- Refer to the intro [tutorials](https://www.placedata.net/posts/) or [notebooks](https://github.com/carlobailey/location-intelligence/tree/main/_notebooks) on Python, Pandas, and Data.

---
---
---
---
-----


## Week-2:
### Define a research agenda and create a Reveal.js presentation


#### Purpose
The purpose of this assignment is to get you thinking about a research agenda for your final project. One of the hardest things about research and data science is finding the right questions to ask with data. You can have all the data in the world, with unlimited time and resources, but if you do not know how to ask interesting questions, you will not be able to produce novel insights, actionable findings or a new body of knowledge. In this assignment you will be tasked with coming up with a research question(s) that you want to answer over the next few weeks/months, gather existing research on the topic, and present it with Reveal.js.

[Reveal.js](https://revealjs.com/) is an open-source presentation framework that enables you to create interactive, highly customizable presentations on the web. Alongside doing visualization and analysis in Python, you will be consolidating your efforts this semester in Reveal.js. This assignment aims to get you familiar with the framework.

#### Task
Define a broad topic or area of interest related to the built environment or location. For example property ownership, zoning, energy usage, retailers, tax credits, etc. Why are you interested in this topic? Narrow it down further to an area you wish to explore. What are the ways you think this topic affects the built environment and location? Articulate your proposed research question, and find 2-3 examples of academic or industry research exploring a similar issue.


#### Example

*Broad Topic*
- Real Estate Financing

*Narrow Area*
- Public Infrastructure and Land Value

*Research Question*
- How does the development of public infrastructure projects affect house prices?

*Similar research*

- [A Smart Commons](https://provocations.darkmatterlabs.org/a-smart-commons-528f4e53cec2)

---

Resources:
- [Google Scholar](https://scholar.google.com/) research portal to help you find similar research
- [arXiv](https://arxiv.org/) *open* research portal to help you find similar research
- [How to formulate a research question](https://ocw.mit.edu/courses/comparative-media-studies-writing/21w-035-science-writing-and-new-media-communicating-science-to-the-public-fall-2016/communication-experiments/sessions-9-21/formulating-a-research-question/)

---
---
---
---
-----



## Week-1

This week is a mini-assignment to get set up with all the necessary tech that we'll use throughout the semester

- Install Python with [Anaconda](https://www.anaconda.com/products/individual) if using windows
- Install Python with [Homebrew](https://realpython.com/installing-python/#how-to-install-from-homebrew) if using a Mac
- Create a [Github](https://github.com/) account
- Install the [Sublime](https://www.sublimetext.com/3) text editor
