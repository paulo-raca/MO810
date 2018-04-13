## 2018-03-06
I signed up for this subject as a special students, months ago, but didn't hear back until the last friday.
Yay, the email said I was enrolled -- One week after the classes started 🤦.

The professor says something about keeping a diary-- No idea what he has talking about, I'll look it up later.

## 2018-03-07
Looking up this diary thing. It seems like, for now, I just have to throw away random ideas to use machine learning on.
Pong and Dinossaur catch my attention, but meh.. Sounds too easy. What about snake?

I've also read the intro on the book and a little bit of review of Linear Algebra.

## 2018-03-08

Hey, have you seen much netflix stock has valued in the last few years? I wish I had all my money in it. Stock marketing preditions sounds like a nice project 🤑

Yes, yes, there is bitcoin too, but netflix is getting lots of real money from real people, while cryptographic money is still only a speculative novelty for now (Yet, I wish I had started mining bitcoins years ago...)


## 2018-03-09
I really enjoyed the pratical class exercises where we trained a little net using derivatives
and an excel spreadsheet. I had a few hours free today and decided to create a [Jupyter Notebook](AnalyticNeuralNetwork.ipynb) from it.

It uses [SymPy](http://www.sympy.org) for all the derivatives and algebraic stuff, which is pretty fun.

## 2018-03-13

The professor has shown my notebook on the class, I'm famous :)

Anyway, We've seen a few other activation functions on the class today, I should update the notebook when I have the time.

## 2018-03-15

The professor says we should have a group by now.. Fuck... Anyway, someone called Ricardo Queiroz seems to be interested on the stock predition problem too.

## 2018-03-16

On the stock predition project, we'll probably  have to split it somehow between me and the other guy.

One idea came to mind: Predicting stock values is only half of the problem, deciding the best moments to buy and sell is a whole other challenge, even assuming you have high-quality data to base on:

- You have a finite amount of money to invest, and you can only buy an integer amount of shares from each company.
- Short selling. I hate the idea of selling what you don't own, but should we consider it?
- Settling time -- After selling stock, it takes 3 days to have the money in your account, so that must be accounted for.
- You should invest on many probably-profitable companies, so that you can be profitable despite random fluctuations (Scandals, leaks, etc)

## 2018-03-17

### Dataset

I briefly looked into datasets, and it seems like, despite being public information, it is impossible to get full stock history for free -- That's a huge volume of data, and seems like professional traders are the only ones willing to spend big bucks to buy it.

On the other hand, daily summaries are easy for get: Min/Max/Open/Close prices and total trade volume.

AFAICT, [Quandl](https://www.quandl.com/) seems to be my favorite source for updated info.

## 2018-03-18

### Data Normalization.

I cannot just dump CSV files into a neural network and expect it to work nicely (Or can I?). The data has to be massaged first.

It doesn't matter if a share is worth \$10 or \$1000, what matter is it's percent growth. Logarithms come to mind...

I have 2 initial ideas for representing data:
- The values for each day could be represented as percentage variations from the previous day's closing value
- The values could be represented as $log(X_i / \bar{X})$ instead

### Missing days

- How to fill the gaps of the non-trading days (weekends and hollidays)? Use aftermarket values if available? Maybe just use interpolation to fill?


## 2018-03-20

### Probabilies

I cannot predict the stock price without taking into consideration the prediction error.

Can you calculate the standard error of each parameter on a neural network (Like a would on a linear regression), and use them to calculate the standard error of the result?

Better yet, can I use a neural network to estimate a non-normal distribution? Assume I can change the cost function to be zero when the $Ŷ$ is larger than a given percentile of the $Y$, I could train the network to calculate all the percentiles of a distribution.

So I can setup buy/sell trades to happen when the prices reaches p10/p90 of the expected price range for a given day, or something like that.


## 2018-04-01

### Putting stuff together

Today I wrote down and organized all my ideas on a new [github project](https://github.com/paulo-raca/mo810-stock-predict), and sent them over to Ricardo for discussion.


## 2018-04-03

After a couple of days discussing, the final project description was sent to the professor by email.


## 2018-04-08

Professor replied to our project description with minor comments / suggestions.


## 2018-04-10

We reviewed all projects during the class today, and had my attention drawn to the group using reinforcement learning to train a computer to play Pac Man.

I'm now considering that instead of trying to model stock values as a regression of the value in the next day from the time series, we should try to train a trader: Given the time series and current assets, the trader generates buy/sell orders, and which are executed or not based on the market values for the following days.

Some extra details have been added on the [project's github](https://github.com/paulo-raca/mo810-stock-predict/blob/master/README.md#semi-automatic-trading)


## 2018-04-12

### Pre-Processing

After realizing that other groups were starting to get results and stuff, while we had barely looked at the dataset, today I started pre-processing the data:

Basically, I'm normalizing the Open/Close/High/Low values to be a milliBells variation from the last day's closing price. (Yes, milliBells -- It's weird, but I wanted a logarithm scale, and the daily variations are just so tiny)

Afterwards, I'm lining up X days in a row to create a feature vector that can be consumed by a neural network (The X-1 days are the input, and the last entry is the expected output)

I still have no idea how to handle Volume, since it varies a lot between companies.
It could probably be normalized by the total number of shares in the company?
Anyway, we'll ignore it for now.
