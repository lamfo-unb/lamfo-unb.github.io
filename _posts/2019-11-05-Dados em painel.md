---
layout: post
title: Dados em Painel
lang: pt
header-img: 2019-09-11 21:12:39
date: 2019-05-30 23:59:07
tags: [Causalidade, Econometria, Panel Data]
author: Mateus Hiro Nagata, Alixandro Werneck Leite
comments: true
---


# Dados em Painel

Nada é por acaso. As coisas acontecem em seu próprio contexto e por algum motivo em seu próprio tempo-espaço. A inferência estatística e a econometria são humildes ferramentas que buscam descobrir causalidade. Por exemplo, A causa B? B causa A? Os dois? Ou será que não são correlacionados? Ou será que foi uma coincidência engraçada? Para descobrir a veracidade de uma hipótese (no caso A causa B), precisamos de dados. Falaremos de um tipo específico de dados que são os dados em painel.

**O que são?**

Dados em painel são úteis e sua aplicabilidade é ubíqua. Possuem funcionalidade em diferentes áreas desde as ciências políticas, economia, finanças, políticas públicas, até casos como as relações internacionais, em campos como de ajuda humanitária. No entanto, é importante frisar que o seu uso é algo simples. Dados em painel são dados nos quais temos *algumas* observações de indivíduos $i$ e os acompanhamos *por um período de tempo* $t$.


**O que não são?**

Isso os diferencia dos dados *cross-section*, pois esse coleta as observações de um único período de tempo. Com base nisso, podemos dizer que um dado *cross-section* é um dado em painel com $t = 1$. Por sua vez, diferem também das séries de tempo, os quais se coleta variáveis para apenas um indivíduo (ou país, ou entidade, ou empresa...). Podemos dizer que séries de tempo são dados em painel com a $quantidade \quad de  \quad indivíduos = $1$.

**Vantagens dos Dados em Painel**

As vantagens dos dados em painel são de acordo com Hsiao (2005), as seguintes:

1. Melhor acurácia para inferência de parâmetros. Uma vez que dados em painel tendem a ter mais graus de liberdade e menos multi-colinearidade que as suas contrapartidas cross-section.

2.  Maior capacidade em capturar a complexidade do comportamento humano que uma série de tempo ou um cross-section:

+ Permite modelar aspectos comportamentais mais complexos. Por exemplo, investigando os efeitos de políticas públicas quanto ao antes e depois.

+ Controlar o efeito das variáveis omitidas. O que discutiremos posteiormente.

+ Desvelar relações dinâmicas entre as variáveis. Assim, podemos analisar como causas passadas podem influenciar o presente por meio de defasagens.

+ Gerar maior acurácia, uma vez que podemos comparar os comportamentos dos agentes com de outros ao longo do tempo, dado circunstâncias semelhantes.

+ Permite estimar agentes heterogêneos. Dessa forma, no lugar de invocarmos a hipótese do "agente representativo", podemos realizar análises levando em conta a individualidade e heterogeneidade das observações.


3.  Simplifica a computação e a inferência estatística:



+ Análise de séries não estacionárias. Não estacionariedade implica que a série não converge para uma média, mas que ela varia ao longo do tempo, assim a distribuição não se mantém normal. O que pode ser tratado se temos dados em painel.

+ Erros de medida. Dados de múltiplos indivíduos ou tempos diferentes permitem identificação de modelos mais precisa.




**Motivação no uso de Dados em Painel**

Analisemos os dados em painel e coloquemos em prática os modelos utilizados para tomar proveito desse formato de dado.

Imagine-se em uma posição de autoridade, decidindo o futuro do país e preocupado com a segurança da população. Criminalidade é o seu inimigo declarado e você quer controlá-lo. Como as pessoas reagem a incentivos (Mankiw, 2014), supõe-se que mudando as regras do jogo, seja reduzido as taxas de criminalidade. Todavia, cada passo dado pelas autoridades geram grandes impactos. O líder deverá proceder com cautela em suas declarações e formulações de políticas públicas, estudando vigorosamente o passado e antevendo o futuro o máximo possível.

Para atacar esse problema, usaremos como base os dados provenientes de Cornwell e Trumbull (1994) e também de Wooldridge (2016), com a intenção de descobrir o que afeta a criminalidade de uma região.

Em termos de pacotes dentro da linguagem Python e R, os principais seriam o linear models e o plm (panel linear models), respectivamente. No primeiro, pode-se criar uma análise em painel de dados a partir do pacote mencionado em associação ao numpy e ao pandas.

