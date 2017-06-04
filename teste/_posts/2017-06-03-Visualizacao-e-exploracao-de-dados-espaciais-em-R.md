---
layout: post
title: Visualização e exploração de dados espaciais em R
date: 2017-06-03 13:55:44
tags: [georrefenciamento]
author: Marcelo Felix
lang: pt
header-img: img/0018.jpg
---
# ***Visualização e exploração de dados espaciais no R***

Neste post iremos nos basear no tutorial de Robin Lovelace et. al. (2017) e o material que será utilizado está disponível em seu [repositorio](https://www.github.com/Robinlovelace/Creating-maps-in-R) no Github, no projeto *Creating-maps-in-R.Rproj*. 
Inicialmente, é preciso instalar os seguintes pacotes no R: 
`"ggplot2", "ggmap","rgdal","rgeos","maptools","dplyr","tidyr","tmap"`

Feita a instalação dos pacotes necessários, o primeiro passo é abrir a shape file. Aqui iremos nos basear no tutorial de Robin Lovelace et. al. (2017) e o material que será utilizado está disponível no [link](https://www.github.com/Robinlovelace/Creating-maps-in-R) no projeto *Creating-maps-in-R.Rproj*. 

O shapefile é um formato de arquivo que armazena dados geoespaciais, como posição, forma e atributos, em forma de vetor. No RStudio a abertura desse tipo de arquivo pode ser feita da seguinte forma: 

```r
library(rgdal)
lnd <- readOGR(dsn = "data", layer = "london_sport")
head(lnd@data, n = 2)
sapply(lnd@data, class)
```

A última linha do código mostra que a classe da variável *Pop_2001* está classificada como fator, enquanto deveria ser do tipo numérico. Portanto, devemos alterá-la:

```r
lnd$Pop_2001 <-  as.numeric(as.character(lnd$Pop_2001))
sapply(lnd@data, class) 
```

Uma primeira análise pode ser feita com apenas selecionando as regiões que possuam determinado índice de participação no esporte: 

```r
sel <- lnd$Partic_Per > 20 & lnd$Partic_Per < 25
plot(lnd, col = "lightgrey")
plot(lnd[sel,], col = "turquoise", add = TRUE)
```

<img src="/img/geospace/chunk-3-1.png" width="500">

Outra análise interessante é a identificação de quadrantes. Essa divisão pode ser feita tendo como base as coordenadas do centroide, que no caso representa o ponto central de Londres. 

``` r
library(rgeos)
## encontrando o centro da região de londres
lat <- coordinates(gCentroid(lnd))[[1]]
lng <- coordinates(gCentroid(lnd))[[2]]
## argumento para testar se a região está ao norte ou ao leste do centro
norte <-  sapply(coordinates(lnd)[,2], function(x) x > lng)
leste <- sapply(coordinates(lnd)[,1], function(x) x > lat)
oeste <- sapply(coordinates(lnd)[,1], function(x) x < lat)
sul <- sapply(coordinates(lnd)[,2], function(x) x < lng)
## testando se a coordenada está a norte ou leste do centro
lnd@data$quadrant[norte & leste] <- "nordeste"
lnd@data$quadrant[norte & oeste] <- "noroeste"
lnd@data$quadrant[sul & leste] <- "sudeste"
lnd@data$quadrant[sul & oeste] <- "sudoeste"

## colorindo os quadrantes no gráfico
sapply(lnd@data, class)
lnd@data$quadrant <- as.factor(as.character(lnd@data$quadrant))
sapply(lnd@data, class)
plot(lnd, col = "lightgrey")
plot(lnd[norte & leste,],col = "red", add = TRUE)
plot(lnd[norte & oeste,], col = "blue", add = TRUE)
plot(lnd[sul & leste,], col = "green", add = TRUE)
plot(lnd[sul & oeste,], col = "yellow", add = TRUE)
```

<img src="/img/geospace/chunk-4-1.png" width="500">

## Criando mapas com tmap, ggplot e leaflet
**Tmap**

A análise pode ser feita gerando um gradiente de cores com base na participação percentual da população no esporte: 

```r
library(tmap)
qtm(shp = lnd, fill = "Partic_Per", fill.palette = "-Blues")
```

<img src="/img/geospace/chunk-5-1.png" width="500">

**ggplot2**

Diferentemente do tmap, o ggplot exige que você forneça a base de dados em formado de data.frame. Isto pode ser feito por meio da função fortify(): 

```r
library(ggplot2)
library(dplyr)
lnd_f <- fortify(lnd)
head(lnd_f, n = 2)
lnd$id <- row.names(lnd) # Atribui um ID para a base sp 
head(lnd@data, n = 2) # ultima checagem antes da junção (os nomes das variáveis precisam ser iguais)
lnd_f <-left_join(lnd_f, lnd@data) # juntou pela única variável com nome em comum
head(lnd_f, n = 2)
```

Agora o objeto lnd_f pode ser utilizado para a criação de um mapa com ggplot:

```r
map<-ggplot(lnd_f, aes(long, lat, group = group, fill = Partic_Per)) + 
  geom_polygon() + coord_equal() + 
  labs(x = "Easting (m)", y = "Northing (m)", fill = "% Sports\nParticipation") + 
  ggtitle("London Sports Participation")
map
## trocando as cores
map + scale_fill_gradient(low = "red", high = "green")
## salvando o grafico com ggsave
ggsave("Sport Participation - London.jpg")
```

<img src="/img/geospace/img_ggplot2.jpg" width="500">

**Mapas interativos com leaflet**

O pacote leaflet permite a criação de mapas interativos e age de forma conjunta com o pacote shiny, tornando possível a visualização online dos mapas. O objeto lnd está com um CRS (Coordinate Reference System) que não é compatível com o leaflet. A visualização será possível depois que o CRS do objeto lnd for alterado para o EPSG:4326, que tem sido o mais utilizado: 

```r
library(leaflet)
lnd84 <- spTransform(lnd, CRS("+init=epsg:4326")) #reprojeta
leaflet() %>%
   addTiles() %>%
   addPolygons(data = lnd84)
```

<iframe  title="London Map" width="700" height="400" src="https://cdn.rawgit.com/jadermcs/fdb08a2c254288844c6ae4673e4e6e0b/raw/a92f7fdf0ec510d30504c934b3dcecbe8ab67fd3/london_map.html" frameborder="0" allowfullscreen></iframe>

Esse mapa se diferencia dos outros por permitir uma interatividade semelhante à que se tem com a utilização do googlemaps no navegador. 

A seguir temos mais um exemplo de visualização de uma determinada região no leaflet, porém agora será utilizado o arquivo shapefile do Distrito Federal, disponível no site do IBGE. Depois de baixar o arquivo e salvá-lo no seu Working Directory atual,  o primeiro passo é atribuir o arquivo à um novo objeto, aqui denominado *df_shp*:

```r
library(raster)
library(sp)
df_shp <- readOGR(dsn = "df_setores_censitarios", layer = "53SEE250GC_SIR")

# Criação do mapa com leaflet
df_shp@proj4string <- CRS("+init=epsg:4326") # reprojeta
df_leaf <- leaflet(data = df_shp) %>% addTiles()%>%
	addPolygons(fill = FALSE, weight = 1, stroke = TRUE, color = "#03F")%>%
	addLegend("bottomright", colors = "#03F", labels = "Distrito Federal")

df_leaf
```

O código acima segue o mesmo raciocínio do criado para o mapa de Londres, porém com algumas configurações a mais. O arquivo .shp foi inserido diretamente na função leaflet e na função addPolygons foram incluídos os argumentos fill, weight, stroke e color, que representam respectivamente se o interior dos polígonos devem receber alguma cor, a espessura da linha de contorno, se as linhas dos polígonos devem ser coloridas e qual a cor escolhida. A função addLegend cria uma legenda para o mapa. Para mais detalhes sobre funções, basta rodar o seguinte comando: ?nomedafunção.

<iframe  title="London Map" width="700" height="400" src="https://cdn.rawgit.com/jadermcs/9f68419b0b05e32b8741861e80c7df02/raw/b1d9d0394fdd4f724dbc591a14dfa0537baca45e/df.html" frameborder="0" allowfullscreen></iframe>
