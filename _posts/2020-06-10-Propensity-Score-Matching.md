---
layout: post
title: Propensity Score Matching
lang: pt
header-img: img/home-bg.jpg
date: 2020-06-10 23:59:07
tags: [Propensity Score Matching, Matching, Propensity Score, Estatística]
author: Cayan Portela, João Pedro Fontoura da Silva, Matheus Kempa Severino
comments: true
---


**Table of Contents**

[TOCM]

[TOC]

# Parte I – Estatística experimental e estatística observacional


Uma aplicação de ferramentas estatísticas é a avaliação da eficácia de uma intervenção, tratamento ou política de ação. Ao separar os indivíduos de nossa amostra em dois grupos, trata-los de forma distinta, e estuda-los com respeito aos resultados obtidos, podemos inferir em como atua o tratamento em questão. Em um simples caso binário, temos um grupo que receberá o tratamento – denominado grupo de tratamento – e outro que não receberá o tratamento – o grupo de controle. Ao comparar os resultados ao fim de um determinado período e aplicar as devidas ferramentas estatísticas, poderemos determinar quão bem sucedido é o tratamento, dada a amostra em questão.

Como os dados chegam ao pesquisador? Eles podem ser provenientes de experimentos ou pela observação, caso no qual as variáveis independentes (isto é, explicativas) não estão sob controle do pesquisador. Um detalhe de grande importância quando da realização de estudos estatísticos é garantir a minimização de vieses, que podem surgir de várias formas. No caso aqui em discussão, uma fonte de viés pode surgir do modo como cada indivíduo é escolhido/alocado a pertencer a determinado grupo de estudo. Uma alocação enviesada pode ocasionar erros de inferência, já que a amostra não é representativa da população que se quer analisar, consequentemente tornando os resultados menos confiáveis, ou mesmo falsos, do ponto de vista estatístico. Queremos, portanto, encontrar modelos que auxiliem a solucionar esse problema de viés de seleção e que possam providenciar estimativas válidas do efeito médio de tratamento (Average Treatment Effect – ATE).

Randomized Control Trials (RCT) são experimentos nos quais a alocação é randomizada – isso significa que a probabilidade do indivíduo pertencer ao grupo de controle ou ao grupo de tratamento é a mesma (50%) em nosso exemplo binário. A garantia de randomização diminui o risco de viés na análise, assim como de associações espúrias, já que temos uma maior certeza em afirmar que os grupos são parecidos em tudo, exceto sua exposição ao tratamento. Contudo, nem sempre a realização de um RCT é possível: os custos podem ser proibitivamente elevados, e em muitos casos o tema de estudo esbarra em questões éticas, inviabilizando a realização de experimentos.



# Parte II – O Propensity Score Matching

Quando não é possível randomizar a alocação dos indivíduos aos grupos, como no caso de estudos observacionais, não se pode realizar inferências causais, já que não é possível determinar se a diferença de resultados entre os grupos de tratamento e controle são provenientes de diferenças “ao acaso” entre os indivíduos participantes do estudo ou se são diferenças sistemáticas.

Devido a essas limitações, o Propensity Score Matching (PSM) é uma alternativa para estimar os efeitos causais de receber o tratamento em uma amostra de indivíduos. O método PSM foi introduzido na literatura por Paul Rosenbaum e Donald Rubin em 1983; a ideia é de atribuir uma probabilidade de receber o tratamento a cada indivíduo da amostra – o propensity score (PS) –, controlando para suas características observadas, e depois parear unidades de ambos os grupos com propensity scores similares, para em seguida comparar os resultados obtidos entre os pares. O modelo pode ajudar a solucionar o problema de viés de seleção e providenciar estimativas não enviesadas do efeito médio do tratamento.