O pacote plm é o mais comumente usado para atividades de dados em painel na linguagem R. No exemplo desse trabalho, associar-se-á o plm ao readr e ao stargazer. O último é empregado para ter uma característica de maior adequação dos resultados das regressões (Hlavac, 2018).




```{r include = FALSE}
#Primeiramente abrimos algumas bibliotecas que serão úteis
library(plm) #biblioteca de dados em painel
library(readxl) #biblioteca de leitura de xls
library(stargazer) #biblioteca de visualização de tabelas

setwd("~/LAMFO/Crime") #Escolhemos o diretório onde colocamos os dados
crime4 <- read_excel("crime4.xls")
```
```{r include = FALSE, echo = FALSE, message = FALSE, warning = FALSE}
#nomeamos as variáveis
names(crime4) <- c("county",    "year",      "crmrte",    "prbarr",    "prbconv",   "prbpris",   "avgsen",    "polpc",    "density",   "taxpc",     "west",      "central",   "urban",     "pctmin80",  "wcon",      "wtuc",     
"wtrd",      "wfir",      "wser",      "wmfg",      "wfed",      "wsta",      "wloc",      "mix",      "pctymle",   "d82",       "d83",       "d84",       "d85",       "d86",       "d87",       "lcrmrte",  "lprbarr",   "lprbconv",  "lprbpris",  "lavgsen",   "lpolpc",    "ldensity",  "ltaxpc",    "lwcon",    
"lwtuc",     "lwtrd",     "lwfir",     "lwser",     "lwmfg",     "lwfed",     "lwsta",     "lwloc",    "lmix",      "lpctymle",  "lpctmin",   "clcrmrte",  "clprbarr",  "clprbcon",  "clprbpri",  "clavgsen",  "clpolpc",   "cltaxpc",   "clmix" )
```
Exploraremos as mesmas variáveis usadas pelos autores. No caso, seria: **lcrmrte** = log do crimes por pessoa, **lprbarr**= log da probabilidade de ser preso,  **lprbcon**= log da probabilidade de condenação, **lprbpri**= log da probabilidade de prisão, **lavgsen**= log da duração da sentença média em dias, **lpolpc**= log de policiais per capita, **county** = município, **year** = ano. Usamos as variáveis em log para estimar efeitos em percentuais

Dessa forma, nosso objetivo é explicar a criminalidade (na verdade, estaremos estimando o log do clcrmrte, que é lclcrmrte) de diferentes formas. A variável a ser explicada é a dependente ($Y$) e as independentes a serem usadas para explicar, ou seja, que supomos ser correlacionadas se chamam variáves explicativas ($X_1$, ...,$X_k$).

# Regressão Agrupoada (Pooling Regression)

O modelo mais simples que se pode usar em dados em painel é a regressão *pooling*. Basicamente, consiste no empilhamento dos dados e a estimação do MQO. Ademais, essa é a regressão que ignora a colinearidade temporal e considera cada observação de tempo e país como independentes para derivar uma relação linear entre as variáveis explicativas $X_1, \dots, X_k$ e a variável dependente $Y$. Como temos as observações para tempos diferentes ($t$) e indivíduos diferentes ($i$), adicionamos os respectivos subscriptos nas variáveis explicativas e na variável dependente.

$$ Y_{it} = \beta_0 + \beta_1X_{1it} + ... \beta_kX_{kit}   + u_i      $$
O objetivo é estimar os valores dos $\beta_1, ...,\beta_k$, os quais representam o efeito das variáveis $X_1,...,X_k$. Como esperamos uma aleatoriedade dessa relação, colocamos um termo de erro $u_i$ que indica o desvio da nossa observação para o nosso modelo predito pelos betas estimados e as variáveis dependentes.

Então, de forma simples, o modelo pooled consiste em empilhar os dados e estimá-los por meio de Mínimos Quadrados Ordinários. A suposição chave é a de que não existem atributos únicos a cada indivíduo (ou, pra nós, para cada município) e nem efeitos que mudam ao longo do tempo.  O método dos mínimos quadrados ordinários é uma forma de estimar os parâmetros de cada $\beta$ de forma que o erro seja o mínimo possível de acordo com algum critério. Nesse caso, o erro quadrado seja o mínimo.

Em suma, é computacionalmente estimado da seguinte forma.

