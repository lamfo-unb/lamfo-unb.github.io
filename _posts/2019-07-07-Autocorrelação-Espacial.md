---
layout: post
title: Autocorrelação Espacial e Dependência Geográfica
lang: pt-br
header-img: img/autocorrelation/globes-1246245_1280.jpg
date: 2019-07-07 23:59:07
tags: [GIS,spatial-statistics, autocorrelation, spatial-dependency]
author: Leonardo Galler
comments: true
---
## A primeira lei da Geografia diz: 
> “Todas as coisas estão relacionadas com todas as outras, mas coisas próximas estão mais relacionadas do que coisas distantes.” Waldo R. Tobler

Com essa afirmação introduzimos um conceito muito importante em Estatística Espacial, o conceito de **_Dependência Espacial_**.
	A estatística espacial nasceu com o objetivo de capturar dos dados, os possíveis padrões que podem existir entre lugares e valores. Ou nas palavras de Luc Anselin(1992):
>”A principal característica da estatística espacial é seu foco em inquirir padrões espaciais de lugares e valores, identificando a associação espacial existente entre eles e a variação sistemática do fenômeno por localização.”
	Ou de forma mais genérica, a força da dependência espacial é uma medida que expressa o relacionamento entre a variação das propriedades e a proximidade espacial.
	É importante ressaltar que as associações significativas encontradas durante a análise estão **necessariamente** associadas à situação inicial dos dados de entrada e que, uma situação inicial diferente pode revelar outros padrões espaciais.

### Autocorrelação
	Comecemos por partes:
__Correlação__: é o relacionamento predominante entre variáveis ou pares de valores em observações individuais. Falando de forma bem simples, correlação é o comportamento que observamos entre variáveis que variam conjuntamente, de forma positiva ou negativa. Lembre-se sempre, __correlação não implica causalidade__.

<Imagem correlação>

	Auto: Significa mesmo, ou ‘o mesmo’.

	Podemos entender que a autocorrelação é o relacionamento entre entre as observações da mesma variável. As observações da mesma variável estão relacionadas, não são independentes.
	Quando incluímos o adjetivo espacial ao termo auto-correlação, queremos dizer que as dependências que existem entre observações estão relacionadas a sua localização geográfica, e estas dependências induzem a padrões em mapas.

## Medindo a dependência espacial

>	A dependência espacial pode ser medida de diferentes formas. O índice de Moran (I) > é a estatística mais difundida e mede a autocorrelação espacial a partir do produto dos   > desvios em relação a média. Este índice é uma medida global da autocorrelação              > espacial, pois indica o grau de associação espacial presente no conjunto de dados.

<Imagem IdeMoran>

## Calculando o I de Moran 
	Antes de calcularmos o I de Moran precisamos calcular os pesos. Os pesos são de forma simplificada, para um conjunto de dados espaciais composto por n localizações, a matriz de pesos espaciais expressa o potencial de interação entre observações em cada par de localizações.
	Para este post estaremos utilizando o shapefile de Crimes do estados de Minas gerais, os dados estão disponíveis aqui <link para meu github <https://github.com/LeoGaller/Posts/blob/master/Autocorrelation-LAMFO/crime_mg.zip>>.

### Pré-requisitos
Estaremos utilizando o pacote Pysal, para ter certeza que o código rodará com sucesso, verifique e instale o pacote.
O arquivo zip deve ser descompactado, pois, o shapefile é composto de vários arquivos.

```
import pysal
# Leia os dados e crie a matriz de pesos.
W = pysal.queen_from_shapefile("/dados/crime_mg/crime_mg.shp")

# Open the cursor to write the weights file to a GAL file, to use on Local Moran's I
out = pysal.open("weights_lista3_mg.gal", 'w')

# Write to a file
out.write(W)
out.close() # Close the connection

# Lendo e salvando o shapefile como dataframe
# setando o caminho do arquivo na variável
import geopandas as gpd
fp = "/dados/crime_mg.shp"
map_df = gpd.read_file(fp)
```
	A autocorrelação pode ser calculada por duas perspectivas, Global e Local.

### Autocorrelação Global
	A análise de autocorrelação global envolve o estudo do padrão de todo o mapa e pergunta se o padrão apresenta agrupamento ou não.
	Hipótese nula: **Aleatoriedade Espacial Completa**

```
# Lendo e salvando a matriz de pesos em uma variável
w = pysal.open("weights_lista3_mg.gal").read()

# criando array para cálculo do índice
y = np.array(map_df["INDICE94"])

# Calculando o Índice de Moran Global
mi = pysal.Moran(y, w, two_tailed=False)

# Apresentando o valor do índice
print('The Morans I Statistic For INDICE94 is: '+ str(mi.I)[0:6] )

# Para uma visão geral do objeto
help(mi)
```
O índice pode ser usado para identificação de padrões de dispersão/aleatoriedade/agrupamento:
Índices próximos a zero [tecnicamente, próximos a -1/(n-1)], indicam padrão aleatório
Índices acima de -1/(n-1) (em direção a +1) indicam tendência a clusterização
Índices abaixo de -1/(n-1) (em direção a -1) indicam tendência à dispersão/uniformização

### Autocorrelação Local
	No cálculo do índice para autocorrelação local é retornado 1 valor para cada unidade espacial. A autocorrelação local tem com objetivo quantificar o grau de associação espacial a que cada localização do conjunto amostral.

