---
layout: post
title: Introdução à Análise de Ações com R
date: 2017-07-18
tags: [acoes, r, investimentos, portfolio]
author: Gustavo Monteiro
lang: pt
header-img: img/0021.png
comments: true
---

A análise de ações e investimentos é um tema que pode ser muito bem explorado na programação. Isso inclui a linguagem R, que já possui vasta literatura, pacotes e funções desenvolvidas no tema. Neste post faremos uma breve introdução ao assunto utilizando o pacote ```quantmod```. e ```ggplot2```.

A ação analisada será a da Petrobras, com dados extraídos do site [Yahoo Finance](https://finance.yahoo.com/) por meio do pacote [quantmod](https://cran.r-project.org/web/packages/quantmod/index.html) que é bastante utilizado para a modelagem quantitativa de dados financeiros. Além disso, o pacote [ggplot2](https://cran.r-project.org/web/packages/ggplot2/index.html) será utilizado para a visualização dos dados.

### Preparando o ambiente

A preparação do ambiente é bem simples, mesmo para quem não está acostumado ou que esteja iniciando. Primeiramente, instalamos e carregamos os pacotes necessários.

```{r}
rm(list=ls())
install.packages("quantmod")
install.packages("ggplot2")
library(quantmod)
library(ggplot2)
```
Acho importante ressaltar que instalamos os pacotes apenas uma vez. Nas próximas vezes que executarmos o código, é necessário apenas carregar os pacotes por meio do comando `library`.

Depois de termos os pacotes instalados e carregados, vamos baixar a série de preços da ação e tratar os dados para que fiquem da melhor forma possível para a realização das análises. Isso é feito por meio do comando `getSymbols`. 

```{r}
pbr <- getSymbols("PBR", src = "yahoo", from = "2013-01-01", to = "2017-06-01", auto.assign = FALSE)
```
Vamos por partes!

O primeiro argumento `PBR` é o símbolo do ativo que queremos analisar no Yahoo Finance. Você pode procurar pelo símbolo de outra ação ou índice que desejar analisar clicando [aqui](https://finance.yahoo.com/lookup/). 

O argumento `src = "yahoo"` indica a fonte dos dados. Também podemos utilizar outras fontes como o [Google Finance](https://www.google.com/finance), [FRED](http://research.stlouisfed.org/fred2/), [Oanda](http://www.oanda.com/), bancos de dados locais, CSVs e vários outros.

O terceiro e quarto argumentos indicam o período no qual os dados serão extraídos, com a data no formato "aaaa-mm-dd".

Por último, o argumento `auto.assign = FALSE` nos permite nomear o dataset com o nome que quisermos. Caso seja `TRUE`, o nome será automaticamente o símbolo que estamos buscando, ou seja, o primeiro argumento.

### Visualização de Preços

Agora vamos dar uma breve olhada nos dados, apenas para conhecê-los melhor.

```{r}
head(pbr)
tail(pbr)
summary(pbr)
str(pbr)
```
```{r}
##            PBR.Open PBR.High PBR.Low PBR.Close PBR.Volume PBR.Adjusted
## 2017-05-23     8.76     8.91    8.74      8.83   22071200         8.83
## 2017-05-24     8.95     9.20    8.88      9.08   25863100         9.08
## 2017-05-25     9.07     9.25    8.81      8.89   30557400         8.89
## 2017-05-26     8.74     9.03    8.72      8.95   22845300         8.95
## 2017-05-30     8.85     8.90    8.69      8.70   21082600         8.70
## 2017-05-31     8.67     8.76    8.44      8.48   23066100         8.48
```
```{r}
##      Index               PBR.Open        PBR.High        PBR.Low     
##  Min.   :2013-01-02   Min.   : 2.88   Min.   : 2.97   Min.   : 2.71  
##  1st Qu.:2014-02-08   1st Qu.: 7.05   1st Qu.: 7.20   1st Qu.: 6.82  
##  Median :2015-03-18   Median :10.25   Median :10.40   Median :10.08  
##  Mean   :2015-03-17   Mean   :10.67   Mean   :10.86   Mean   :10.46  
##  3rd Qu.:2016-04-23   3rd Qu.:14.42   3rd Qu.:14.60   3rd Qu.:14.15  
##  Max.   :2017-05-31   Max.   :20.83   Max.   :20.94   Max.   :19.96  
##    PBR.Close        PBR.Volume         PBR.Adjusted   
##  Min.   : 2.900   Min.   :  6046400   Min.   : 2.900  
##  1st Qu.: 7.005   1st Qu.: 17193900   1st Qu.: 7.005  
##  Median :10.210   Median : 24066300   Median :10.230  
##  Mean   :10.657   Mean   : 27527831   Mean   :10.814  
##  3rd Qu.:14.330   3rd Qu.: 33655150   3rd Qu.:14.615  
##  Max.   :20.650   Max.   :164885500   Max.   :20.650
```
```{r}
## An 'xts' object on 2013-01-02/2017-05-31 containing:
##   Data: num [1:1111, 1:6] 18.9 18.7 19.2 19.2 18.9 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : NULL
##   ..$ : chr [1:6] "PBR.Open" "PBR.High" "PBR.Low" "PBR.Close" ...
##   Indexed by objects of class: [Date] TZ: UTC
##   xts Attributes:  
## List of 2
##  $ src    : chr "yahoo"
##  $ updated: POSIXct[1:1], format: "2017-07-16 20:52:31"
```
Com isso, vemos que são 6 colunas com: preço de abertura, preços máximo e mínimo do dia, preço de fechamento, volume de transações e preço ajustado. Pelo comando `summary` verificamos as estatísticas descritivas de cada série de preços e volume. Já o comando `str` fornece a estrutura do objeto. Neste caso, é um objeto [xts](https://cran.r-project.org/web/packages/xts/vignettes/xts.pdf), uma série temporal.

Vamos agora plotar os preços diários, utilizando a coluna de Preço Ajustado, visto que ela incorpora eventos como [splits](https://pt.wikipedia.org/wiki/Desdobramento_de_a%C3%A7%C3%B5es) e distribuição de dividendos, que podem afetar a série.

```{r}
ggplot(pbr, aes(x = index(pbr), y = pbr[,6])) + geom_line(color = "darkblue") 
+ ggtitle("Série de preços da Petrobras") 
+ xlab("Data") + ylab("Preço ($)") + theme(plot.title = element_text(hjust = 0.5)) + 
scale_x_date(date_labels = "%b %y", date_breaks = "6 months")
```
<img src="/img/acoes1/image1.png">

Criamos esse gráfico usando o comando `ggplot`. Primeiro utilizamos o objeto `pbr` como a série a ser plotada. Depois indicamos quais elementos serão os eixos: `index(pbr)`, a data no eixo x, e a coluna de preço ajustado, `pbr[,6]`, no eixo y. Em seguida, adicionamos o elemento a ser plotado, no caso, uma linha azul: `geom_line(color = "darkblue")`. 

Depois, acrescentamos o título e nomes dos eixos, com os comandos `ggtitle("Série de preços da Petrobras")`, `xlab("Data")`, `ylab("Preço ($)")`. Por padrão, o título do gráfico fica alinhado à esquerda. Para centralizá-lo, utilizamos `theme(plot.title = element_text(hjust = 0.5))`.

Por último, para deixar o eixo temporal com mais informações, colocamos o *tick* de data a cada seis meses e na forma de `mmm aa` utilizando `scale_x_date(date_labels = "%b %y", date_breaks = "6 months")`.

Vemos que ação estava em tendência de queda entre jan/12 a mar/16, grosso modo. É notável também tendência de aumento de mar/16 a out/16, com posterior estabilização. Para verificarmos melhor isso, podemos traçar uma linha de tendência no gráfico:

```{r}
ggplot(pbr, aes(x = index(pbr), y = pbr[,6])) + geom_line(color = "darkblue") 
+ ggtitle("Série de preços da Petrobras")
+ xlab("Data") + ylab("Preço ($)") 
+ theme(plot.title = element_text(hjust = 0.5), panel.border = element_blank())
+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months") 
+ geom_smooth(method = "loess", se = FALSE, color = "black", size = 0.3)
```
<img src="/img/acoes1/image2.png">

Para adicionar essa linha de tendência, acrescentamos o comando `geom_smooth(method = lm, se = FALSE, color = "black", size = 0.3)`. O `method = "loess"` diz que a tendência utilizará o modelo de [regressão local](https://en.wikipedia.org/wiki/Local_regression) (digite `?loess` no console para mais informações). Outros métodos estão disponíveis [aqui](http://ggplot2.tidyverse.org/reference/geom_smooth.html).

### Retornos!

Vimos como o preço da ação tem variado ao longo do tempo. Vamos agora verificar como se comportou o retorno da ação no mesmo período. Para isso, precisamos primeiro criar um novo objeto com os retornos calculados, utilizando também a coluna de preço ajustado:

```{r}
pbr_ret <- diff(log(pbr[,6]))
pbr_ret <- pbr_ret[-1,]
```

O que fizemos aqui foi utilizar as propriedades do logaritmo para calcular o retorno logarítmico da ação. Ou seja, fizemos:

$$r_{t} = ln(1+R_t) = ln\bigg(\frac{P_t}{P_{t-1}}\bigg) = ln(P_{t})-ln(P_{t-1}) \approx R_{t} $$

O comando `diff` calcula a diferença de todos os valores de algum vetor ou elemento. Com isso, apenas aplicamos a diferença aos logaritmos naturais dos preços da ação.

Vamos agora verificar algumas estatísticas básicas dos retornos:

```{r}
summary(pbr_ret)

##      Index             PBR.Adjusted       
##  Min.   :2013-01-03   Min.   :-0.1852413  
##  1st Qu.:2014-02-10   1st Qu.:-0.0209915  
##  Median :2015-03-18   Median :-0.0005999  
##  Mean   :2015-03-18   Mean   :-0.0007548  
##  3rd Qu.:2016-04-24   3rd Qu.: 0.0177905  
##  Max.   :2017-05-31   Max.   : 0.1555103
```
```{r}
sd(pbr_ret)

## [1] 0.03713089
```

Ou seja, em média a ação não tem tido um bom desempenho. Podemos agora plotar um gráfico dos retornos e ver como eles desempenharam ao longo do tempo:

```{r}
ggplot(pbr_ret, aes(x = index(pbr_ret), y = pbr_ret)) + geom_line(color = "deepskyblue4") 
+ ggtitle("Série de retornos da Petrobras") + xlab("Data") + ylab("Retorno") 
+ theme(plot.title = element_text(hjust = 0.5)) 
+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months")
```
<img src="/img/acoes1/image3.png">

Para plotar este último gráfico, utilizamos os mesmos parâmetros do gráfico de preços, alterando apenas a cor da linha.

Dando uma breve analisada no gráfico, é possível ver que o menor retorno da série ocorreu por volta de maio. Mais precisamente, no dia 18 de maio, um dia após a divulgação de matéria com trechos da gravação do presidente Michel Temer dando aval para a compra de silêncio de políticos. Esse fato impactou fortemente o mercado de ações, especialmente de empresas brasileiras e públicas, como a Petrobras.

Vamos fazer agora uma breve checagem dos retornos da ação em 2017:

```{r}
pbr_ret17 <- subset(pbr_ret, index(pbr_ret) > "2017-01-01")

ggplot(pbr_ret17, aes(x = index(pbr_ret17), y = pbr_ret17)) + geom_line(color = "deepskyblue4") 
+ ggtitle("Série de retornos da Petrobras em 2017") + xlab("Data") + ylab("Retorno") 
+ theme(plot.title = element_text(hjust = 0.5)) 
+ scale_x_date(date_labels = "%b %y", date_breaks = "1 months")

<img src="/img/acoes1/image4.png">

summary(pbr_ret17)

sd(pbr_ret17)
```

```{r}
##      Index             PBR.Adjusted      
##  Min.   :2017-01-03   Min.   :-0.185241  
##  1st Qu.:2017-02-08   1st Qu.:-0.014350  
##  Median :2017-03-17   Median :-0.002689  
##  Mean   :2017-03-17   Mean   :-0.001707  
##  3rd Qu.:2017-04-24   3rd Qu.: 0.014977  
##  Max.   :2017-05-31   Max.   : 0.068795

## [1] 0.03089105
```

A função `subset` é utilizada para dividir a base de acordo com algum critério. Neste caso, utilizamos o período e separamos em um objeto todos os retornos de 2017.

As estatísticas descritivas continuam indicando um retorno médio negativo em 2017. Porém, em 2017 o desvio padrão tem sido menor, o que indica menor risco. Além disso, esse retorno negativo na média se dá em maioria pela grande queda do dia 18.

### Conclusão

Bom, isso é tudo para esse tutorial, pessoal. Em posts futuros pretendo abordar análises mais elaboradas e também análise de portfólio e risco. Espero que tenham gostado e até a próxima!