```{r results = 'asis'}
library(plm) #Library para regressão em dados de painel

p1 <- plm(lcrmrte ~ lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, model = "pooling", index = c("county", "year"), data = crime4) #Função para especificar a regressão
#Note que é necessário indicar no index um vetor de variáveis de identidade e de tempo.

stargazer::stargazer(p1,  header=FALSE, type='html') #Comando para revelar a tabela
```

A interpretação é a seguinte: o aumento de 1% na probabilidade de ser pego (lprbarr) diminui 0.721% a criminalidade (lcrmrte), mantendo todos os outros fatores constantes. O argumento é análogo para as outras variáveis que são por exemplo, um aumento de 1% na probabilidade  de sentença de prisão (lprbpris) aumenta a criminalidade em 0.389%.

Costumamos dar crédito ao efeito dessas variáveis se seu p-valor é menor que 0.05, ou equivalentemente, se há 2 asteriscos no número. Quanto mais asteriscos, maior significância, no caso do efeito da probabilidade de prisão (lpolpc) há 3 asteríscos, então seu p-valor é menor que 0.01. Isso quer dizer que em no máximo 1\% das vezes nós estaríamos errado em dizer que *mais policiais implicam em mais criminalidade*.

Cremos ter uma anomalia na análise.

Como pode ser que maior chance de cadeia induz as pessoas a cometer mais crimes? É nesse momento que percebemos que em econometria, não podemos só contar com os números e as técnicas, mas precisamos conhecer o caso que estamos analizando.

Uma hipótese é de que municípios que sofrem com mais criminalidade podem ter equipes mais bem preparadas para prender os criminosos. Veja que a causalidade está do lado contrário!

Voltando a sua posição de realizador de políticas públicas. No que acreditar? Nos números ou na sua intuição? Você deve decidir se devemos melhorar os nossos mecanismos de aprisionamento ou não. Vemos que é necessário conhecer várias técnicas e discernir a adequada para cada momento.


# Efeitos Fixos (Modelo Within)

Uma preocupação que existe nesse modelo é o viés de variável omitida. Quiçá exista variáveis que são necessárias para explicar o modelo, mas não especificamos na equação. Caso tais variáveis sejam únicas a cada indivíduo/município e não mudem ao longo do tempo, podemos eliminá-lo com a seguinte técnica. 

No nosso caso, podemos estar preocupados com o fato de características geográficas, culturais,como proximidade de áreas metropolitanas, poder local de organizações criminosas, câmeras de segurança. No final, você só quer não quer se preocupar com essas coisas e só quer responder uma pergunta: devo ou não melhorar o nosso sistema?

A boa notícia é que podemos isolar esses efeitos de nossa equação usando o modelo de efeitos fixos ou também conhecido como *within estimator*. Comecemos adicionando ao modelo variáveis que explicam o valor de $Y$ variáveis dummies que identificam cada indivíduo. Nós o chamaremos de $\alpha_i$. Cada um dos nossos $\alpha_i$ funciona como um intercepto para cada observação, então não é necessário incluí-lo.

$$ Y_{i t}= \beta_{1} X_{1i t} + ... + \beta_kX_{kit} +\alpha_{i}+u_{i t} $$



O segundo passo é formar uma regressão com base na média da relação entre as variáveis explicativas e a variável dependente ao longo do tempo. Veja que obviamente as dummies de identificação  $\alpha_i$ permanecem inalteradas (o qual justifica chamá-lo de efeito fixo).

$$     \overline{Y}_i = \beta_1\overline{X}_{1i} + ... + \beta_k\overline{X}_{ki} + \alpha_i + \overline{u}_i $$

Então a mágica acontece. Subtraímos a média da variável explicativa ($\overline{Y}_i$) para cada observação. Faremos o mesmo nas variáveis dependentes.
$$ Y_{i} - \overline{Y}_{i} = \beta_1(X_{1it}-\overline{X}_{1i}) + ... + \beta_k(X_{kit} - \overline{X}_{ki}) + (u_{it}-\overline{u_i}) $$

Como os efeitos fixos sumiram, nós rearranjamos a equação acima para uma notação mais concisa:
$$     \tilde{Y}_i = \beta_1\tilde{X}_{1i} + ... + \beta_k\tilde{X}_{ki} +  \tilde{u}_i  $$

Na prática, a biblioteca faz isso diretamente para nós, bastando especificar o índice de identidade (county) e de tempo (year) na especificação. No nosso caso, queremos estimar os seguintes betas:

