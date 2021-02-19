---
layout: post
title: Self-organizing maps
lang: pt
header-img: img/home-bg.jpg
date: 2020-09-28 23:59:07
tags: [Self-organizing maps, Images, Machine Learning, Estatística]
author: Stefano Dantas, Neuremberg Matos e Ricardo Pinho
comments: true
---


**Table of Contents**

[TOCM]

[TOC]
# Self-organizing maps

## Definição
O *self-organizing map* (SOM) é um modelo neural não supervisonado, ou seja, não demanda intervenção humana durante o treinamento. Além disso, o SOM utiliza aprendizado competitivo, onde os neurônios recebem padrões de entradas e competem tendo como resultado um vencedor, para cada padrão, que será ativado. Os neurônios vencedores podem representar diversos padrões de entrada formando *clusters* de padrões similares. Após ser determinado vencedor, esse neurônio tem seus pesos ajustados ficando mais próximo do padrão de entrada associado a ele. A repetição dessa lógica resulta em pesos que convergem para a estabilidade atuando como centros de massa dos agrupamentos de padrões de entrada.  
A proposição do algoritmo de *self-organizing maps* (SOM) foi feita pelo acadêmico finlandês Teuvo Kohonen em 1982.  

![](https://hs.mediadelivery.fi/img/468/fc61cf87757b4e90a02b5918d78fe9de.jpg)


## Aplicações

A simplicidade do método em processar parâmetros de entranda mais complexos e agrupá-los em clusters de dimensão menor, normalmente bidimensionais, fez com que o algoritmo tivesse grande aceitação e utilização. Por exemplo, utiliza-se o modelo em meteorologia na visualização de cenários climáticos e na oceanografia para visualização do mapeamento do solo marinho. A imagem abaixo representa a aplicação de *self-organizing map* na meterologia para vizualição de nuvens.

![](https://i.imgur.com/UXBvgxD.png)
Fonte: *Self-Organizing Maps: A Powerful Tool for the Atmospheric Sciences*



## Intuição Matemática

A idéia principal de um mapa auto-organizável é pegar dados multidimensionais e produzir uma representação desses dados em um espaço bidimensional (o "mapa").
Como os dados não têm classes pré-definidas, queremos utilizar um algoritmo que seja capaz de se "auto-organizar". Em outras palavras, como não temos um "supervisor" para indicar a saída esperada, o algoritmo tem de criar uma representação desses dados de acordo com suas características.

Formalmente, queremos encontrar um função que mapeie o espaço de entrada (*input space*) $\mathcal{X}$ pra um espaço de saída bidimensional (*output space*) $\mathcal{Y}$.
$$f: \mathcal{X} \rightarrow \mathcal{Y}, \mathcal{X} \in \mathbb{R}^D, \mathcal{Y} \in \mathbb{R}^2$$


Essa função $f$ realizará esse mapeamento utilizando uma série de parâmetros (pesos) $\mathbf{W}$. Uma das formas mais utilizadas para se aproximar uma função é usando uma rede neural artificial. Esse método é extremamente versátil e poderoso. Porém, a forma clássica de treinar esse modelo é utilizando *backpropagation*. Como não temos classes para utilizar no treinamento, não podemos usar essa técnica.

A grande questão é então como treinar uma rede neural sem usar o *backpropagation*? Isso é feito por meio do aprendizado competitivo. 

Nessa técnica, cada neurônio da rede "compete" com outros neurônios para representar parte dos dados de entrada. Desse modo, com o passar do tempo, certos neurônios se especializam na representação de determinada parte do espaço de entrada dos dados.

### Algoritmo

Suponha que queremos treinar um mapa com dados de entrada $\mathbf{X} = \{X_1,X_2,...,X_N\}$, e cada exemplo tem $D$ parâmetros $X_i = \{x_{i,1},x_{i,2},...,x_{i,D}\}$. O mapeamento será feito por meio de um conjunto de $M$ pesos, onde cada peso tem $D$ parâmetros. Ou seja, $\mathbf{W} = \{W_1,W_2,...,W_M\}, W_i \in \mathbb{R}^D ~\forall i \in {1,...,M}$.

O algoritmo para treinar um mapa auto-organizável é divido em 5 etapas. 

1. Inicializar os neurônios com pesos aleatórios e o número da iteração $n = 0$
2. Selecionar uma entrada $X_i aleatoriamente$
3. Calcular a distância entre $X_i$ e todos os pesos $\mathbf{W}$. O peso com menor distância é selecionado $L(X)$.
4. Atualizar os pesos usando $$W_v(n+1) = W_v(n) + \eta(n)h_{j,L(X)}(n)(X_i-W_v)$$
5. Incrementar $n$, repetir passos 2 à 5 até o número de iterações desejado


A atualização dos pesos $\Delta W$ é composta de dois termos que merecem ser comentados mais detalhadamente.

O primeiro termo é a taxa de aprendizado $\eta$. No começo do treinamento, todos os pesos são iniciados aleatoriamente. Logo, é desejado que os pesos sejam atualizados mais rapidamente no começo e, a medida que o treinamento vai progredindo, as atualizações sejam menores. A taxa de aprendizado é decrescida após cada iteração seguindo um decaimento exponencial:

$$\eta(n) = \eta_0 exp\left(\frac{-n}{\tau_\eta}\right),$$

em que $\eta_0$ é a taxa de aprendizado inicial e $\tau_\eta$ é uma constante que controla o decaimento.

O segundo termo é a função de vizinhança $h_{j,L(X)}(n)$, definida como:

$$h_{j,L(X)}(n) = exp\left(\frac{-d^2_{j,L(X)}}{2 \sigma(n)^2}\right),$$


em que $d_{j,L(X)}$ representa a distância euclidiana entre o neurônio $W_j$ e o neurônio $L(X)$. Lembre-se que o neurônio $L(X)$ é o neurônio mais próximo do $X_i$ sorteado no passo 2 do algoritmo. Formalmente, podemos definir esse neurônio como:

$$L(X) = W_k, ~k=\underset{j}{argmin}||X - W_j||_2$$

Se observamos a equação de $h_{j,L(X)}(n)$, podemos notar que essa equação remete a uma distribuição normal centrada em zero e desvio padrão $\sigma(n)$. Quanto maior a distância do neurônio $W_j$ em relação ao neurônio $L(X)$, menor o valor de $h_{j,L(X)}(n)$. Desse modo, neurônios distantes entre si têm menos influência na atualização de seus respectivos pesos. A medida que o número de iterações aumenta, a largura dessa distribuição vai diminuindo seguindo um decaimento exponencial $\sigma(n) = \sigma_0 exp(\frac{-n}{\tau_0})$, similar a taxa de aprendizado. Ou seja, as atualizações vão ficando cada vez mais restritas à vizinhança do peso em questão.

### Exemplo

Para ilustrar o algoritmo, considere o seguinte exemplo onde temos 3 neurônios e 2 dados de entrada $X_1 = [1,1,2],X_2=[3,5,1]$. 

Os pesos foram iniciados aleatoriamente.


![](https://i.imgur.com/DphJaUB.png)


Suponha que selecionamos $X_1$ na primeira iteração. O próximo passo é então medir a distância entre $X_1$ e todos os outros pesos. 

Calcula-se então $d_{X_1,W_1},d_{X_1,W_2},d_{X_1,W_3}$. Os valores encontrados são $d_{X_1,W_1} = 1.80,d_{X_1,W_2}=1.48,d_{X_1,W_3}=1.87$. Logo, o neurônio mais próximo de $X_1$ é $W_2 = L(X)$.

Os pesos então são atualizados seguindo as seguinte equações:
$$\Delta W_1 = \eta(1)h_{1,2}(1)(X_1-W_1)$$
$$\Delta W_2 = \eta(1)h_{2,2}(1)(X_1-W_2)$$
$$\Delta W_3 = \eta(1)h_{3,2}(1)(X_1-W_3)$$

Incrementa-se $n$ e o processo de treinamento é repetido.

### Representação em duas dimensões


Tendo a distância entre neurônios e dados de entrada, conseguimos representar essas informações em um espaço bidimensional.

Suponha $d_{X_1,W_3} = 1, d_{X_1,W_2} = 1.5, d_{W_2,W_3} = 2$.

Podemos representar essas distância no gráfico abaixo:


![](https://i.imgur.com/NJFIgwN.png)


Desse modo, conseguimos transformar dados multidimensionais em distâncias e, consequentemente, em uma representação bidimensional.

## Usando SOM para visualizar em Python a facilidade de abrir negócios no mundo

Inicialmente vamos carregar os pacotes necessários. Além do *pandas* vamos carregar a função `scale` do *sklean* para transformar as variáveis para o mesmo range de valores, a classe `MiniSom` do biblioteca *minisom* para realizar o treinamento do *Self-organizing maps*, por fim, a classe `Mapa` que foi criada por nós para plotar os mapas.

A biblioteca *MiniSom* é uma implementação minimalista do algoritmo *Self-organizing maps*, você pode encontrar a página da biblioteca [aqui](https://github.com/JustGlowing/minisom) e vários exemplos de uso [aqui](https://github.com/JustGlowing/minisom/tree/master/examples). Esta aplicação e a classe `Mapa` são fortemente baseadas no exemplo *Democracy Index*.



```python
import pandas as pd
from sklearn.preprocessing import scale
from minisom import MiniSom

from modulos.mapa import Mapa
```

Agora que importamos as bibliotecas necessárias, vamos carregar os dados que serão usados. A seguir são apresentadas as primeiras 5 linhas dos dados:


```python
path_data = 'data/processed/start-business.csv'
read_params = {'sep': ';', 'decimal': ',', 'encoding': 'cp1252'}
dados = pd.read_csv(path_data, **read_params)
dados.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>country_code</th>
      <th>economy</th>
      <th>region</th>
      <th>income_group</th>
      <th>facility_group</th>
      <th>score</th>
      <th>procedures_man</th>
      <th>time_men</th>
      <th>cost_men</th>
      <th>procedures_woman</th>
      <th>time_woman</th>
      <th>cost_woman</th>
      <th>paid_in_min</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>Afghanistan</td>
      <td>South Asia</td>
      <td>Low income</td>
      <td>High facility</td>
      <td>92.04130</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>6.4</td>
      <td>5.0</td>
      <td>9.0</td>
      <td>6.4</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ALB</td>
      <td>Albania</td>
      <td>Europe &amp; Central Asia</td>
      <td>Upper middle income</td>
      <td>High facility</td>
      <td>91.70203</td>
      <td>5.0</td>
      <td>4.5</td>
      <td>11.3</td>
      <td>5.0</td>
      <td>4.5</td>
      <td>11.3</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DZA</td>
      <td>Algeria</td>
      <td>Middle East &amp; North Africa</td>
      <td>Upper middle income</td>
      <td>Low facility</td>
      <td>77.94755</td>
      <td>12.0</td>
      <td>18.0</td>
      <td>11.8</td>
      <td>12.0</td>
      <td>18.0</td>
      <td>11.8</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AGO</td>
      <td>Angola</td>
      <td>Sub-Saharan Africa</td>
      <td>Lower middle income</td>
      <td>Low facility</td>
      <td>79.04553</td>
      <td>8.0</td>
      <td>36.0</td>
      <td>13.9</td>
      <td>8.0</td>
      <td>36.0</td>
      <td>13.9</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ATG</td>
      <td>Antigua and Barbuda</td>
      <td>Latin America &amp; Caribbean</td>
      <td>High income</td>
      <td>Lower middle facility</td>
      <td>81.74620</td>
      <td>9.0</td>
      <td>22.0</td>
      <td>8.7</td>
      <td>9.0</td>
      <td>22.0</td>
      <td>8.7</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


O conjunto de dados é oriundo de uma pesquisa do Banco Mundial sobre a facilidade de fazer negócios na maioria dos países do mundo. Nesse exemplo, pegamos apenas os dados relacionados com a facilidade de abrir um negócio para o ano de 2019, mas você pode encontrar a pesquisa completa [aqui](https://www.doingbusiness.org/) com todos os detalhes.

Nesta base, temos 13 variáveis das quais 8 são numéricas. A variável *score* mede a facilidade de construir negócio por cada país e é construída a partir das demais variáveis numéricas. Quanto maior a variável *score* maior a facilidade de abrir um negócio. Para esses dados, a Nova Zelândia ficou em primeiro lugar, já o Brasil ficou na posição 149 de 203 países em ordem decrescente de facilidade de abrir um negócio. A última posição é ocupada pela Venezuela.

O principal objetivo dessa aplicação é visualizar a posição relativa dos países com base nas sete variáveis que compõem o índice de facilidade de abrir negócios e verificar se as posições relativas dos países estão relacionadas com o índice.

Para isso, vamos selecionar apenas as variáveis que compõem o índice de facilidade e colocar essas variáveis na mesma escala.


```python
features = ['procedures_man', 'time_men', 'cost_men', 'procedures_woman', 'time_woman', 'cost_woman','paid_in_min']
labels_contry = dados['country_code']
X = dados[features].values
X = scale(X)
```

Agora vamos treinar o *Self-organizing maps* com um mapa $15X15$ usando uma função de vizinhança *gaussiana* e treinando $1.000$ *epochs*.


```python
size = 15
som = MiniSom(x=size, y=size, input_len=len(X[0]), sigma=1.5, neighborhood_function='gaussian', random_seed=1)
som.pca_weights_init(X)
som.train_random(X, 1000, verbose=True)

```

     [ 1000 / 1000 ] 100% - 0:00:00 left 
     quantization error: 0.4487733862565572
    

Vamos agora instanciar a classe `Mapa` para plotar os mapas:


```python
mapa = Mapa(som, X, labels_contry, size)
```

No gráfico vamos plotar a posição relativa dos países e atribuir uma cor para cada país de acordo com a variável `facility_group`. Essa variável foi construída a partir dos quartis da variável `score`: os países pertencentes ao primeiro quartil dessa variável foram classificados como *Low facility*, para indicar a baixa facilidade de abrir um novo negócio, e assim sucessivamente até o quarto quartil, no qual os países pertencentes a eles foram classificados como *High facility*.


```python
facility_colors = {
    'Low facility': '#DC143C', 'Lower middle facility': '#FFA500', 'Upper middle facility': '#9370DB',
    'High facility': '#4B0082'
}
country_start_colors = [facility_colors[facility] for facility in dados['facility_group'].to_list()]
```


```python
mapa.plot(country_start_colors, facility_colors)
```


![](https://i.imgur.com/ZsuazxB.png)


A posição relativa de cada país no gráfico é determinada pela proximidade de cada país no conjunto de dados original com sete variáveis. Assim, estamos representando um conjunto de dados de dimensão mais alta em um gráfico de duas dimensões.

De modo geral o gráfico mostra que o índice de facilidade de fazer negócio caracteriza bem as distâncias relativas entre os países. Isto porque os países que estão no mesmo grupo de facilidade de fazer negócios aparecem mais próximos entre si formando *clusters*.

A preenchimento de fundo do gráfico representa a matriz de dissimilaridade entre da região em relação às observações que a rodeiam. Essa matriz pode ser usada para clusterizar as observações no mapa por meio de clusterização hierárquico ou mesmo o *Kmeans*, assim, as áreas mais escuras forneceriam as fronteiras entre os clusters.

No gráfico a seguir vamos plotar a posição relativa dos países e atribuir uma cor para cada país de acordo com a variável `icome_group`. Essa variável é uma classificação de países do Banco Mundial de acordo com o nível de renda, os países são classificados como:

* **Low income**: Países de renda baixa;
* **Lower middle income**: Países de renda média baixa;
* **Upper middle income**: Países de renda média alta, o Brasil se encontra nesse grupo;
* **High income**: Países de renda alta;

Desta forma, o gráfico abaixo irá mostrar a relação entre o nível de renda e as posições relativas entre os países:

```python
income_colors = {
    'Low income': '#DC143C', 'Lower middle income': '#FFA500', 'Upper middle income': '#9370DB', 'High income': '#4B0082'
}
country_income_colors = [income_colors[income] for income in dados['income_group'].to_list()]
```


```python
mapa.plot(labels_colors=country_income_colors, legend_colors=income_colors)
```

![](https://i.imgur.com/NBUtjIE.png)


Diferentemente do primeiro gráfico, a relação entre a variável `income_group` e a posição relativa dos países não é tão clara. Isto significa que relação entre essa variável a facilidade de abrir um novo negócio não é tão direta. O que é esperado, uma vez que o nível de renda de um país é o resultado de um período relativamente longo de tempo em comparação ao tempo necessário para mudar a facilidade de abrir um negócio.

Em outras palavras, mesmo que a facilidade de abrir um novo negócio afetasse diretamente o nível de renda, caso esse efeito não produza efeitos imediatos é razoável esperar que o gráfico acima não capture esse efeito entre as variáveis.

Aqui encerra o nosso post. Os códigos e os dados usados nessa aplicação estão no nosso repositório no [GitHub](https://github.com/lamfo-unb/self-organizing-maps).

