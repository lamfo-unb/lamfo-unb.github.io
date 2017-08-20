# Introdução básica à Clusterização

## O que é Clusterização?

Clusterização é o agrupamento automático de instâncias similares, uma **classificação não-supervisionada** dos dados. Ou seja, um algorítmo que clusteriza dados classifica eles em conjuntos de dados que 'se assemelham' de alguma forma - independentemente de classes predefinidas. **Os grupos gerados por essa classificação são chamados *clusters***.

Uma forma de clusterização seria, por exemplo, a partir de dados de animais em um zoológico aproximar animais por suas características. Ou seja, a partir dos dados como 'quantidade de pernas', 'quantidade de dentes', 'põe ovo', 'tem pêlos' e vários outros, procuramos animais que estão mais próximos. Poderíamos assim clusterizar os dados, separar animais em mamíferos, aves ou répteis mas sem "contar" ao algorítmo sobre estas classificações. Apenas comparando a distância entre dados o algorítmo mostraria que um tigre "está mais próximo" de um leão do que de uma garça.

As imagens a seguir ilustram uma clusterização bem simples de dados com apenas duas dimensões (duas "características"):

Dados |  Dados agrupados em clusters 
:-------------------------:|:-------------------------:
![](/img/clustering/01-clustering.png)  |  ![](/img/clustering/02-clustering.png)

Muitas vezes, a similaridade entre os dados é encontrada por métricas de distância. Um dos algorítmos mais básicos para Clusterização chama-se **K-Means**.

## K-Means

O algorítmo se chama assim pois **encontra k *clusters* diferentes** no conjunto de dados. O **centro de cada *cluster* será chamado centróide** e terá a média dos valores neste cluster.

A tarefa do algorítmo é encontrar o centróide mais próximo (por meio de alguma métrica de distância) e atribuir o ponto encontrado a esse cluster. Após este passo, os centróides são atualizados sempre tomando o valor médio de todos os pontos naquele cluster. Para este método são necessários valores numéricos para o cálculo da distância, os valores nominais então podem ser mapeados em valores binários para o mesmo cálculo. Em caso de sucesso, os **dados são separados organicamente** podendo assim ser rotulados e **centróides viram referência** para classificar novos dados.

Para o exemplo utilizaremos o [*Dow Jones Index Data Set*](http://archive.ics.uci.edu/ml/datasets/Dow+Jones+Index#) do *UCI Machine Learning Repository*. A partir da flutuação de preços de ações ao longo de um certo período, podemos tentar clusterizar empresas de acordo com seu comportamento no mercado. Com noções deste comportamento e similaridades entre empresas, a clusterização pode contribuir com uma composição e diversificação de uma carteira de ações.


### Passo a passo do algorítmo

O Κ-means aprimora de forma iterativa seus resultados até alcançar um resultado final. O algoritmo recebe o número de clusters Κ e o conjunto de dados sob análise. Em seguida são estabelecidas estimativas iniciais para os K centróides, que podem ser gerados aleatoriamente ou selecionados aleatoriamente dentro conjunto de dados. O algoritmo faz a iteração entre dois passos:

- **Associação de cada instância a um centróide** - cada centróide define um cluster, então cada instância será associada a seu cluster mais semelhante (centróide mais próximo). A distância será calculada por alguma métrica de distância, em geral utiliza-se a distância euclidiana entre as duas instâncias;

- **Atualização dos centróides** - centróides dos clusters são recalculados, a partir da média entre todas as instâncias associadas àquele cluster.

### Na prática com Python

Para que possamos testar o algorítmo utilizaremos a **linguagem Python** e algumas de suas bibliotecas

- Se você ainda não tem Python em sua máquina, dê uma olhada no nosso post [Instalando Python para Aprendizado de Máquina](https://lamfo-unb.github.io/2017/06/10/Instalando-Python/);
- Crie um diretório de trabalho em sua máquina, uma pasta que conterá seu programa e os dados utilizados. Decidi chamar meu diretório de *'learn-clustering'*;
- Faça download do [*dow_jones_index.zip*](http://archive.ics.uci.edu/ml/machine-learning-databases/00312/) no seu novo diretório e descompacte lá mesmo, gerando uma pasta que conterá o arquivo de dados *'dow_jones_index.data'*;
- Crie um novo arquivo python chamado *'k-means-dji.py'* onde escreveremos nosso programa no mesmo diretório principal. Os trechos de código a seguir devem ser codificados dentro do novo arquivo criado.

#### 1. Importando as bibliotecas

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
```

#### 2. Lendo o Dataset

A leitura dos dados é feita a partir da biblioteca Pandas e os dados estão organizados no formato csv (*comma-separated values*) apesar da extensão *'.data'*. Chamaremos o dataset completo de *full_dataset*

```python
full_dataset = pd.read_csv("./dow_jones_index/dow_jones_index.data")
```

Trecho do Dataset:

quarter | stock | date | open | high | low | close | volume | percent_change_price | percent_change_volume_over_last_wk | previous_weeks_volume | next_weeks_open | next_weeks_close | percent_change_next_weeks_price | days_to_next_dividend | percent_return_next_dividend
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---
1 | AA | 1/7/2011 | $15.82 | $16.72 | $15.78 | $16.42 | 239655616 | 3.79267 | NaN | NaN | 16.71 | 15.97 | -4.42849 | 26 | 0.182704
1 | AA | 1/14/2011 | $16.71 | $16.71 | $15.64 | $15.97 | 242963398 | -4.42849 | 1.380223028 | 239655616 | 16.19 | 15.79 | -2.47066 | 19 | 0.187852
1 | AA | 1/21/2011 | $16.19 | $16.38 | $15.60 | $15.79 | 138428495 | -2.47066 | -43.02495926 | 242963398 | 15.87 | 16.13 | 1.63831 | 12 | 0.189994
| ... |
2 | XOM | 6/10/2011 | $80.93 | $81.87 | $79.72 | $79.78 | 92380844 | -1.42098 | 17.50851907 | 78616295 | 80.00 | 79.02 | -1.225 | 61 | 0.58912
2 | XOM | 6/17/2011 | $80.00 | $80.82 | $78.33 | $79.02 | 100521400 | -1.225 | 8.8119524 | 92380844 | 78.65 | 76.78 | -2.37762 | 54 | 0.594786
2 | XOM | 6/24/2011 | $78.65 | $81.12 | $76.78 | $76.78 | 118679791 | -2.37762 | 18.06420424 | 100521400 | 76.88 | 82.01 | 6.67274 | 47 | 0.612139

Podemos perceber que cada coluna é referente a um parâmetro que descreverá aquela ação numa certa data.

#### 3. Pré-processando os dados

Para simplificar nosso exemplo, podemos excluir algumas a coluna 'quarter' que não é relevante para o comportamento das ações no conjunto de dados completo:

```python
dataset = full_dataset.drop('quarter', 1)
```

A coluna de datas também pode ser alterada de forma conter uma contagem de dias para melhor processamento:

```python
dataset['date'] = pd.to_datetime(dataset['date'])
dataset['date'] = (dataset['date'] - dataset['date'].min()) / np.timedelta64(1,'D')
```

As colunas que contém preços também devem ter o cifrão removido:

```python
dataset = dataset.replace({'\$':''}, regex = True)
```