$$     \tilde{lcrmrte}_i = \beta_1\tilde{lprbarr}_{i} + \beta_2\tilde{lprbconv}_{i} +\beta_3\tilde{lprbpri}_{i} +\beta_4\tilde{lavgsen}_{i} +\beta_5\tilde{lpolpc}_{i} +  \tilde{u}_i  $$

Computacionalmente, o pacote plm faz isso para nós. 

```{r p2,  results='asis'}
library(plm) 

p2 <- plm(lcrmrte ~ lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, model = "within", index = c("county", "year"),data = crime4) # Especificação do modelo de efeito fixo

stargazer::stargazer(p2,  header=FALSE, type='html') # Gerar a tabela
```


A interpretação é análoga. Variação percentual na variável explicativa afeta a variável dependente em tal grau positivamente ou negativamente. Todavia, podemos afirmar menos ruído devido às características peculiares dos municípios e, assim, concentrarmos apenas naquelas que estamos nos preocupando e mudam ao longo do tempo.



# Efeitos Aleatórios

As vantagens desse próximo modelo são que os betas estimados mostram-se mais eficientes, ou seja, que há menor variância e portanto, mais certeza de que o estimador está próximo do valor real (dado que não é viesado). Todavia, temos que supor que os as covariâncias entre as dummies de identidade e as variáveis explicativas deve ser zero. Ou seja, $Cov(X_{itj}, \alpha_i) = 0,  t=1,2, ..., T; j = 1,2, ..., k.$ Caso contrário, existe grande endogeneidade nas nossas variáveis e, nesse caso, devemos usar os efeitos fixos.

Quanto a forma funcional supomos a mesma que os efeitos fixos, mas com o intercepto $\beta_0$.

$$ Y_{i t}= \beta_0 +   \beta_{1} X_{it1} + ... + \beta_kX_{itk} +\alpha_{i}+u_{i t} $$


Em seguida, vamos colocar os efeitos fixos $\alpha_i$ no termo de erro. O novo termo tornando-se $v_{it}$ = $\alpha_i$ +  $u_{it}$.


$$Y_{i t}=\beta_{0}+\beta_{1} X_{i t 1}+\ldots+\beta_{k} X_{i t k}+v_{i t}$$

Pela suposição acima, temos que  $v_{it}$ é correlacionado com $v_{is}$ pra quaisquer períodos de tempo $t$ e $s$, ou seja, existe uma dependência temporal do valor do erro no passado e o erro atual por meio dessa expressão, o qual $\sigma_{a}^{2}$ é a variância dos efeitos fixos $a_i$ e $\sigma_{u}^{2}$ é a variância do erro $u_{it}$.


$$
\operatorname{Corr}\left(v_{i t}, v_{i s}\right)=\sigma_{a}^{2} /\left(\sigma_{a}^{2}+\sigma_{u}^{2}\right), \quad t \neq s
$$

O que é um problema, pois necessitamos que os erros do momento sejam não correlacionados com o de tempos diferentes. Para eliminarmos essa correlação, definimos um coeficiente teta tal que está entre [0,1]:


$$
\theta=1-\left[\sigma_{u}^{2} /\left(\sigma_{u}^{2}+T \sigma_{a}^{2}\right)\right]^{1 / 2}
$$


Multiplicando esse por todos os termos da equação
$$
\begin{aligned}\theta \bar{Y}_{i}=& \beta_{0}\theta+\beta_{1}\theta \bar{X}_{i 1}+\ldots+\beta_{k}\theta \bar{x}_{i k}+\theta \bar{v}_{i}\end{aligned}
$$

E então fazemos um procedimento semelhante ao dos efeitos fixos, mas subtraindo a média penalizada pela constante teta. Finalmente temos o modelo de efeitos aleatórios o qual é denotado abaixo.

$$
\begin{aligned} Y_{i t}-\theta \bar{Y}_{i}=& \beta_{0}(1-\theta)+\beta_{1}\left(X_{i t 1}-\theta \bar{X}_{i 1}\right)+\ldots \\ &+\beta_{k}\left(X_{i k}-\theta \bar{X}_{i k}\right)+\left(v_{i t}-\theta \bar{v}_{i}\right) \end{aligned}
$$

Interessante notar que nos casos extremos que $\theta = 0$, a equação torna-se uma regressão pooled (erros de anos diferentes estão totalmente correlacionados) e torna-se um modelo de efeitos fixos quando $\theta = 1$ (erros de anos diferentes não são correlacionados). Vamos reescrever a situação acima usando o suscrito.

