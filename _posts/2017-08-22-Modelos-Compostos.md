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

Determinar preços de determinados itens antes de sua entrada no mercado é essencial para boa aceitação e consumo. Disponibilizar um produto no mercado abaixo do preço de mercado não te gera bons retornos, mas também um valor muito alto não agrada aos compradores, modelos regressivos nesse caso são de grande ajuda para a tomada de decisão acerca da precificação de um insumo. A performace preditiva de modelos compostos comparados a modelos puros tem sido notável nas mais diversas áreas[1] e nessa postagem buscarei aprensentar formas eficientes de combinar modelos para minimizar o erro das predições de preços de imóveis em Boston.

### Preparando os Dados
Aqui usarei um dataset famoso de preços de casa, mas a técnica aqui abordada pode ser estendida para precificação de quase qualquer coisa. Primeiro importarei e carregarei meu conjunto de dados na variavel boston utilizando o pandas.


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



Temos informações como criminalidade da região, idade média da população, etc..
Embora não seja o foco dessa postagem, a distribuição dos nossos dados poderá causar grande dificuldade para nosso regressor modela-la, sendo assim aplicarei uma "feature engineering" simples para tornar nossa distribuição mais normal, em posts futuros explicarei em detalhes o que é feature engineering e como utiliza-la para melhorar suas predições. Primeiro vamos ver como está a distribuição que queremos prever ao lado da distribuição "normalizada" por f.log(x+1), (acrescentar um ao valor nos evita ter problemas com zeros).


```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(style="whitegrid", palette="coolwarm")
```


```python
prices = pd.DataFrame({"Preço":df["MEDV"], "log(Preço + 1)":np.log1p(df["MEDV"])})

prices.hist(color="#F1684E", bins=50)
plt.ylabel("Quantidade")
```




    <matplotlib.text.Text at 0x7f93a58cf908>




![png](/img/ensemble/output_5_1.png)


Podemos ver que nossa distribuição ficou menos espaçada e um pouco mais próxima de uma distribuição normal, mas o python conta com uma função estatística que nos ajuda avaliar se isso será necessário ou não, através do teste de skewness.


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
O teste de boxcox nos diz que um skew acima de 0.75 pode ser linearizado pela função log(x+1), vamos olhar o antes e depois de aplicar essa função a nossas distribuições. (Suprimi algumas variáveis para não encher demais o gráfico).


```python
dfnorm = (df - df.mean()) / (df.std())
for x in ["CRIM", "ZN", "CHAS","MEDV"]:
    sns.kdeplot(dfnorm[x])
plt.title("Distribuição Valor Médio")
plt.xlabel("Preço")
plt.ylabel("Quantidade")
```




    <matplotlib.text.Text at 0x7f93a5663e80>




![png](/img/ensemble/output_9_1.png)



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




    <matplotlib.text.Text at 0x7f93a5530ac8>




![png](/img/ensemble/output_11_1.png)


Vemos que as distribuições ficaram muito mais centradas e tendendo a distribuição gaussiana, o que será excelente para o ajuste dos nossos estimadores. Sendo a função logaritimica e a função f.x+1 bijetoras, poderemos retornar ao nosso valor original assim que acabarmos o ajuste do modelo.

#### Correlações?

-defender PCA aqui-
Por isso aumentar linearidade(?) irá nos ajudar a prever melhor nossos valores. Variáveis distorcidas, tendem a piorar nosso modelo, por isso elas também devem ser controladas.


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




```python
corr = df.corr()

mask = np.zeros_like(corr, dtype=np.bool)
mask[np.triu_indices_from(mask)] = True

# Set up the matplotlib figure
f, ax = plt.subplots(figsize=(11, 9))

# Generate a custom diverging colormap
cmap = sns.diverging_palette(220, 10, as_cmap=True)

# Draw the heatmap with the mask and correct aspect ratio
sns.heatmap(corr, mask=mask, cmap=cmap, vmax=.3, center=0,
            square=True, linewidths=.5, cbar_kws={"shrink": .5})
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f93acdd4240>




![png](/img/ensemble/output_14_1.png)


explicar pq precisamos do pca segundo a variavel que obtemos


```python
#fazer pca
```


```python

```

Como já discutido, devemos separar os dados em um conjunto de treino e teste, para treinar nosso modelo e para saber quão bem nosso modelo ira prever para casos desconhecidos.


```python
from sklearn.model_selection import train_test_split
xtrain, xtest, ytrain, ytest =\
    train_test_split(df.drop('MEDV',1).values, df['MEDV'].values, random_state=201)
```

Agora importaremos nossos dois modelos, o primeiro é o XGBoost, algoritmo que vem se demonstrando extremamente eficiente e o Ridge famoso algoritmo regressor. Iremos avaliar nossos modelos pelo r2_score.


```python
import xgboost as xgb
from sklearn.linear_model import Ridge

from sklearn.metrics import mean_squared_error
```

Aqui executo uma pequena melhoria nos hiperparametros com o GridSearchCV.


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

    0.0335608501413



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

    0.0238186511358


Vamos analisar se nossas predições tem baixa correlação


```python
predictions = pd.DataFrame({"XGBoost":np.expm1(xgbpred), "Ridge":np.expm1(linpred)})
predictions.plot(x = "XGBoost", y = "Ridge", kind = "scatter", color="#85C8DD")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f93ae0c8828>




![png](/img/ensemble/output_26_1.png)


Como já explicado, uma baixa correlação tende a melhorar significativamente nossa predição, visualmente temos algo significante, vamos olhar agora isso em números


```python
from scipy import stats
_, _, r_value, _, std_err = stats.linregress(np.expm1(xgbpred),np.expm1(linpred))
print(r_value, std_err)
```

    0.917257969067 0.0348655371974


Devido nosso r-valor não ser muito alto, podemos nos beneficiar da combinação das estimativas.


```python
err3 = mean_squared_error(xgbpred * 0.8 + linpred * 0.2, ytest) # media ponderada
err4 = mean_squared_error((xgbpred + linpred)/2, ytest) # media simples
err5 = mean_squared_error(stats.hmean([xgbpred, linpred]), ytest)# media harmonica
print(err3, err4, err5)
```

    0.0227898468751 0.0240377959853 0.0237927935853



```python
1-err3/err2
```




    0.043193220925209719



Está ai, quase 5% de melhora do nosso melhor estimador, bem significativo para algo tão simples!

https://seaborn.pydata.org/examples/pointplot_anova.html

https://seaborn.pydata.org/examples/many_facets.html

https://seaborn.pydata.org/examples/jitter_stripplot.html

https://seaborn.pydata.org/examples/anscombes_quartet.html

http://matplotlib.org/examples/statistics/boxplot_color_demo.html

#### Referências
[1] Polikar, R. (2006). "Ensemble based systems in decision making". IEEE Circuits and Systems Magazine. 6 (3): 21–45. doi:10.1109/MCAS.2006.1688199

https://stats.stackexchange.com/questions/298/in-linear-regression-when-is-it-appropriate-to-use-the-log-of-an-independent-va

https://stats.stackexchange.com/questions/18844/when-and-why-should-you-take-the-log-of-a-distribution-of-numbers

http://www.itl.nist.gov/div898/handbook/eda/section3/eda336.htm

https://en.wikipedia.org/wiki/Inverse-variance_weighting

https://en.wikipedia.org/wiki/Bootstrap_aggregating

https://www.kaggle.com/apapiu/regularized-linear-models


```python

```