O propensity score é uma probabilidade condicional de que o participante de um estudo receba o tratamento dadas as covariáveis observadas; assim, não só aos participantes que de fato receberam o tratamento lhes é atribuído um propensity score, mas àqueles que não receberam também lhes é atribuída essa probabilidade. Além disso, condicionando para o PS, cada participante apresenta a mesma probabilidade de alocação para o tratamento, assim como em um experimento randomizado. O propensity score pode ser encarado como um balancing score (cujo conceito é explicado mais à frente) representante de um conjunto de covariáveis; deste modo, um par de indivíduos do grupo de controle e de tratamento que apresentem um propensity score similar podem ser comparáveis, mesmo que apresentem diferenças nas covariáveis específicas.

Como dito anteriormente, o propensity score é uma medida de equilíbrio que sumariza a informação do vetor de covariáveis. Participantes do grupo de controle e do grupo de tratamento com o mesmo valor do propensity score têm a mesma distribuição das covariáves observadas; ou seja, em um set de pares que é homogêneo para o PS, ambos os indivíduos podem apresentar valores diferentes para as covariáveis, mas essas diferenças serão puramente por acaso, e não diferenças sistemáticas.


# Parte III – Exemplos simples

O PSM pode ser aplicado a uma ampla gama de campos de estudo, desde análises na biologia e medicina, até mesmo em aplicações nas ciências sociais. Sejam os seguintes exemplos:
1. Queremos avaliar a eficácia de um determinado tratamento médico em meio a uma amostra de pacientes que padeceram de uma doença. Ao seguir a metodologia do PSM, atribuiremos Z=1 àqueles pacientes que receberam o tratamento, e Z=0 àqueles que não receberam. Ao compararmos pacientes com características semelhantes, poderemos determinar o grau de sucesso do tratamento.
2. Queremos avaliar a eficácia da adoção do regime de metas de inflação no combate à inflação. Seja a variável binária Z; Z=1 para países que adotam esse regime de metas, e Z=0 para aqueles que não adotam. Ao aplicarmos a metodologia do PSM, poderemos comparar países em cada grupo semelhantes quanto às variáveis independentes escolhidas, de forma a determinar o grau de sucesso dessa política em reduzir a taxa de inflação.

# Parte IV – Run-through metodológico simples

Criamos aqui então um **exemplo hipotético**, mas atual para que pudéssemos entender aplicações do PSM. Imagine que queiramos entender o efeito do tratamento de da Cloroquina no tempo de recuperação de pacientes que tiveram o COVID-19 afim de enterdemos se essa intervenção/tratamento realmente tem eficácia.

Com isso criamos uma** base fake** com as seguintes colunas:

* Febre (X) - Sintoma
* Idade (X) - Sintoma
* Sexo (X) - Sintoma
* Dor no peito (X) - Sintoma
* Dificuldade Respirar (X) - Sintoma
* **Cloroquina (A) - Intervenção**
* **Tempo Recuperação (Y) - Variável a ser analisada**

## Passo 1 - Importar as bibliotecas
***
Mesmo importando algumas bibliotecas aqui agora, estarei importando ao longo do código para facilitar a compreensão, mas já importarei algumas bibliotecas que podem ser necessárias no trato das informações que usaremos


```python
import pandas as pd
import numpy as np
from sklearn.linear_model import LogisticRegression
%matplotlib inline
```
## Passo 2 - Bases + Logistic Regression
***

Para calcular o propensity score, podemos fazer uso de um modelo de regressão logística ou de um modelo probit. Adicionamos covariáveis observadas apropriadas no modelo e com isso obtemos uma probabilidade de exposição ao tratamento para toda a amostra. É importante notar que, quando da construção do modelo, as variáveis a serem incluídas não devem ser efeitos do tratamento, e sim preditoras do resultado; podemos incluir termos de interação e não precisamos nos importar com parcimônia ou perda de graus de liberdade.


>Mas antes vamos estar tratando algumas informações para enfim aplicarmos a  regressão logística


