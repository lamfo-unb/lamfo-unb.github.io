# Modelo Tweedie Poisson Compound para dados de sinistros de veículos

 Situações onde se deseja verificar/quantificar a influência de certos fatores em uma variável de interesse são comuns em problemas estatísticos. 

Para as seguradoras, a análise e quantificação dos riscos de sinistros é peça fundamental para o ajuste de modelos de precificação de tarifas. Desta forma, é necessária saber quais fatores apresentam características com riscos diferentes dentre suas classes. Por exemplo, se uma certo tipo de automóvel tende a gerar mais custos de pagamento do que outros, etc. 

Assim, espera-se que fatores em que não apresentam diferenças de risco entre si, não possuam diferença no preço do seguro. Em contra partida, fatores que possuam diferençsa de risco, apresentam diferentes valores de seguros para suas 'classes'.

Aqui, iremos analisar um banco de dados de sinistros de automovéis na Suécia no ano de 1977 (http://www.statsci.org/data/general/motorins.html).
 
 ## Modelagem
 
 Para problemas com este tipo interesse (influência de fatores), uma das técnicas mais clássicas é a regressão linear, onde se pode ajustar um modelo para verificar a influência de certas covariáveis (variáveis explicativas) em uma variável de interesse. Entretanto, a regressão linear possui vários pressupostos que frequentemente não são satisfeitos, restringindo sua aplicabilidade. Alguns desses pressupostos são erros independentes e identicamente distribuídos, centrados em zero com distribuição normal, homogeneidade de variância, variáveis explicativas não correlacionadas, entre outros.
 
 
 Nelder e Wedderburn (1972) propuseram uma classe de modelos de regressão mais flexível, os modelos lineares generalizados. Esta nova classe abrange as distribuições pertencentes à família exponencial (Normal, Gamma, Poisson, Binomial, Normal-Inversa), aumentando a quantidade de problema a serem abordados.  Uma distribuição que pertença à família exponencial pode ser representada da seguinte forma: 

$$f(y_i;\theta_i,\phi) = \mbox{exp}\left\{\phi^{-1} [y_i \theta_i - b(\theta_i)] + c(y_i,\phi)\right\},$$
em que $\theta_i$ é o parâmetro canônico; $b(\cdot)$ e $c(\cdot)$ são funções conhecidas. 
Assim, denota-se $Y_i \sim \mbox{FE}(\mu_i , \phi)$. A média e variância de $Y_i$ são dadas, respectivamente, por: $$E(Y_i) = b'(\theta_i) = {\mu_i}$$ $$Var(Y_i) = \phi b''(\theta_i) = \phi V(\mu_i),   $$
em que $V(\mu_i)$ é a função de variância, que depende unicamente de $\mu_i$.


Outra classe de modelos importante é a classe dos modelos de dispersão, que inclui distribuições para dados positivos, positivos com massa de probabilidade em zero, contagem e dados na reta real e podem ser classificados como:
- Modelos de dispersão próprio;
- Modelos de dispersão exponencial;

Aqui, focaremos no segundo caso.

Modelos de Dispersão Exponencial (ED) têm função densidade de probabilidade dada por:
$$f(y_i;\theta_i,\lambda) = a(y_i,\lambda) \mbox{exp}\left\{\lambda [y_i \theta_i - k(\theta_i)]\right\}, \qquad y_i   \in R$$

com parâmetro canônico $\theta_i$, função adequada $a(\cdot)$ e $k(\theta_i)$ função definida como
$$k(\theta) = log  \int  e^{\theta y} \nu dy $$

em que $\nu$ é uma medida finita em $R$.

Por definição, temos que:
- $\mu_i$ = $E(Y_i)$ = $k'(\theta_i)$
- $Var(Y_i)$ = $\frac{1}{\lambda} Var(\mu_i)$ = $\frac{1}{\lambda}k''(\theta_i)$

Considerando a reparametrização $\sigma²$ = $\frac{1}{\lambda}$, temos que $\sigma²$ representa o parâmetro de dispersão.

O modelo ED, denotado por $ED(\mu , \sigma²)$, pode ser escrito da seguinte maneira: 
$$f(\textbf{y};\mu,\sigma²) = a(\textbf{y},\sigma²)    \mbox{exp}\left\{-\frac{1}{2\sigma²} d(\textbf{y};\mu)\right\},$$

em que $a(\textbf{y};\sigma²) \geq 0$, $d(\textbf{y};\mu)$ representa a função deviance, $\mu$ é o parâmetro de locação e $\sigma²$ é o parâmetro de escala (dispersão).


Percebe-se então, a similaridade entre os modelos lineares generalizados e modelos de dispersão exponencial. 

Um caso especial dos modelos de dispersão exponencial, são as distribuições Tweedi (Tweedie, 1984). Nessas distribuições, a função de variância definida anteriormente, tem a forma: 
$$V(\mu) = \mu^p , \qquad p \notin (0,1)$$

A distribuição Poisson Compound, caracteriza-se no caso $1 < p < 2$. Dentre outros casos particulares, observa-se a distribuição Poisson ($p = 1$) e a distribuição Gamma ($p = 2$).
### Número de Sinistros e Pagamento Total

Os dados referem-se ao número de sinistros e total de pagamento (valores monetários) gerados, por grupo. Desta maneira, um grupo corresponde a uma observação no banco de dados.

Dados referentes a pagamentos pelo total de sinistros por grupo, são contínuos (pois a variável valor de pagamento, geralmente está em unidades de moeda nacional) mas possuem uma massa de probabilidade em zero, vinda dos grupos em que não obtiveram nenhum sinistro no período observado.

Esta característica em particular torna inviável o ajuste de distribuições que já pensaríamos em primeira hipótese em se tratando de dados contínuos à direita, como a distribuição gamma e gaussiana-inversa.

Então como modelar?

Segundo Withers e  Nadarajah (http://www.kybernetika.cz/content/2011/1/15/paper.pdf, 2011), a soma de v.a.'s com distribuição gamma (valor monetário de cada sinistro) com seu tamanho dado por v.a.'s independentes de distribuição Poisson (número de sinistros observados), resulta na distribuição Poisson-Gamma Composta (Poisson Compound). Então temos que:

Seja $T$ o número de sinistros em determinado grupo e $X_i$ o valor de pagamento do i-ésimo sinistro. Definimos $Y$ como
$$Y = \sum_{i=1}^{T} X_i$$

Dado que $T \sim Poisson(\lambda)$ e $X_i \stackrel{i.i.d.}{\sim} Gamma(\alpha, \gamma)$, temos que $Y$ possui distribuição Poisson Compound..

### Aplicação

Iremos utilizar o banco de dados de sinistros de automovéis na Suécia no ano de 1977, disponível em http://www.statsci.org/data/general/motorins.html. A descrição das variáveis está dispnível no link, mas aqui utilizaremos apenas: Payment (Pagamento total do grupo), Zone (Zona Geográfica; 1 corresponde às maiores cidades) e Make (tipos de carros, agrupados em classes).


Inicialmente, vamos ler os dados e transformar a variável 'Make' em fator.

```r
library(dplyr)
library(tweedie)
library(faraway)
library(statmod)

dados <- read.table("http://www.statsci.org/data/general/motorins.txt", 
                    sep = "\t", head = TRUE)
                    
dados$Make <- as.factor(dados$Make)
```

Filtrando os dados, selecionando apenas valores com 'Zone' igual a 1 (maiores cidades) e 'Make' diferente de 9 (carros mais 'raros').

```r
dados <- dados %>% filter( Zone == 1 & Make != 9)

head(dados)

  Kilometres Zone Bonus Make Insured Claims Payment
1          1    1     1    1  455.13    108  392491
2          1    1     1    2   69.17     19   46221
3          1    1     1    3   72.88     13   15694
4          1    1     1    4 1292.39    124  422201
5          1    1     1    5  191.01     40  119373
6          1    1     1    6  477.66     57  170913
> 
```

A estimação do parâmetro $p$ é dado atraves de uma log-verossimilhança perfilada. Sendo assim, para cada valor fixo de $p$, são calculados os parâmetros $\mu$ e $\phi$ e a log-verossimilhança é observada. Escolhe-se então, o valor de $p$ que  levou à maior log-verosimilhança. Desta maneira, o $p$ estimado foi de $1,73$.  
```R=
perfil <- tweedie.profile( Payment ~ Make,
                           p.vec = seq(1.1,2,length = 10), 
                           method = "interpolation",
                           do.plot = FALSE,
                           data = dados)

perfil$p.max
[1] 1.736735
```
Ajustamos um modelo simples, onde deseja-se verificar se os diferentes grupos de tipos de carro diferem em valor de pagamento, em relação  à classe de referência. No nosso modelo a classe de referência é Make igual a 1.

```r 	
modelo <- glm(Payment ~ Make, 
              family = tweedie(var.power = perfil$p.max), 
              data = dados)
```
Verificando os residuos. O modelo não parece estar mal ajustado.
```r
res <- qres.tweedie(modelo)
qqnorm(res)
```
![](/image/tweeres.png)

Através dos resultados, podemos observar que todas os demais tipos de carro diferem do tipo utilizado como referência no modelo. Tal conclusão auxilia num embasamento para que diferentes tarifas sejam cobradas, por exemplo.

```r
summary(modelo)

Call:
glm(formula = Payment ~ Make, family = tweedie(var.power = perfil$p.max), 
    data = seguros2)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-12.771   -6.329   -2.474    1.764   17.402  

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.146e-04  1.511e-05   7.583 5.35e-13 ***
Make2       1.579e-04  4.512e-05   3.500 0.000544 ***
Make3       2.024e-04  5.312e-05   3.810 0.000172 ***
Make4       2.467e-04  6.143e-05   4.016 7.65e-05 ***
Make5       2.288e-04  5.803e-05   3.942 0.000103 ***
Make6       1.573e-04  4.501e-05   3.494 0.000555 ***
Make7       3.678e-04  8.543e-05   4.305 2.33e-05 ***
Make8       5.276e-04  1.193e-04   4.423 1.41e-05 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for Tweedie family taken to be 39.68757)

    Null deviance: 14409  on 279  degrees of freedom
Residual deviance: 10958  on 272  degrees of freedom
AIC: NA

Number of Fisher Scoring iterations: 7
```

Observando as médias por grupo, nota-se que o grupo 1 demonstra possuir uma média de pagamentos maior que os demais, de modo que também seria valido observar o número de sinistros para futuras conclusões.

```r
tapply(dados$Payment , dados$Make , mean)

        1         2         3         4         5         6         7         8 
295303.29  88720.63  71936.57  59976.89  64369.14  89016.20  40156.29  26995.97 
```

Analogamente, modelos podem ser ajustados incluindo as demais covariáveis, dando suporte na tomada de decisões e precificações de seguros, no caso.