```
# Local
# Calculando Moran Local
mi_local = pysal.Moran_Local(y, w)

# Verificando sua estrutura
help(mi_local)

# Observando indicadores
print(mi_local.Is)

```
	Vamos encontrar agora os valores LISA(Local Indicators of Spatial Association) significantes:
```
# numpy vetorização
sig = mi_local.p_sim<0.05
mi_local.p_sim[sig]
---
2
layout: post
3
title: Autocorrelação Espacial e Dependência Geográfica
4
lang: pt-br
5
header-img: img/autocorrelation/globes-1246245_1280.jpg
6
date: 2019-07-07 23:59:07
7
tags: [GIS,spatial-statistics, autocorrelation, spatial-dependency]
8
author: Leonardo Galler
9
comments: true
10
---
11
​
12
-- #Dependência espacial?
13
## A primeira lei da Geografia diz: 
14
> “Todas as coisas estão relacionadas com todas as outras, mas coisas próximas estão mais relacionadas do que coisas distantes.” Waldo R. Tobler
15
​
16
Com essa afirmação introduzimos um conceito muito importante em Estatística Espacial, o conceito de **_Dependência Espacial_**.
17
 
18
​
19
​
20
-- #O que é a autocorrelação?
21
​
22
-- #Como é a espacial?
23
​
24
-- #Quando utilizar?
25
​
26
-- #Aplicação?
27
​
28
-- #Conclusão?
29
​
30
​
31
​
32
## Referências:
33
1 - Dependência Espacial - http://www.sinaldetransito.com.br/artigos/espacial.pdf
# Encontrando o quadrante de cada unidade
mi_local.q[sig]

```
	Agora vamos gerar o gráfico de espalhamento.
```
# Gerando o gráfico de espalhamento
map_df['significant_INDICE94'] = lmINDICE94.p_sim < 0.05

map_df['quadrant_INDICE94'] = lmINDICE94.q

########### LISA for INDICE94 ###########

# Setup the figure and axis
f, ax = plt.subplots(1, figsize=(9, 9))
# Plot all
_all = map_df['geometry']
gpd.plotting.plot_series(_all, color='white', ax = ax)
# Plot HH clusters
hh = map_df.loc[(map_df['quadrant_INDICE94']==1) & (map_df['significant_INDICE94']==True), 'geometry']
gpd.plotting.plot_series(hh, color='red', ax = ax)
# Plot ll clusters    
ll = map_df.loc[(map_df['quadrant_INDICE94']==3) & (map_df['significant_INDICE94']==True), 'geometry']
gpd.plotting.plot_series(ll, color='green', ax = ax)
# Plot LH clusters
lh = map_df.loc[(map_df['quadrant_INDICE94']==2) & (map_df['significant_INDICE94']==True), 'geometry']
gpd.plotting.plot_series(lh, color='blue', ax = ax)
# Plot HL clusters
hl = map_df.loc[(map_df['quadrant_INDICE94']==4) & (map_df['significant_INDICE94']==True), 'geometry']
gpd.plotting.plot_series(hl, color='pink', ax = ax)
# Style and draw
f.suptitle('LISA - Variavel INDICE94', size=20)
f.set_facecolor('0.75')
ax.set_axis_off()
# Legend
red_patch = mpatches.Patch(color='red', label='HH')
green_patch = mpatches.Patch(color='green', label='LL')
blue_patch = mpatches.Patch(color='blue', label='LH')
pink_patch = mpatches.Patch(color='pink', label='HL')
plt.legend(handles=[red_patch,pink_patch,blue_patch,green_patch])
# End legend
plt.axis('equal')
plt.show()

```
<Imagem do mapa>

	O mapa acima apresenta os agrupamentos de localidades por seus valores do I de Moran, classificados em HH (alto-alto), HL(alto-baixo), LH(Baixo-Alto), LL(Baixo-Baixo). Os itens em branco são não significantes.

## Conclusão
	Observando o mapa podemos ter uma boa percepção de como estava distribuída a violência no estado de Minas Gerais em 1994, as áreas que apresentavam mais violência são as áreas mais a esquerda do mapa, já as com menor violência a direita, sendo que podemos observar áreas de alta violência com áreas de baixa violência ao redor.
	

## Referências:
1 - Dependência Espacial - http://www.sinaldetransito.com.br/artigos/espacial.pdf
2 - The Concept of Spatial Dependency - http://www.gitta.info/DiscrSpatVari/en/html/spat_depend_conc_spat_de.html
3 - Anselin, Luc ; Getis, Arthur. / Spatial statistical analysis and geographic information systems. In: The Annals of Regional Science. 1992 ; Vol. 26, No. 1. pp. 19-33.
4 - Griffith, Daniel A. "What Is Spatial Autocorrelation? Reflections on the past 25 Years of Spatial Statistics." L'Espace Géographique21, no. 3 (1992): 265-80. http://www.jstor.org/stable/44381737.
5 - Correlation - https://hackernoon.com/correlation-and-causation-by-example-e7fd627475e5
6 -Spatial Autocorrelation
 https://pysal.readthedocs.io/en/v1.11.0/users/tutorials/autocorrelation.html
7 - Weights - https://pysal.readthedocs.io/en/v1.11.0/users/tutorials/weights.html