```python
base = pd.read_excel("Oficina_PSM.xlsx") #abre a base

## Tratamos o nosso X
x = base[["Febre_(X)",'Idade_(X)','Sexo_(X)',"Dor_no_peito_(X)",'Dificuldade_Respirar_(X)']] #escolhe as colunas que queremos
X = x.values #transformamos em matriz



## Tratamos o nosso Y
y = base[["Cloroquina_(A)"]] #escolhe as colunas que queremos até a intervenção
y = y.values #transformamos em vetor

x.describe() #descreve a base
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
      <th>Febre_(X)</th>
      <th>Idade_(X)</th>
      <th>Sexo_(X)</th>
      <th>Dor_no_peito_(X)</th>
      <th>Dificuldade_Respirar_(X)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>100.000000</td>
      <td>100.00000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.470000</td>
      <td>55.45000</td>
      <td>0.490000</td>
      <td>0.560000</td>
      <td>0.580000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.501614</td>
      <td>20.39973</td>
      <td>0.502418</td>
      <td>0.498888</td>
      <td>0.496045</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>20.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>38.75000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>54.00000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>73.25000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>90.00000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>

Antes de estimar os efeitos do tratamento, temos de ajustar os propensity scores para diferenças entre os grupos de tratamento. Devemos checar se os propensity scores nos permitem balancear a distribuição das variáveis explicativas com respeito aos dois grupos; um balancing score (b(x)) é uma função das covariáveis (x) tal que a distribuição condicional das covariáveis dado b(x) é similar para ambos os grupos de estudo (de tratamento e de controle). Isso pode ser observado em um gráfico de distribuição das variáveis explicativas dentro de quintis do propensity score para ambos os grupos.

Um conceito importante é o de forte ignorabilidade (strong ignorability); se a alocação ao tratamento é fortemente ignorável dadas as covariáveis, então os resultados são condicionalmente independentes da variável binária Z (que indica a exposição ou não ao tratamento) dadas as variáveis explicativas. Feito isso aplicamos a regressão logística e teremos o scores.

```python
clf = LogisticRegression(random_state=0).fit(X, y) #define o modelo

score = pd.DataFrame(clf.predict_proba(X), columns = (["P(0)","P(1)"])) #estimamos os scores

score = score[["P(1)"]] #utilizamos somente o score referente a probabilidade do tratamento

base_identidade = pd.concat([base, score], axis=1)#concateno as bases
base_score = base_identidade[["P(1)","Cloroquina_(A)"]] #crio uma base específica
base_tempo = base_identidade[["P(1)","Cloroquina_(A)","Tempo_Recuperação_(Y)"]] #crio uma base específica
base_identidade
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
      <th>Febre_(X)</th>
      <th>Idade_(X)</th>
      <th>Sexo_(X)</th>
      <th>Dor_no_peito_(X)</th>
      <th>Dificuldade_Respirar_(X)</th>
      <th>Cloroquina_(A)</th>
      <th>Tempo_Recuperação_(Y)</th>
      <th>P(1)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>78</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>26</td>
      <td>0.215053</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>88</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>20</td>
      <td>0.434960</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>36</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>14</td>
      <td>0.512854</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>69</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0.313836</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>84</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>11</td>
      <td>0.459133</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>1</td>
      <td>81</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>22</td>
      <td>0.581955</td>
    </tr>
    <tr>
      <th>96</th>
      <td>1</td>
      <td>74</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>13</td>
      <td>0.386607</td>
    </tr>
    <tr>
      <th>97</th>
      <td>0</td>
      <td>40</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>11</td>
      <td>0.501556</td>
    </tr>
    <tr>
      <th>98</th>
      <td>1</td>
      <td>21</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>12</td>
      <td>0.645221</td>
    </tr>
    <tr>
      <th>99</th>
      <td>0</td>
      <td>45</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>23</td>
      <td>0.374955</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 8 columns</p>
</div>


