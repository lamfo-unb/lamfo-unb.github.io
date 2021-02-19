---
layout: post
title: "Regressão Logística"
lang: pt
header-img: img/manipulacao_data.table/img_data.table.png
date: 2020-04-30 23:59:07
tags: [Regressão, Linear, Logistíca, Machine Learning]
author: Alixandro Werneck Leite, Lucas Moreira Gomes e Alícia Isaias Macedo
comments: true
---

# Regressão Logística
## Introdução
A análise de regressão tem como principal função, a relação entre uma variável dependente e as outras chamadas independentes. Como parte disso, existem diferentes formas de regressão. A forma estudada nesta postagem é a logística, um tipo de classificação supervisionada. O objetivo desse post é dar uma ideia sobre as compreensões básicas do que seria uma regressão logística e a sua aplicação, tanto no viés acadêmico como no profissional.

A regressão logística é importante no sentido de classificar de maneira discreta a variável de interesse(0 ou 1, verdadeiro ou falso, etc) . Sua aplicação tem viabilidade em diferentes campos e uma abertura pra aplicar diferentes formas de encaixe na modelagem. Um exemplo clássico considerado seria a maior ou menor chance de sobreviver ao desastre do navio de passageiros Titanic baseado em características como sexo, idade, habilidades, etc.

A postagem divide-se em uma abordagem do que é a regressão logística, uma explicação breve de sua forma linear, a versão logística (alvo desse trabalho) e as diferenças existentes entre os dois tipos de regressão. Por último, é aplicado um exemplo de regressão logística na análise da taxa de mortalidade da COVID-19.

## Regressão Linear: uma primeira abordagem
A regressão linear consiste na forma de expressar um padrão de comportamento dentro de uma correlação de uma variável dependente sobre outra independente. Caso fossemos fazer um raciocínio rápido, seria como entender a quantidade de vendas (dependente) pelos dias de uma pizzaria em um período determinado (independente) ou compreender o aumento ou a diminuição de uma taxa de juros em um ano. Isso em função de variáveis independentes (as quais queremos saber como/se influenciam a variável dependente). O exercício proposto pela regressão é possibilitar tanto a análise atual (descritiva), quanto prover uma estimativa de um fenômeno (preditiva). Ademais, a lógica usada nesse modelo em termos de relação entre as variáveis é replicada nos níveis mais baixos de quase todos os modelos mais avançados de aprendizado de máquina, incluindo as redes neurais.

Um ponto a se destacar é a diferença entre a regressão e a correlação. A primeira explica a forma da relação entre as variáveis, enquanto o segundo entende a quantificação da força ou grau da relação entre as variáveis.  Com isso, precisa-se criar uma relação entre as variáveis, o que pode ser feito com um diagrama de dispersão. Tal fator permite que possa determinar uma relação entre os dois aspectos, e se essa relação é fraca ou forte, conforme os pontos constados na reta que se cria pelos pontos formados. No entanto, esse post se focará somente na regressão em si. Em oportunidades, retornaremos com uma explicação sobre correlações. Dessa maneira, a equação principal de uma regressão é a da reta. Assim:
\begin{equation}
y = \alpha*(x)+\beta
\end{equation}
Em que $\alpha$ é a inclinação da reta, $x$ é a variável (assumidamente) independente e $\beta$ é o ponto que a reta corta o eixo das ordenadas (Y). Um ponto a mais seria que a linha da regressão é o que minimiza o erro quadrático. Dessa forma, o resultado se conformaria em um gráfico como o exemplo abaixo.
<center>