$$
\begin{aligned}\ddot{Y}_{i}=& \beta_{0}+\beta_{1} \ddot{X}_{i 1}+\ldots+\beta_{k} \ddot{X}_{i k}+ \ddot{v}_{i}\end{aligned}
$$
E assim a equação que queremos estimar se torna 

$$     \ddot{lcrmrte}_i = \beta_1\ddot{lprbarr}_{i} + \beta_2\ddot{lprbconv}_{i} +\beta_3\ddot{lprbpri}_{i} +\beta_4\ddot{lavgsen}_{i} +\beta_5\ddot{lpolpc}_{i} +  \ddot{v}_i  $$

Estimamos o modelo computacionalmente:

```{r p3,  results='asis'}
library(plm) 

p3 <- plm(lcrmrte ~  lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, index = c("county", "year"), model = "random",
        data = crime4)
        
stargazer::stargazer(p3,  header=FALSE, type='html')
```

As interpretações dos interceptos é idêntica. 


# Pooled VS Fixed VS Random

Podemos comparar o resultado das 3 estimações abaixo. Neste caso, muitas estimações tiveram magnitudes semelhantes, mas modelos usados equivocadamente podem nos dar conclusões errôneas. Vale lembrar que uma forma de ver a robustez de algum coeficiente é ver se sua significância, magnitude e sinal são parecidos em modelos diferentes, mas não é uma condição *sine qua non*. Muitas vezes certos modelos não são apropriados para dada situação.

```{r  results='asis'}
library(plm) 

p1 <- plm(lcrmrte ~ lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, model = "pooling", index = c("county", "year"), data = crime4) #
p2 <- plm(lcrmrte ~ lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, model = "within", index = c("county", "year"),data = crime4)
p3 <- plm(lcrmrte ~  lprbarr + lprbconv + lprbpris + lavgsen + lpolpc, index = c("county", "year"), model = "random",
        data = crime4)

stargazer::stargazer(p1, p2, p3, header = FALSE,  type = 'html')
```

Em suma, cada um dos modelos é adequado para alguma situação diferente. A grande questão é a forma como pensamos nossos dados. Devemos explorar o jogo de causalidades tendo em vista as limitações de cada modelo. 

Caso nossa teoria não tenha nenhuma objeção em acreditar que há variável omitida, podemos usar a regressão agrupada sem problemas. Caso acreditemos que há variável omitida, existe um teste que nos auxilia para a escolha entre efeitos fixos e efeitos aleatórios que é o teste de Hausman.  

Em essência, o teste de (especificação de) Hausman testa se a covariância entre as dummies de identificação e as variáveis explicativas são correlacionadas ou não. $Cov(\alpha_i, X_{it}) = 0$ ou não.

Em caso de covariância zero, que é a nossa hipótese nula, os betas de efeitos fixos e efeitos aleatórios são consistentes, ou seja, quando as observações são suficientemente numerosas, os estimadores são suficientemente próximos dos valores reais e os betas de efeitos aleatórios são mais eficientes que os de efeitos fixos.  Em caso de covariância diferente de zero, somente os coeficientes dos efeitos fixos são consistentes. 

Resumidamente, se o teste de Hausman nos der $Cov(\alpha_i, X_{it}) = 0$, efeitos aleatórios. Caso contrário, efeitos fixos.

Computacionalmente:

```{r}
phtest(p2,p3)
```

Como o p-valor é menor do que 0.05, nós rejeitamos a hipótese nula. Dessa forma, dizemos que o modelo de efeitos fixos é o adequado para o modelo.

Sua conclusão é que nós devemos sim aumentar o policiamento do nosso município, que técnicas tem que ser combinadas com o conhecimento da situação e que causalidade é muito mais difícil e muito mais divertido do que você imaginou. Mais uma vez, o dia foi salvo graças à você. 


# Bibliografia

+ Hlavac, Marek. Stargazer:
beautiful LATEX, HTML and ASCII tables from R statistical output.(2018). Available in: https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf.

+ Hsiao, C. (2005). Why Panel Data ? (September).

+ Cornwell, C., & Trumbull, W. N. (1994). Estimating the economic model of crime with panel data. The Review of economics and Statistics, 360-366.

+ Mankiw, N. G. (2014). Principles of economics. Cengage Learning.

+ Wooldridge, J. M. (2016). Introductory econometrics: A modern approach. Nelson Education.