### Overlap
***
Então analisamos como estão os nossos dados através da etapa do Overlap. Um bom overlap significa que em qualquer lugar que olhemos da imagem teremos chance de acontecimento para os dois casos. E podemos ver que nesse exemplo ficou um pouco assim, pois se pegarmos qualquer lugar no gráfico teremos elementos para de ambos conjuntos, como veremos no gráfico mais abaixo.

```python
selecao_tratados = base_score.loc[base_score['Cloroquina_(A)'].isin([1])] #filtro a base
selecao_tratados_tempo = base_tempo.loc[base_score['Cloroquina_(A)'].isin([1])] #filtro a base
selecao_tratados = selecao_tratados[["P(1)"]] #renomeio a coluna

selecao_n_tratados = base_score.loc[base_score['Cloroquina_(A)'].isin([0])] #filtro a base
selecao_n_tratados_tempo = base_tempo.loc[base_score['Cloroquina_(A)'].isin([0])] #filtro a base
selecao_n_tratados = selecao_n_tratados[["P(1)"]] #renomeio a coluna


print("Tratados %s e Não tratados:%s"% (selecao_tratados.shape[0],selecao_n_tratados.shape[0]))


```

 Output:   Tratados 45 e Não tratados:55
    


```python
import seaborn as sns
import matplotlib.pyplot as plt


plt.rcParams['figure.figsize'] = 10, 6 #arruma fonte
plt.rcParams['font.size'] = 13 #arruma fonte

sns.distplot(selecao_n_tratados, label='Não Tratados') #não tratados
sns.distplot(selecao_tratados, label='Tratados') ##tratados
plt.xlim(0, 1) #escala
plt.title('Ditribuição do Propensity Score para Não tratados vs Tratados')
plt.ylabel('Densidade')
plt.xlabel('Scores')
plt.legend()
plt.tight_layout()
plt.show()
```    


![png](output_10_1.png)







## Matching
***

Em seguida, pareamos os indivíduos de grupos distintos com base em similitude de propensity score (ou seja, realizamos um match de um indivíduo do grupo de tratamento com um do grupo de controle). Existem vários métodos de pareamento – nearest neighbour, greedy matching, caliper matching, kernel matching, stratified matching, etc; a escolha do procedimento cabe ao pesquisador. O pareamento tipicamente leva a uma perda por descarte de indivíduos da amostra original; assim, o processo de pareamento também é uma forma de reamostragem.


