---
layout: post
title: "Reinforcement Learning in kdb+/q: Q-learning"
date: 2017-01-14 12:00:00
---

[Reinforcement Learning](https://en.wikipedia.org/wiki/Reinforcement_learning)
is an area of machine learning that studies how autonomous agents learn to
behave by taking actions and receiving rewards.

We present an approach for training an agent to behave optimally in a trading
environment. We model such an environment with some simplifying assumptions:

 * There is only one opportunity to trade per day which must be done by paying
 the `close` price provided in historical market data.
 * An agent either has a long or short position in one security. The exact
 number of shares is made irrelevant by using cumulative returns.
 * No transaction costs are taken into account.

Using a simple set of technical indicators derived from market data an agent
is trained using the [Q-learning](https://en.wikipedia.org/wiki/Q-learning)
algorithm. Once trained, the agent's knowledge is tested by performing
out-of-sample trading. Market data from 500 stocks are used, spanning 10+ years.

### Performance

Initial results suggest lack luster average returns, but seemingly better than
random trading, which produces similar average return with higher standard
deviation. A return of 1.0 means the agent has as much money at the end of
trading as it did at the start.

[<img src="https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/return.png">](https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/return.png)

[<img src="https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/returnrand.png">](https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/returnrand.png)

### alpha & gamma

In the Q-learning algorithm alpha and gamma control the learning and discount
rates, respectively. Our results show a marginal improvement in returns when
training with higher alpha and gamma values.

[<img src="https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/alpha.png">](https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/alpha.png)
[<img src="https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/gamma.png">](https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/gamma.png)

### Trading SPY

Here we present the best learned trading strategies from a batch of 100 trainings.

[<img src="https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/topSPY.png">](https://raw.githubusercontent.com/jlas/ml.q/master/rl/experiment/trade/results/topSPY.png)

Link to code: [github.com/jlas/ml.q/tree/master/rl/experiment/trade](https://github.com/jlas/ml.q/tree/master/rl/experiment/trade)