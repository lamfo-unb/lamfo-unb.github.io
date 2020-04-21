---
layout: post
title: Statistical analysis of the chinese COVID-19 data with Benford's Law and clustering.
lang: en
header-img: img/covid_capa.jpg
date: 2020-04-21
tags: [COVID-19, Coronavirus, exponential growth, hypothesis test]
author: Yaohao Peng and Mateus Hiro Nagata.
comments: true
---

### "真金不怕火煉"
#### *(Chinese saying: "True gold does not fear the test of fire")*


---

###### The views expressed in this work are of entire responsibility of the authors and do not necessarily reflect those of their respective affiliated institutions nor those of its members.

---

## Motivation

COVID-19 (SARS-CoV-2) is a ongoing pandemic that infected more than 2.5 million people around the world and claimed more than 170,000 lifes as of 21 April 2020. Unlike all other pandemics recorded in history, a large volume of data and news concerning COVID-19 are flows in with great speed and coverage, mobilizing scholars from various fields of knowledge to focus their efforts on analyzing those data and proposing solutions.

In epidemic data, it is natural to observe an exponential growth in the infected cases, especially in the early stages of the disease. As addressed in posts like [this](https://medium.com/@tomaspueyo/coronavirus-the-hammer-and-the-dance-be9337092b56), measures of social isolation seek to "flatten the curve", reducing the number of affected at the peak, but prolonging the "wave" over the time. Similarly, the number of deaths also follows an exponential trend.

Analyzing data from countries affected by the pandemic, it is possible to observe patterns that are common to all. Despite the existence of several peculiarities such as territorial extension, population density, temperature, season, degree of underreporting, social discipline to comply with isolation measures, etc., which differ significantly between different countries, the virus has not (yet) suffered radical mutations since its appearance in China, such that the general parameters of infectivity and lethality are similar between countries. However, **the chinese data are a notable exception**, showing a behavior that differs from the others -- despite being the first country affected by the disease, the number of infected people evolution remained close to a linear trend in the early stages , followed by <ins>few moments that suffered abrupt variation and prolonged periods marked by the absence of variance</ins>, both unusual patterns in nature. Here are some graphs:

![](/img/covid19/f1.png)
***Image 1**: Cumulative COVID-19 cases in China, as of 18 Apr 2020*

![](/img/covid19/f2.png)
***Image 2**: Same as image 1, comparing the province of Hubei with all others*

![](/img/covid19/f3.png)
***Image 3**: Same as image 2, with all provinces except Hubei, re-scaled for better viewing*


**Image 1** above shows the cumulative number of confirmed cases of COVID-19 in China (Taiwan not included). Note that the exponential pattern appears at the beginning but the concavity of the curve changes rapidly, contrary to expectations and to what was observed in almost all other countries. The growth happened with "jumps" at the beginning of the series, followed by a practically linear trend in the first days of February, a single day of great growth, and a long period with decreasing new cases per day, until the curve became practically a straight line since the early March.

Furthermore, looking at the decomposition of the aggregate data for province level in **image 2**, we can notice that a single province -- Hubei -- is responsible for almost all cases from China, while all other provinces (which also differ considerably among themselves in area, population density, temperature, distance to the virus' city of origin, etc.) exhibited practically the same pattern, resulting in a sigmoid curve with surgical precision.

At the provincial level, the only place that showed some degree of variance over time was Hong Kong; in Shandong, there was a great variation in a single day -- 21 February 2020, when an outbreak of the virus was reported in the Rencheng Prison in the city of Jining: on that day 203 people were added to the confirmed statistic, an isolated point in a "well behaved" curve in all periods besides that day. In Guangdong (in southern China) and Heilongjiang (in northeastern China) the series returned to grow in late March and early April, respectively, both after a long period with virtually no variation. See the behavior of the series of these provinces in **image 3**.

![](/img/covid19/f4.png)
***Image 4**: Cumulative COVID-19 cases per state in the Unites States, as of 30 Mar 2020. Retrieved from ["Coronavirus: Out of Many, One" by Tomas Pueyo](https://medium.com/@tomaspueyo/coronavirus-out-of-many-one-36b886af37e9)*

![](/img/covid19/f5.png)
***Image 5**: Same as image 4, with all states except New York and New Jersey. Retrieved from ["Coronavirus: Out of Many, One" by Tomas Pueyo](https://medium.com/@tomaspueyo/coronavirus-out-of-many-one-36b886af37e9)*

It is well known that <ins>the derivative of an exponential function is also an exponential</ins>, so it is expected that the daily variation of the confirmed cases would also follow an exponential. The Chinese data, however, showed something quite different:

![](/img/covid19/f6.png)
***Image 6**: Daily variation of COVID-19 cases in China, as of 18 Apr 2020*

The initial section of the series seems more like a straight line than an exponential curve, considering the two peaks on January 28 and February 02, while the number of new cases started to decrease rapidly and without major variations -- except for the peak of 15136 new cases on 13 February 2020, which clearly differs from the other periods. Disregarding this anomalous point, we can see a practically linear trend between Febryary 02 and February 23, from which the series resembles a straight line -- again, a rare pattern in contagious diseases.

See below the same graph comparing Hubei with all other provinces:

![](/img/covid19/f7.png)
***Image 7**: Daily variation of COVID-19 cases in China, as of 18 Apr 2020, comparing Hubei with all other provinces*

![](/img/covid19/f8.png)
***Image 8**: Same as image 7, with all provinces except Hubei, re-scaled for better viewing*

It is worth noting that, [on the same day that anomalous peak occurred, Jiang Chaoling and Ma Guoqiang were exonerated](http://www.xinhuanet.com/renshi/2020-02/13/c_1125568253.htm): they were the No. 1 and No. 2 in the Hubei command hierarchy (secretary-general and deputy secretary-general of the Party in the province, respectively).

**The Chinese data for deaths from COVID-19 are also anti-intuitive**, see below in **images 9 to 11**. Note that the second province with the most recorded deaths after Hubei was its neighbor Henan, with only 22 deaths. The daily variation in deaths is a practically stationary series, with the extra spice of the bizarre "adjustment" of 1290 deaths on April 17 after more than a month with almost no official deaths:

![](/img/covid19/f9.png)
***Image 9**: Cumulative COVID-19 deaths in China, as of 18 Apr 2020, comparing Hubei with all other provinces*

![](/img/covid19/f10.png)
***Image 10**: Same as image 9, with all provinces except Hubei, re-scaled for better viewing*

![](/img/covid19/f11.png)
***Image 11**: Daily variation of COVID-19 deaths in China, as of 18 Apr 2020*

The adoption of severe measures of social isolation influences the shape of the curves directly. However, the exponential pattern still remains at least in the initial stages, and effects of those measures also take some time to become evident. Let's compare the data from Japan, Singapore and South Korea, which responded early to the disease, as well as from Italy, Spain and the United Kingdom, which acted more intensely only in more advanced stages:

![](/img/covid19/f12.png)
***Imagem 12**: Cumulative COVID-19 cases in Spain, Italy, United Kingdom, China, South Korea, Japan, and Singapore, as of 18 Apr 2020*

![](/img/covid19/f13.png)
***Imagem 13**: Same as image 12, with the asian countries except China. Note that Japan and Singapore also exhibited a late exponential growth. Only South Korea's curve exhibited a similar shape to the chinese data*

With the visual analysis above, we can deduce the existence of some underlying "pattern" in the COVID-19 data, but for some reason it does not appear in the Chinese data. Next, let's do an exercise trying to identify this pattern using **Benford's Law**.

## Benford's Law

Life is mysterious and unexpected patterns rule the world. As they say, “life imitates art”. However, when art tries to imitate life, we can feel something strange, as if the complexity of life cannot be replaced by naive human engineering.

One of those patterns that seems to "emerge" in nature is Benford's Law, discovered by [Newcomb (1881)](https://www.semanticscholar.org/paper/Note-on-the-Frequency-of-Use-of-the-Different-in-Newcomb/4136337f95c88d446a5577d9331c8fc0309c11af) and popularized by [Benford (1938)](https://en.scribd.com/document/209534421/The-Law-of-Anomalous-Numbers), and widely used to check frauds in databases.
Um desses padrões que parecem "emergir" na natureza é Lei de Benford, descoberta por [Newcomb (1881)](https://www.semanticscholar.org/paper/Note-on-the-Frequency-of-Use-of-the-Different-in-Newcomb/4136337f95c88d446a5577d9331c8fc0309c11af) e popularizada por [Benford (1938)](https://pt.scribd.com/document/209534421/The-Law-of-Anomalous-Numbers), muito usada para verificar fraudes em base de dados.

Benford, testing for more than 20 variables from different contexts, such as river sizes, population of cities, physics constants, mortality rate, etc., found out that **the chance of the first digit of a number to be equal to 1 was the highest, chance that decreased progressively for the subsequent numbers**. That is, it is more likely that the first digit of a number is 1, then 2, and successively up to 9.

[Okhrimenko and Kopczewski (2019)](https://www.nbp.pl/badania/seminaria/8ii2019.pdf), two behavioral economists, tested people's ability to create false data in order to circumvent the tax base. The authors found evidence that by the criteria of Benford's law, the system would easily identify false data. Other applications of Benford's Law for manipulated data identification and fraud detection include [Hales et al. (2008)](https://www.sciencedirect.com/science/article/abs/pii/S0377221706011702), [Abrantes- Metz et al. (2012)](https://www.sciencedirect.com/science/article/abs/pii/S0378426611002032) and [Nigrini (2012)](https://www.amazon.com/Benfords-Law-Applications-Accounting-Detection/dp/1118152859).

How can we intuitively explain this seemingly random regularity? The answer is related to two known concepts: the exponential function and the logarithmic scale. Let's review them as they are everywhere, especially during the current pandemic...

![](/img/covid19/expo.jpg)

Epidemics such as the Coronavirus, which we are experiencing at the moment, are classic examples to explain the exponential function. The modeling happens as follows: the amount of infected tomorrow, $$I_1$$, can be modeled as a constant $$\alpha$$ times the amount of infected today, $$I_0$$; that is, $$I_1 = \alpha \cdot I_0$$.

Assuming that the rate is the same for tomorrow (we can interpret it as having no new policies, or no changes in population habits have occurred), the number of people infected the day after tomorrow, $$I_2$$, is a proportion of what will be tomorrow ($$I_2=\alpha \cdot I_1$$), which in turn can be substituted by $$I_2=\alpha \cdot I_1 = \alpha \cdot \alpha \cdot I_0$$. This expression can be generalized for $$t$$ days from now: being $$t$$ any positive integer, the generalization is $$I_t=\alpha^t \cdot I_0$$. The virtually omnipresent compound interest from the financial numbers also follows the same logic ($$F = P(1+i)^n$$).

Here is the secret of Benford's law. Let's look at a exponentially growing epidemic simulation, that is, the number of infected at each day is a fixed multiple of the previous day. Let's look at the standard scale and the logarithmic scale over time (in this case we use exponentials of 2, but it could be any positive integer). Note that in both cases, each gradation of blue is the area referring to a digit. The first corresponds between 10 and 20, the second between 20 and 30, so on until 100. Note that the width of the bands is different between the standard scale and the logarithmic scale.

![](/img/covid19/f14.png)
***Imagem 14**: Exponential function and logarithmic scale*

This graph already gives us a clue as to why exponential phenomena may obey Benford's law. When we look through the logarithmic lens, an exponential function looks like a function that grows linearly, and each observation is equidistant from the previous and the next observations. However, in this same logarithmic lens, the area between 10 and 20 is bigger than the area between 20 and 30, and so on. This means that the probability of the variable falling in the first range is greater than falling in the following ranges.

Using a technical jargon, <ins>**the log mantissa is uniformly distributed**</ins>. For a number $$x$$ taken from a sample, let's apply his log -- for example, apply the log to the number 150: $$log_{10}(150) \approx 2,176$$. We can decompose the result into the integer part $$m = 2$$ and decimal part $$d = 0.176$$. The decimal part is what we call *mantissa*.


### Mantissa and theoretical distribution $$D_T$$

From the logarithm properties, $$log_{10} (150) = log_{10}(100 \cdot 1.5) = log_{10}(100) + log_{10}(1.5) = 2 + 0.176$$. Note that the base 10 log of any integer power of 10 (100, 1000, 10000 ...) will result in an integer. In this sense, the integer part of the base 10 log returns the number of digits that the original number has (since the Indo-Arabic number system has 10 digits). The mantissa, on the other hand, is responsible for saying which is the first digit.

For numbers at the hundreds, when the mantissa is in the $$[0, 0.301)$$ range, the original number will be between $$[100, 200)$$. Doing the same procedure as above, when the number reaches 200, its log will result in $$ log_{10}(200) = log_{10}(100 \cdot 2) = log_{10}(100) + log_{10}(2) $$ which results in $$ 2 + 0.301 $$.

Now that we know that the mantissa is decisive for knowing the first digit, we can finally understand Benford's law. Its idea is that the mantissa has a uniform distribution for digits 1 to 9. Thus, it is easier for 1 to be the leading digit because it has the largest interval of the mantissa $$ [0, 0.301) $$.

We can define, according to Benford's law, the probability for a number to have first digit equal to $$ d $$, given by:

\begin{equation}
P(d)=\log_{10}\left(1+\frac{1}{d}\right), d = 1,...,9
\end{equation}


To save the reader's time, we calculated the probability distribution for each digit to be the first according to Benford's Law ($$D_T$$), as well as the respective intervals on the mantissa:

<center>

\begin{array}{|c|c|c|c|}
\hline d & P(d) & Probability & Mantissa \\
\hline 1 & log_{10} (1+1) = 0.30103  & 30.1\% & [0, 0.301)\\
\hline 2 & \log _{10}\left(1+\frac{1}{2}\right) =  0.1760913  & 17.6\% & [0.301, 0.477)\\
\hline 3 & \log _{10}\left(1+\frac{1}{3}\right) = 0.1249387 & 12.5\% & [0.477, 0.602) \\
\hline 4 & \log _{10}\left(1+\frac{1}{4}\right) = 0.09691001 & 9.7\% & [0.602, 0.699)\\
\hline 5 & \log _{10}\left(1+\frac{1}{5}\right) = 0.07918125 & 7.9\% & [0.699, 0.778)\\
\hline 6 & \log _{10}\left(1+\frac{1}{6}\right) = 0.06694679 & 6.7\% & [0.778, 0.845)\\
\hline 7 & \log _{10}\left(1+\frac{1}{7}\right) = 0.05799195 & 5.8\% & [0.845, 0.912)\\
\hline 8 & \log _{10}\left(1+\frac{1}{8}\right) = 0.05115252 & 5.1\% & [0.912, 0.954)\\
\hline 9 & \log _{10}\left(1+\frac{1}{9}\right) = 0.04575749 & 4.6\% & [0.963, 1.00]\\
\hline
\end{array}

</center>
***Tabela 1** : First digit distribution according to Benford's Law*

For more details on Benford's law and its applications, take a look at these links [[1](http://prorum.com/?qa=2157/existem-formas-de-detectar-fraudes-em-bases-de-dados); [2](https://epublications.marquette.edu/cgi/viewcontent.cgi?article=1031&context=account_fac); [3](https://towardsdatascience.com/what-is-benfords-law-and-why-is-it-important-for-data-science-312cb8b61048)]

## Empirical distributions $$D_E$$ of COVID-19 data

For this exercise, we picked the <ins>countries with more than 10,000 confirmed cases of COVID-19 as of April 18, 2020</ins>, according to the data available in [this link](https://github.com/RamiKrispin/coronavirus-csv). Let's see below the first digit distributions of the time series of COVID-19 cases and deaths for the 24 selected countries:

![](/img/covid19/f15.png)
***Image 15**: First digit distributions of COVID-19 cases*

**No database follows Benford's Law perfectly, but China's empirical distributions appear to be particularly different from those of other countries**. By visual inspection, the data from China seems to be significantly desynchronized with Benford's Law. For greater robustness, we will compare the empirical distributions $$D_E$$ with the theoretical distribution $$D_T$$ that comes from Benford's Law by performing some hypothesis tests.

## Hypothesis tests

For this exercise, we will perform three hypothesis tests:

* Chi-squared test
* Kolmogorov–Smirnov test (two-tailed)
* Kuiper's test

The three tests above are similar, <ins>all of them have as a null hypothesis of equality between the empirical ($$D_E$$) and theoretical ($$D_T$$) distributions</ins>. The chi-squared test is the most commonly used, however it tends to reject the null hypothesis more easily, while the KS test is less sensitive to pointwise differences; Kuiper's test works in the same way as the KS test, with the difference that it considers separately positive and negative differences between the distributions (the case "$$D_E$$ greater than $$D_T$$" is seen as different from the case "$$D_T$$ greater than $$D_E$$"). The tables with the associated p-values are displayed below:


<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-1wig">Country/Test</th>
    <th class="tg-1wig">Chi-squared</th>
    <th class="tg-1wig">Kolmogorov-Smirnov</th>
    <th class="tg-1wig">Kuiper</th>
  </tr>
  <tr>
    <td class="tg-1wig">Austria</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Belgium</td>
    <td class="tg-1wig">0.0004</td>
    <td class="tg-1wig">0.0366</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brazil</td>
    <td class="tg-0lax">0.2685</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canada</td>
    <td class="tg-1wig">0.0117</td>
    <td class="tg-0lax">1.0000</td>
    <td class="tg-0lax">1.0000</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-1wig">0.0086</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">France</td>
    <td class="tg-1wig">0.0363</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">1.0000</td>
  </tr>
  <tr>
    <td class="tg-1wig">Germany</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">India</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Iran</td>
    <td class="tg-0lax">0.1284</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Ireland</td>
    <td class="tg-0lax">0.8036</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.5245</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Italy</td>
    <td class="tg-0lax">0.0705</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japan</td>
    <td class="tg-0lax">0.0509</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">South Korea</td>
    <td class="tg-1wig">0.0002</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Netherlands</td>
    <td class="tg-0lax">0.4804</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.9629</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.6247</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Russia</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Spain</td>
    <td class="tg-1wig">0.0027</td>
    <td class="tg-1wig">0.0366</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Sweden</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Switzerland</td>
    <td class="tg-1wig">0.0111</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turkey</td>
    <td class="tg-0lax">0.6985</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">United Kingdom</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">United States</td>
    <td class="tg-1wig">0.0156</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
</table>
</center>
***Table 2** : P-values of the hypothesis tests for COVID-19 cases data, rounded to 4 digits. Values with significance at 95% confidence level are marked in bold*

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-1wig">Country/Test</th>
    <th class="tg-1wig">Chi-squared</th>
    <th class="tg-1wig">Kolmogorov-Smirnov</th>
    <th class="tg-1wig">Kuiper</th>
  </tr>
  <tr>
    <td class="tg-1wig">Austria</td>
    <td class="tg-0lax">0.2883</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Belgium</td>
    <td class="tg-0lax">0.3746</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brazil</td>
    <td class="tg-0lax">0.9773</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canada</td>
    <td class="tg-0lax">0.7868</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">France</td>
    <td class="tg-1wig">0.0454</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Germany</td>
    <td class="tg-0lax">0.5473</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">India</td>
    <td class="tg-0lax">0.3685</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Iran</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Ireland</td>
    <td class="tg-0lax">0.7039</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.4175</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Italy</td>
    <td class="tg-0lax">0.2414</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japan</td>
    <td class="tg-1wig">0.0203</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">South Korea</td>
    <td class="tg-0lax">0.1442</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Netherlands</td>
    <td class="tg-0lax">0.4993</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.5246</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.3712</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Russia</td>
    <td class="tg-0lax">0.6750</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Spain</td>
    <td class="tg-0lax">0.1228</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Sweden</td>
    <td class="tg-0lax">0.7078</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Switzerland</td>
    <td class="tg-0lax">0.6034</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turkey</td>
    <td class="tg-0lax">0.5745</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">United Kingdom</td>
    <td class="tg-0lax">0.8325</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">United States</td>
    <td class="tg-0lax">0.9284</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
</table>

</center>
***Table 3** : P-values of the hypothesis tests for COVID-19 deaths data, rounded to 4 digits. Values with significance at 95% confidence level are marked in bold*

Basically, the **smaller** the p-value, **less** the data for the respective country seems to "obey" Benford's Law. <ins>Using this evaluation metric, the Chinese data are clearly anomalous regarding Benford's Law, while the vast majority of other countries seem to follow it reasonably well.</ins>

## KL divergence and DBSCAN clustering

Now let's see how "similar" the country data are to each other, using a metric called **Kullback-Leibler divergence** (henceforth "KL divergence"), which is a <ins>measure of relative entropy between two probability distributions</ins>. Its calculation for discrete distributions is given by the following expression:

\begin{equation}
KL(D_1||D_2) = \sum\limits_{x \in \mathcal{P}}{D_1(x)\cdot log\left(\frac{D_1(x)}{D_2(x)}\right)}
\end{equation}

The KL divergence gives the expected value of the logarithmic difference between two distributions $$ D_1 $$ and $$ D_2 $$ defined in the same probability space $$ \mathcal{P} $$; the theoretical discussion of this concept lies beyond the scope of this post, as it involves knowledge of information theory and measure theory. In simplified terms, the KL divergence measures how different two probability distributions are -- the closer to zero its value, the more "similar" they are. As there are 24 countries, by comparing all dempirical distributions $$D_E$$ we yield 24x24 matrices that measure the "difference" between the data of the countries considered (one for the cases data and another for the deaths data), matrice whose main diagonals are all zero (the "difference" of something to itself is equal to zero!).

In **table 4** below we present the matrix of KL divergenves between the empirical distributions of confirmed cases data from the 24 selected countries. To save space, we omitted the matrix calculated from the deaths data.

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0lax"></th>
    <th class="tg-1wig">Austria</th>
    <th class="tg-1wig">Belgium</th>
    <th class="tg-1wig">Brazil</th>
    <th class="tg-1wig">Canada</th>
    <th class="tg-1wig">China</th>
    <th class="tg-1wig">France</th>
    <th class="tg-1wig">Germany</th>
    <th class="tg-1wig">India</th>
    <th class="tg-1wig">Iran</th>
    <th class="tg-1wig">Ireland</th>
    <th class="tg-1wig">Israel</th>
    <th class="tg-1wig">Italy</th>
    <th class="tg-1wig">Japan</th>
    <th class="tg-1wig">South Korea</th>
    <th class="tg-1wig">Netherlands</th>
    <th class="tg-1wig">Peru</th>
    <th class="tg-1wig">Portugal</th>
    <th class="tg-1wig">Russia</th>
    <th class="tg-1wig">Spain</th>
    <th class="tg-1wig">Sweden</th>
    <th class="tg-1wig">Switzerland</th>
    <th class="tg-1wig">Turkeyth>
    <th class="tg-1wig">United Kingdom</th>
    <th class="tg-1wig">United States</th>
  </tr>
  <tr>
    <td class="tg-1wig">Austria</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.0734</td>
    <td class="tg-0lax">0.1005</td>
    <td class="tg-0lax">0.5631</td>
    <td class="tg-0lax">0.0387</td>
    <td class="tg-0lax">0.0447</td>
    <td class="tg-0lax">0.1739</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.0371</td>
    <td class="tg-0lax">0.0395</td>
    <td class="tg-0lax">0.0595</td>
    <td class="tg-0lax">0.0921</td>
    <td class="tg-0lax">0.0778</td>
    <td class="tg-0lax">0.0313</td>
    <td class="tg-0lax">0.0268</td>
    <td class="tg-0lax">0.0146</td>
    <td class="tg-0lax">0.1832</td>
    <td class="tg-0lax">0.0362</td>
    <td class="tg-0lax">0.0214</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0826</td>
    <td class="tg-0lax">0.1109</td>
    <td class="tg-0lax">0.0410</td>
  </tr>
  <tr>
    <td class="tg-1wig">Belgium</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0452</td>
    <td class="tg-0lax">0.1612</td>
    <td class="tg-0lax">0.7024</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0956</td>
    <td class="tg-0lax">0.1827</td>
    <td class="tg-0lax">0.1964</td>
    <td class="tg-0lax">0.0727</td>
    <td class="tg-0lax">0.0819</td>
    <td class="tg-0lax">0.0488</td>
    <td class="tg-0lax">0.1466</td>
    <td class="tg-0lax">0.1303</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0704</td>
    <td class="tg-0lax">0.0556</td>
    <td class="tg-0lax">0.1564</td>
    <td class="tg-0lax">0.0539</td>
    <td class="tg-0lax">0.0455</td>
    <td class="tg-0lax">0.0995</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1662</td>
    <td class="tg-0lax">0.1019</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brazil</td>
    <td class="tg-0lax">0.0734</td>
    <td class="tg-0lax">0.0452</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0981</td>
    <td class="tg-0lax">0.5960</td>
    <td class="tg-0lax">0.1057</td>
    <td class="tg-0lax">0.1721</td>
    <td class="tg-0lax">0.1647</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0601</td>
    <td class="tg-0lax">0.0887</td>
    <td class="tg-0lax">0.0252</td>
    <td class="tg-0lax">0.0707</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0235</td>
    <td class="tg-0lax">0.0650</td>
    <td class="tg-0lax">0.0670</td>
    <td class="tg-0lax">0.0532</td>
    <td class="tg-0lax">0.0693</td>
    <td class="tg-0lax">0.1201</td>
    <td class="tg-0lax">0.0533</td>
    <td class="tg-0lax">0.1100</td>
    <td class="tg-0lax">0.0990</td>
    <td class="tg-0lax">0.1111</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canada</td>
    <td class="tg-0lax">0.1005</td>
    <td class="tg-0lax">0.1612</td>
    <td class="tg-0lax">0.0981</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.3342</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.1370</td>
    <td class="tg-0lax">0.2187</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.0376</td>
    <td class="tg-0lax">0.1266</td>
    <td class="tg-0lax">0.0310</td>
    <td class="tg-0lax">0.0433</td>
    <td class="tg-0lax">0.1267</td>
    <td class="tg-0lax">0.0698</td>
    <td class="tg-0lax">0.0839</td>
    <td class="tg-0lax">0.2046</td>
    <td class="tg-0lax">0.1453</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1335</td>
    <td class="tg-0lax">0.0830</td>
    <td class="tg-0lax">0.0824</td>
    <td class="tg-0lax">0.1181</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-0lax">0.5631</td>
    <td class="tg-0lax">0.7024</td>
    <td class="tg-0lax">0.5960</td>
    <td class="tg-0lax">0.3342</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.7432</td>
    <td class="tg-0lax">0.7605</td>
    <td class="tg-0lax">0.6701</td>
    <td class="tg-0lax">0.5725</td>
    <td class="tg-0lax">0.6681</td>
    <td class="tg-0lax">0.5067</td>
    <td class="tg-0lax">0.6763</td>
    <td class="tg-0lax">0.4182</td>
    <td class="tg-0lax">0.3305</td>
    <td class="tg-0lax">0.6036</td>
    <td class="tg-0lax">0.5808</td>
    <td class="tg-0lax">0.6312</td>
    <td class="tg-0lax">0.8489</td>
    <td class="tg-0lax">0.7006</td>
    <td class="tg-0lax">0.7566</td>
    <td class="tg-0lax">0.6211</td>
    <td class="tg-0lax">0.6705</td>
    <td class="tg-0lax">0.4789</td>
    <td class="tg-0lax">0.6572</td>
  </tr>
  <tr>
    <td class="tg-1wig">France</td>
    <td class="tg-0lax">0.0387</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.1057</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.7432</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.1576</td>
    <td class="tg-0lax">0.0771</td>
    <td class="tg-0lax">0.0246</td>
    <td class="tg-0lax">0.0361</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.1107</td>
    <td class="tg-0lax">0.1180</td>
    <td class="tg-0lax">0.0807</td>
    <td class="tg-0lax">0.0137</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.2226</td>
    <td class="tg-0lax">0.0879</td>
    <td class="tg-0lax">0.0524</td>
    <td class="tg-0lax">0.1224</td>
    <td class="tg-0lax">0.0256</td>
    <td class="tg-0lax">0.1278</td>
    <td class="tg-0lax">0.0244</td>
  </tr>
  <tr>
    <td class="tg-1wig">Germany</td>
    <td class="tg-0lax">0.0447</td>
    <td class="tg-0lax">0.0956</td>
    <td class="tg-0lax">0.1721</td>
    <td class="tg-0lax">0.1370</td>
    <td class="tg-0lax">0.7605</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.2417</td>
    <td class="tg-0lax">0.1751</td>
    <td class="tg-0lax">0.0690</td>
    <td class="tg-0lax">0.0461</td>
    <td class="tg-0lax">0.1260</td>
    <td class="tg-0lax">0.1635</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.1170</td>
    <td class="tg-0lax">0.0521</td>
    <td class="tg-0lax">0.0420</td>
    <td class="tg-0lax">0.2765</td>
    <td class="tg-0lax">0.0858</td>
    <td class="tg-0lax">0.0206</td>
    <td class="tg-0lax">0.1607</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.1935</td>
    <td class="tg-0lax">0.0691</td>
  </tr>
  <tr>
    <td class="tg-1wig">India</td>
    <td class="tg-0lax">0.1739</td>
    <td class="tg-0lax">0.1827</td>
    <td class="tg-0lax">0.1647</td>
    <td class="tg-0lax">0.2187</td>
    <td class="tg-0lax">0.6701</td>
    <td class="tg-0lax">0.1576</td>
    <td class="tg-0lax">0.2417</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.2459</td>
    <td class="tg-0lax">0.2386</td>
    <td class="tg-0lax">0.2244</td>
    <td class="tg-0lax">0.0706</td>
    <td class="tg-0lax">0.2892</td>
    <td class="tg-0lax">0.2965</td>
    <td class="tg-0lax">0.1888</td>
    <td class="tg-0lax">0.2027</td>
    <td class="tg-0lax">0.2205</td>
    <td class="tg-0lax">0.3063</td>
    <td class="tg-0lax">0.3947</td>
    <td class="tg-0lax">0.2911</td>
    <td class="tg-0lax">0.3474</td>
    <td class="tg-0lax">0.1517</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.2509</td>
  </tr>
  <tr>
    <td class="tg-1wig">Iran</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.1964</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.5725</td>
    <td class="tg-0lax">0.0771</td>
    <td class="tg-0lax">0.1751</td>
    <td class="tg-0lax">0.2459</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0947</td>
    <td class="tg-0lax">0.0674</td>
    <td class="tg-0lax">0.1353</td>
    <td class="tg-0lax">0.0491</td>
    <td class="tg-0lax">0.1192</td>
    <td class="tg-0lax">0.1351</td>
    <td class="tg-0lax">0.0598</td>
    <td class="tg-0lax">0.1113</td>
    <td class="tg-0lax">0.2188</td>
    <td class="tg-0lax">0.1537</td>
    <td class="tg-0lax">0.1645</td>
    <td class="tg-0lax">0.1445</td>
    <td class="tg-0lax">0.0292</td>
    <td class="tg-0lax">0.1040</td>
    <td class="tg-0lax">0.0680</td>
  </tr>
  <tr>
    <td class="tg-1wig">Ireland</td>
    <td class="tg-0lax">0.0371</td>
    <td class="tg-0lax">0.0727</td>
    <td class="tg-0lax">0.0601</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.6681</td>
    <td class="tg-0lax">0.0246</td>
    <td class="tg-0lax">0.0690</td>
    <td class="tg-0lax">0.2386</td>
    <td class="tg-0lax">0.0947</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0321</td>
    <td class="tg-0lax">0.0540</td>
    <td class="tg-0lax">0.0644</td>
    <td class="tg-0lax">0.0742</td>
    <td class="tg-0lax">0.0472</td>
    <td class="tg-0lax">0.0068</td>
    <td class="tg-0lax">0.0459</td>
    <td class="tg-0lax">0.1446</td>
    <td class="tg-0lax">0.0583</td>
    <td class="tg-0lax">0.0663</td>
    <td class="tg-0lax">0.0632</td>
    <td class="tg-0lax">0.0446</td>
    <td class="tg-0lax">0.0790</td>
    <td class="tg-0lax">0.0411</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.0395</td>
    <td class="tg-0lax">0.0819</td>
    <td class="tg-0lax">0.0887</td>
    <td class="tg-0lax">0.0376</td>
    <td class="tg-0lax">0.5067</td>
    <td class="tg-0lax">0.0361</td>
    <td class="tg-0lax">0.0461</td>
    <td class="tg-0lax">0.2244</td>
    <td class="tg-0lax">0.0674</td>
    <td class="tg-0lax">0.0321</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.0621</td>
    <td class="tg-0lax">0.0432</td>
    <td class="tg-0lax">0.0809</td>
    <td class="tg-0lax">0.0184</td>
    <td class="tg-0lax">0.0456</td>
    <td class="tg-0lax">0.2038</td>
    <td class="tg-0lax">0.0847</td>
    <td class="tg-0lax">0.0467</td>
    <td class="tg-0lax">0.1104</td>
    <td class="tg-0lax">0.0476</td>
    <td class="tg-0lax">0.0952</td>
    <td class="tg-0lax">0.0578</td>
  </tr>
  <tr>
    <td class="tg-1wig">Italy</td>
    <td class="tg-0lax">0.0595</td>
    <td class="tg-0lax">0.0488</td>
    <td class="tg-0lax">0.0252</td>
    <td class="tg-0lax">0.1266</td>
    <td class="tg-0lax">0.6763</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.1260</td>
    <td class="tg-0lax">0.0706</td>
    <td class="tg-0lax">0.1353</td>
    <td class="tg-0lax">0.0540</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1031</td>
    <td class="tg-0lax">0.1094</td>
    <td class="tg-0lax">0.0357</td>
    <td class="tg-0lax">0.0558</td>
    <td class="tg-0lax">0.0724</td>
    <td class="tg-0lax">0.1237</td>
    <td class="tg-0lax">0.1236</td>
    <td class="tg-0lax">0.1072</td>
    <td class="tg-0lax">0.1077</td>
    <td class="tg-0lax">0.0759</td>
    <td class="tg-0lax">0.0870</td>
    <td class="tg-0lax">0.1073</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japan</td>
    <td class="tg-0lax">0.0921</td>
    <td class="tg-0lax">0.1466</td>
    <td class="tg-0lax">0.0707</td>
    <td class="tg-0lax">0.0310</td>
    <td class="tg-0lax">0.4182</td>
    <td class="tg-0lax">0.1107</td>
    <td class="tg-0lax">0.1635</td>
    <td class="tg-0lax">0.2892</td>
    <td class="tg-0lax">0.0491</td>
    <td class="tg-0lax">0.0644</td>
    <td class="tg-0lax">0.0621</td>
    <td class="tg-0lax">0.1031</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0537</td>
    <td class="tg-0lax">0.0528</td>
    <td class="tg-0lax">0.0651</td>
    <td class="tg-0lax">0.0831</td>
    <td class="tg-0lax">0.0927</td>
    <td class="tg-0lax">0.0679</td>
    <td class="tg-0lax">0.1534</td>
    <td class="tg-0lax">0.0386</td>
    <td class="tg-0lax">0.1049</td>
    <td class="tg-0lax">0.0596</td>
    <td class="tg-0lax">0.1089</td>
  </tr>
  <tr>
    <td class="tg-1wig">South Korea</td>
    <td class="tg-0lax">0.0778</td>
    <td class="tg-0lax">0.1303</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0433</td>
    <td class="tg-0lax">0.3305</td>
    <td class="tg-0lax">0.1180</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.2965</td>
    <td class="tg-0lax">0.1192</td>
    <td class="tg-0lax">0.0742</td>
    <td class="tg-0lax">0.0432</td>
    <td class="tg-0lax">0.1094</td>
    <td class="tg-0lax">0.0537</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0883</td>
    <td class="tg-0lax">0.0726</td>
    <td class="tg-0lax">0.0863</td>
    <td class="tg-0lax">0.2317</td>
    <td class="tg-0lax">0.1205</td>
    <td class="tg-0lax">0.1145</td>
    <td class="tg-0lax">0.1041</td>
    <td class="tg-0lax">0.1254</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.1473</td>
  </tr>
  <tr>
    <td class="tg-1wig">Netherlands</td>
    <td class="tg-0lax">0.0313</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0235</td>
    <td class="tg-0lax">0.1267</td>
    <td class="tg-0lax">0.6036</td>
    <td class="tg-0lax">0.0807</td>
    <td class="tg-0lax">0.1170</td>
    <td class="tg-0lax">0.1888</td>
    <td class="tg-0lax">0.1351</td>
    <td class="tg-0lax">0.0472</td>
    <td class="tg-0lax">0.0809</td>
    <td class="tg-0lax">0.0357</td>
    <td class="tg-0lax">0.0528</td>
    <td class="tg-0lax">0.0883</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0457</td>
    <td class="tg-0lax">0.0423</td>
    <td class="tg-0lax">0.0785</td>
    <td class="tg-0lax">0.0317</td>
    <td class="tg-0lax">0.0859</td>
    <td class="tg-0lax">0.0276</td>
    <td class="tg-0lax">0.1009</td>
    <td class="tg-0lax">0.0683</td>
    <td class="tg-0lax">0.0821</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.0268</td>
    <td class="tg-0lax">0.0704</td>
    <td class="tg-0lax">0.0650</td>
    <td class="tg-0lax">0.0698</td>
    <td class="tg-0lax">0.5808</td>
    <td class="tg-0lax">0.0137</td>
    <td class="tg-0lax">0.0521</td>
    <td class="tg-0lax">0.2027</td>
    <td class="tg-0lax">0.0598</td>
    <td class="tg-0lax">0.0068</td>
    <td class="tg-0lax">0.0184</td>
    <td class="tg-0lax">0.0558</td>
    <td class="tg-0lax">0.0651</td>
    <td class="tg-0lax">0.0726</td>
    <td class="tg-0lax">0.0457</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0328</td>
    <td class="tg-0lax">0.1594</td>
    <td class="tg-0lax">0.0626</td>
    <td class="tg-0lax">0.0530</td>
    <td class="tg-0lax">0.0767</td>
    <td class="tg-0lax">0.0290</td>
    <td class="tg-0lax">0.0820</td>
    <td class="tg-0lax">0.0257</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.0146</td>
    <td class="tg-0lax">0.0556</td>
    <td class="tg-0lax">0.0670</td>
    <td class="tg-0lax">0.0839</td>
    <td class="tg-0lax">0.6312</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.0420</td>
    <td class="tg-0lax">0.2205</td>
    <td class="tg-0lax">0.1113</td>
    <td class="tg-0lax">0.0459</td>
    <td class="tg-0lax">0.0456</td>
    <td class="tg-0lax">0.0724</td>
    <td class="tg-0lax">0.0831</td>
    <td class="tg-0lax">0.0863</td>
    <td class="tg-0lax">0.0423</td>
    <td class="tg-0lax">0.0328</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1745</td>
    <td class="tg-0lax">0.0625</td>
    <td class="tg-0lax">0.0453</td>
    <td class="tg-0lax">0.0908</td>
    <td class="tg-0lax">0.0611</td>
    <td class="tg-0lax">0.0931</td>
    <td class="tg-0lax">0.0314</td>
  </tr>
  <tr>
    <td class="tg-1wig">Russia</td>
    <td class="tg-0lax">0.1832</td>
    <td class="tg-0lax">0.1564</td>
    <td class="tg-0lax">0.0532</td>
    <td class="tg-0lax">0.2046</td>
    <td class="tg-0lax">0.8489</td>
    <td class="tg-0lax">0.2226</td>
    <td class="tg-0lax">0.2765</td>
    <td class="tg-0lax">0.3063</td>
    <td class="tg-0lax">0.2188</td>
    <td class="tg-0lax">0.1446</td>
    <td class="tg-0lax">0.2038</td>
    <td class="tg-0lax">0.1237</td>
    <td class="tg-0lax">0.0927</td>
    <td class="tg-0lax">0.2317</td>
    <td class="tg-0lax">0.0785</td>
    <td class="tg-0lax">0.1594</td>
    <td class="tg-0lax">0.1745</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0920</td>
    <td class="tg-0lax">0.2830</td>
    <td class="tg-0lax">0.0342</td>
    <td class="tg-0lax">0.2555</td>
    <td class="tg-0lax">0.1539</td>
    <td class="tg-0lax">0.2438</td>
  </tr>
  <tr>
    <td class="tg-1wig">Spain</td>
    <td class="tg-0lax">0.0362</td>
    <td class="tg-0lax">0.0539</td>
    <td class="tg-0lax">0.0693</td>
    <td class="tg-0lax">0.1453</td>
    <td class="tg-0lax">0.7006</td>
    <td class="tg-0lax">0.0879</td>
    <td class="tg-0lax">0.0858</td>
    <td class="tg-0lax">0.3947</td>
    <td class="tg-0lax">0.1537</td>
    <td class="tg-0lax">0.0583</td>
    <td class="tg-0lax">0.0847</td>
    <td class="tg-0lax">0.1236</td>
    <td class="tg-0lax">0.0679</td>
    <td class="tg-0lax">0.1205</td>
    <td class="tg-0lax">0.0317</td>
    <td class="tg-0lax">0.0626</td>
    <td class="tg-0lax">0.0625</td>
    <td class="tg-0lax">0.0920</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0725</td>
    <td class="tg-0lax">0.0254</td>
    <td class="tg-0lax">0.1403</td>
    <td class="tg-0lax">0.1252</td>
    <td class="tg-0lax">0.0915</td>
  </tr>
  <tr>
    <td class="tg-1wig">Sweden</td>
    <td class="tg-0lax">0.0214</td>
    <td class="tg-0lax">0.0455</td>
    <td class="tg-0lax">0.1201</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.7566</td>
    <td class="tg-0lax">0.0524</td>
    <td class="tg-0lax">0.0206</td>
    <td class="tg-0lax">0.2911</td>
    <td class="tg-0lax">0.1645</td>
    <td class="tg-0lax">0.0663</td>
    <td class="tg-0lax">0.0467</td>
    <td class="tg-0lax">0.1072</td>
    <td class="tg-0lax">0.1534</td>
    <td class="tg-0lax">0.1145</td>
    <td class="tg-0lax">0.0859</td>
    <td class="tg-0lax">0.0530</td>
    <td class="tg-0lax">0.0453</td>
    <td class="tg-0lax">0.2830</td>
    <td class="tg-0lax">0.0725</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1327</td>
    <td class="tg-0lax">0.1115</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.0723</td>
  </tr>
  <tr>
    <td class="tg-1wig">Switzerland</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0995</td>
    <td class="tg-0lax">0.0533</td>
    <td class="tg-0lax">0.1335</td>
    <td class="tg-0lax">0.6211</td>
    <td class="tg-0lax">0.1224</td>
    <td class="tg-0lax">0.1607</td>
    <td class="tg-0lax">0.3474</td>
    <td class="tg-0lax">0.1445</td>
    <td class="tg-0lax">0.0632</td>
    <td class="tg-0lax">0.1104</td>
    <td class="tg-0lax">0.1077</td>
    <td class="tg-0lax">0.0386</td>
    <td class="tg-0lax">0.1041</td>
    <td class="tg-0lax">0.0276</td>
    <td class="tg-0lax">0.0767</td>
    <td class="tg-0lax">0.0908</td>
    <td class="tg-0lax">0.0342</td>
    <td class="tg-0lax">0.0254</td>
    <td class="tg-0lax">0.1327</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1652</td>
    <td class="tg-0lax">0.0918</td>
    <td class="tg-0lax">0.1397</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turkey</td>
    <td class="tg-0lax">0.0826</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1100</td>
    <td class="tg-0lax">0.0830</td>
    <td class="tg-0lax">0.6705</td>
    <td class="tg-0lax">0.0256</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.1517</td>
    <td class="tg-0lax">0.0292</td>
    <td class="tg-0lax">0.0446</td>
    <td class="tg-0lax">0.0476</td>
    <td class="tg-0lax">0.0759</td>
    <td class="tg-0lax">0.1049</td>
    <td class="tg-0lax">0.1254</td>
    <td class="tg-0lax">0.1009</td>
    <td class="tg-0lax">0.0290</td>
    <td class="tg-0lax">0.0611</td>
    <td class="tg-0lax">0.2555</td>
    <td class="tg-0lax">0.1403</td>
    <td class="tg-0lax">0.1115</td>
    <td class="tg-0lax">0.1652</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1053</td>
    <td class="tg-0lax">0.0319</td>
  </tr>
  <tr>
    <td class="tg-1wig">United Kingdom</td>
    <td class="tg-0lax">0.1109</td>
    <td class="tg-0lax">0.1662</td>
    <td class="tg-0lax">0.0990</td>
    <td class="tg-0lax">0.0824</td>
    <td class="tg-0lax">0.4789</td>
    <td class="tg-0lax">0.1278</td>
    <td class="tg-0lax">0.1935</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.1040</td>
    <td class="tg-0lax">0.0790</td>
    <td class="tg-0lax">0.0952</td>
    <td class="tg-0lax">0.0870</td>
    <td class="tg-0lax">0.0596</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0683</td>
    <td class="tg-0lax">0.0820</td>
    <td class="tg-0lax">0.0931</td>
    <td class="tg-0lax">0.1539</td>
    <td class="tg-0lax">0.1252</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.0918</td>
    <td class="tg-0lax">0.1053</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1764</td>
  </tr>
  <tr>
    <td class="tg-1wig">United States</td>
    <td class="tg-0lax">0.0410</td>
    <td class="tg-0lax">0.1019</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.6572</td>
    <td class="tg-0lax">0.0244</td>
    <td class="tg-0lax">0.0691</td>
    <td class="tg-0lax">0.2509</td>
    <td class="tg-0lax">0.0680</td>
    <td class="tg-0lax">0.0411</td>
    <td class="tg-0lax">0.0578</td>
    <td class="tg-0lax">0.1073</td>
    <td class="tg-0lax">0.1089</td>
    <td class="tg-0lax">0.1473</td>
    <td class="tg-0lax">0.0821</td>
    <td class="tg-0lax">0.0257</td>
    <td class="tg-0lax">0.0314</td>
    <td class="tg-0lax">0.2438</td>
    <td class="tg-0lax">0.0915</td>
    <td class="tg-0lax">0.0723</td>
    <td class="tg-0lax">0.1397</td>
    <td class="tg-0lax">0.0319</td>
    <td class="tg-0lax">0.1764</td>
    <td class="tg-0lax">0.0000</td>
  </tr>
</table>

</center>

***Table 4**: Pairwise KL divergences between the analyzed countries COVID-19 cases first digit distributions, rounded to 4 digits*

The matrix above does not have a very immediate practical interpretation, so we applied a clustering algorithm to define which countries are most similar to each other -- pairs of countries with a larger KL divergence are less similar to each other than pairs of countries with a smaller KL divergence. The chosen algorithm was DBSCAN, which assign each point in the sample to *clusters* based on the minimum number of points in each *cluster* ($$mp$$) and the maximum distance that a point have to another point of the same *cluster* ($$ \varepsilon$$). <ins>Points that do not have at least $$mp$$ points within the radius of $$\varepsilon$$ are classified as *outliers* that belong to no *cluster*</ins>. A good introductory material on DBSCAN can be found [here](https://medium.com/@elutins/dbscan-what-is-it-when-to-use-it-how-to-use-it-8bd506293818).

One of the advantages of DBSCAN is the fact that the *cluster* number is automatically defined instead of being chosen by the user, making it a good tool for anomaly detection. For this exercise, we used $$mp = 3$$ and $$\varepsilon$$ as the average of the KL divergences between countries plus three sample standard deviations. The result of this clustering is easier to interpret:

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">Country</th>
    <th class="tg-7btt">Cluster</th>
  </tr>
  <tr>
    <td class="tg-c3ow">Austria</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Belgium</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Brazil</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Canada</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">China</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">France</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Germany</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">India</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Iran</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Ireland</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Israel</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Italy</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Japan</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">South Korea</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Netherlands</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Peru</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Portugal</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Russia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Spain</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Sweden</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Switzerland</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Turkey</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">United Kingdom</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">United States</td>
    <td class="tg-c3ow">1</td>
  </tr>
</table>

</center>

***Table 5**: DBSCAN clustering between the KL divergences of COVID-19 cases time series first digit distributions of the analyzed countries*


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">Country</th>
    <th class="tg-7btt">Cluster</th>
  </tr>
  <tr>
    <td class="tg-c3ow">Austria</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Belgium</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Brazil</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Canada</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">China</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">France</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Germany</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">India</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Iran</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Ireland</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Israel</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Italy</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Japan</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">South Korea</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Netherlands</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Peru</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Portugal</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Russia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Spain</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Sweden</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Switzerland</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Turkey</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">United Kingdom</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">United States</td>
    <td class="tg-c3ow">1</td>
  </tr>
</table>

***Table 6**: DBSCAN clustering between the KL divergences of COVID-19 deaths time series first digit distributions of the analyzed countries*

Again, as suggested by the visual inspection and hypothesis tests aforementioned, the results indicate that the data from China show distinct patterns from the majority of the countries most affected by the pandemic, which in turn showed similar patterns of infectivity and lethality: the algorithm returned only one *cluster*, classifying China as "*outlier*" for both data of case numbers and death numbers. Indeed, countries categorized as "*cluster* 1" seem to follow the distribution of Benford's Law better.

Although China is the place of origin of the disease, given the great divergence between its data and the data from other locations, Chinese data should be used with special caution for analyzes such as estimating COVID-19's pathogen parameters (basic reproducing number, serial interval, and case-fatality ratio, for instance), modeling the geographic dispersion of the pathogen, diagnosing the effectiveness of intervention scenarios, etc.

## Final remarks

As the pandemic affects more and more people and has an increasingly deeper impact on economic activities and social life at global level, discussing the underreporting of COVID-19 data becomes especially relevant, both for the assessment of the situation severity and for the proposal of solutions and means to overcome the crisis. Given that scholars, researchers and policy-makers around the world are dedicated to this cause, having accurate and reliable data at hand is of paramount importance, as the quality of the data directly affects the quality of all analysis derived from them. As the saying goes: **"* Garbage in, garbage out! *"**

---

**Sometimes, "coincidence" is just a way for someone to avoid the facts that he failed to explain.**

![tsuru](https://cdn.shopify.com/s/files/1/1218/4290/products/GOLD13391_46faec9e-ca27-48f7-997d-e152c18f4349_800x.JPG?v=1515022654)
