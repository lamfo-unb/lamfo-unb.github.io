---
layout: post
title: "Fatos Estilizados em Finanças"
lang: pt
header-img: img/manipulacao_data.table/img_data.table.png
date: 2020-11-07 23:59:07
tags: [Finanças, autocorrelações, distribuição de retornos]
author: Marcius Lima, Eduardo Rubik, Rafael Morais
comments: true
---

# Fatos Estilizados em Finanças

Padrões que ocorrem recorrentemente no mercado financeiro são de suma importância para praticantes e estudiosos da área. Eles possibilitam criar e ajustar modelos que ajudarão no melhor entendimento dos movimentos do mercado. Mas quais são alguns desses padrões e como são tratados no campo das Finanças?

Ao observar uma série de elementos em diferentes contextos e períodos, em determinados ocasiões é possível fazer formulações teóricas de fenômenos ou características observadas empiricamente. A essas formulações, dá-se o nome de fatos estilizados.

Em Finanças, os fatos estilizados referem-se a características de ativos financeiros que são consistentes tanto entre diferentes mercados quanto entre diferentes períodos, e que, devido a isso, são generalizadas. Essas generalizações são importantes, visto que norteiam o processo de modelagem estatística dos retornos.

Um ponto importante a se observar é que todas as características referem-se aos retornos dos ativos, e não seus preços. Isso porque uma característica importante nesse processo é a *estacionariedade*, ou seja, a "consistência" do objeto analisado ao longo do tempo. Os preços são muito afetados por fatores como a conjuntura econômica e o estado da arte tecnológico, por exemplo. Já os retornos costumam ser menos afetados, e portanto, manter uma constância maior.

Nesse texto, abordaremos os seguintes fatos estilizados:

- Insignificância de autocorrelações
- Caudas grossas
- Assimetria de ganhos e perdas
- Gaussianidade agregacional
- Agrupamento de volatilidade
- Efeito alavancagem
- Correlação Volume X Volatilidade

## Insignificância de Autocorrelações
Na estatística, o conceito de autocorrelação diz respeito a correlação entre uma série temporal e sua versão com lag em diferentes intervalos de tempo. Ou seja, ao medir a correlação entre os retorno de uma série no momento *t* e no momento *t-1* para vários valores de *t*, o resultado é baixíssimo, ao ponto de ser insignificante.

Talvez uma forma mais intuitiva de entender esse conceito seja pelo jargão "retornos passados não garantem retornos futuros", ou seja, que o retorno de determinada série em determinado momento é insignificantemente influenciado pelo anterior.

### Insignificância de autocorrelação na prática: *Momentum*