![i6](https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/LinearRegression.svg/1280px-LinearRegression.svg.png)
***Imagem 1**: [Regressão linear](https://en.wikipedia.org/wiki/Linear_regression)*
</center>

A regressão linear tem, dessa forma, a função também de ser preditiva. Ela apresenta a evolução de um processo a partir dos dados existentes e da relação entre as variáveis.

## Regressão Logística
A regressão logística é compreendida, basicamente, como uma regressão linear ajustada por um parâmetro (log). Ela foi uma maneira de resolver um problema comum dentro da forma linear: a existência de uma variável dependente categórica binário como por exemplo 0 ou 1 (ao invés de variáveis contínuas). No modelo padrão de regressão linear, temos somente uma variável contínua e o entendimento é conformado em torno de uma reta imaginária como na imagem 2.

<center>

![i6](https://miro.medium.com/max/1280/0*gKOV65tvGfY8SMem.png)
***Imagem 2**: [Regressão Logística com Python](https://medium.com/@ODSC/logistic-regression-with-python-ede39f8573c7)*
</center>

No caso da versão logística, há mais de um valor discreto para a variável observada (chamada de variável categórica) em que se faz necessário algo que as una como uma função de ativação sigmoid (formato de um S), pois cada variável de Y corresponde assim a um mínimo e a um máximo (y0 = 0 e y1 = 1), em uma nova situação, agora entendida como uma probabilidade de algo ocorrer em um teste de hipótese binário.

<center>

![i6](https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/1280px-Logistic-curve.svg.png)
***Imagem 3**: [Regressão Logística](https://en.wikipedia.org/wiki/Logistic_regression)*
</center>

Alguns aspectos são importantes ao considerar uma regressão logística, como o formato dos dados, a existência ou não de correlação entre as variáveis, entre outros. Em um primeiro ponto, enquanto em uma regressão linear, não é (usualmente) necessário ter um banco de dados grande; já no caso da versão logística ter uma quantidade maior de observações. Outros pontos a se considerar também estaria na classificação ser binária, ou seja, apresentar mais de uma variável (no caso duas), serem relevantes no sentido de determinar resultados e explicações claras. No entanto, podem ter mais n's.

Os pontos descritos acima dão as características para compreender que a regressão logística tem um papel determinante em exemplos como o caso do Titanic(famoso naufrágio ocorrido no Oceano Atlântico em 1912), em que uma mulher teria 12,4 mais chances de sobreviver à tragédia do que um homem, ou seja, a existência de uma razão de probabilidade que demonstraria o quanto uma variável é mais ou menos passível de ocorrência que a outra.

Tal aspecto é acrescido da criação do chamado *odds ratio*, uma razão de probabilidade concebida a partir da chance de dar certo como errado. Isso ocorre por regressão logística ser parte da família exponencial de Generalized Linear Models (GLM), o que o possibilita fazer uso da função de ligação “logit” e gera a interpretação dos resultados em função da Razão de Chances (Odds Ratio).

Isso é importante porque age como um qualificador da possibilidade de ocorrer. No exemplo do Titanic, a chance de ser uma mulher a sobreviver seria maior do que a de um homem. Em outra análise, as pessoas da primeira classe tinham mais chances que as demais. Tal aspecto partiria da fórmula de relação abaixo aplicado à função logit que ajustaria o modelo:
\begin{equation}
 log(y) = log(p/1-p)
\end{equation}

## Aplicação prática em Python

O código utilizado para apresentação da oficina aplicou o processo de aprendizado em um data-set de casos de coronavírus, utilizando as informações de sexo e idade para prever os casos de óbitos.


Os dados utilizados para os casos de cornavírus são disponibilizados no Kaggle.

- https://www.kaggle.com/sudalairajkumar/novel-corona-virus-2019-dataset/version/47

Primeiramente importamos os dados e procuramos por valores inadequados.
```python
import pandas as pd
import random

data = pd.read_csv("COVID19.csv",usecols=["age","death","gender"])
print(len(data))

print(data) #checando dataframe

print(data.death.unique()) #checando por valores ruins
print(data.age.unique())
print(data.gender.unique())
```
Agora corrigimos os valores onde precisamos

```python
data = data.dropna(subset=['gender']) #tirando linhas que não tenham informação sobre o gênero ..

def gender(x): # função para converter as variável categórica para valor numérico
    if x == "male":
        return 0
    else:
        return 1

data['gender'] = data['gender'].apply(lambda x: gender(x)) #aplica a função gender no dataframe data coluna gender
print(data.gender.unique())

data = data[(data["death"] == "1" ) | (data["death"] == "0" )] # deixamos apenas os resultados para 1 e 0.

print(data.death.unique())


print(data.mean())
data = data.fillna(data.mean()) #completamos valores não disponíveis pela média de cada coluna.

```

Em seguida preparamos os dados para serem processados. X são nossas observações (teoricamente) independentes (informações que possam explicar o fenômeno de interesse) e y as observações dependentes (nesse caso, a informações sobre a morte ou não-morte de um paciente, sendo a morte (1) ou não morte (0)).

```python
y = data["death"] #coluna Death
X = data.drop("death",axis=1) #Todas as colunas exceto Death
```

Separamos os dados em dois: Treino e teste. Aqui, usamos 10% para teste, e 90% para treinamento. Mais sobre a utilização de dados de treino e teste em [What is Training Data its types and why it is important?](https://becominghuman.ai/what-is-training-data-its-types-and-why-it-is-important-f998424c3c9)

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.1, random_state = 42)
# print(X_test)
```
Antes de converter nossos dados para um vetor que possa ser analisado pelo modelo, precisamos fazer com que todos os valores de X estejam na mesma proporção ([Why Data Normalization is necessary for Machine Learning models](https://medium.com/@urvashilluniya/why-data-normalization-is-necessary-for-machine-learning-models-681b65a05029)). Para isso, fazemos com que eles fiquem entre -1 e 1, para qualquer variável em X usando a função StandardScaler().


```python
from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
X_train = sc_X.fit_transform(X_train)
X_test = sc_X.transform(X_test)
```

Agora treinamos o modelo, e predizemos os resultados para os valores de treinamento

```python
from sklearn.linear_model import LogisticRegression
classifier = LogisticRegression(random_state=0,class_weight = "balanced")
classifier.fit(X_train, y_train)

y_pred = classifier.predict(X_test)
```

Para avaliar a precisão do modelo, utilizamos uma matriz de confusão.

```python
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, y_pred)
print(cm)
```

Por último, plotamos os resulados para conferir visualmente nosso classificador (verde é saudável e vermelho indica óbito).

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
X_set, y_set = X_train, y_train
X1, X2 = np.meshgrid(np.arange(start = X_set[:, 0].min() - 1, stop = X_set[:, 0].max() + 1, step = 0.01), np.arange(start = X_set[:, 1].min() - 1, stop = X_set[:, 1].max() + 1, step = 0.01))
plt.contourf(X1, X2, classifier.predict(np.array([X1.ravel(), X2.ravel()]).T).reshape(X1.shape), alpha = 0.75, cmap = ListedColormap(('green', 'red')))
plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())
for i, j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set == j, 0], X_set[y_set == j, 1], c = ListedColormap(('green', 'red'))(i), label = j)
plt.title('(COVID) Logistic Regression (Training set)')
plt.xlabel('Sexo')
plt.ylabel('Idade')
plt.legend()
plt.show()
```

<center>

![](https://i.imgur.com/q9blpwt.png)
***Imagem 4**: Mortes vs Idade-Sexo.*
</center>

Como observado na imagem acima, podemos ver que nosso modelo é capaz de corretamente traçar uma reta que parece separar bem os dois grupos. É possível ainda perceber que existe uma leve tendência para uma taxa de mortalidade mais elevada de homens começando em uma faixa etária mais jovem que as das mulheres, conforme [algumas evidências recentem tendem a corroborar](https://g1.globo.com/bemestar/coronavirus/noticia/2020/03/01/coronavirus-por-que-ha-mais-homens-que-mulheres-infectados.ghtml).

No entanto, é importante lembrar que este modelo é de grande simplicidade, apresentando apenas duas colunas de informação (Sexo e Idade) por questões didáticas. Isso implica numa menor variação na otimização do nosso modelo (o que tende a ser positivo), porém em um maior viés (tanto explicito, ao escolhermos o que é importante ou não entrar no modelo; quanto implícito, ao incorrermos no risco de usarmos dados auto-correlacionados ou falsamente correlacionados por questões de amostragem inadequadada).

<center>

![i6](https://files.ai-pool.com/a/eba93f5a75070f0fbb9d86bec8a009e9.png)
***Imagem 5**: [Bias-Variance Tradeoff in Machine Learning](https://ai-pool.com/a/s/bias-variance-tradeoff-in-machine-learning).*
</center>

Como explicado no post [*Um Olhar Descontraído Sobre o Dilema Viés-Variância*](https://lamfo-unb.github.io/2017/04/29/Um-Olhar-Descontraido-Sobre-o-Dilema-Vies-Variancia/), nosso objetivo principal não é descrever os dados, mas sim os utilizar de maneira que possamos generalizar o modelo para que este possa ser aplicado a dados não utilizados no treinamento. Na prática, é preciso achar um equilíbrio que nos permita treinar o modelo da maneira mais efetiva possível, buscando uma variação adequada dos dados (fornecendo variáveis indepentes suficientes), ao mesmo tempo sem produzir um (grande) viés que prejudique a generalização do modelo.



# Apêndice

### Máxima Verossimilhança


Na regressão linear, para estimar os parâmetros desconhecidos $\beta_{i}$ utiliza-se como critério de estimação o modelo de [Mínimos Quadrados Ordinários](https://lamfo-unb.github.io/2020/02/07/O-M%C3%A9todo-dos-M%C3%ADnimos-Quadrados-Ordin%C3%A1rios-e-Regress%C3%A3o-Linear-Simples/).

Já para a Regressão Logística, utilizamos o método da Máxima Verossimilhança. Esse método fornece valores para os parâmetros desconhecidos que maximizam a probabilidade de se obter determinado conjunto de dados.



#### *Definição*


A verossimilhança $(L)$ de um conjunto de parâmetros $(\theta)$, dado observação $(x)$ é igual a probabilidade daquela observação ter ocorrido dados os valores daqueles parâmetros.

$$
L (\theta;x)= P(x;\theta)
$$

A função de Verossimilhança $L$ é definida como:

$$
L (\theta;x)=\prod_{i=1}^{n}{f(x_{i;\theta})}
$$

Uma vez que a função $log$ é contínua e crescente, maximizar a verossimilhança é equivalente a maximizar o seu logaritmo.

$$
l(\theta;x) = log \ L(\theta;x)
$$

Derivando:

$$
l'(\theta;x) = \frac {\delta \ {l(\theta;x)}}{\delta \ \theta} = 0
$$

Ao calcular a primeira derivada, tem-se um candidato a ponto máximo. Esse é o valor que maximiza a probabilidade de uma determinada amostra ser observada.


### Coeficientes


A variável resposta da regressão logística possui distribuição de Bernoulli, com probabilidade de sucesso %$p$% e probabilidade de fracasso %$1-p$%.

$$
% p(y_{i}) = p_i^{y_{i}} (1-p_i)^{1-y_{i}} % \\
% P(y_{i} = 1) = p_i^1 (1-p_i)^{1-1} = p_{i} % \\
% P(y_{i} = 0) = p_i^0 (1-p_i)^{1-0} = 1- p_{i} %
$$

A probabilidade de observar uma amostra e dada pelo produto das probabilidades individuais:

$$
P(y_{1}, ..., y_{n} ) = \prod^{n}_{i=1}{% p_i^{y_{i}}(1-p_i)^{1-y_{i}} %}
$$

Sabe-se que $L (\theta;x)= P(x;\theta)$, portanto,

$$
L (\beta;x) = \prod^{n}_{i=1}{% p_i^{y_{i}} (1-p_i)^{1-y_{i}} %}
$$
Aplicando $log$ dos dois lados

$$
l (\beta;x) = \sum^{n}_{i=1}{% {y_{i} \log p_i} + ({1-y_{i})\log(1-p_i)} %}
$$

Ao maximizar a função log-verossimilhança, encontra-se os parâmetros $b_{0}, b_{1}, ..., b_{n}$ para os quais a função $L (\beta;x)$ atinge um valor máximo.


### Erro


Na regressão linear pode-se encontrar o erro através dos mínimos quadrados
ordinários (MQO).

$$
    (\hat{y}-y)^2
$$

Já na regressão logística, não é possível utilizar esse mesmo método, é utilizado a *"Binary Cross Entropy"* que pode ser representada pela seguinte expressão:

$$
-y \ log(\hat{y}) -(1-y) \ log(1-\hat{y})
$$

Como na regressão os valores são contínuos, é possivel calcular o erro como a diferença do valor previsto da hipótese e o valor real.

Note que quando $y = 1$, a expressão acima pode ser representada como:

$$
\begin{split}
-1 \ log(\hat{y}) -(1-1) \ log(1-\hat{y}) = \\
-1 \ log(\hat{y}) -(0) \ log(1-\hat{y}) = \\
-1 \ log(\hat{y})
\end{split}
$$

e quando $y = 0$

$$ \begin{array}
= - \ 0 \ log(\hat{y}) -(1-0) \ log(1-\hat{y}) \\
= -1 \ log(1-\hat{y}) \\
\end{array}
$$

O custo total (Logistic Loss) é representado por

$$
\frac{1}{N} \sum_{i=1}^{N}-y \ log(\hat{y}) -(1-y) \ log(1-\hat{y})
$$

Ao minizarmos os erros com a *função de custo*, por não termos mais uma solução única (como no caso da regressão linear), temos de considerar uma curva não-convexa com mínimos e máximos locais. Essa função, no caso da regressão logística, se chama *Logistic Loss*. A diferença reside no fato de, ao sabermos que as probabilidades da função sigmoid são complementares (P=1 - P'), usarmos ambas as função simulâneamente na função Logistic Loss para chegar no mínimo global. Mais em [Loss Function (Part II): Logistic Regression](https://towardsdatascience.com/optimization-loss-function-under-the-hood-part-ii-d20a239cde11).





## Referências

- https://www.marktechpost.com/2019/06/12/logistic-regression-with-a-real-world-example-in-python/
- https://towardsdatascience.com/introduction-to-logistic-regression-66248243c148
- https://dataaspirant.com/2017/03/02/how-logistic-regression-model-works/
- https://g1.globo.com/bemestar/coronavirus/noticia/2020/03/01/coronavirus-por-que-ha-mais-homens-que-mulheres-infectados.ghtml
- https://lamfo-unb.github.io/2017/04/29/Um-Olhar-Descontraido-Sobre-o-Dilema-Vies-Variancia/
- https://towardsdatascience.com/optimization-loss-function-under-the-hood-part-ii-d20a239cde11
