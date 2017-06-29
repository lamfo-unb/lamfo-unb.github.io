---
layout: post
title: Bootstrap
lang: en
header-img: img/bootstrap/juskteez-vu-1041.jpg
date: 2017-06-28 10:00:00
tags: [statistics,finance]
author: Mariana Rosa Montenegro
comments: true
---
<link href='https://fonts.googleapis.com/css?family=Rock Salt' rel='stylesheet'>
<link href='https://fonts.googleapis.com/css?family=Sofia' rel='stylesheet'>

# Bootstrap

In this post, we present a statistic technique called Bootstrap. Nothing better than an example of brigadeiro (a typical and popular desert in Brazil) for a preliminary taste on the subject.

<img src="/img/bootstrap/Avelã.jpg" width="700">

Imagine you own a candy store. A customer expects that, when buying a brigadeiro, it will have, on average, the same weight as the one of another customer. However, it is impossible to weigh all the brigadeiros that will be produced in your store!!

What should we do then??

You, the owner of the establishment, can use sampling techniques to randomly select 150 brigadeiros and calculate the average weight of these brigadeiros. Done this, you can conclude that the population mean is within an margin error relative to the sample mean.

Suppose you want to know, after a few months, what the average weight of the brigadeiro would be on a given production line date, taking into account greater accuracy.

As several variables have emerged, we can no longer use the sample of brigadeiros produced today as a baseline. Among these variables involved, we consider different types of sugar, cocoa and butter, new flavors, type of confectionery, the filling, among others.

Let us assume that the only information we have is the weight of a sample of 150 brigadeiros on the initial testing date. We can consider the initial error margin as our most relevant information.

Fortunately, to solve this problem there is

<h1>
<div style="font-family: 'Rock Salt';font-size: 60px;">
Bootstrap!! :tada:
</div>
</h1>

In this scenario, we create our Bootstrap sample, which arises from the random sampling of the 150 known weights. If weight replacement is allowed, this bootstrap sample will not necessarily be identical to the initial sample. Some data may be omitted and others may be repeated. In this scenarion, numerous bootstrap samples are quickly created.

## Technical View

Let's now introduce Bootstrap with more detail.

This statistical technique was initially proposed by Bradley Efron in 1979. It has been gaining strength, especially due to the advances in computer power.

<div style="font-family: 'Sofia';font-size: 30px;">What is the origin of the word Bootstrap??</div>

Bootstrap term has English origin and refers to the lace on the back of a boot which helps to put it on.

Its use comes from the phrase "To lift himself up by his bootstraps", that is, "Get up using your bootstrap". Here, there is an allusion to something impossible, difficult to achieve.

Imagine getting up in the air pulling only pieces of leather from your boot. This is impossible! And the idea that this model wants to bring is to be able to do the impossible.

The story of Baron Munchausen, also mentioned in the origin of the Bootstrap concept, tells that he was able to raise himself and his horse from a swamp by pulling his own hair (specifically, his ponytail). However, in this history there is no use of the word bootstrap as we know it and no other explicit reference to bootstraps has been found elsewhere in the various versions of Munchausen tales.

<div class="row">

<div class="col-md-6">
<img class="img-responsive thumbnail" src="/img/bootstrap/boot2.jpg" alt="mat_elis_1" style="width:0.000001"/>
<div class="caption">
</div>
</div>

<div class="col-md-6">
<img class="img-responsive thumbnail" src="/img/bootstrap/pic2.png" alt="mat_elis_2" style="width:0.000001"/>
<div class="caption">
</div>
</div>
</div>

### Theory 

Think about the case of a random sample of size *n* , from a probability distribution function *F* undefined:
$$X_i=x_i,     X_i$$ $$\sim$$ $$F_{i}$$, $$i=1,2,...,n$$.

Let $$X= (X_1,X_2,...X_n)$$ and $$x=(x_1,x_2,...,x_n)$$ be the random variables and the observations of the independent random variables, with the same distribution function *F* . We will investigate the variability and the sampling distribution of the local estimation calculation from the sample of size *n*. We denote this local estimate as $$\widehatθ$$. Note that $$\widehatθ$$ is a function of the random variables $$(X_1, X_2, ... X_n)$$ and, therefore, has a probability distribution, its sample distribution, which is determined by *n* and *F*.