![Lo e MacKinlay, 1990](https://i.imgur.com/tsHJggA.png "Lo e MacKinlay, 1990")
###### Lo e MacKinley, 1990

![Lo e MacKinlay, 1990(2)](https://i.imgur.com/AjQTBPD.png "Lo e MacKinley, 1990")
###### Lo e MacKinley, 1990



## Queda cadenciada de volatilidade das autocorrelações de retornos absolutos ou quadrados

Importante observar que a insignificância das autocorrelações dos retornos é uma propriedade relacionadas ao valor dos retornos. Porém, ao observar os retornos absolutos ou quadrados há uma autocorrelação que decai lentamente.

Ou seja, o retorno em determinado período tem pouca correlação com o valor no período seguinte, mas a "dimensão" desses valores possuem correlações, que vai diminuindo a medida que a diferença temporal aumenta.

![](https://i.imgur.com/fuxNfUd.png)
###### Taylor, 2005



## Caudas Grossas

Quanto a distribuição de probabilidade dos retornos, é possível observar que eles não apresentam uma curva normal, mas sim uma curva com maior concentração nas caudas da distribuição. Na prática, mais retornos apresentam valores menores e menos retornos apresentam valores maiores #(muita gente tem baixos retornos e pouca gente tem altos retornos).

### Caudas Grossas na prática

![Silva et al, 2019](https://i.imgur.com/5X8DuJn.png)
###### Silva *et al.*, 2019


## Assimetria de ganhos e perdas

Outra característica associada a distribuição dos retornos é a assimetria de ganhos e perdas, representada graficamente por uma das caudas possuir uma maior "inclinação" que a outra. Essa propriedade diz respeito aos movimentos de queda (fundos) serem maiores que os de subida (topos).  Utilizando um outro jargão, é como se os retornos "subissem de escada e descessem de elevador".

## Gaussianidade agregacional

A distruibuição dos retornos são associadas, porém, a um intervalo de tempo determinado. A gaussianidade agregacional diz respeito a distribuição ir se aproximando de uma normal (também chamada de distribuição Gaussiana, daí o nome) a medida que o horizonte de tempo é expandido.

Visto que o aumento no intervalo de tempo está associado ao tamanho da amostra analisada, o Teorema Central do Limite garante essa propriedade.

## Agrupamento de volatilidade

![Nghi, 2012](https://i.imgur.com/UnBqz8h.png)
###### Nghi, 2012


## Efeito Alavancagem
O Efeito Alavancagem foi explicado pela primeira vez em 1976 (Black, 1976) e atesta que existe uma correlação entre a alavancagem dos ativos da empresa e a volatilidade do preço de suas ações. Empresas com alavancagem financeira e operacional tendem a ficar mais alavancadas com a queda do valor da firma, o que aumenta a volatilidade dos retornos de suas ações.

## Correlação Volume X Volatilidade

De acordo com Cont(2001), o volume é correlacionado com todas as medidas de volatilidade. Importante notar que a intensidade da correlação positiva entra os dois fatores dificulta a separação dos efeitos de cada um sobre os ativos (Bianco e Renò,2006).

![](https://i.imgur.com/SgE96Vr.png)
http://businessforecastblog.com



## Como a indústria de gestão de recursos pode utilizar Machine Learning?

A previsão de preços dos ativos talvez seja a aplicação mais popular. No entanto, existem diversas aplicações tão ou mais importantes como:

- construção de portfólio;
- detecção de outliers;
- análise de sentimento;
- market making;
- tamanho de "apostas";
- outros.



> "Nós precisamos de aprendizado de máquinas para melhorar teorias financeiras, e precisamos de teorias financeiras para restringir a propensão que o aprendizado de máquinas tem de *sobreajustar*."
> Marcos López de Prado


## Referências
Empirical properties of asset returns: stylized facts and statistical issuesin: Quantitative Finance, Vol 1, No 2, (March 2001) 223-236.

Lewellen, Jonathan. “Momentum and Autocorrelation in Stock Returns.”The Review of Financial Studies, vol.  15, no.  2, 2002, pp.  533–563.JSTOR, www.jstor.org/stable/2696788. Accessed 28 Oct. 2020.

Lo, Andrew MacKinlay, A. (1990). When Are Contrarian Profits Due ToStock Market Overreaction?. Review of Financial Studies. 3. 175-205.10.1093/rfs/3.2.175.

MALMSTEN, Hans; TERÄSVIRTA, Timo.  Stylized Facts of FinancialTime Series and Three Popular Models of Volatility. European Journal ofPure and Applied Mathematics, [S.l.], v. 3, n. 3, p. 443-477, may 2010.ISSN 1307-5543.

Girard, Eric Biswas, Rita. (2007). Trading Volume and Market Volatility:Developed versus Emerging Stock Markets.  The Financial Review.  42.429-459.

Sewell, Martin. Characterization of Financial Time Series. Research Note.January, 2011.

Black, Fisher. (1976). Studies in Stock Price Volatility Changes. Proceedings of The American Statistical Association, Business and Economic Statistics Section. 177.

Bianco, Simone & Renò, Roberto. (2006). Dynamics of intraday serial correlation in the Italian futures market. Journal of Futures Markets. 26. 61 - 84. 10.1002/fut.20182.
