---
layout: post
title: Incerteza em Modelos de Deep Learning
lang: pt
header-img: img/0026.png
date: 2017-10-22 14:15:07
tags: [statistics, ML, Deep Learning, Bayesian]
author: Matheus Facure
comments: true
---

## Conteúdo
1. [Introdução](#intro)
2. [Por que Incerteza é Importante](#incerteza)
3. [Monte-Carlo Dropout: um olhar Bayesiano para *Deep Learning*](#mc-dropout)
4. [Obtendo Incerteza](#obtendo-incerteza)
5. [Incerteza em Regressão](#regressao)
6. [Incerteza em Classificação](#classificacao)
7. [Resumindo os Pontos Importantes](#takeaways)
8. [Referências](#ref)


<a name="intro"></a>
## Introdução

Por um bom tempo, uma grande felicidade que tive com Aprendizado de Máquina era quase não precisar me preocupar com os formalismos dos testes econométricos; para mim (e acredito que para muitos praticantes de AM), validação cruzada era uma regra de ouro que bastava: se generaliza para os dados de teste, o modelo é bom. Recentemente, no entanto, o campo de Segurança em IA tem ganhado proeminência e nos mostrado que os modelos de Aprendizado de Máquina são suscetíveis a vários pontos cegos e ataques adversários que simplesmente não são capturados nos sets de teste. Segurança em IA e incerteza de modelo são tópicos extremamente vastos e além do que se pretende este post. O que tentarei fazer aqui será lhe convencer que construir modelos para obter apenas a melhor previsão não basta. Em seguida, mostrarei uma técnica extremamente simples para obter incerteza em modelos de *Deep Learning*.

<a name="incerteza"></a>
## Por que Incerteza é Importante

Digamos que eu treine um modelo de visão computacional para distinguir entre as varias raças de cães a partir de fotos. O que aconteceria se eu então mostrasse ao modelo a imagem de um gato? Bom, naturalmente, ele será forçado a prever qual raça de cão é aquele gato; no entanto, seria bom se o modelo também pudesse dizer que ela está extremamente incerto sobre a previsão da imagem do gato.

Troque o exemplo infantil acima por algo como um carro autônomo, um modelo que prevê o diagnóstico de um câncer, ou dá empréstimos para as pessoas. Nesses ambientes em que o modelo atua é de alto risco e o custo de um erro pode ser altíssimo, uma métrica de incerteza pode ser extremamente útil para a tomada de decisão. Nesses casos, ao tomar uma decisão como seguir em frente, recomendar uma quimioterapia ou emprestar dinheiro é fundamental que o modelo tenha bastante certeza sobre a previsão que está fazendo. Além disso, tendo em mão a incerteza do modelo, poderemos diagnosticar melhor regiões no dados onde o modelo não é bom e expandir a base de dados de acordo.

Colocando em termos mais gerais, tão importante quanto o saber é o saber sobre o saber, isto é, conhecer os limites do que se sabe e isso é estamos tentando dar aos nossos modelos quando falamos de incerteza. Idealmente, queremos que os sistemas de inteligência que construirmos consigam também reconhecer por si próprios as fronteiras do conhecimento neles presente e responder de acordo com o seu nível de ignorância.

<a name="mc-dropout"></a>
## Monte-Carlo Dropout: um olhar Bayesiano para *Deep Learning*

Em 2015, [Yarin Gal](http://www.cs.ox.ac.uk/people/yarin.gal/website/) mostrou que é possível obter incerteza a partir de redes neurais quase que gratuitamente, se olhássemos técnicas de regularização estocásticas sob uma perspectiva Bayesiana. A seguir tentarei resumir um pouco dessa teoria. **Cuidado!** Matemática a frente!

Sob a perspectiva Bayesiana, os parâmetros \\(\pmb{W}\\) do modelo são vistos como variáveis aleatórias. Podemos então definir uma distribuição de probabilidade \\(P(\pmb{W})\\) a priori para esses parâmetros. Sendo a verossimilhança \\(P(\pmb{Y}\|\pmb{W},\pmb{X})\\), pelo teorema de Bayes temos que a distribuição dos parâmetros dado o que foi observado nos dados é

$$P(\pmb{W}|\pmb{X}, \pmb{Y}) = \frac{P(\pmb{Y}|\pmb{W}, \pmb{X})P(\pmb{W})}{P(\pmb{Y}|\pmb{X})}$$

Note que, desta forma, uma previsão é definida como uma distribuição e a **incerteza pode ser entendida como a dispersão desta distribuição**. 

$$P(y_{test}|\pmb{x}_{test}, \pmb{X}, \pmb{Y}) = \int P(y_{test}|\pmb{x}_{test}, \pmb{W}) P(\pmb{W|\pmb{X}, \pmb{Y}}) d\pmb{W}$$

O problema é que a distribuição a posteriori \\(P(\pmb{W}\|\pmb{X}, \pmb{Y})\\) é praticamente impossível de computar. Isso porque estimar o componente normalizador (também chamado de evidência do modelo) \\(P(\pmb{Y}\|\pmb{X})\\) envolve computar uma integral que não pode ser resolvida analiticamente

$$ P(\pmb{W}|\pmb{X}, \pmb{Y}) = \int P(\pmb{Y}|\pmb{X}, \pmb{W})P(\pmb{W})d\pmb{W}$$

Para lidar com esse problema, nós definimos uma nova probabilidade mais simples \\(q_\theta(\pmb{W})\\), parametrizada por \\(\theta\\), e minimizamos a divergência de Kullback-Leibler entre \\(q(\pmb{W})\\) e \\(P(\pmb{W}\|\pmb{X}, \pmb{Y})\\). Isso é equivalente a maximização do limite inferior da evidência (ELBO)

$$\mathcal{L}(\theta) = -\int q_\theta(\pmb{W}) \log P(\pmb{Y}|\pmb{X}, \pmb{W})d\pmb{W} - KL(q_\theta(\pmb{W})\|P(\pmb{W})) $$

Para facilitar, nós aproximamos o primeiro termo acima com integral com Monte-Carlo \\(\hat{\pmb{W}} \sim q_\theta(\pmb{W}) \\)

$$\mathcal{\hat{L}}(\theta) = - \log P(\pmb{Y}|\pmb{X}, \pmb{\hat{W}}) - KL(q_\theta(\pmb{W})\|P(\pmb{W})) $$

Normalmente, nós definimos nossa probabilidade a priori como uma gaussiana centrada em zero \\(P(\pmb{W}) \sim \mathcal{N}(0, \sigma)\\), o que faz com que o segundo termo aja como um regularizador, mantendo os parâmetros pequenos. Mas e \\(q_\theta(\cdot)\\)? Uma forma de defini-la é por meio de uma distribuição Bernoulli, onde retiramos um vetor \\(\pmb{z} \sim \mathcal{B(\theta)}\\) com o mesmo número de elementos que colunas em \\(\pmb{W}\\). Então podemos definir uma amostra \\(\pmb{\hat{W}}\\) como a multiplicação de cada elemento de \\(\pmb{z}\\) por uma coluna de uma matriz de parâmetros \\(\pmb{M}\\), com as  mesmas dimensões de \\(\pmb{W}\\). Em outras palavras, a nós estamos **zerando algumas colunas** de \\(\pmb{M}\\) para obter \\(\pmb{\hat{W}}\\). Isso soa familiar???

Toda a matemática acima é bastante complexa, mas se olharmos com calma podemos resumi-la em um algoritmo de otimização com dois passos bastante simples:  

1. Aleatoriamente, zere colunas de \\(\pmb{M}\\) para obter \\(\pmb{\hat{W}}\\). 
2. Minimize por uma iteração \\(- \log P(\pmb{Y}\|\pmb{X}, \pmb{\hat{W}}) - KL(q_\theta(\pmb{W})\|\|P(\pmb{W}))\\)

Caso você ainda não tenha percebido, isso é exatamente o que fazemos quando treinamos uma rede neural com *Dropout* e regularização \\(L_2\\). Alias, olhe novamente para o passo 2 e repare que o primeiro termo é a tradicional função custo que otimizamos com gradiente descendente e o segundo termo é análogo a regularização \\(L_2\\). Isso significa que **qualquer rede neural treinada com *Dropout* é um modelo Bayesiano**! Isso também explica porque *Dropout* costuma ser tão efetivo no treinamento de redes neurais: **essa técnica equivale a integrar os parâmetros do modelo**.

<a name="obtendo-incerteza"></a>
## Obtendo Incerteza 

Agora que temos um fundamento teórico sólido para seguir, resta ver como obter incerteza a partir de redes neurais treinadas com *Dropout* (e regularização \\(L_2\\)). Durante o treinamento do modelo, nada muda; mas, **durante o teste matemos a probabilidade de *Dropout* fixada durante o treino e realizamos \\(T\\) *forward-pass* pela rede, coletando assim \\(T\\) previsões \\(\hat{y}\\) para cada amostra**. Assim para cada ponto teremos uma previsão para a média e uma previsão para a variância, que será nossa medida de incerteza.


$$\mathbb{E}(\hat{y}) \approx \frac{1}{T} \sum_{t=1}^T \hat{y_t} $$

$$ \begin{align*}
	\mathbb{E} (\hat{y}) &\approx \frac{1}{T} \sum_{t=1}^T \hat{y}_t \\
	Var ( \hat{y} ) &\approx \tau^{-1} \\
	&\quad+ \frac{1}{T} \sum_{t=1}^T \hat{\hat{y}_t}^T \hat{y}_t \\
	&\quad- \mathbb{E} (\hat{y})^T \mathbb{E} (\hat{y})
\end{align*}$$

Onde \\(\tau = \frac{l^2 p}{2 N \lambda}\\) é uma medida de precisão, \\(p\\) é a probabilidade de *Dropout*, \\(\lambda\\) é a força da regularização \\(L_2\\), \\(l\\) define a nossa crença a priori da escala de \\(W\\) e \\(N\\) é a quantidade de dados. Note que \\(\tau\\) é negligenciável quando há muitos dados \\(N \rightarrow \infty\\). Em Python, isso se traduz para umas poucas linhas de código

{% highlight python %}
l = 10
tau = l**2 * (1 - p_dropout) / (2 * X_train.shape[0] * lbd)

y_hat_mean = np.mean(y_hat, axis=1)

y_hat_variance = np.var(y_hat, axis=1)
y_hat_variance += tau**-1
{% endhighlight %}

<a name="regressao"></a>
## Incerteza em Regressão

Vamos ver como a técnica apresentada acima lida com um problema de regressão. A seguir, vamos tentar prever a quantidade de bicicletas alugadas na hora seguinte, dada as quantidades de bicicletas alugadas nas últimas 10 horas. Para maior conveniência, coloquei o código todo [aqui](https://github.com/matheusfacure/Tutoriais-de-AM/blob/master/Redes%20Neurais%20Artificiais/Bayesian-NN/BNN-Regression.ipynb) e focarei apenas nas parte mais interessantes para não extender demais este post.

Vamos usar o pacote [Keras](https://keras.io/) com TensorFlow para construir e rodar a rede neural do nosso experimento. Nossa rede neural terá apenas 2 camadas ocultas, cada uma com 100 neurônios. Após cada camada oculta, utilizaremos uma camada de *Dropout* após cada camada oculta, zerando 50% das unidades. Também colocaremos uma camada de *Dropout* logo no começo da rede, mas como temos poucas variáveis, a probabilidade de zerar as unidade aqui será apenas 5%.

{% highlight python %}
from keras import backend as K
from keras.models import Sequential
from keras.layers import Dense, Dropout
from keras.optimizers import Adam
from keras.regularizers import l2

n_input = X.shape[-1] # n de variáveis
num_out = 1 # n de previsões

p_dropout = 0.5 # probabilidade de Dropout
lbd = 1e-4 # força da regularização L2

model = Sequential()
model.add(Dropout(.05, input_shape=(n_input,)))
model.add(Dense(100, activation='relu', input_shape=(n_input,), kernel_regularizer=l2(lbd)))
model.add(Dropout(p_dropout))
model.add(Dense(100, activation='relu', kernel_regularizer=l2(lbd)))
model.add(Dropout(p_dropout))
model.add(Dense(num_out, activation=None))
model.summary()

opt = Adam(lr=1e-3)
model.compile(loss='mean_squared_error',optimizer=opt)
{% endhighlight %}

Definido nosso modelo, podemos treiná-lo. Cada mini-lote terá \\(\frac{1}{5}\\) das amostras e assim treinaremos o nosso modelo por 3000 épocas ou 15000 iterações.

{% highlight python %}
epochs = 3000
model.fit(X_train, y_train,
            batch_size=X_train.shape[0] // 5,
            epochs=epochs,
            verbose=0)
{% endhighlight %}

Após o treinamento, a performance do nosso modelo no set de treino é de \\(R^2=0.73\\) e no set de teste, \\(R^2=0.77\\). 

{% highlight python %}
from sklearn import metrics

y_hat_train = model.predict(X_train)
print(metrics.r2_score(y_train, y_hat_train))

y_hat_test = model.predict(X_test)
print(metrics.r2_score(y_test, y_hat_test))
{% endhighlight %}
```
0.729823
0.775525
```

Acima, a previsão é feita colocando a probabilidade de *Dropout* em 0%, usando assim toda a capacidade da rede. Esse é o padrão do Keras e precisaremos rescrevê-lo para usar as probabilidade de *Dropout* de treino também durante as previsões. Abaixo, vamos definir uma função que retornará a última camada da rede, isto é, as previsões, dada a camada de entrada, isto é, as variáveis. Além disso, vamos definir que está função será usada tal como durante o treinamento, passando `K.learning_phase()`. Assim, as probabilidades de *Dropout* serão mantidas.

{% highlight python %}
from sklearn import metrics

y_hat_train = model.predict(X_train)
metrics.r2_score(y_train, y_hat_train)

y_hat_test = model.predict(X_test)
metrics.r2_score(y_test, y_hat_test)
y_hat_mc.shape
{% endhighlight %}
```
(500, 1000)
```

Como podemos ver, agora temos 1000 previsões para cada ponto no set de teste. Podemos então obter a média e a variância destas previsões

{% highlight python %}
l = 10
y_hat_test_mean = np.mean(y_hat_mc, axis=1)
y_hat_test_variance = np.var(y_hat_mc, axis=1)
tau = l**2 * (1 - p_dropout) / (2 * X_train.shape[0] * lbd)
y_hat_test_variance += tau**-1

metrics.r2_score(y_test, y_hat_test_mean)
{% endhighlight %}

```
0.91519
```

Note como a performance de teste melhorou com esse procedimento. Mais ainda, agora temos uma métrica de incerteza para cada ponto e podemos saber onde o modelo não sabe bem o que está acontecendo. Também podemos plotar as previsões contra os valores observados, e colocar um intervalo de confiança em torno das previsões. Abaixo, mostramos as últimas 200 horas do set de teste. Em torno da linha de previsões, colocamos 4 tons de roxo, cada tom denotando meio desvio padrão. 

<img class="img-responsive center-block thumbnail" src="/img/BayesianNN/rnn_demanda_test.png" alt="incerteza-demanda" />

<a name="classificacao"></a>
## Incerteza em Classificação

Em problemas de classificação estamos tentando estimar a probabilidade de uma amostra \\(\pmb{x}\\) pertencer às classes \\(\pmb{y}\\). Um erro que muitos praticantes cometem é supor que a probabilidade de pertencer a classe \\(c\\), \\(P(y = y_c)\\), produzida pelo modelo é equivalente a sua confiança. O que devemos lembrar é que, assim como modelos de classificação prevem o valor esperado de uma amostra, condicionado nos seus covariantes \\(\pmb{x}\\), modelos de classificação prevem uma probabilidade esperada mas, normalmente, nada nos dizem quão dispersa é essa estimativa. 

Para melhor entender a diferença entre incerteza a probabilidade prevista, vamos usar parte da base MNIST, da qual vamos filtrar os números 3, 4 e 5. Vamos treinar uma rede neural para distinguir os 5s dos 3s. Será portanto, um problema de classificação binária, onde, nesse caso, 3 será a classe 1 e 5 será a classe 0. Depois disso, mostraremos ao modelo os 4s, nos quais ele não foi treinado. O código muda muito pouco do exemplo de classificação, por isso vou omiti-lo quase inteiro aqui. Caso queira conferi-lo, todo esse experimento [está disponível no meu GitHub](https://github.com/matheusfacure/Tutoriais-de-AM/blob/master/Redes%20Neurais%20Artificiais/Bayesian-NN/BNN-MNIST.ipynb).

A rede neural utilizada aqui foi de duas camadas densamente conectadas, cada uma com 512 neurônios e camadas *Dropout* após as camadas ocultas e de entrada. A probabilidade de *Dropout* em todas as camadas foi de 50%. Após treinadas, realizamos 1000 *forward-passes* pela rede para obter as estimativas de Monte-Carlo *Dropout*. A probabilidade média dessas 1000 estimativas será nossa previsão final e o desvio padrão será nossa métrica de incerteza.

A incerteza média do modelo, computada nos 5s e 3s de teste, é de 0.05. Podemos também plotar \\(P(y=3)\\) conta o desvio padrão dessa probabilidade. Isso deixará claro que incerteza e probabilidade prevista, embora relacionadas **não são a mesma coisa**.

<img class="img-responsive center-block thumbnail" src="/img/BayesianNN/incerteza_prob1.png" alt="incerteza-vc-prop1" />

No gráfico acima, note como há pontos que o modelo prevê como sendo um 3 com 80% de chance, mas, para essa mesma probabilidade prevista, existem amostras com incertezas variando de 0.2 até 0.27 (pontos azuis). A variação na incerteza é ainda maior nas probabilidades previstas muito baixas ou altas. Note como há muitos pontos cuja probabilidade prevista é maior que 95% (ou menor que 5%) e como a incerteza nesses pontos varia de 0 até 0.1. Por fim, vale ressaltar que, embora diferentes, **probabilidade prevista é incerteza estão relacionadas**. Se não fosse o caso, não haveria nenhuma tendência no gráfico acima. O que podemos ver é que a incerteza tende a aumentar perto da fronteira de decisão, isto é, perto dos lugares onde a probabilidade prevista é de 50%. 

Acima, só analisamos a incerteza do modelo nas imagens de dígitos que ele viu durante o treinamento (3 e 5), mas o que aconteceria se mostrássemos ao modelo imagens com dígitos 4? Sabemos que ele terá que prever para essas imagens a probabilidade delas ser um 3, afinal é para isso que ele foi treinado, mas queremos que ele faça isso ao mesmo tempo que nos diz estar bastante incerto sobre essas previsões. Isso de fato acontece!

A incerteza média do modelo, quando computada nos 4s, é de 0.28, que é bem maior do que a média de 0.05 obtida no set de teste de 5s e 3s. Além disso, quando plotamos a probabilidade prevista contra o desvio padrão dessas probabilidades, podemos ver que a maioria dos pontos está mais para a direita, reforçando a incerteza geral do modelos ao tentar prever os 4s.

<img class="img-responsive center-block thumbnail" src="/img/BayesianNN/incerteza_prob2.png" alt="incerteza-vc-prop2" />

Outro aspecto interessante de Monte-Carlo *Dropout* é que podemos estudar as incertezas particulares a cada amostra. Por exemplo, abaixo pegamos uma amostra qualquer dos dados de teste (5s e 3s) e plotamos o histograma das duas probabilidades previstas. Note como na maioria das estimativas de Monte-Carlo *Dropout* o modelo dá a essa imagem uma probabilidade de 1, indicando que há uma alta chance dela ser um 3. Nesse exemplo particular, o desvio padrão é 0.12.

<img class="img-responsive center-block thumbnail" src="/img/BayesianNN/hist1.png" alt="hist1" />

Podemos também ver o que acontece quando mostramos um 4 ao modelo. Note como, desta vez, as probabilidades previstas estão muito mais dispersa. O modelo acha que isso é um 5, já que a maioria das probabilidades previstas são menores de 0.5. Ao mesmo tempo, ele não está nem um pouco certo dessa previsão. Nesse caso particular, o desvio padrão das previsões é de 0.45.

<img class="img-responsive center-block thumbnail" src="/img/BayesianNN/hist2.png" alt="hist2"/>

<a name="takeaways"></a>
## Resumindo os Pontos Importantes

Métricas incerteza podem ser extremamente úteis no processo de tomada de decisão. Por exemplo, é possível delimitar um limite máximo de incerteza que se está disposto a tolerar. Além disso, com informações sobre incerteza por amostra podemos definir quando é necessário ir atrás de mais informações antes de tomar uma decisão.

Além de saber o que o modelo prevê para uma amostra, **Monte-Carlo *Dropout* nos permite saber qual a certeza que o modelo tem** sobre essa previsão. Em aplicações de regressão, isso nos permite traçar um intervalo de confiança em torno do valor previsto. Nos casos de classificação, vimos que **incerteza e probabilidade prevista são diferentes**, embora relacionadas. A primeira deve ser entendida como uma média de centralidade das previsões enquanto a segunda nos mostra como é a dispersão de probabilidade em torno desta previsão.

<a name="ref"></a>
## Referências

Este post é baseado no trabalho de Yarin Gal sobre incerteza em modelos de *Deep Learning*. Para este post, usei informações de sua tese de mestrado, [Uncertainty in Deep Learning](http://www.cs.ox.ac.uk/people/yarin.gal/website/thesis/thesis.pdf)(Gal, 2015), seu artigo [Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning](https://arxiv.org/abs/1506.02142)(Gal & Ghahramani, 2015) e um post fantástico do seu blog, [What my deep model doesn't know...](http://www.cs.ox.ac.uk/people/yarin.gal/website/blog_3d801aa532c1ce.html).

Se você está interessado em saber mais sobre abordagens Bayesianas para *Deep Learning*, o curso [Bayesian Methods for Machine Learning](https://www.coursera.org/learn/bayesian-methods-in-machine-learning) acabou se ser lançado no Coursera. Além disso, no NIPS de 2016 houve um [workshop dedicado exclusivamente a Deep Learning Bayesiano](https://www.youtube.com/channel/UC_LBLWLfKk5rMKDOHoO7vPQ). Por fim, sugiro que de uma olhada na biblioteca de programação [Edward](http://edwardlib.org/). Ela é construida em cima do TensorFlow e conta com a maioria dos modelos Bayesianos usados atualmente.