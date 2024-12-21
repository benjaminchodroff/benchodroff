---
title: Comparing diamonds with linear regressions using python R in jupyter notebooks
tags:
  - big data
  - data science
  - jupyter
  - linear regression
  - model
  - notebooks
  - python
  - r
  - statistics
id: '13'
categories:
  - - uncategorized
alias: >-
  /2017/02/18/comparing-diamonds-with-linear-regressions-using-python-r-in-jupyter-notebooks/
date: 2017-02-18 11:53:25
---

Buying a ring is a big decision. You have the whole "are they the one" decision. I can't help you with that. Then you have the reality that this could likely be the first major financial decision that will impact both of you. Wouldn't it be nice if you could save hundreds or even thousands of dollars?

I am not here to convince you to avoid buying a diamond (thanks, De Beers). Instead, I am going to show you a basic statistical programming technique with python and R known as a "linear regression model." I will use a jupyter notebook to execute data analysis so you can see step by step how it works.

You might be able to use this to shop smartly by allowing you to compare an actual cost in store to a predicted price. My wife and I built and used this code in 2013 while engagement ring shopping together. Hope it helps others!

Let's get started!
<!-- more -->
[![](http://www.benchodroff.com/wp-content/uploads/2017/02/prediction.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/prediction.png)

Evaluating a diamond, as any good jewelry sales rep will teach you, can be summarized by the four C's: cut, clarity, carats, and color. That's great, but do all four C's matter the same when predicting cost? Your jewelry rep probably doesn't want to tell you this, but as it turns out, "definitely not!"

Should you pay more dollars for a perfectly clear diamond that has worse color, as compared to another? Should you buy that diamond that is finally marked "50% Off" because, wow, that must be a great sale? How about buying from that guy in the alley who knows a friend... no, wait, seriously, don't ever do that.

While this is a subjective decision, using a price calculator can you make you a more informed objective consumer.  How do we find out which C's matter most when predicting cost, and by how much? To answer that we will need standardized data. Lots of it.

Luckily, the GIA has done us a huge favor by providing a standard report by which many diamonds are measured. Using this standard data, we can make a prediction model which evaluates which of these C's matter and by how much.

\[caption id="attachment\_81" align="aligncenter" width="881"\][![GIA Diamond Grading Report](http://www.benchodroff.com/wp-content/uploads/2017/02/Diamond-Grading-Report-881x690_1355957637795.jpg)](http://www.benchodroff.com/wp-content/uploads/2017/02/Diamond-Grading-Report-881x690_1355957637795.jpg) GIA Diamond Grading Report\[/caption\]

Now all we need is a lot of this data. The fine folks over pricescope.com, the premier diamond and jewelry community, have amassed a searchable database of 450,000 diamonds of all carat, color, clarity, and cut from retailers around the internet. There is only one problem - there is no public API to extract this information. We will need to "screen scrape" to get this data into a useable format.

If you do not know how to screen scrape data or want to see how I learned how to pull data from Pricescope.com, I would recommend reading my previous blog post Learning to screen scrape data using Google Chrome and curl

Now that you know how to screen scrape, here is the link to my jupyter notebook and code in GitHub if you would like to try executing it yourself:

[https://github.com/benjaminchodroff/rockbuster](https://github.com/benjaminchodroff/rockbuster)

Or, if you are just curious here is the output of the notebook!

Bonus round tips:

1.  Plan at least 1-2 financial quarters ahead before buying the diamond. Use end of month negotiating to help pressure an in store sales rep. They are likely compensated on a quota basis and you want them going to their manager to get approvals to do extra discounting.
2.  Negotiate on the center stone commodity, not the total ring setting package. This is especially true if you are trying to buy a larger center stone diamond. Remember: total weight of all the small diamonds means very little compared to that largest stone.
3.  Check the fine print. Some online retailers will let you buy a diamond with a free no-risk return policy, with free shipping back. Use these offers to pressure other in store retailers by showing the diamond from the online shop. By actually having that diamond in hand, it really demonstrates how easily the in store diamonds could be replaced
4.  Do not assume or even mention that the in store addon services will be valuable. They use these services to justify their higher prices. You can easily void these benefits by claiming, "I will be moving in 6 months." Bam. Suddenly all those free cleanings, refitings, and champagne are effectively worthless. It gives you an upper hand to get to the true lowest cost.
5.  Be friendly. Honestly, this should be #1. At the end of the day they are just sales reps trying to make a living. Nobody wants to give a jerk a discount - and trust me they won't. You can learn a ton from them and you should. Have them teach you everything they know about gemstones and settings. Become a smart buyer. Be a good human being.