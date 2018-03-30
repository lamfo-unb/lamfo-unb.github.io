---
layout: post
title: Mantendo o controle na era do *Big Data*: Manipulação de dados em R com o pacote *data.table*.
lang: pt
header-img: img/manipulacao_data.table/img_data.table.png
date: 2018-03-30
tags: [R, BigData, data.table,packages]
author: João Victor Freitas Machado e Peng Yahao.
comments: true
---


### "*With Big Data, comes big responsibilities...*"

<center>



![imagem](img/manipulacao_data.table/data.table_r.png)

</center>




## Desafios da era da informação e o papel do *data scientist*

"*Big Data*" é uma expressão que está sendo constantemente utilizada em variados contextos, e sintetiza a era de integração tecnológica, velocidade de transmissão de informações e abundância de dados na qual vivemos. Porém, *Big Data*, que traduz literalmente para "Grandes dados", por si só não significa nada... Mais que ter grande quantidade de informações, nunca foi mais importante saber fazer uso eficiente e consciente dessa massa de dados -- conferir propósito à quase-infinitude de números e letras, transformar meros *bytes* em valor, em algo que seja útil. Em uma era em que "inteligência artificial" é glorificada e encarada como uma janela para um futuro mais conveniente e confortável, o pensamento crítico típico à inteligência humana assume um papel central -- agora mais que nunca -- para que haja garantias de que a evolução tecnológica possa solucionar as demandas daqueles que a criou

Para saber fazer bom uso dos dados, primeiro é preciso garantir sua qualidade. Seja para uma análise estatística simplista para um trabalho escolar, ou para uma modelagem complexa que coloca em xeque o bem-estar de milhões de pessoas e a alocação eficiente de bilhões em recursos, a validade de um estudo de *data science* está intimamente vinculada à qualidade de seu insumo mais fundamental -- dados. A qualidade dos dados é um fundamento sobre o qual se assenta a definição de um problema relevante, a elegância das formalizações matemáticas, a compostura daquele que apresenta o PowerPoint dos resultados, a potencial repercussão da análise para o mundo acadêmico, profissional ou social... dados bons são condição necessária para se realizar uma boa análise; dito de outra forma, dados ruins são **garantia** para para uma análise ruim -- ou, ainda pior, pode gerar uma análise aparentemente relevante, porém enviesada, de modo que o pesquisador acaba por aceitar algo equivocado que pode se propagar e influenciar negativamente esforços futuros.

Limpeza dos dados é uma das partes menos divertidas do ofício de um *data scientist*, porém é, disparado, uma das suas habilidades mais primordiais. Saber lidar com um banco de dados complicado, manipular e criar variáveis, filtrar classes de interesse e descartar de observações indesejadas fazem parte da rotina de um cientista de dados, e representam uma grande quantidade de trabalho até que se chegue a uma tabela organizada para, só então, servir de *input* para algum modelo sofisticado e gerar conclusões potencialmente úteis. No contexto do *Big Data*, em especial, saber manipular uma base de dados se torna ainda mais essencial, dado que o grande volume de dados gera desafios adicionais -- não basta mais apenas tratar bem os dados, mas é necessário fazer isso de maneira **eficiente**

Este post apresenta algumas funcionalidades do pacote *data.table*, uma ferramenta útil para se ter no "canivete" de um cientista de dados. Esperamos que possa ajudar você a controlar suas grandes bases de dados de maneira prática!

<center>

![SPSS-sucks](img/manipulacao_data.table/Stock_IoT.jpg)

</center>


## *data.table*: Um instrumento poderoso

Usuários R (principalmente iniciantes) lutam, com muita dificuldade diga-se de passagem, ao lidar com grandes conjuntos de dados. Eles são assombrados por avisos repetitivos, mensagens de erro de uso insuficiente de memória. A maioria deles chega a uma conclusão imediata de que a especificação de sua máquina não é poderosa o suficiente. Para desmistificar o assunto resolvi encontrar um conjunto de dados suficientemente grande que, obviamente, conseguisse rodar no meu computador de 8gb de RAM. 


