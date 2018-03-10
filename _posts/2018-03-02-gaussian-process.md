---
layout: post
title: Processos Gaussianos
lang: pt
header-img: img/0026.png
date: 2018-03-04 14:15:07
tags: [Statistics, ML, Bayesian, Gaussian Process]
author: Matheus Facure
comments: true
---

## Conteúdo
1. [Introdução](#intro)


<a name="intro"></a>
## Introdução

Quando diante de um problema não linear, podemos [extender lineares simples usando mais parâmetros, fazendo uma regressão pilinomial](https://matheusfacure.github.io/2017/03/01/regr-poli/). Podemos aumentar o grau do polinônio tornando-o arbitrariamente complexo, conseguindo assim se ajustar a dados bastante não lineares. 

<img class="img-responsive center-block thumbnail" src="/img/gp/polregr.png" style="width: 60%;" alt="plinomial_regression"/>

Por outro lado, e se não quisermos especficar de antemão o número de parâmetors do modelo? Uma possibilidade é considerar todas as funções que se encaixam nos nossos dados, cada uma com quantos parâmetros forem necessários. É isso que Processos Gaussianos são capazes de conseguir: um método *não paramétrico* para regressão não linear. Note que *não paramétrico* não deve ser confindido como um modelo sem parâmetros, mas sim entendido como algo que tem muitos parâmetors (infitos, possivelmente), tanto mais quanto maior for a quantidade de dados. 

Mas, mais do que isso, Processos Gaussianos são modelos probabilisticos ou bayesianos. Isso significa que, além de nos fornecer uma estimativa pontual de uma previsão (geralmente a média condicional), os Processos Gaussianos também espeficam toda a distribuição preditiva a posteriori, tornando a obtenção de estatísticas de incerteza trivial. Isso é extremamente importante em aplicações de Aprendizado de Máquina em cenários de alto risco, como medicina, carros autônomos ou concessão de crédito. Nessas áreas, além da previsão, estamos frequentemente interessados no grau de certeza do modelo para que, caso ele seja muito alto, possamos passar a decisão para um especialista humano. 

## Intuição

De uma forma bastante simplista, podemos entender Processos Gaussianos como um **interpolador de dados**. Assumindo que não há ruido, um interpolador prevê sempre o valor observado onde há dados. A parte complicada então é saber o que prever entre um ponto. A intuição diz que nossas previsões não mudem bruscamente se as variáveis explicativas não mudarem muito. Por exemplo, se eu observo \\(y=0\\) quando \\(x=0\\) então devo esperar que \\(y\\) seja próximo de zero quando \\(x\\) por próximo de zero, digamos \\(x=0.1\\).   Na imagem abaixo podemos ver como Processos Gaussianos estão de acordo com essa intuição e fazer uma interpolação bastante elegante. 

<figure class="figure center-block thumbnail" style="width: 60%;">
  <img src="/img/gp/gprDemoNoiseFree_02.png" class="img-responsive center-block" alt="">
  <figcaption class="figure-caption text-center">Retirada do <a href="https://github.com/probml/pmtk3/blob/30d7a1952f3979b16e92dbfa4cd1ce0e402cf7d8/docs/demoOutput/bookDemos/(15)-Gaussian_processes/gprDemoNoiseFree_02.png">livro de Kevin Murphy</a></figcaption>
</figure>

imagem mostra 3 amostras de um processo gaussiano definido pelas 5 observações marcadas com`x`. Note que, onde há dados, o valor previsto é o mesmo que o observado e não há incerteza (lembre-se de que assumimos ruído zero). Conforme nos afastamos dos dados, a incerteza da interpolação aumenta, denotada pelo alagramento do intervalo de confiância de 95% (em cinza).  

Então, recapitulando, se podemos entender Processos Gaussianos como uma espécie de interpolador, que, de querba no dá informações de incerteza, precisamos que ele defina duas coisas: uma noção de distância e uma noção de incerteza. Para entender isso melhor, precisaremos entrar um pouco na matemática

## Matemática

### Kernel
 
A noção de distância entre dois pontos é definida num Processo Gaussiao pelo que chamamos de **kernel**, uma função de dois pontos no espaço, geralmente não linear, que pode ser interpretada como a distância entre os seus pontos de entrada. Um kernal bastante popular é o exponencial quadrático, também conhecido como função de base radial ou kernel gaussiano: 

$$
k(x,x') = \sigma^2 exp\Bigg(-\frac{(x-x')^2}{2\mathcal{l}^2}\Bigg)
$$

