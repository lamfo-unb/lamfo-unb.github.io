---
layout: post
title: Modelos Regressivos Compostos para Estimativas de Preço
lang: pt
header-img: img/ensemble/0020.png
date: 2017-08-17 10:00:00
tags: [regressão, modelos-compostos]
author: Jader Martins
comments: true
---

# Modelos Regressivos Compostos para Estimativas de Preço

Determinar preços de determinados itens antes de sua entrada no mercado é essencial para boa aceitação e consumo. Disponibilizar um produto no mercado abaixo do preço de mercado não te gera bons retornos, mas também um valor muito alto não agrada aos compradores, modelos regressivos nesse caso são de grande ajuda para a tomada de decisão acerca da precificação de um insumo. A performance preditiva de modelos compostos comparados a modelos simples tem sido notável nas mais diversas áreas[^1], modelos simples são aqueles que usam [algoritmos puros do aprendizado de máquina](https://pt.wikipedia.org/wiki/Aprendizado_de_m%C3%A1quina#Abordagens), já modelos compostos combinam as predições de dois ou mais algoritmos na tentativa de melhorar a predição. Nessa postagem buscarei apresentar formas eficientes de combinar modelos para minimizar o erro das predições de preços de metro quadrado de imóveis em Boston.

### Preparando os Dados
Aqui usarei um dataset famoso de preços de casa, mas a técnica aqui abordada pode ser estendida para precificação de quase qualquer coisa. Primeiro importarei e carregarei meu conjunto de dados na variável “boston” utilizando o Pandas, modulo do Python famoso por seus dataframes voltado a analise em finanças. O conjunto de dados advém do módulo Scikit-Learn que usaremos no decorrer desse post para trabalhar com AM, ele forneça ferramentas desde o tratamento dos dados até uma _pipeline_ de aprendizado de máquina. Também usaremos o modulo Numpy.


```python
%matplotlib inline

from sklearn.datasets import load_boston
import numpy as np
import pandas as pd

boston = load_boston()

df = pd.DataFrame(
    np.column_stack([boston.data, boston.target]), 
    columns=np.r_[boston.feature_names, ['MEDV']])
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.00632</td>
      <td>18.0</td>
      <td>2.31</td>
      <td>0.0</td>
      <td>0.538</td>
      <td>6.575</td>
      <td>65.2</td>
      <td>4.0900</td>
      <td>1.0</td>
      <td>296.0</td>
      <td>15.3</td>
      <td>396.90</td>
      <td>4.98</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.02731</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>6.421</td>
      <td>78.9</td>
      <td>4.9671</td>
      <td>2.0</td>
      <td>242.0</td>
      <td>17.8</td>
      <td>396.90</td>
      <td>9.14</td>
      <td>21.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.02729</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>7.185</td>
      <td>61.1</td>
      <td>4.9671</td>
      <td>2.0</td>
      <td>242.0</td>
      <td>17.8</td>
      <td>392.83</td>
      <td>4.03</td>
      <td>34.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.03237</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>6.998</td>
      <td>45.8</td>
      <td>6.0622</td>
      <td>3.0</td>
      <td>222.0</td>
      <td>18.7</td>
      <td>394.63</td>
      <td>2.94</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.06905</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>7.147</td>
      <td>54.2</td>
      <td>6.0622</td>
      <td>3.0</td>
      <td>222.0</td>
      <td>18.7</td>
      <td>396.90</td>
      <td>5.33</td>
      <td>36.2</td>
    </tr>
  </tbody>
</table>
</div>



Aqui carrego meus dados na variável df e mostro as 5 primeiras linhas com o comando head.

Temos informações como criminalidade da região, idade média da população, etc..
Embora não seja o foco dessa postagem, a distribuição dos nossos dados poderá causar grande dificuldade para nosso regressor modela-la, sendo assim aplicarei uma "feature engineering" simples para tornar nossa distribuição mais normal, em posts futuros será explicado em detalhes o que é feature engineering e como utiliza-la para melhorar suas predições. Primeiro vamos ver como está a distribuição que queremos prever ao lado da distribuição "normalizada" por f.log(x+1), (acrescentar um ao valor nos evita ter problemas com zeros).


```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(style="whitegrid", palette="coolwarm")
```


```python
df.plot.box(figsize=(12,6), patch_artist=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f4ab6772cf8>




![png](/img/ensemble/output_6_1.png)


Primeiro carrego as bibliotecas de gráfico que utilizarei no decorrer do texto, defino configurações como estilo e paleta de cores para o gráfico, em seguida monto um dataframe _prices_ para receber duas colunas de valores, uma com o preço sem transformação, outra com o preço tranformado pela função log1p (f.log(x+1)).


```python
prices = pd.DataFrame({"Preço":df["MEDV"], "log(Preço + 1)":np.log1p(df["MEDV"])})

prices.hist(color="#F1684E", bins=50)
plt.ylabel("Quantidade")
```




    <matplotlib.text.Text at 0x7f4ab6535e10>




![png](/img/ensemble/output_8_1.png)


Podemos ver que nossa distribuição ficou menos espaçada e um pouco mais próxima de uma distribuição normal, mas o Python conta com uma função estatística que nos ajuda avaliar se isso será necessário ou não, através do teste de Box-Cox que terá indícios com o grau de Obliquidade (Skewness).


```python
from scipy.stats import skew

for col in df.keys():
    sk = skew(df[col])
    if sk > 0.75:
        print(col, sk)
```

    CRIM 5.222039072246122
    ZN 2.219063057148425
    CHAS 3.395799292642519
    DIS 1.0087787565152246
    RAD 1.0018334924536951
    LSTAT 0.9037707431346133
    MEDV 1.104810822864635


#### Um Pouco de Feature Engeneering
O teste de Box-Cox[^5] nos diz que um skew acima de 0.75 pode ser linearizado pela função log(x+1), fazendo a distribuição ficar mais normalizada, abaixo disso posso manter o valor como estava sem necessidades de modificação, vamos olhar o antes e depois de aplicar essa função a nossas distribuições. (Suprimi algumas variáveis para não poluir demais o gráfico).


```python
dfnorm = (df - df.mean()) / (df.std())
for x in ["CRIM", "ZN", "CHAS","MEDV"]:
    sns.kdeplot(dfnorm[x])
plt.title("Distribuição Valor Médio")
plt.xlabel("Preço")
plt.ylabel("Quantidade")
```




    <matplotlib.text.Text at 0x7f4ab63bfe80>




![png](/img/ensemble/output_12_1.png)



```python
for col in df.keys():
    sk = skew(df[col])
    if sk > 0.75:
        df[col] = np.log1p(df[col])
```


```python
dfnorm = (df - df.mean()) / (df.std())
for x in ["CRIM", "ZN", "CHAS","MEDV"]:
    sns.kdeplot(dfnorm[x])
plt.title("Distribuição Valor Médio")
plt.xlabel("Preço")
plt.ylabel("Quantidade")
```




    <matplotlib.text.Text at 0x7f4ab62d1240>




![png](/img/ensemble/output_14_1.png)


Vemos que as distribuições ficaram muito mais centradas e tendendo a distribuição gaussiana[^2], o que será excelente para o ajuste dos nossos estimadores[^3]. Sendo a função logarítmica e a função f.x+1 bijetoras, poderemos retornar ao nosso valor original assim que acabarmos o ajuste do modelo.


#### Simplificando nossos dados

Nossos dados ainda podem estar muito complexos, a escala em que se encontram e talvez um excesso de informação necessária podem impossibilitar que nosso modelo atinja a perfeição. Aqui iremos aplicar duas técnicas, a primeira e escalonamento de variáveis pelo máximo-mínimo, transformação que também é reversível é poderá ser desfeita ao preço final, bastando eu guardar as variáveis da minha transformação.


```python
df.std()
```




    CRIM         1.020192
    ZN           1.620831
    INDUS        6.860353
    CHAS         0.176055
    NOX          0.115878
    RM           0.702617
    AGE         28.148861
    DIS          0.413390
    RAD          0.751839
    TAX        168.537116
    PTRATIO      2.164946
    B           91.294864
    LSTAT        0.539033
    MEDV         0.386966
    dtype: float64



É visível que algumas variáveis estão extremamente dispersas, podemos mudar isso com a seguinte formula 

$$ z_i=\frac{x_i-\min(x)}{\max(x)-\min(x)} $$

Assim nossas variáveis estarão entre zero e um, ficando mais simplificada a predição.


```python
dfmin, dfmax = df.min(), df.max()
df = (df - df.min())/(df.max()-df.min())
df.std()
```




    CRIM       0.227050
    ZN         0.351200
    INDUS      0.251479
    CHAS       0.253994
    NOX        0.238431
    RM         0.134627
    AGE        0.289896
    DIS        0.227300
    RAD        0.297672
    TAX        0.321636
    PTRATIO    0.230313
    B          0.230205
    LSTAT      0.202759
    MEDV       0.180819
    dtype: float64



Excelente!!

#### Tudo Pronto

Finalizado nosso ajuste nos dados após tanto trabalho vamos agora para o ajuste dos nossos modelos, acostume-se, tratar os dados é o que lhe consumirá mais tempo em um processo de aprendizado de máquina. Mas por fim vamos dar uma olhada final em como eles ficaram distribuídos. Usarei a função interna do Pandas, boxplot, se tem dúvida do que esse gráfico representa, veja [aqui](https://pt.wikipedia.org/wiki/Diagrama_de_caixa).


```python
df.plot.box(figsize=(12,6), patch_artist=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f4ab60049b0>




![png](/img/ensemble/output_20_1.png)


Como já discutido em outras postagens, devemos separar os dados em um conjunto de treino e teste, para treinar nosso modelo e para saber quão bem nosso modelo irá prever para casos desconhecidos. Leia [essa publicação](/2017/04/29/Um-Olhar-Descontraido-Sobre-o-Dilema-Vies-Variancia/) para entender melhor.

Aqui usamos a função interna do Scikit-Learn para separar os dados, para informações adicionais sobre todas as variáveis das funções abaixo sugiro consultar a [documentação oficial](http://scikit-learn.org/stable/documentation.html). Como primeiro argumento passo meu X, atributos, e segundo argumento meu y, valor que eu desejo prever, por fim passo um inteiro para tornar meus resultados reprodutíveis, tornando os processos aleatórios das funções não-aleatórios.


```python
from sklearn.model_selection import train_test_split
xtrain, xtest, ytrain, ytest =\
    train_test_split(df.drop('MEDV',1).values, df['MEDV'].values, random_state=201)
```

Agora importaremos nossos dois modelos, o primeiro é o XGBoost, algoritmo que vem se demonstrando extremamente eficiente em competições e o Ridge famoso algoritmo regressor. Iremos avaliar nossos modelos pelo [erro médio quadrático](https://pt.wikipedia.org/wiki/Erro_quadr%C3%A1tico_m%C3%A9dio).


```python
import xgboost as xgb
from sklearn.linear_model import Ridge

from sklearn.metrics import mean_squared_error
```

Aqui executo uma pequena melhoria nos hiperparametros com o GridSearchCV para buscar a combinação dos hiperparametros que me dará uma melhor predição, em seguida ajusto meu modelo aos dados e tendo ele treinando, prevejo para dados que ele desconhece, em seguida avalio o desempenho do modelo como dito.


```python
from sklearn.model_selection import GridSearchCV

params = {'alpha': np.linspace(0.1,1,200),
          'random_state':[2020]}

model1 = GridSearchCV(estimator = Ridge(), param_grid = params)
model1.fit(xtrain,ytrain)
linpred = model1.predict(xtest)

err1 = mean_squared_error(linpred, ytest)
print(err1)
```

    0.00736161092505



```python
params = {'reg_alpha': np.linspace(0,1,10),
          'gamma': np.linspace(0,1,1),
          'reg_lambda': np.linspace(0,1,1)}

model2 = GridSearchCV(estimator = xgb.XGBRegressor(), param_grid = params)
model2.fit(xtrain, ytrain)
xgbpred = model2.predict(xtest)

err2 = mean_squared_error(xgbpred, ytest)
print(err2)
```

    0.00526337776169


Resultados muito bons, mas será que podemos deixá-los ainda melhor?!
Vamos analisar se as nossas predições têm baixa correlação.


```python
predictions = pd.DataFrame({"XGBoost":np.expm1(xgbpred), "Ridge":np.expm1(linpred)})
predictions.plot(x = "XGBoost", y = "Ridge", kind = "scatter", color="#85C8DD")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f4ab32e67b8>




![png](/img/ensemble/output_30_1.png)


Como já explicado, uma baixa correlação tende a melhorar significativamente nossa predição, visualmente temos algo significante, vamos olhar agora isso em números


```python
from scipy import stats
_, _, r_value, _, std_err = stats.linregress(np.expm1(xgbpred),np.expm1(linpred))
print(r_value, std_err)
```

    0.923252641379 0.0321275120299


Devido nosso r-valor não ser muito alto (<.98), podemos nos beneficiar da combinação das estimativas. Chegamos a parte da motivação inicial combinar os modelos para aumentar o desempenho preditivo. Testarei 3 combinações das predições, média ponderada, media simples e média harmônica.


```python
err3 = mean_squared_error(xgbpred * 0.8 + linpred * 0.2, ytest) # media ponderada
err4 = mean_squared_error((xgbpred + linpred)/2, ytest) # media simples
err5 = mean_squared_error(stats.hmean([xgbpred, linpred]), ytest)# media harmonica
print(err3, err4, err5)
```

    0.00499853754395 0.00524298328056 0.00517761354333


Excelente, ouve uma melhora significativa, mas o quão significativa?


```python
1-err3/err2
```




    0.050317539369457931



Está aí, 5% de melhora do nosso melhor estimador, bem significativo para algo tão simples, e tais aprimoramentos acima de algoritmos de alto desempenho são de extrema importancia no mundo da ciência de dados, talvez até nos ajudaria a pular milhares de posições rumo ao topo em uma competição valendo 1,2 milhões de dólares[^4].

### Concluindo

O objetivo principal dessa publicação era demonstrar que uma combinação simples entre dois modelos podem impactar significamente na sua predição, mas durante esse processo fiz alguns tratamentos nos dados que irão te impressionar sobre o impacto na redução do nosso erro, experimente avaliar os modelos sem realizar alguns dos tratamentos que dei aos dados... Em publicações futuras, será explicado mais sobre cada técnica vista aqui.

#### Referências
[^1]: Polikar, R. (2006). "Ensemble based systems in decision making". IEEE Circuits and Systems Magazine. 6 (3): 21–45. doi:10.1109/MCAS.2006.1688199

[^2]: https://stats.stackexchange.com/questions/298/in-linear-regression-when-is-it-appropriate-to-use-the-log-of-an-independent-va

[^3]: https://stats.stackexchange.com/questions/18844/when-and-why-should-you-take-the-log-of-a-distribution-of-numbers

[^4]: https://www.kaggle.com/c/zillow-prize-1

[^5]: http://www.itl.nist.gov/div898/handbook/eda/section3/eda336.htm

[Inverse Variance](https://en.wikipedia.org/wiki/Inverse-variance_weighting)

[Bootstrap_aggregating Wikipedia](https://en.wikipedia.org/wiki/Bootstrap_aggregating)

[Regularized Linear Models Kernel](https://www.kaggle.com/apapiu/regularized-linear-models)