Nesse post abordamos os primeiros passo para aprender a manipular um grande conjunto de dados via Data.table, o que de forma alguma substitui uma leitura [**clique aqui**](https://cran.r-project.org/web/packages/data.table/data.table.pdf) da documentação completa do pacote, e aproveitando o embalo, recomendo muito o curso do próprio criador do data.table [**clicando aqui**](https://www.datacamp.com/courses/data-table-data-manipulation-r-tutorial) , que aborda de forma simplista, porém muito completa, uma grande gama de possibilidades dentro do pacote do Data.table, inclusive a imagem utilizada é do datacamp feita para o curso.

Para que fosse um desafio, resolvi escolher um banco de dados suficientemente grande que funções nativas no R não fossem capazes de lidar facilmente com ele, uma boa possibilidade para você que procura banco de dados, é o site Kaggle que possui muitos dados armazenados além de serem bem explicados, de diversas áreas, o site é [**esse**](https://www.kaggle.com/datasets). O banco que foi utilizado no e projeto, os dados vêm do sistema de compartilhamento de bicicletas Divvy de Chicago, bem como as informações sobre o tempo em Chicago e pode ser encontrado [**aqui**](https://www.kaggle.com/yingwurenjian/chicago-divvy-bicycle-sharing-data)



![ola](img/manipulacao_data.table/fob_transit-divvy-expansion-west-magnum.jpg)

</center>

## Lendo o banco de dados
```
#  Lendo com a funcão fread() 

> chicago_ciclistas <- fread("chicago-divvy-bicycle-sharing-data\\data_raw
                             .csv",fill=TRUE)
Read 13774715 rows and 27 (of 27) columns from 3.244 GB file in 00:08:31

# Leitura com a função padrão do R

> system.time(chicago_ciclistas <- read.csv("D:\\Dados\\chicago-divvy-bicycle-sharing-data\\data_raw.csv",fill=TRUE))
usuário   sistema decorrido 
439.54     47.54   1771.05 

```
 
Ou seja, em 8 minutos o R leu o banco de dados que tem tamanho de 3gb, esse tempo de performance ainda pode ser melhorado caso você passe o argumento especificando o que cada coluna é, inteiro/integer ou carácter/fator, enquanto a função nativa do R demorou 47 minutos. 

Podemos ver as variáveis do nosso banco de dados.

```
names(chicago_ciclistas)
[1] "trip_id"           "usertype"          "gender"            "starttime"         "stoptime"          "tripduration"      "from_station_id"  
[8] "from_station_name" "latitude_start"    "longitude_start"   "dpcapacity_start"  "to_station_id"     "to_station_name"   "latitude_end"     
[15] "longitude_end"     "dpcapacity_end"    "temperature"       "windchill"         "dewpoint"          "humidity"          "pressure"         
[22] "visibility"        "wind_speed"        "precipitation"     "events"            "rain"              "conditions"       
```
## Ordenando o banco
O que mais chama atenção no data.table é a simplicidade do código, muitas funções você passa diretamente ao banco, por exemplo a ordenação do banco de dados, pegando a menor duração de viagem:

```
# função order aplicada dentro do data.table

> chicago_ciclistas <- chicago_ciclistas[order(tripduration)]
    trip_id   usertype gender           starttime            stoptime tripduration from_station_id          from_station_name latitude_start
1:     4192 Subscriber   Male 2013-06-27 12:15:00 2013-06-27 12:16:00           60              28 Larrabee St & Menomonee St       41.91468
2:     5453 Subscriber   Male 2013-06-28 11:01:00 2013-06-28 11:02:00           60              51     Clark St & Randolph St       41.88458
3:     6511 Subscriber   Male 2013-06-28 16:04:00 2013-06-28 16:05:00           60              48 Larrabee St & Kingsbury St       41.89776
4:    14100 Subscriber   Male 2013-07-01 13:54:00 2013-07-01 13:55:00           60              92    Carpenter St & Huron St       41.89456
5:    14113 Subscriber   Male 2013-07-01 13:56:00 2013-07-01 13:57:00           60              92    Carpenter St & Huron St       41.89456

```

Ou pegando de maior duração:

```
> chicago_ciclistas <- chicago_ciclistas[order(-tripduration)]
```

## Excluindo NA's do banco

Outra coisa importante é saber excluir NAs do nosso banco de dados , caso não sejam necessários, da mesma forma podemos usar a função is.na() aplicada dentro do data.table:

```
chicago_ciclistas[!is.na(starttime)]
```

Reparem que ele identifica os nomes da coluna, na maioria das vezes sem a necessidade das aspas, o que para um usuário R padrão soa um pouco estranho, mas sim, este é o jeito data.table.

## Fazendo um corte nos Dados

Outra coisa fundamental no processo de análise de dados é o corte ou subset do banco de dados, o processo não foge muito do que já foi feito para tirar as informações perdidas e na ordenação dos dados...

```
# Podemos estar interessados em apenas nos dados do sexo masculino

> sexo_masculino<-chicago_ciclistas[gender=="male"]

# ou viagens acima de 180 segundos 

> viagem_longa<-chicago_ciclistas[tripduration>180]

# Ou nos dois juntos 

> viagem_longa<-chicago_ciclistas[tripduration>180 & gender=="male"]

```

## Desduplicando variáveis.

Função que rende muitas postagens no stack overflow é como retirar informações duplicadas do nosso conjunto de dados, além disso retirar informações duplicadas em conjunto de 2 variáveis em conjunto. Por exemplo, no nosso conjunto de dados tem a trip_id, suponha que seja o usuário dessa estação,e eu quero apenas 1 informação por usuário, e depois eu quero um conjunto de dados com só uma informação de usuário por dia, no segundo precisamos primeiro criar um conjunto de dados so com as duas colunas.

```
# Apenas uma informação de usuário 

>apenas_uma_informacao_id <- chicago_ciclistas[!duplicated(trip_id)]

# Apenas uma informação de usuário por dia
>informacoes_usuario <-chicago_ciclistas[,c("trip_id","starttime")]

>apenas_uma_informacao_id_day <- chicago_ciclistas[!duplicated(informacoes_usuario)]
```
## Criando nova variável

O data table permite alta performance também na criação de novas variaveis usando o formato **:=**, o que pode muito interessante quando combinado com a função by...

Vamos supor que não existisse a variável duração da viagem, poderiamos usar facilmente a variavel starttime e stoptime e criar a variável:

```
>chicago_ciclistas[,tempo_viagem:=starttime-stoptime]
```

Utilizando o by podiamos criar um tempo de viagem médio por sexo...

```
# criando a variável media de viagem por sexo e selecionando as 10 primeiras obs das duas colunas

>chicago_ciclistas[,media_viagem_sexo:=mean(tripduration),by=gender][1:10,c("gender","media_viagem_sexo")]
   gender media_viagem_sexo
1:   Male          685.9940
2:   Male          685.9940
3:   Male          685.9940
4:   Male          685.9940
5:   Male          685.9940
6:   Male          685.9940
7:   Male          685.9940
8:   Male          685.9940
9: Female          807.1566
10:   Male          685.9940



```

## Trabalhando com resumo dos dados (.N)

Outra questão muito fundamental nesses primeiros passos é o .N que, caso quem se interessar e se aprofundar no pacote, tem muita utilidade dentro de diversas funções, uma por exemplo é resumo, vamos supor que queiramos quantas viagens de uma estação a outra foram feitas, podemos fazer isso via data.table da seguinte forma:

```
>chicago_ciclistas[,.N,by=c("from_station_name","to_station_name")]

             from_station_name               to_station_name    N
1:   Larrabee St & Menomonee St    Larrabee St & Menomonee St  809
2:       Clark St & Randolph St        Clark St & Randolph St 1196
3:   Larrabee St & Kingsbury St    Larrabee St & Kingsbury St  906
4:      Carpenter St & Huron St       Carpenter St & Huron St  436
5:    Franklin St & Chicago Ave     Franklin St & Chicago Ave  813

```

### Recapitulando o que foi visto em apenas uma função
Vamos agora em apenas um comando recapitular boa parte do que foi visto aqui, criando um tempo médio de viagem ao mesmo tempo pelo clima, se choveu ou não, e por sexo. Depois pegamos a quantidade de dados por sexo, clima e a média, e por fim ordenamos pela variável de quantidade(N), pareceu complicado? o código vai parecer um pouco denso, mas é importante que você capitule o funcionamento para ganho de eficiência:

```
>chicago_ciclistas[,media_viagem_condicao_clima_0_1:=mean(tripduration),by=c("rain","gender")][,.N,by=c("rain","media_viagem_condicao_clima_0_1","gender")][order(-N)]

   rain media_viagem_condicao_clima_0_1 gender       N
1:    0                        687.7778   Male 7262934
2:    0                       1792.7700        3671283
3:    0                        809.0070 Female 2411877
4:    1                        637.4476   Male  266872
5:    1                       1649.3833          85649
6:    1                        748.5133 Female   76100
```

### Outros posts interessantes em Linguagem R.

Recomendo fortemente outros posts interessantes em linguagem R e abordando outros pacotes em outros temas, por exemplo do [**Post**](https://lamfo-unb.github.io/2017/07/22/intro-analise-acoes-1/) analisando ações no programa R, uma abordagem muito interessante utilizando os pacotes ggplot e quantmod, pacote esse que permite fazer diretamente download de ações, e o outro [**Post**](https://lamfo-unb.github.io/2017/08/17/Teste-de-Eventos/)  com aplicações de teste de Eventos, que tem como base a Teoria dos Mercados Eficientes de Fama (1970), em que se acredita que o mercado absorve as informações públicas disponíveis e realiza o ajuste do preço dos ativos, além disso o [**Coursera**](https://www.coursera.org/)  contém uma grande gama de cursos, que podem ser feitos de graça como ouvinte(sem ganho de certificado) e muito bons como o [**R programming**](https://www.coursera.org/learn/r-programming).
<center>