```python
#rodamos o modelo de vizinhos mais próximos

from sklearn.neighbors import NearestNeighbors

knn = NearestNeighbors(n_neighbors=1) #importa o modelo
knn.fit(selecao_n_tratados.values.reshape(-1, 1)) # faz o fit

distances, indices = knn.kneighbors(selecao_tratados.values.reshape(-1, 1)) #aplico o modelo

#transformo em datafram

indices = pd.DataFrame(indices, columns = {"Indices"}) #crio o dataframe
selecao_tratados = pd.DataFrame(selecao_tratados)


#### Processo de identificacao dos Matching 

### merge do tempo Y para os tratados

lista_1 = [] #crio lista
lista_2 = [] #crio lista

for i in indices.Indices:#preencho a lista
    lista_1.append(i)
    
for i in selecao_tratados["P(1)"]:#preencho a lista
    lista_2.append(i)    
    
tratados = pd.DataFrame(list(zip(lista_1, lista_2)), 
               columns =['Indices','Scores_Tratados'])


### merge do tempo Y para os tratados

lista_1 = [] #crio lista

    
for i in selecao_tratados_tempo["Tempo_Recuperação_(Y)"]:#preencho a lista
    lista_1.append(i)    
    
tratados_tempo = pd.DataFrame(list(zip(lista_1)), 
               columns =['Tempo_Recuperação_(Y)'])



### merge do tempo Y para os não tratados

lista_3 = []
lista_4 = []

a=0
for i in selecao_n_tratados_tempo["P(1)"]:#preencho a lista
    a+= 1 #regra para novo indice
    lista_3.append(a)
    
    
for i in selecao_n_tratados_tempo["P(1)"]: #preencho a lista
    lista_4.append(i)    #faço o append    
    
    
n_tratados = pd.DataFrame(list(zip(lista_3, lista_4)), 
               columns =['Indices','Score_Nao_Tratados']) #unifico as listas

lista_3 = []
lista_4 = []

a=0
for i in selecao_n_tratados_tempo["P(1)"]:#preencho a lista
    a+= 1 #regra para novo indice
    lista_3.append(a)
    
    
for i in selecao_n_tratados_tempo["Tempo_Recuperação_(Y)"]: #preencho a lista
    lista_4.append(i)    #faço o append    

n_tratados_tempo = pd.DataFrame(list(zip(lista_3, lista_4)), 
               columns =['Indices','Tempo_Recuperação_(Y)_N']) #unifico as listas


base_indices_Score = tratados.merge(n_tratados, on='Indices',right_index=True, how='left') #mesclo os matches para verificação
base_indices_tempo = base_indices_Score.merge(n_tratados_tempo, on='Indices',right_index=True, how='left') #mesclo os matches para verificação

#### Processo de identificacao dos Matching Finalizado 

base_indices_tempo_final = pd.concat([base_indices_tempo, tratados_tempo], axis=1, join='inner') #concateno os dataframes
base_indices_tempo_final["Cloroquina_(A)_T"] = 1
base_indices_tempo_final["Cloroquina_(A)_NT"] = 0
#Agora já temos a base geral, mas precisamos ajeita-la, padronizando as colunas que representam a mesma coisa

base_indices_tempo_final.head()

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
      <th>Indices</th>
      <th>Scores_Tratados</th>
      <th>Score_Nao_Tratados</th>
      <th>Tempo_Recuperação_(Y)_N</th>
      <th>Tempo_Recuperação_(Y)</th>
      <th>Cloroquina_(A)_T</th>
      <th>Cloroquina_(A)_NT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>48</td>
      <td>0.512854</td>
      <td>0.509889</td>
      <td>29</td>
      <td>14</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>0.459133</td>
      <td>0.457321</td>
      <td>28</td>
      <td>11</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>29</td>
      <td>0.357378</td>
      <td>0.356476</td>
      <td>17</td>
      <td>24</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>45</td>
      <td>0.555223</td>
      <td>0.555844</td>
      <td>11</td>
      <td>20</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28</td>
      <td>0.484609</td>
      <td>0.481999</td>
      <td>29</td>
      <td>11</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### O que é essa base?

* Indices = Indice do score nao tratado gerado pelo KNN
* Scores_Tratados = Scores_Tratados
* Scores_Nao_Tratados = Scores_Nao_Tratados
* Tempo_Recuperação_(Y)_N = Tempo de recuperacao para os n_tratados
* Tempo_Recuperação_(Y)	 = Tempo de recuperacao para os tratados
* Cloroquina_(A)_T = Cloroquina para os tratados
* Cloroquina_(A)_T = Cloroquina para os n_tratados


```python
tratados =  base_indices_tempo_final[["Scores_Tratados","Cloroquina_(A)_T","Tempo_Recuperação_(Y)"]] #filtro as colunas que eu 
#quero
tratados.columns = ["Score","Cloroquina_(A)","Tempo_Recuperação_(Y)"] #renomeio as colunas 

n_tratados = base_indices_tempo_final[["Score_Nao_Tratados","Cloroquina_(A)_NT","Tempo_Recuperação_(Y)_N"]]#filtro as colunas que eu 
#quero
n_tratados.columns = ["Score","Cloroquina_(A)","Tempo_Recuperação_(Y)"] #renomeio as colunas

# Geramos nossa babse final com todos os casos encaixados nas tres variaveis

