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



## 2018-04-15

### AI Gym

Over the weekend I looked into OpenAI Gym as a mean to perform reinforcement learning of our trader.

It's actually a very simple API: 
1. Send state to AI
2. Receive action from AI
3. Compute next state and reward (Profit!)
4. Repeat

While I still haven't coded the stock trading environment, it is straightforward enough.

On the other hand, it is completely open on the learning side of the things, and I really haven't scrapped the surface on how to do it (with tensorflow or whatever)




## 2018-04-16

So I got the book -- Somehow, it was cheaper to buy from Amazon US than in Brazil, but the (cheapest) shipping took a while.

I'll skip Part I for now and I'm reading about FNN.

## 2018-04-18

Found this project: https://github.com/deependersingla/deep_trader

Seems interesting, we'll take a deeper look into it later.

Also, since we shifted our project from estimating stock prices to creating a stock trader, I sent this explanation to the professor:

> We will keep using the Kaggle dataset with daily summaries of stock value.
> 
> We are looking at a more long-term trader (Particularly since the dataset only has 1 day resolution).
> 
> The idea is to train a network such as:
>
> As input:
> - Historical values of the stock of several companies
> - Amount of stocks owed of each company
> - Amount of money available.
>
> As output:
> - it produces a set of buy and sell orders valid for the next day.
> - Each order in the form {Buy|Sell} {amount} from {company} for ${cost}
>   (Possibly only one Buy and one Sell order will be created per day per company)
> For evaluation, we will simulate trading using historical data
> Knowing the actual price range for the next day, we can tell which orders succeeded and compute the next state (amount of money + amount of stocks owned of each company)
> We evaluate the performance at a given day using the change on the account's Market value at the end of the next day (Market Value = amount of money + Σamount of stocks * price of stock).
> Likewise, we will evaluate the performance over a longer amount of time by running it several times consecutively and comparing the initial and final market values.

## 2018-04-21

I have been doing some reading on reinforcemente learning, and apparently it works best on problems with discrete outputs (left or right, etc). Bummer.

But it seems that it also works well if you can provide the derivatives for the goal function (In which case it isn't really different from a standard FNN), so I guess this is the path we will have to take. While doing that within a day seems straightforward, doing it on a longer time stretch may be a challenge.


## 2018-04-27

Time is running up!

I have to give this project some love over the long weekend.

This is a rough TODO list:
- Create an OpenAI Gym environment for trading.
- Create a dumbed down trading algorithm that buys for `mean(low)` and sells for `mean(high)` and see how it performs
- Use tensorflow as a regression to estimate Low/High/Open/Close prices on the next trading day.
- Improve trading algorithm that buys for `estimate(low)` and sells for `estimate(high)`
- Estimate derivatives of the trading algorithm and try to use reinforcement learning.




## 2018-04-29

Time continues to run up 😭

I spent the last couple of days reading on [Hands-on Machine Learning with Scikit-Learn and TensorFlow](https://github.com/ageron/handson-ml). It's a really good book!

It gave several new ideas and insights to evolve my trading network:
- I considered trying to perform 1D convolutions on the historic data to see if it can extract any useful features from it, but meh, it doesn't fit too well on the model of a continuous process.
- I now mostly understand how reinforcement learning works on non-discrete problems, but it seems like too complicated and slow to train
- Recurrent neural networks are the way to go -- The internal state on a LSTM / GRU cells does roughly the kind of feature extraction I expected from a convolutional layer, and the usual gradient descent method can be applied almost directly during training.

Despite changing my mind from reinforcement to recurrent networks, the architecture is basically the same: The network is split in 2 parts, the environment (static, behaves according to the historic data and rules of the stock market) and the trader, which is optimized to maximize the account value at the end of the time series:

For each time `t`, the environment sends to the network inputs: (for each entry in the minibatch)
- The high/low/open/close values of the last day for several stock symbols (Historical data should be embedded in the network state)
- Amount of stocks owned for each of the symbols
- Amount of money available.

The network outputs: (for each entry in the minibatch)
- A set of buy or sell orders to be executed (Amount of stocks and threshold prices), for each company
or 
Based on the historical data, the environment checks which orders were executed and prepares the state of the next iteration.


A few implementation considerations:
- Ideally, all this should happen inside a magic tensorflow node that knows how to deal with recurrent neural networks. Unfortunately, I'm not familiar enough with the implementation details to make an environment that works along with it. Instead, I'm probably going to use a network statically unrolled on time.
- The optimizer is set to maximized the account value at the last iteration (Some normalization might be applied)
- On real stock markets, orders are discrete:
    - You either hit a buy/sell threashold or you don't
    - You may only own an integer amount of stocks
  This might be rough for the optimizer. Instead, I'll try trainning on an environment where the amount of stocks can be non-integer, and there is a transition curve on order price thresholds. Still, I'm not sure and that will have to be tested.


## 2018-04-30

### Some progress, at last!

I managed to build a tensorflow environment that simulates trading over a time window and evaluates the mean profitability per day.

Unfortunately, I didn't really train a good trader yet -- It's just a basic heuristic that doesn't seem to perform very well.

Nevertheless, that's a significant progress!

More details on [the commit](https://github.com/paulo-raca/mo810-stock-predict/commit/5f162d1ef9e98166f44200ca54173476fc131584)



## 2018-05-01

Ok, I have now replace the dumb heuristics by a simple linear function of the latest days's data.
As soon as it starts trainning, my performance increases form -0.18%/day to zero -- And stays stuck forever :/

Well, at least it's not negative. Interestingly, if I try to minimize the profit instead, I do a few positive results -- As high as 0.28%/day, but that's probably just a glitch
