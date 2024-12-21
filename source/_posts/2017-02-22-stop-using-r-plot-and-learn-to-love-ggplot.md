---
title: Stop using R plot and learn to love ggplot
tags:
  - data science
  - ggplot
  - jupyter
  - pandas
  - python
  - r
  - rpy2
  - statistics
id: '209'
categories:
  - - uncategorized
alias: /2017/02/21/stop-using-r-plot-and-learn-to-love-ggplot/
date: 2017-02-22 07:17:38
---

When I first got started I always found myself using R's "plot" capability because, well, it is easy! Unfortunately, it lacks some advanced features -- and the plots it produces are really ugly looking (subjective, but I bet you will agree with me). Luckily, there is a better tool for the job - ggplot. With only a few tricks you will find it just as easy to use.
<!-- more -->
In my previous post [Comparing diamonds with linear regressions using python R in jupyter notebooks](http://www.benchodroff.com/2017/02/18/comparing-diamonds-with-linear-regressions-using-python-r-in-jupyter-notebooks/) you will see exactly how I produced this R plot.

[![](http://www.benchodroff.com/wp-content/uploads/2017/02/prediction.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/prediction.png)

With only a few lines of code changes, we can import the same data into a pandas DataFrame (it is basically a spreadsheet representation of your data) and create a colorful ggplot.

\[caption id="attachment\_210" align="aligncenter" width="640"\][![ggplot Diamond Comparison](http://www.benchodroff.com/wp-content/uploads/2017/02/ggplotdiamond-1024x1024.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/ggplotdiamond.png) ggplot Diamond Comparison\[/caption\]

I was really impressed with how easy it is to "factor" my diamond color data as a new attribute to modify the colors of my data points. This simple feature revealed a whole new world of information about my price comparison. We can clearly see from this picture that the colors are not sporadically spread throughout. In fact, "D" color diamonds are misbehaving quite a bit from the linear regression model. Said simply, "if you want to buy a diamond with a perfect heirloom quality "D" color (the best), you are likely going to pay a significant premium well outside of the norm." This makes common sense, but ggplot makes it very easy to present this information.

Enough talk, here is the code.