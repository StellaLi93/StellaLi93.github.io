---
layout:     post
title:      Studying the Association between Electricity generation sources and Greenhouse Gas Emission
subtitle:   A Country Level Study
date:       2018-04-23
author:     Jianan Li
header-img: img/bg-greenhouse.png
catalog: true
tags:
    - R
    - Data Visualization
    - Tableau

## Abstract 
Renewable energy and sustainable development are the controversial issue around the whole world. The study is trying to explore the association between electricity generation sources and greenhouse gas emission.

The study compare electricity generation sources, such as coal, gas and oil, renewable energy, and nuclear energy and clarify which electricity generation source can effectively control the greenhouse gas emission, that is, which electricity production is best for the environment.

The study assume that the more electricity production from nuclear source and renewable energy, the less the greenhouse gas emission.

## Problem Statement
Is there a correlation between electricity generation sources indicators and greenhouse gas emission indicators?
Rationale: Less the electricity generated from fossil fuel source, less the greenhouse gas emission.

## Hypotheses 
There is a negative correlation between renewable energy consumption and total greenhouse gas emission. 
There is a negative correlation between electricity production from renewable sources and total greenhouse gas emission. 
There is a negative correlation between electricity production from renewable sources and CO2 emission. 

## Methodology
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/9.png)

## Variables
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/1.png)

## Data Description
Our data has 7 independent variables, 5 dependent variables and 4 control variables of 79 Countries from 1990 to 2012.
Data Size: 23 (1990 to 2012) × 16 (lines) × 134(rows) = 49312
Data Type: Numeric, Categorical
Data Scale: Nominal, Ratio
Year: 23 years
Period:1990-2012
Source: Worldbank

## The Association between Electricity generation sources and Greenhouse Gas Emission
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/2.png)
The trend line shows the correlation between energy consumption and total greenhouse gas emission.
Both total final energy consumption and renewable energy consumption have positive correlation with total greenhouse gas emission.
The best way to reduce total greenhouse gas emission is to reduce total energy consumption.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/3.png)
Figure 2 is a pie chart that represents electricity production structure of different income groups. 
Low income group have the largest percentage of hydroelectricity; however, other three groups mainly focus on using fossil fuel. High income group greatly develop and use nuclear energy.
Nuclear energy and other renewable energy should be paid more attention, developed, and vigorously promoted, which can substitute the usage of fossil fuel.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/4.png)
Figure 3 is a bar chart showing the percentage of CO2, Methane, Nitrous oxide and other gas emission in total emission for each income group.
Higher income group tend to have larger percentage of CO2 emission, while lower income group tend to have larger percentage of Methane, Nitrous oxide and other gas emission. 
There are differences in the structure of gas emission among four income groups. We assume these differences are impacted by the electricity generation structure. 

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/5.png)
Regression models display the correlation between electricity production from renewable sources and different greenhouse gas.
CO2 is more difficult to be reduced by increase electricity production from renewable sources and other greenhouse gas is easier to be reduced. 
Countries facing with other greenhouse gas emission problem could generate electricity more from renewable sources.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/6.png)
Figure 5 is a side-by-side bar chart that represents the trend of greenhouse gas emission, renewable energy consumption, and total energy consumption from 1990 to 2020 among different income groups. (Forecast is from 2013 to 2020.)
Based on the actual and estimate trend, we can see that high income group focus on developing and consuming renewable energy. Total greenhouse gas emission of high income is in declining trend.
All the countries should pay more attention on the development and usage of renewable energy.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/7.png)
Correlation matrix displays the correlation between each two variables.
Electricity production from hydroelectric is negatively correlated with total greenhouse gas emission and renewable energy consumption is positively correlated with total greenhouse gas emission.
Electricity production sources have no linear relationship with total greenhouse gas emission.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Greenhouse/8.png)
Cluster countries by using electricity generation sources data as input. The result shows 2-cluster perform better.
Electricity production from fossil fuel and hydroelectric sources are more important factors when applying clustering model.
The biggest difference among countries is electricity production from fossil fuel. Their renewable sources electricity production level are quite similar.

## Conclusion
The total energy consumption has positive correlation with greenhouse gas emissions.
Higher percentage of renewable energy consumed to generate electricity, lower percentage of Methane and Nitrous Oxide gas emissions. 
In lower income countries, improving percentage of renewable energy obviously helps reduce greenhouse gas emissions. 
However, in higher income countries, adjusting structure has limited effect on reducing emissions.