Based on Efron and Tibshirani (1993), the basic steps for creating Bootstrap are:

#### Step 1:
Construct, from the sample, an empirical probability distribution $$F_n$$, inserting, in each point $$x_1, x_2, ... x_n$$ of the sample, a probability $$1/n$$.

Characterized as an empirical distribution function of the sample, this is a non-parametric estimate of the maximum probability from the population distribution, *F*.

#### Step 2:
Perform the resample: draw a random sample of size *n* with replacement from the $$F_n$$ distribution function.

#### Step 3:
Calculate for this resampling the statistic of interest $$T_n$$, generating $$T_n^*$$.

#### Step 4:
Repeat steps 2 and 3 *B* times. To create *B* resamples, *B* must be a large value. The size of *B* depends on tests that will be performed with the data. When an estimate with confidence interval of $$T_n$$ is required, it is suggested that *B* should be at least equal to 1000.

#### Step 5:
Build, from the value *B* of $$T_n$$, the relative frequency of the histogram, assigning probability of $$1/B$$ to each point $$T_n^1$$, $$T_n^2$$,..., $$T_n^B$$.

The obtained distribution is the Bootstrap estimate of the sampling distribution of $$T_n$$. One can make inferences from parameter θ, using this distribution, estimating $$T_n$$.

## Bootstrap and stocks in financial market using R

In the following example, we will create the Bootstrap sample for the daily returns of Apple shares and the S&P500 index from 06/02/2016 to 05/31/2017. To do so, first download the base *bootstrap.txt* from the following [file](https://raw.githubusercontent.com/lamfo-unb/lamfo-data/master/dadosBootstrap.txt). Now, read the database in R using the code below:

```R
dados<-read.table(file.choose())
```
To work with this file type the following command:

```R
spxret <- dados[, 'SP500index']
applret <- dados[,'APPLE']

names(dados)[names(dados)=="SP500index"] <- "sp500"
dados$sp500 <- as.numeric(as.character(dados$sp500))

dados$sp500<-c(NA,diff(log(dados$sp500)))
dados$APPLE<-c(NA,diff(log(dados$APPLE)))

spxret<-as.numeric(spxret)    
   ```

The `spxret` and` applret` commands extract a column to create a vector. In the following lines of the code above, we work on the database to turn all data into numerics, removing NA's (No Data values) and transforming returns into log returns.
   
In our analysis we obtained, in one year, 251 daily returns. Randomly a bootstrap sample was obtained from daily returns. In our case, you will notice that some days will appear several times in the bootstrap sample while others will not appear at all. This is due to the fact that we perform sampling with replacement. From our bootstrap sample, we chose to perform the sum of the values. Typically several bootstrap samples are created.

The code below shows how to use Bootstrap to generate 1000 Bootstrap samples. We can see below that we sample with integers from 1 to 251. We created a sample of size 251 and sample it with replacement. With the command `this.samp` we get a year with daily returns that could have happened. We collect annual returns for each hypothetical year in the following code line.

 ```R
spx.boot.sum <- numeric(1000) # numeric vector 1000 long

for(i in 1:1000) {
  this.samp <- spxret[ sample(251, 251, replace=TRUE) ]
  spx.boot.sum[i] <- sum(this.samp)
}
   ```
You can also plot the annual returns distribution obtained using Bootstrap with the following code:

```R
plot(density(spx.boot.sum), lwd=3, col="orange")
abline(v=sum(spxret), lwd=3, col='blue')
   ```
According to the image below, the annual return is quite variable. If you use a different initial database, your chart will change. In this case we find that our annual return is very close to the middle of the distribution. It is noteworthy that this result may be different due to statistical biases or the database.

 ![](https://i.imgur.com/HrxEhHM.png)


### Let's practice? 
   
## References
 
Efron, B. (1979). Bootstrap methods: another look at the jackknife. The annals of Statistics, 1-26.

Efron, B, and Tibshirani, R.J. (1993). An introduction to the bootstrap. Chapman and Hall, London.