base_final_1 = tratados.append(n_tratados)
base_final_1.describe()
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
      <th>Score</th>
      <th>Cloroquina_(A)</th>
      <th>Tempo_Recuperação_(Y)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>90.000000</td>
      <td>90.000000</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.476043</td>
      <td>0.500000</td>
      <td>18.733333</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.090493</td>
      <td>0.502801</td>
      <td>6.363166</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.273230</td>
      <td>0.000000</td>
      <td>10.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.397510</td>
      <td>0.000000</td>
      <td>12.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.485462</td>
      <td>0.500000</td>
      <td>18.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.551437</td>
      <td>1.000000</td>
      <td>23.750000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.645221</td>
      <td>1.000000</td>
      <td>30.000000</td>
    </tr>
  </tbody>
</table>
</div>


## Após o Matching
***
Poderíamos usar o Caliper que basicamente é a a distância que iremos tolerar entre os matches, filtrando assim melhor a base. Mas nesse caso não usaremos. Vamos dar uma olhada como ficou o gráfico após o match:


```python
plt.rcParams['figure.figsize'] = 10, 6 #arruma fonte
plt.rcParams['font.size'] = 13 #arruma fonte

sns.distplot(n_tratados["Score"], label='Não Tratados') #não tratados
sns.distplot(selecao_tratados, label='Tratados') ##tratados
plt.xlim(0, 1) #escala
plt.title('Ditribuição do Propensity Score para Não tratados vs Tratados')
plt.ylabel('Densidade')
plt.xlabel('Scores')
plt.legend()
plt.tight_layout()
plt.show()
```    


![png](output_17_1.png)

Um pouco diferente, não?



Por fim, calculamos então a média do efeito do tratamento por meio de uma análise multivariada dentro dessa nova amostra. A média da diferença dos resultados entre os pares é uma medida não enviesada do efeito médio do tratamento.


>Nesse momento então tiramos a prova conceito adaptando ao modelo de reg linear e vendo seus p-valor e o seu beta



