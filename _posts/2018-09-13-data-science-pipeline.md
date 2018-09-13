---
category: tech
tags: Data Science
title: Getting started with the Data Science pipeline
meta:  A beginners framework/introduction to the Data Science pipeline
author: Harry Odell
comments: true
---

The buzz around data and advanced analytics has marred the true value of an effective data science process in addressing real world problems. Getting some data and throwing some science at it does not equate to a successful business (or other) solution. In this post, we will look to identify some key principles and processes within the data science pipeline that will help you to maximise the value gained from future data science and analytics ventures.

![Image](/assets/buzz-worrds.png)

### Framing a data science problem

The pipeline naturally begins here, and it should not be overlooked – 30% of Data Scientists report the lack of a [clear question](https://www.kaggle.com/surveys/2017) to have been a barrier at work. Accurately defining a data science problem is more difficult than it seems. Often stakeholders are not sure what they want, often the scope of a project is not well defined, and often a data scientist might be expected to solve all data problems (not all data problems are data science problems). These issues are best managed with an open discussion on expectations between technical and non-technical stakeholders. Ultimately, be specific and realistic with the problem you want to solve and define a question with a measurable outcome. 


### Collecting data

With 49% of Data Scientists reporting [dirty data](https://www.kaggle.com/surveys/2017) to be a common problem at work, it is essential to get this step right. Good practice here begins with identifying all the available datasets that could be *useful in the context of the question you are trying to answer*. More is more is a rhetoric that often leaves Data Scientists overwhelmed and wasting time – make sure to collect relevant and valuable data. Then, the process of extracting the data begins. Look ahead to identify formats that will be most appropriate to the analysis and modelling that will occur later in the pipeline.

Generating your own data, where appropriate, comes highly recommended. It may seem daunting at first but start with focusing primarily on your most important issues, bearing in mind how the data you capture is necessary to answer your research questions. Think creatively about ways in which you can generate data specifically for your needs and avoid the common pitfall of capturing highly correlated predictors (multicollinearity) – this may increase the explanatory power of your future model but often at the expense of the models stability.


### Pre-processing or exploratory data analysis, which first?

There is conflict in industry as to which of the next two steps comes first. Without exploring the data, we might lack some validity in the decisions we make whilst cleaning the data, but without cleaning the data our exploratory data analysis may be difficult to perform. Chicken or the egg? Personally, I lean towards making pre-processing a priority with the injection of some exploratory analysis methods to decrease the chances of overlooking insights useful for data cleaning. 

### Pre-processing 

A Data Scientists biggest pain and the most time-consuming step in the process. Good pre-processing practice involves a combination of scientific methodologies, subject familiarity and intuition. It is essential to be thorough at this stage to avoid the classic "garbage in, garbage out" conundrum. Here are a few pointers to help you get started with the process…
- Start simple. Check the head and tail of the data and ask yourself does the data make sense in the context of your subject? Do the column values match the given predictor variable? Are the units of measurement appropriate? Are the data types fit for purpose? 
- Take summary statistics where possible. Do the values match our expectations?
- Check for null/missing values. Make assumptions as to why these values are missing, look for patterns of missing values and then assess the impact of removing incomplete observations. Given substantial data, look to impute missing values to prevent loss of data. 
- Check for duplicate observations and remove where appropriate. 
- Identify and remove anomalies *if you believe the level of variance from the mean to be suspicious*. Removing large or small values for being larger or smaller than other observations is poor practice – it may boost your model’s performance but at the cost of important information. 
- Dummy code categorical variables where appropriate.
- Consider removing predictors that are clearly not relevant to understanding your research question.
- Depending on your future model type, consider standardising your data to increase model stability, reduce impact of multicollinearity and balance the weighting of variables.


### Exploratory data analysis

The aim here is to get a feel for the dataset, understanding the key themes and patterns within the data. With EDA, start by picking the low hanging fruit. You want to generate insight quickly and broadly at first, before moving on to insights more specific to your research question. Plot the distributions of numerical features and observe outputs. Are they what we expect? What might these results mean in the context of our research? Has this new understanding effected our research hypothesis? Next up, categorical features. Simple bar plots are sufficient to identify those classes of importance, and sparse classes that might need some later attention. Finally, you want to assess the correlations between your numerical features. Plotting a heatmap will identify features strongly correlated with the target variable (this can be positive or negative). Again, given some intuition and subject expertise, is this what we expect? Are there correlations that we find particularly interesting? 

### Analysis and model building

We now have clean data, an understanding of the data and a refined research question upon which our model is to be framed. So where do we begin with our analysis? The answer is project dependent. It requires domain knowledge and analytical expertise to identify the methods, models and algorithms that are most appropriate to our question. Things to consider:
-	Split your data into a training and test set (an 80/20 split is advised). The principal here is that when comparing model performance on the training vs test sets we can identify and avoid overfitting. 
-	Decide which metrics you will use to evaluate model performance. These metrics will inform us which models are performing the best and should be chosen to best assess how well the model answers the original research question. Classification metrics may include accuracy, log loss or area under ROC curve and regression metrics may include mean squared error, mean absolute error or r-squared. Again, choose the metric most appropriate for the question and then seek a model that maximises/minimises this function. 
-	Choosing a model, method or algorithm. This depends on the research question and is dictated by whether we are dealing with a classification or prediction problem. Experience and a review of scholarship may indicate methods that would work well for the problem however it is recommended that you test several methods to find the best fit. 
-	Consider cross validation. This is a process which enables us to tune our models and estimate how our results will generalise to an independent data set using only the training data. 
-	Fine tune your model. Here, we tune our hyperparameters and run the model, recording how well the model performs for each set of models and hyperparameter values. 
-	Consider feature engineering to boost performance. This is considered as one of the most important elements in applied machine learning and model building; so much so that Andrew NG (co-founder of Google Brain) famously said *"Applied machine learning is basically feature engineering"*. Feature engineering involves using domain knowledge (and intuition) to create new input features from existing features. The process empowers algorithms to focus and learn the essential elements of the data. Generally, better features equate to better performance.
-	Iterate. Run through the process several times, playing with features, models and parameters until you reach a level of performance you are satisfied with. 

![Image](/assets/DS.png)

### Communication and visualisation

This final stage will determine whether your insights are acted upon or not. At this point, only you (and maybe your team) know the importance of your results. All that's left to do is to make others aware of why what you have found matters – a task more difficult than it sounds. You want to tell a compelling story that is persuasive but true to the data. It is critical to communicate your findings in a concise and clear way such that non-technical audiences understand the importance of your results. Think creatively about how to craft a narrative of your data driven results and consider how your findings will drive business decisions. Visuals resonate well with all audiences but again, keep these simple and effective. 


Be advised, this is not an exhaustive description of the data science process and often you will enact steps in a nonlinear manner. It is however a framework from which you can build. Use it to find what *works best for you and your business*. 