Ele tem esses nomes pois é igual a [função gaussiana](https://en.wikipedia.org/wiki/Normal_distribution), tirando o fator de normalização. Isso lhe confere algumas propriedades de distância bastante interessentas. pode ser entendido como colocando uma função gaussiana onde `x` é o ponto de máximo e distância aumenta conforme nos afastamos desse ponto. O hyper-parâetro \\(\mathcal{l}\\) é como se fosse a variância dessa gaussiana, controlando quão larga é a cruva. Dessa forma, \\(\mathcal{l}\\) muito pequeno nos diz que a distância aumenta rapidamente quando nos afastamos de `x` e, por conseguinte, espera-se que a nossa função interpoladora seja bastante varie muito e rapidamente, sendo pouco suave. Um \\(\mathcal{l}\\) por sua vez introduz uma noção de distância que diminui lentamente conforme nos afastamos de `x` e, por conseguinte, a função interpoladorea será mais suave.

<figure class="figure center-block thumbnail" style="width: 60%;">
  <img src="/img/gp/l.png" class="img-responsive center-block" alt="">
  <figcaption class="figure-caption text-center">l pequeno (esquerda) e l grande (direita). Retirada de <a href="http://evelinag.com/Ariadne/covarianceFunctions.html">Covariance functions</a></figcaption>
</figure>

Já o fator de dimensionamento \\(\sigma^2\\) tem uma interpretação mais simples: eles nos diz o quanto esperamos que nossa distância seja algo fora da média.

<figure class="figure center-block thumbnail" style="width: 60%;">
  <img src="/img/gp/sigma.png" class="img-responsive center-block" alt="">
  <figcaption class="figure-caption text-center">sigma pequeno (esquerda) e sigma grande (direita). Retirada de <a href="http://evelinag.com/Ariadne/covarianceFunctions.html">Covariance functions</a></figcaption>
</figure>

Claro que existem muitos outros kerneis além do gaussiano, mas falar sobre eles não é o porpósito deste post. Caso queira saber mais, confira o [Kernel cookbook](http://www.cs.toronto.edu/~duvenaud/cookbook/), com alguns exemplos de kernels e suas aplicações. Por hora, o que é importante ter em mente é que o kernel é uma função que pode ser **interpretada como a distância entre dois pontos**.

### Matriz de Covariancia

Outra coisa que carrega uma noção de distância são matrizes de covariancia. Caso não lembre muito sobre matriz de covariancia, de uma olhada aqui [nessas que eu fiz](https://matheusfacure.github.io/2017/03/04/aval-modelo-am/#explo) ou, melhor ainda, pegue um DataFrame qualquer no Pandas e use [`df.cov()`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.cov.html). Intuitivamente, a matriz de convariância nos diz como cada uma das variáveis são parecidas entre si (ou linearmente dependentes). Isso porque a uma boa parte da [operação de covariância](https://en.wikipedia.org/wiki/Covariance) é simplesmente computar a produto interno entre duas colunas, que nada mais é do que a noção mais simples que há de distância.

código

E se, em vez de usar a covariância tradicional de uma base de dados, a gente antes virasse a tabela? Nesse caso a matriz covariância obitida seria \\(NxN\\) e nos daria informações sobre o quão similares são as **observações** entre si! Isso parece exatamente o que precisamos num interpolador, isto é, uma noção de distância entre as amostras para prever coisas parecidas para observações também parecidas.

Uma outra ideia é que talves usar um simples produto interno na hora de computar a covariância talvez não seja a melhor coisa que possamos fazer. E se quisermos usar uma distância não linear? Para isso, podemos substituir o produto interno por uma **função kernel**, que também codifica distância, mas numa forma muito mais flexível e complexa.

### Processo Gaussiano

Vimos que kerneis e matriz de covariância são formas de representar uma noção de distância, que é o que precisamos para criar um bom interpolador. Agoa podemos juntat tudo isso que vimos para entender os **Processos Gaussianos**.

Formalmente, uma funçõ \\(\pmb{f}\\) é um **Processos Gaussianos** se qualquer conjunto finito \\( \\\{ \pmb{f}(\pmb{x}_1), \pmb{f}(\pmb{x}_2), ..., \pmb{f}(\pmb{x}_n) \\\} \\) segue uma distribuição normal multivariada, onde \\(\pmb{x}\\) tipicamente representam um vetor observação (linha) numa tabela de dados.

Assim como uma gaussiana, um PG é definido por uma função de média \\(\pmb{m(x)}\\), que é usualmente assumida como zero. Não há perda de generalidae com isso, pois sempre podemos normalizar os dados para que tenham média zero. Além disso, um PG é definido por uma função de covariância \\(k(\boldsymbol x, \boldsymbol x')\\), também conhecid por **kernel**. A funcão de covariância é usada para construir uma matriz de covariância \\(NxN\\). Juntando isso, podemos retirar amostras de um GP com

$$\boldsymbol f \sim \mathcal N(\boldsymbol m_{\boldsymbol X}, \boldsymbol K_{\boldsymbol X \boldsymbol X})$$

code


