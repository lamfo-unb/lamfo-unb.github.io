---
layout: post
title: Event Study
lang: en
header-img: lamfo-unb.github.io/img/teste_eventos/wooden-table-texture_4460x4460.jpg
date: 2017-08-15 10:00:00
tags: [econometrics, finance]
author: Ana Julia Akaishi Padula
comments: true
---
# Event Study

Hello everyone! This week LAMFO brings to you the methodology known as Event Study, we will start with a breaf introduction, referencial and then an exemple using R.

Nowadays isn't hard to hear or read news about events that change contries or the world. We receive updates, new information and opinions about different topics and we reflect it on our behavior and adjust our plans, but about the market? Do you think that it reacts about those event?

To answer this question was created the Event Study.

## Methodology

The author Campbell, Lo and Mackinley (1997) were the firsts to define and write about this methodoly. The basic idea is to observe if the value of a company suffers a variation when a event appears, in other words, if the expected "normal return" if affected by an "annomaly". The development of Event Study has it fundamentals on the Theory of Efficient Markets of Fama (1970), which believes that the markets absorbes the public information and reflects it in stocks prices.

Here are four simple steps to start an Event Study!

### Step 1: Difine the Event

*But what kind of event are we talking about?*

We can study any kind of event, we only need to have enough data and the event must be known by the public. As exemples of events we havey:
1. Dividends distribution;
2. Fusion or aquisition of other company;
3. Reaquisition of stocks;
4. New laws or regulations;
5. Privatization or nationalization;
6. Political scandals.

One the event has been chosen, a time line will be defined with the windows for analysis.

![](https://i.imgur.com/QJzEnPH.png)

**Estimation Window:** this window stabilish the normal return expected from the company, from $$T_0$$ to $$T_1$$. Here we will have a standart to compare the event, since this data hasn't been "contaminated" by the news of the event.

**Event Window:** here is the time where the news about the event get in. The zero moment ($$0$$) will be the exact date that the event came public, after that we stabilish a safaty window to be get the information leak before, from $$T_1$$ to $$0$$, and to be sure that the market has absorbed all the information, from $$2$$ to $$T_2$$

An important detail, there is no fixed number to create the safety window. We can choose three months, fifthteen days or one year before the event, one way to identify the best safety window is testing different periods and analyse the statistic error.

**Pos Event Window**: moment after the event and market position.

### Step 2: Companies

Once the dates are difined, now we need to select the companies that will be analysed and why. Usually, the first option is to get companies with stock exchange, since the returns are easy to get.

The second filter may be the sector or area that the companies are. For exemple, if we were studying the impact of new environmental law, would be interesting select companies from oil or mining industry.

Now we need to colect the return data from those companies. Is important that the data time format be the same, if the event will be marked by a day so the data of the returns must be daily returns.

### Step 3: Stabilish Normal Returns and Abnormal

Following the time line, we first calculate the normal return of the stocks so then we can analyse the anormality.

A data base with the returns is enough to start the building the normal returns base. Remeber that the time format must be the same and they start at $$T_0$$ and be over at $$T_2$$.

To verify the existence of abnormal returns, or not, we will use the equation:

$$Ra_{i,t}=R_{i,t} - \mathbb E[R_i|X_t ]$$ 

Where $$Ra_{i,t}$$ is the abnormal return, since it is the difference between the actual return in t($$R_{i,t}$$) and the normal return ($$E[R_i|X_t ]$$).

Once we already had the base with companies return from $$T_0$$ to $$T_2$$, we must choose the best model to stablish the standart return:
1. Constant Mean Return Model
2. Adjusted Market Return
3. Market Model
4. Economic Model

:::	info
Constant Mean Return Model
:::
The expected normal return is defined by a simple mean of the real return in the estimation window ($$T_0$$ a $$T_1$$).

$$Ra_{i,t}=R_{i,t} - \overline R_i$$ 

One critic to this model is that the assumption that the returns will be constants with the pass of time. In some time formats, where the volatility is high, this characteristic alrady has a problem.

:::info
Adjusted Market Return
:::
This case, the normal return will be difined by the return of the market portfolio ($$Rm$$). This method will require a new data base, in the same time format, from a benchmark.

$$Ra_{i,t}=R_{i,t} -  Rm_i$$

:::info
Market Model
:::
The Market Model has it base on statistic, representing an upgrade from the previous model.

Different from the Adjusted Market Return, the Market Model don't set as default a market portfolio, but it uses an index such as S&P500 for north-america analysis or IBOVESPA for Brazilian stocks, been defined by:

$$Ra_{i,t}=R_{i,t} - \widehat \alpha_i - \widehat \beta_iR_{m,t}$$ 

The abnormal return ($$Ra_{i,t}$$) is defined by the difference from the real return of the asset in ***t*** and the parameters alpha ($$\alpha$$) and beta ($$\beta$$), estimated by a linear regression in the estimation window.

Using the linear regression in this model represent a big step in forecasting, since it depends on $$R^2$$. The bigger the $$R^2$$, less variance the abnormal return will have and more accurate the model will be.

:::info
Economic Models
:::

Based on the market actions and since our objective is to etimate the stock future return, there are two methodologies available: (1) Capital Asset Pricing Model (CAPM) and (2)Arbitrage Princing Theory (APT). Both of them may be used to the calculus of normal returns, but with their requisits.

1. *Capital Asset Princing Model* (CAPM)

$$Ra_{i,t} = R_i - Rf_i - \beta (Rm_i - Rf_i)$$

The abnormal return is the result of the difference between the real return and the estipulated return, considering the sensibility of the asset and market retun ($$Rm$$) and free risk assets returns ($$Rf$$).

2. *Arbitrage Pricing Theory* (APT)

$$Ra_{i,t} = R_i - R_f + \beta_1 R_1 + \beta_2 R_2 + \beta_n R_n$$

In this model, we have different risk prizes for different factos, each one with a specific sensibility, this mean that each factor has a beta. To aquire the betas ($$\beta_n$$) will be needed to do a linear regression for factor and asset.

### Step 4: Measuring and Analyse the Abnormal Returns

When the model is set, we are ready to calculate the returns. Usually, we analyse the mean of abnormal return and the accumulative abnormal return.

To validate the results, look the P-VALOR of the model and the $$R^2$$. Both of them must be statistic relevant.

### Applying the model

This exercise will be done using R, the following packages are recommended:
* quantmod : colect financial data from companies.
* eventstudies : automatic package to this methodology
* zoo : data base treatment

In order to demonstrate the usual practice, we will use the Market Model to estimate the abnormal returns for an economic event.

:::info
Housing Bubble in USA (2008) effects on Brazilian Companies
:::
