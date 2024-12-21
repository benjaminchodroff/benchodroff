---
title: Algorithmic trading using 100 lines of python code, using OANDA v20 API
tags:
  - api
  - cryptocurrency
  - forex
  - jupyter
  - oanda
  - python
id: '273'
categories:
  - - uncategorized
alias: >-
  /2017/03/07/algorithmic-trading-using-100-lines-python-code-using-oanda-v20-api/
date: 2017-03-07 11:23:11
---

After reading Dr. Yves Hilpisch's article, ["Algorithmic trading using 100 lines of python code,"](https://www.oreilly.com/learning/algorithmic-trading-in-less-than-100-lines-of-python-code) I was inspired to give it a shot. I wanted to apply his guide on how to use a time series momentum algorithm because I have been interested in forex trading with cryptocurrencies. I set up a free forex trial account on [OANDA](https://www.oanda.com/), jumped into a jupyter notebook, and got to work. I hit an issue. OANDA changed their API from "v1" to "v20" and all new accounts default to the new API. I ended up rewriting his sample code to work with the new OANDA v20 API using a third party python library.
<!-- more -->
If you are still wanting to use the original v1 API sample code, you could reach out to OANDA chat support and manually request a v1 legacy demo account. It takes 2 business days. Ain't nobody got time for that! Plus, you can't hedge in the v1 API.

\[caption id="attachment\_276" align="aligncenter" width="640"\][![OANDA v20 - Do you even hedge, bro?](https://benchodroff.com/wp-content/uploads/2017/03/oanda-1024x642.png)](https://benchodroff.com/wp-content/uploads/2017/03/oanda.png) OANDA v20 - Do you even hedge, bro?\[/caption\]

Here is my github repo with both the v1 and v20 sample code in jupyter notebooks, for comparison:

[https://github.com/benjaminchodroff/oandamomentum](https://github.com/benjaminchodroff/oandamomentum)

Here is the gist of my end result using the OANDA v20 API
<script src="https://gist.github.com/benjaminchodroff/2243502e71bb06ae55d82e76ce1dc21f.js"></script>
Thanks again, Dr. Hilpisch!