```python
import statsmodels.api as sm #chamo o modelo

X_base= base_final_1[["Score", "Cloroquina_(A)"]] #filtro as colunas
y_base= base_final_1[["Tempo_Recuperação_(Y)"]] #filtro as colunas

X = sm.add_constant(X_base.values.ravel()) #aplicamos o modelo
results = sm.OLS(y_base,X_base).fit() #computamos os resultados
results.summary() #visualizamos os resultados
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>    <td>Tempo_Recuperação_(Y)</td> <th>  R-squared:         </th> <td>   0.856</td>
</tr>
<tr>
  <th>Model:</th>                     <td>OLS</td>          <th>  Adj. R-squared:    </th> <td>   0.853</td>
</tr>
<tr>
  <th>Method:</th>               <td>Least Squares</td>     <th>  F-statistic:       </th> <td>   261.5</td>
</tr>
<tr>
  <th>Date:</th>               <td>Sun, 26 Apr 2020</td>    <th>  Prob (F-statistic):</th> <td>9.37e-38</td>
</tr>
<tr>
  <th>Time:</th>                   <td>00:22:57</td>        <th>  Log-Likelihood:    </th> <td> -309.10</td>
</tr>
<tr>
  <th>No. Observations:</th>        <td>    90</td>         <th>  AIC:               </th> <td>   622.2</td>
</tr>
<tr>
  <th>Df Residuals:</th>            <td>    88</td>         <th>  BIC:               </th> <td>   627.2</td>
</tr>
<tr>
  <th>Df Model:</th>                <td>     2</td>         <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>        <td>nonrobust</td>       <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
         <td></td>           <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Score</th>          <td>   35.8065</td> <td>    2.301</td> <td>   15.564</td> <td> 0.000</td> <td>   31.235</td> <td>   40.378</td>
</tr>
<tr>
  <th>Cloroquina_(A)</th> <td>    1.8723</td> <td>    1.576</td> <td>    1.188</td> <td> 0.238</td> <td>   -1.260</td> <td>    5.005</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>19.945</td> <th>  Durbin-Watson:     </th> <td>   2.153</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.000</td> <th>  Jarque-Bera (JB):  </th> <td>   4.685</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.014</td> <th>  Prob(JB):          </th> <td>  0.0961</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 1.883</td> <th>  Cond. No.          </th> <td>    2.60</td>
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



## Conclusão do Exemplo

Em geral testamos a hipótese nula: O tratamento não tem efeito.

Se rejeitarmos a hipótese nula, dizemos que ele tem efeito. Em termos pratico, um p-valor abaixo de 0.05 nos leva a rejeitar a hipótese nula, essa hipótese é testada em cima daquele coeficiente beta ( 1.8723 ). Então a conta que ele faz é ( Beta / Erro padrão ). Na tabela, 1.8723 / 1.576 que irá dar exatamente o valor t.

Ai olhamos a probabilidade desse valor  ocorrer um valor maior que | t | -> (**P>|t|**), ou seja, maior que t positivo e menor que t  negativo, se ela for alta (em termos práticos, acima de 0.05) dizemos que esse Beta não é diferente de zero, ou seja, **nesse caso FICTÍCIO o tratamento não tem efeito**.


# Parte V – Vantagens e desvantagens do PSM

Dentre as principais vantagens do PSM, podemos incluir: a sumarização dos dados em uma “pontuação”, o propensity score, para estimar os efeitos do tratamento ao invés de observar e comparar múltiplas covariáveis; a amostra de indivíduos participantes do estudo pode ser mais representativa da população do que em um RCT; o cálculo do PS não inclui qualquer informação do resultado, logo a estimação do efeito do tratamento não sofre viés; como o modelo usado para estimar o PS não é o foco do estudo, ele não necessita ser parcimonioso, o que permite a inclusão de um número maior de covariáveis e termos de interação entre elas; entre outros.

As principais desvantagens do PSM surgem a partir de sua natureza não randômica de alocação dos indivíduos aos grupos de controle ou tratamento. De fato, o método PSM balanceia a análise com base em covariáveis observadas, enquanto experimentos randômicos balanceiam tanto sobre covariáveis conhecidas como desconhecidas. Ao não controlar para covariáveis desconhecidas, há o risco de viés afetando a probabilidade dos indivíduos receberam tratamento ou não. Além disso, é preferível, senão necessário, que o método seja aplicado em amostras grandes. Há a necessidade de sobreposição significativa entre os propensity scores dos grupos, de modo a melhor realizar o pareamento. O próprio pareamento, ao ser feito apenas em observância das variáveis observadas, abre a possibilidade de aumento do viés devido à existência de covariáveis não observadas.


## Referências

- COWLING, Ben. Propensity Score Analysis. The University of Hong Kong. 2017. Disponível em : <http://web.hku.hk/~bcowling/examples/propensity.htm>
- FRASER, Mark W.; GUO, Shenyang. Propensity Score Analysis: statistical methods and applications - 2nd edition. SAGE Publications, 2015. 
- PROPENSITY score. Columbia Mailman School of Public Health, 2012. Disponível em: <https://www.mailman.columbia.edu/research/population-health-methods/propensityscore>
- PROPENSITY Score Matching: Definition & Overview . Statistics How To, 2017. Disponível em: <https://www.statisticshowto.com/propensity-score-matching/>
- THAVANESWARAN, Arane. Propensity Score Matching in Observational Studies. Faculty of Health and Sciences, University of Manitoba, 2008. Disponível em:  <https://www.umanitoba.ca/faculties/health_sciences/medicine/units/chs/departmental_units/mchp/protocol/media/propensity_score_matching.pdf>
- ROSEMBAUM, Paul R.; RUBIN, Donald B. The central role of the propensity score in observational studies for causal effects. Biometrika, 1983; 70:41-55

