---
title: Python cryptocurrency trading bot
tags:
  - cryptocurrency
  - ethereum
  - exchange
  - forex
  - jupyter
  - neo
  - python
  - trading
id: '337'
categories:
  - - uncategorized
alias: /2017/11/18/python-cryptocurrency-trading-bot/
date: 2017-11-18 11:31:15
---

As a fun toy to explore trading, I built a "flipper" cryptocurrency trading bot in python for the Bittrex exchange. It has a trading strategy of attempting to flip between two cryptocurrencies, such as Ethereum and NEO, in hopes to obtain a small position growth each time it flips. Of course, this is just a very basic "strategy" and you will certainly need to modify it to avoid getting stuck on a losing bet.
<!-- more -->
To give my flipper a bit more robust capabilities, it does attempt to auto-resume if the program crashes. It also has an amount of time to wait before canceling a "flip" and trying again. It is far from perfect... Currently, I am stuck on a trade for the past two months and see no chance it will ever complete the flip. Luckily, both currency pairs are up compared to the dollar - so I win either way. Have fun but be safe!

You can explore the latest code in my repository here:  
[https://github.com/benjaminchodroff/BittrexFlipper](https://github.com/benjaminchodroff/BittrexFlipper)

Big thanks to the community building and supporting the python-bittrex python api liberary! [https://github.com/ericsomdahl/python-bittrex](https://github.com/ericsomdahl/python-bittrex)

Happy trading!

<script src="https://gist.github.com/benjaminchodroff/ca037379060de0f3b5f77e643be5215d.js"></script>

