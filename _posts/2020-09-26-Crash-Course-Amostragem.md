---
layout: post
title: "Crash Course de Amostragem"
lang: pt
header-img: img/home-bg.jpg
tags: [amostragem, estatística, crash course]
author: Alex Rodrigues do Nascimento, Igor Ferreira do Nascimento
date: 2020-09-26 23:59:08
comments: true
--

### Amostragem clássica


O censo é a coleta das informações de toda a população, enquanto a amostragem é a coleta de um sobconjunto representativo dessa população. 

A principal vantagem do censo é a desvantagem da amostragem, e vice-versa. Por exemplo, ao coletarmos apenas um subconjunto da população, fazemos isso de forma mais **rápida** e com menor **custo**, quando comparamos com o censo. No entanto, ao realizarmos o censo, não há erro, pois foi coletada todas as informações da população. Já o processo de amostragem, as informações coletadas dependem da amostra, e por isso, há o chamado **erro amostral**.

O principal objetivo da teoria amostral é determinar um método de coleta de uma amostra, bem como calcular o **erro que se comete** ao coletar informações de um subconjunto da população. Chamamos essa amostragem de **amostragem probabilística**.

A população possui **parâmetros**, normalmente, desconhecidos, e por meio do processo de amostragem, temos **estimadores amostrais** que, baseado na amostra coletada, nos fornecem **estimativas**. Obviamente, essas estimativas depende da amostra, isto é, variam entre si e, principalmente, variam do valor real verdadeiro do parâmetro, denominado **erro amostral**.



A principal referência para o estudo clássico em amostragem é @cochran2007sampling. Nesse livro, o autor apresenta 11 etapas para o processo de amostragem. A seguir, apresentamos as principais:

 * Definição dos objetivos da pesquisa
 
 * Definição da população a ser amostrada
 
 * Quais informações da população serão coletadas
 
 * Nível de precisão. Aqui temos o tão conhecido: ["O nível de confiança da pesquisa é de 95%"](https://www.youtube.com/watch?v=6x2IKleWgOQ)
 
 * [Plano amostral](https://faculty.elgin.edu/dkernler/statistics/ch01/1-4.html).
 
 * Pré-teste
 
 * Análise dos dados


Conforme detalhado em @cochran2007sampling e no [link](https://faculty.elgin.edu/dkernler/statistics/ch01/1-4.html), há inúmeras formas de se realizar a amostragem. A mais simples dela, é a amostragem aleatória simples, em que são selecionadas, aleatoriamente, elementos dessa população.


![*Parâmetro, estimador e erro amostral da amostragem aleatória simples. Fonte:@cochran2007sampling*](SLID1.png)


Outra forma de se realizar a amostragem é utilizando planos amostrais mais elaborados. A amostragem estratificada pode ser utilizada quando a população é naturalmente ou intensionalmente dividida por meio de atributos que criam subgrupos heterogêneos. A determinação desse atributo deve estar diretamente relacioada aos objetivos da pesquisa. Por exemplo, caso uma empresa de cosméticos deseje fazer uma pesquisa de mercado para um produto unisex, o atributo de **sexo** parece ser extremamente relevante, pois a opnião das mulheres pode ser completamente diferente dos homens. Dessa forma, se a população é formada por 200 homens e 800 mulheres, temos um estrato por sexo. Após a divisão da população em estratos, realiza-se o processo de amostragem simples em cada estrato.

![*Processo de seleção estratificada. Fonte: [https://faculty.elgin.edu](https://faculty.elgin.edu/dkernler/statistics/ch01/1-4.html)*](stratified.png)

Obviamente, para esse tipo de divisão precisamos **conhecer tal atributo para cada elemento da população**, para só então, realizarmos o processo de seleção estratificada.
A definição do tamanho a ser coletado em cada estrato para a amostra deve ser escolhido de acordo com os objetivos da pesquisa e podem ser encontrados em @cochran2007sampling. O mais utilizado é a alocação proporcional ao tamanho dos estratos, isto é, o tamanho amostral em cada estrato é proporcional ao tamanho do estrato.
 
Nas equações a seguir $N_h$ é o tamanho do estrato $i$ em uma população de tamanho $N$, e $n_h$ é o tamanho da amostra no estrato $h$ em uma amostra de tamanho $n$. Dessa forma, $n_h$ é proporcional a $N_h$. Além disso, o estimador amostral é uma combinação do estimador em cada estrato $h$ ponderado pelo peso do estrato na amostra $\frac{n_h}{n}$

![*Estimador e tamanho  amostral da amostragem estratificada, sendo L o número de estratos. Fonte:@cochran2007sampling*](SLID2.png)

O erro amostral do processo de amostragem estratificado é feito combinando os erros de cada estrato.

![*Variância do estimador da amostragem estratificada, sendo L o número de estratos. Fonte:@cochran2007sampling*](SLID3.png)


Existem inúmeras formas de realizar amostragem. Neste *post*, vamos tratar da amostragem em múltiplos estágios, quando são realizados mais de uma etapa de selação. Por exemplo, no processo de seleção estratificada, acessamos todos os estratos e selecionamos aleatoriamente apenas 1 vez, isto é, dentro de cada estrato. No entanto, é possível que dentro de cada estágio a população esteja dividida em outros atributos, o que requer 2 ou mais estágios. Por exemplo, imagine que a população a ser coletada esta dividida em municípios (estratos), bairros (primeira unidade de seleção) e domicílios (segunda unidade de seleção). Todos os municípios serão selecionados na pesquisa, mas serão **selecionados alguns** bairros de cada município (aleatória simples, por exemplo), e em cada bairro selecionado, serão **selecionados alguns** domicílios (aleatória simples, por exemplo). Dessa forma, temos uma **amostragem em múltiplos estágios**.


O Instituto Brasileiro de Geografia e Estatística - IBGE em conjunto com o  Sistema Integrado de Pesquisas Domiciliares por
amostragem (SIPD), tendo em conhecimento o  Cadastro Nacional de Endereços para Fins Estatísticos (CNEFE), criaram a [Amostra Mestra](https://www.ibge.gov.br/arquivo/projetos/sipd/segundo_forum/segundo_amostra.php) que é feita nas seguintes etapas.



* Estratos: Unidades da Federação (UFs) subdivididos em:
    - Capital;
    - Demais municípios pertencentes à RM ou à RIDE;
    - colar ou expansão metropolitana ou a outra RM;
    - à RIDE com sede em outra UF e
    - Demais municípios da UF.
    
* Etapa 1: Unidades Primárias de Amostragem (UPAs): selecionadas com probabilidade proporcional ao tamanho, medido pelo número de domicílios particulares permanentes ocupados e vagos (DPPs).

* Etapa 2: selecionado amostra de 14 domicílios particulares permanentes ocupados dentro de cada UPA da amostra, por amostragem aleatória simples, baseado no cadastro CNEFE 

Dentre as pesquisas que utilizam esse planejamento amostral, temos Pesquisa de Orçamentos Familiares (POF),  Pesquisa de Orçamentos Familiares
Simplificada (POFs),  Pesquisa Nacional de Saúde (PNS), PME (Pesquisa Mensal de
Emprego), PNAD (Pesquisa Nacional por Amostra de Domicílios) e Pesquisa Nacional por Amostra de Domicílios Contínua (PNAD Contínua).  

### Aplicação PNAD contínua


Adicionalmente, na PNAD contínua, tem-se o [grupo de rotação e número da visita](https://www.ibge.gov.br/arquivo/projetos/sipd/texto_discussao_23.pdf). Essas características foram planejadas pela periodicidade de coleta trimestral, isto é, os domicílios serão coletados ao longo de 3 meses. O esquema de rotação da amostra adotado onde $v=5$ seria o número de visitas a serem realizadas. Neste esquema o domicílio é entrevistado 1 mês e sai da amostra por 2 meses seguidos, sendo esta seqüência repetida $v=5$
vezes. Esse esquema é mais eficiente quando o interesse é a inferência considerando a mudanças dos indicadores ao longo de três meses, com sobreposição de 25% da amostra de um trimestre para
o mesmo trimestre entre anos  @de2007amostra.

O estimador para o total considerando o plano amostral da Amostra Mestra é apresentado por @de2014amostra:

$$   \hat{Y}^{tri}_{r} = \sum_g\sum_i \sum_j\sum_k w^{**}_{gij}\times y_{gijk}    $$

 * $y_{gijk}$ é o valor da variável de interesse $y$ para a pessoa $k$ do domicílio $j$ da UPA $i$ do
grupo de rotação $g$

 * Peso ajustado por calibração (totais populacionais):
$$w^{**}_{gij} = \frac{1}{m_g}\times \frac{N_g}{N_{gi}}\times
    \frac{N^{*}_g}{n_{gi}}\times\frac{n^{*}_{gi}}{n^{**}_{gi}}\times\frac{P_a^{tri}}{\hat{P}_a^{tri}} $$

 * $P_a^{tri}$ é a estimativa populacional produzida pela Coordenação de População e
Indicadores Sociais (COPIS) para o nível geográfico a para o dia 15 do mês do meio do trimestre 


* $\hat{P}_a^{tri}$ é a estimativa populacional obtida com os dados da pesquisa para o nível geográfico ano trimestre


![Estimador do erro amostral PNAD. Fonte:@de2014amostra](SLIDE10.png)
Variância do estimador da PNAD pode ser calculada pela aproximação apresentada por  @de2014amostra:

$$  V(\hat{\theta}) = \sum_{h} \frac{m_h}{m_h - 1} \sum_{i} (\hat{Z}_{hi} - \bar{Z}_{h})^2$$

Onde

$$\hat{Z}_{hi} = \sum_{j} \sum_{k} w^{**}_{gij}\times z_{gijk}$$


$$\bar{Z}_{h} = \frac{1}{m_h}\sum_{i} \hat{Z}_{hi}$$


Esse seria o processo manual para o cálculo do erro. No entanto, é possível calcular o erro amostral em planos amostrais complexos, como o plano de múltiplos estágios da amostra mestra utilizando o pacote @pkgsuurby.

Vamos utilizar os dados da PNAD contínua e comparar as estimativas do pacote Survey e utilizando os procedimentos de aproximação da variância do estimador por meio da Linearização de Taylor @de2014amostra, implementada  por meio da função **estimativaplano**, apresentada a seguir. 

```{r pacotes,message=FALSE,warning=FALSE}
library(PNADcIBGE)
library(readxl)
library(data.table)
library(tidyverse)
library(reldist)
library(survey)
library(knitr)

# Função para cálculo manual
estimativaplano <- function(base,vars,y,flt=NULL){
  
  .vars <- rlang::syms(vars)
  base <- base %>% 
    mutate(bys =  paste(!!!.vars, sep = "-" ))
  
  .vars <- rlang::syms(y)
  base <- base %>% 
    mutate(y =  as.numeric(!!!.vars))
  
  # DEPENDENCIA DA ANÁLISE
  if(length(flt)>0){
    for(itss in 1:length(flt)){
    .vars <- rlang::syms(flt[itss])
    base <- base %>% 
      filter(!is.na(!!!.vars))
    }
  }
  
  
  
  base2 <- base %>%
    group_by(UPA,Estrato,bys) %>%
    summarise(N = sum(ifelse(is.na(y),0,1),na.rm = T),
              peso = sum(ifelse(is.na(y),NA,V1028),na.rm = T),
              zhi = sum(V1028*y,na.rm = T)) %>%
    filter(N>0) %>% as.data.table()
  
  base2[,mh:=length(unique(UPA)),by=c("Estrato","bys")]
  base2[,zhb := sum(zhi*(1/mh),na.rm = T),by=c("Estrato","bys")]
  
  base2_t <- base2 %>%
    group_by(Estrato,mh,bys) %>%
    summarise(vtheta = sum((zhi-zhb)^2,na.rm = T)) 
  
  
  base2_t <- left_join(base %>%
                                    group_by(bys) %>%
                                    summarise(N=sum(ifelse(is.na(y),0,1),na.rm = T),
                                              peso = sum(ifelse(is.na(y),NA,V1028),na.rm = T),
                                              total = sum(y*V1028,na.rm = T)) ,
                                  base2_t %>% 
                                    group_by(bys) %>%
                                    summarise(SE= (sum(vtheta*(mh/(mh-1)),na.rm = T))^(0.5)),
                                  by = "bys") %>%
    mutate(media = total/peso,
           SEmedia = SE/peso,
           CVmedia = (SEmedia/media))
  
  
  
  a <- data.frame(do.call(
    "rbind",
    (str_split(base2_t$bys, "-",n=length(vars)))
  ))
  
  names(a) <- vars
  base2_t <- cbind(a,base2_t %>% select(-bys))
  
  
  return(base2_t)
  
}


```



### Dados PNAD contínua


Inicialmente, os microdados da PNAD contínua e o dicionário de variáveis podem ser encontrados no [FTP do IBGE.](ftp://ftp.ibge.gov.br/Trabalho_e_Rendimento)

![Dicionário de variáveis.](dir.png)


Vamos utilizar a amostragem feita na PNAD para estimativas de renda por variáveis categóricas.A importação dos dados da PNAD são feitos utilizando o pacote @PNADcIBGE de forma direta do ftp do IBGE. Entre os principais parâmetros:

* ano e trimestre;  

* variáveis do do questionário e que podem ser consultadas no [dicionário](ftp://ftp.ibge.gov.br/Trabalho_e_Rendimento) da PNAD

* data de referência (deflaciona IPCA para data específica)

Na programação a seguir vamos importar os dados do segundo trimestre de 2020, e colocando a preços de 2020. O processo de importação é fundamental para comparação de informações monetárias entre trimestres.

```{r ck0, echo=TRUE,warning=FALSE,message=FALSE}


anot <- 2020
trimestret <- 2



  t0 <- Sys.time()
  
pnadc.svy <- get_pnadc(year=anot, quarter=trimestret,
                       vars=c("V2007","V3009A","V2005",
                              "V4074A","V4078A","V1028",
                              "V2010","VD4001","VD4002",
                              "VD4020",# renda total
                              "VD4030","VD3004",# escolaridade
                              "VD2003",# numero de pessoas no domicílio
                              "V1008",#numero do domicilio
                              "V1022",#Situação do domicílio
                              "V1023",#capital RIDE
                              "V2009",#idade
                              "V2001","V4010","VD4007",
                              "VD3006","VD4005","VD4002",
                              "VD4004A","VD3005","VD2002"),
                       labels=TRUE, design=TRUE)

  print(Sys.time()-t0)
  
```


## Tratamento


O tratamento pode ser feito como em qualquer base de dados, utilizando, por exemplo, os pacotes @tidy e @datatable. O processo de importação do pacote @PNADcIBGE já faz a conversão dos códigos para as descrições. Muitas vezes essas conversão não é muito útil, por exemplo, na análise do tempo de estudo. Para isso, algumas transformações são necessárias.

Vamos criar uma nova variável relacionada à variável V2010 (raça) como "branca" e "não branca". Além disso, a variável tempo de estudo foi convertida para numérica para podermos gerar medidas descritivas. Para isso, utilizamos algumas funções para tratamento de texto por meio de [expressões regulares](https://bookdown.org/davi_moreira/txt4cs/stringR.html).


```{r ck1, echo=TRUE,warning=FALSE,message=FALSE}

base <- pnadc.svy$variables 

base <- base %>%
  mutate(CODUF = substr(Estrato,1,2),
         racabranca = ifelse(V2010=="Branca","Branca","Não Branca"),
         tempestudo = as.numeric(ifelse(gsub("(\\d*)\\s.*","\\1",VD3005)=="Sem",0,gsub("(\\d*)\\s.*","\\1",VD3005))))


```


## Totais 


Baseado no plano da amostra mestra (múltiplos estágios), podemos verificar que o peso expansivos das observações permitem estimar verificar os quantitativos.

O primeiro total a ser analisado é a população total, somando os pesos de todos os registros. Fazemos isso por meio da função estimavaplano colocando como variávavel desagregadora a variável "Ano", que é a mesma para todos os registros, uma variável quantitativa "y" qualquer (peso após a pós-estratificação "V1028") e selecionando apenas a coluna  "peso".


```{r total, echo=TRUE,message=FALSE,warning=FALSE}


#  População total
vars <- c("Ano")
y <- c("V1028")
estimativaplano(base,vars,y) %>% select(peso) %>%kable()

```

Também é possível obter os totais de peso por estratos, isto é, as combinações de UF (UF) e  tipo de municípios (V1023). Os totais, apresentandos na coluna "peso" são iguais as informações que são disponibilizadas na própria base de dados na variável "V1029".


Por exemplo, para o estrato **Acre e capital**, temos 4045 registros que totalizam 412726 considerando a expansão dos pesos amostrais, que é exatamente igual ao total apresentado na V1029.

```{r total2, echo=TRUE,message=FALSE,warning=FALSE}

#  População UF (ESTRATO)
vars <- c("Ano","UF","V1029","V1023")
y <- c("V1028")
UFS <- estimativaplano(base,vars,y) 

head(UFS) %>%  select(UF,V1029,V1023,N,peso)%>%kable()
```

Unidades Primárias de Seleção (UPAs), selecionadas de por meio de aleatória simples e proporcional ao número de domicílios, são subdivisões de domicílios dentro de cada estrato. A UPA 120000037 do Acre tem 29 registros totalizam 2465.152 considerando a expansão dos pesos amostrais. Perceba que, diferente dos estratos, os pesos não são inteiros, pois a plano amostral da PNAD contínua é feito para, no máximo, estimativas a nível de UF.

```{r total3, echo=TRUE,message=FALSE,warning=FALSE}

#  População UF (UPA)
vars <- c("Ano","UF","UPA")
y <- c("V1028")
UPAs <- estimativaplano(base,vars,y)

head(UPAs) %>%  select(UF,UPA,N,peso) %>%kable()
```


Também é possível obter o total de domicílios selecionados em cada UPA. É importanto destacar que esse total é planejado para ser fixo. Após a coleta e com base nas informações coletadas, os pesos são ajustados pós-estratificação. Na UPA 110000016 foram amostrados 11 domicílios, que totalizam, considerando os pesos, um total de 6190.673 pessoas. Para a UPA 120000037, o domicílio 01 tem 2 pessoas que totalizam 170.0105 como peso.
Nessa mesma UPA,  o domicílio 04 foram coletadas informações de 3 pessoas, que totalizam peso de 255.0157.


```{r total4, echo=TRUE,message=FALSE,warning=FALSE}

vars <- c("Ano","UF","UPA","V1008")
y <- c("V1028")
doms <- estimativaplano(base,vars,y)

# Domicílios e pessoas por UPA
domsUPAs <- doms %>%
  group_by(UPA) %>%
  summarise(domicilios = length(Ano),
            pessoas = sum(peso))

head(domsUPAs) %>% kable()


head(doms)  %>%  select(UPA,V1008,N,peso) %>%kable()
```


## Referências e Pacotes

Baseado na referência de @de2014amostra, podemos comparar os resultados do pacote @pkgsuurby com o que a literatura sugere. A tabela a seguir compara a renda média dos brasileiros.


```{r ck2, echo=TRUE,message=FALSE,warning=FALSE}


vars <- c("Ano")

y <- "VD4020"

t0 <- Sys.time()
rendaBR <- estimativaplano(base,vars,y)
Sys.time() - t0

t0 <- Sys.time()
mediapacote <- survey::svymean(x=~VD4020, design=pnadc.svy, na.rm=TRUE)
Sys.time() - t0

#Pacote
mediapacote%>% kable()
#Função estimaplano
rendaBR %>% select(media,N,SEmedia,CVmedia) %>% kable()
 
```


Percebe-se que o erro estimado pela função "estimativaplano" é maior do que a observação pelo pacote @pkgsuurby. Há inúmeros explicações para tal diferença, a começar pela falta de conhecimento específico dos autores com relação às etapas de estimação em planos amostrais complexos. No entanto, baseado nas informações da aproximação proposta por @de2014amostra e tendo em vista que as estimativas dos erros são próximas, mas subestimados, considerando o pacote como verdadeiro, torna-se razoável a utilização da função proposta, principalmente pelos fins didáticos do post. Além disso, a função gera os valores é, pelo menos, 2 vezes mais rápido do que o pacote.

O exercício a seguir compara a renda média dos brasileiros por raça. . Os brasileiros que se declaram com raça "Branca" tem, em média, renda média de R\$ 2971.044, enquanto os que se declaram com raça "Preta", tem, em média, R\$ 1718.876. As duas estimativas tem coeficiente de variação muito próximos. A diferença em mais de R\$ 1000  apresenta evidências da diferença entre remuneração das raças.


```{r ck222, echo=TRUE,message=FALSE,warning=FALSE}


vars <- c("V2010")

y <- "VD4020"

t0 <- Sys.time()
rendaRACA <- estimativaplano(base,vars,y)
Sys.time() - t0


rendaRACA %>% select(V2010,N,media,SEmedia,CVmedia) %>% kable()


ggplot(rendaRACA) +
  geom_bar(aes(weight = media,x = V2010))+
  xlab("Anos de estudo") +
  ylab("Renda média (R$)")




```

No entanto, podemos melhorar essa análise, considerando outras variáveis que possam tornar mais comparável a remuneração recebida. E se a diferença de remuneração média for reflexo do tempo de estudo, e não da raça? Vamos verificar adicionado a desagregação de tempo de estudo.



```{r ck3, echo=TRUE,message=FALSE,warning=FALSE}


vars <- c("tempestudo","V2010")

y <- "VD4020"
rendaestudo <- estimativaplano(base,vars,y)


rendaestudo <- rendaestudo %>% mutate(tempestudo  = as.numeric(tempestudo)) %>% select(V2010,tempestudo,N,media,SEmedia,CVmedia) %>% filter(V2010 %in% c("Branca","Preta") )

rendaestudo <- rendaestudo %>%
  arrange(tempestudo)

ggplot(rendaestudo,aes(x = tempestudo, y = media,col=V2010)) +
  geom_line(size=1)+
  xlab("Anos de estudo") +
  ylab("Renda média (R$)")


```

Por meio das tabela a seguir, percebe-se que aqueles que se declaram com raça "Preta" tem em média, 8.79 anos de estudo, enquanto os que se declaram com raça "Branca" têm, em média, 9.84 anos de estudo. Então, o fator tempo de estudo também pode explicar a diferença de remuneração.

Agora, vamos fazer a diferença entre as remuneração para cada nível de estudo. Percebe-se que a diferença é superior a R$ 1000 apenas nos que estudam mais de 15 anos.

```{r ck33, echo=TRUE,message=FALSE,warning=FALSE}



vars <- c("V2010")

y <- "tempestudo"
TEMestudo <- estimativaplano(base,vars,y) 

TEMestudo %>% select(V2010,media,SEmedia) %>% kable()


difrendaestudo <- rendaestudo %>% select(tempestudo,V2010,media) %>% 
                   filter(!is.nan(media)) %>% spread(key = "V2010",value="media")

difrendaestudo$Dif <- difrendaestudo$Branca - difrendaestudo$Preta

difrendaestudo %>% kable()



```

Vamos fazer a mesma análise utilizando a variável sexo. A análise geral, a mulher ganha em média R$ 500 a menos do que o homem.  No entanto, o tempo médio de estudo da mulher é maior do que o homem. Dessa forma, temos que para o mesmo tempo de estudo, a diferença entre sexo é maior do que entre raças. 


```{r ck3333, echo=TRUE,message=FALSE,warning=FALSE}



vars <- c("V2007")

y <- "tempestudo"
TEMestudo <- estimativaplano(base,vars,y) 

TEMestudo %>% select(V2007,media,SEmedia) %>% kable()


vars <- c("tempestudo","V2007")

y <- "VD4020"
rendasexo <- estimativaplano(base,vars,y)


rendasexo<- rendasexo %>% mutate(tempestudo  = as.numeric(tempestudo)) %>% 
          select(V2007,tempestudo,media,SEmedia,CVmedia) %>% filter(!is.nan(media))


ggplot(rendasexo,aes(x = tempestudo, y = media,col=V2007)) +
  geom_line(size=1)+
  xlab("Anos de estudo") +
  ylab("Renda média (R$)")



difrendasexo <- rendasexo %>% select(tempestudo,V2007,media) %>% 
  spread(key = "V2007",value="media")

difrendasexo$Dif <- difrendasexo$Homem - difrendasexo$Mulher

difrendasexo %>% kable()



```


## Amostragem em *Machine Learning*

Apesar das técnicas de amostagem clássicas não estarem inseridas de forma convecional em *Machine Learning* (ML), muitos conceitos da área são utilizados. Esta seção relata algumas técnicas de ML que utilizam conceitos de amostragem por construção, não com objetivo de apresentar toda a extensa teoria de cada método, mas apresentá-los expondo a utilização de amostragem neste contexto.

### *Bootstrap*

O *bootstrap* foi desenvolvido por  @efron1979 e ajuda a aprender sobre as características da amostra pela obtenção de reamostragem (reamostragem da amostra original com reposição) e utiliza-se essa informação para inferir sobre à população [@casella2011inferencia].

O Método é utilizado, em geral, para estimar o erro padrão de estimadores. Suponha que tenhamos uma amostra $\mathbf{x} = (x_1,x_2,\ldots, x_n)$ e uma estimativa $\hat{\theta}( x_1,x_2,\ldots, x_n) = \hat{\theta}$, ao selecionarmos $B$ reamostras (ou amostras *bootstrap* ) podemos calcular a variabilidade de $\hat{\theta}$:
$$
Var^{*}_{B}(\hat{\theta}) = \frac{1}{B-1}\sum_{i=1}^{B} (\hat{\theta^{*}_i} - \overline{\hat{\theta^{*}}})^2.
$$
A operacionalização da técnica pode ser resumida em $4$ passos: 

(1). Iniciar com uma amostra;

```{r, echo=FALSE, message=FALSE, warning=FALSE}

set.seed(150)
library(ggplot2)
library(data.table)
library(dplyr)
library(tidyverse)

library(datarium)


amostra.original <- data.frame(x= rnorm(150,3,1))

ggplot(amostra.original, aes(x=x,y=..density..)) +
  geom_histogram() + xlab("Amostra Original")


```

(2). Reamostrar da amostra original B vezes;

```{r, message=FALSE, warning=FALSE}
B <- 1000
x.boot <- replicate(B,sample(amostra.original$x,replace=T)) #Não-Paramétrico


```

```{r, echo=FALSE, message=FALSE, warning=FALSE}
x.boot.sample <-  data.frame(B=c(1,2,"3...","...101",102,"103...","...501","502...","...B"),x = t(x.boot[,c(1:3,101:103,501:502,1000)])) %>% gather("Cenario","Amostra Bootstrap",-B)

ggplot(x.boot.sample %>% mutate(B=ordered(B, c(1,2,"3...","...101",102,"103...","...501","502...","...B"))), aes(x=`Amostra Bootstrap`,y=..density..)) +
  geom_histogram() + xlab("Amostras Bootstrap") + facet_wrap(~B)

```


(3). Computar a estatística de interesse para cada amostra;

```{r, message=FALSE, warning=FALSE}
x.bar <- apply(x.boot,2,mean) 
```

```{r, echo=FALSE, message=FALSE, warning=FALSE}
x.boot.sample <-  data.frame(B=c(1,2,"3...","...101",102,"103...","...501","502...","...B"),x = t(x.boot[,c(1:3,101:103,501:502,1000)])) %>% gather("Cenario","Amostra Bootstrap",-B)

ggplot(data.frame(x.bar=x.bar), aes(x=x.bar,y=..density..)) +
  geom_histogram() + xlab(expression(paste("Distribuição Bootstrap de ",bar(x),sep = "")))
```

(4). Utilizar distribuição *bootstrap* para fazer inferência. A distribuição *bootstrap* se trata das **B** estimativas da estatísica de interesse, pois ao se aplicar o método não se tem apenar uma estimativa calculada, mas **B** baseadas nas reamostras.

```{r, message=FALSE, warning=FALSE}
(ep.bootstrap <- sqrt(var(x.bar))) # Estimativa bootstrap não-paramétrica para o erro padrão
round(quantile(x.bar,probs = c(.025,.975)),2) # Intervalor de Confiança Percentil Bootstrap

```

Apesar de apresentarmos um método simples para cálculo do intervalo de confiança bootstrap, alertarmos ao leitor
que é necessário muito cuidado quando calculamos estes intervalos em problemas mais complexos como
regresão linear, robusta ou não-linear.

Em muitos casos, *bootstrap* oferece um estimador razoável que é consistente.
  $$
     Var^{*}_{B}(\hat{\theta}) \xrightarrow{B \xrightarrow{} \infty }  Var^{*}(\hat{\theta}), 
  $$
  $$
    Var^{*}(\hat{\theta})  \xrightarrow{n \xrightarrow{} \infty }  Var(\hat{\theta}), 
  $$
  
no qual,  $Var^{*}(\hat{\theta}) = \frac{1}{n^n-1}\sum_{i=1}^{n^n} (\hat{\theta^{*}_i} - \overline{\hat{\theta^{*}}})^2.$


Apesar do conceito ser intuitivo e de fácil aplicabilidade, a teoria por trás do *bootstrap* é bastante sofisticada, tendo como base as expansões de *Edgeworth*, expansões de funções de distribuição acumulada em torno de uma distribuição normal. A teoria recebe abordagem completa em  @hall2013bootstrap.

@diciccio1988review cita quatro razões para popularidade do método Bootstrap:

* Elegância: como muitas das melhores idéias matemáticas, o princípio em que se baseia a técnica bootstrap,
de reamostragem (*resampling*) dos dados observados para estimaçãao da distribuição populacional,é
simples, elegante e muito poderoso;

* Entendimento: o processo de *resampling* torna fácil a compreensão da idéia básica do método;

* Sutileza Matemática: existe complexidade teórica suficiente no método para fascinar os intelectuais da
área de matemática;

* Facilidade de uso: em contrapartida, para o prático, existe a esperança de uma metodologia automática
cuja implantação em softwares é questão de tempo.


Sua fácil aplicabilidade torna o método uma poderosa ferramenta, com possibilidade de ser utilizado em muitas técnicas de *machine learning*, incluindo algumas que uma medida de variabilidade é difícil de se obter. Ressalta-se que a técnica de *Bootstrap* utiliza, basicamente, o conceito de amostragem aleatória simples com reposição. 

### Avaliação da Qualidade de Ajuste

Com a concepção que nenhum método de ML domina todos os outros sobre todos possíveis conjuntos de dados, existe a necessidade de avaliar cada técnica testada e, assim, identificar a melhor para cada caso e propósito. O objetivo de muitas aplicações de *machine learning* é fazer previsões para observações não vistas anteriormente e, usualmente, utiliza-se alguma medida que dimensione o quanto  o valor da resposta previsto para uma determinada observação está próximo do valor de resposta verdadeiro para essa observação. O erro quadrático médio, dado pela expressão abaixo, é amplamente utilizado no contexto de regressão.

$$
EQM = \frac{1}{N}\sum_{i=1}^{N} ((y_i) - \hat{f}(x_i))^2,
$$
no qual $\hat{f}(x_i)$ é a previsão para a observação $i$. Quanto menor o valor do $EQM$, mais perto a previsão está do valor verdadeiro. A utilização míope desta regra pode gerar conclusões equívocadas sobre a qualidade de ajuste, abaixo apresentamos um exemplo.

![ Dados simulados de uma função ($f$) hipotética, mostrada em preto. Três estimativas ($\hat{f}$) são mostradas: a linha de regressão linear (curva laranja) e dois ajustes da interpolação spline com suavização (curvas azul e verde). Fonte: @james2013introduction ](regression.png)

Com parâmetros suficientes, um modelo ajusta qualquer conjuntos de dados e a utilização direta de um critério de seleção que minimize o erro quadrático médio pode incorporar *overfitting* a análise. Pode ser observado na imagem acima que a inclusão de mais termos ao modelo (linhas azul e verde) melhorou o ajuste, mas isso pode restringir a capacidade do modelo de generalização para novas situações.  Johnny Von Neumann disse uma famosa frase: "com quatro parâmetros eu posso ajustar um elefante, e com cinco eu posso fazê-lo mexer seu tronco”[@mayer2010drawing].

Com objetivo de mitigar essa possível complicação, as técnicas de ML, em geral, utilizam a abordagem de dividir a amostra original em amostras de treino e de teste para avaliação da qualidade de ajuste. A técnica consite em utilizar o conceito de amostragem aleatória simples e dividir o conjuntos de dados em duas partes, uma para estimar os coeficientes do modelo e outra para avaliar a qualidade de previsão do modelo em observações que não fizeram parte do processo de estimação. O  menor erro de teste (diferença entre valor previsto e verdadeito para amostra de teste) pode ser definido como critério de escolha do modelo.

### Validação Cruzada

A estimativa do erro de teste (erro calculado utilizando a amostra de teste) pode apresentar alta variabilidade porque é dependente da amostragem realizada no conjuntos de dados. De forma mais específica, o processo de determinação das amostras de treino e teste envolve uma amostragem aleatória simples e, assim, se o processo for repetido $k$ vezes, ou seja, composiçaõ de $k$ amostras de teste, teremos $k$ diferentes estimativas de erro de teste as quais podem apresentar alta variabilidade. As técnicas de Validação Cruzada (*Cross Validation*) refinam o conceito de validação do modelo.

A técnica de *Leave-one-Out* é diretamente relacionada ao conceito de avaliação de qualidade de ajuste e divisão da amostra original em duas, mas ao invés de dividir a amostra original em duas de tamanho $m$ e $(n-m)$, divide a amostra em uma de tamanho $n-1$ e outra de tamanho $1$. Utiliza-se as $n-1$ observações para estimar o modelo e a observação deixada de fora para avaliar a qualidade de previsão fora da amostra (validação),  repete-se esse processo iterativamente até que todas as observações tenham sido deixado de fora em algum momento. Dessa forma, se tem não apenas uma estimativa de erro de teste, mas $n$ que são utilizadas para estimar o erro de teste da validação cruzada (média dos erros individuais). Esse método reduz o viés da estimativa do erro de teste porque utiliza $n-1$ observações para estimar o modelo e não é afetado pela aleatoriedade característico de um processo de amostragem.

Uma desvantagem de *Leave-one-Out* é o custo computacional porque é necessário estimar o modelo $n$ vezes e no contexto de *big data* isto se torna impraticável. Uma alternativa é o método de *k-folds* que envolve dividir aleatoriamente o conjunto de observações em k grupos de tamanho aproximadamente igual. O primeiro grupo é tratado como um conjunto de validação e o método é ajustado nos k - 1 grupos restantes. As figuras abaixo ilustram as técnicas. Vale ressaltar que as técnicas de Validação Cruzada não são utilizadas apenas para avaliar a qualidade de ajuste, mas também para definir parâmetros de ajuste (*tuning parameter*) que não podem ser estimados diretamente da base de dados (ex. o parâmetro de penalidade de uma regressão *ridge*).


![ *Leave-one-Out* ](loocv.png){width=40%} ![ *k-folds* ](kfolds.png){width=40%}

O exemplo em R abaixo aplica o conceito no contexto de regressão linear. O código foi desenvolvio com objetivo de auxiliar no entendimento dos métodos, mas a maior parte das técnicas de Validação Cruzada estão implementadas de forma otimizada no pacote *caret*.


```{r, message=FALSE, warning=FALSE}

##BASE DE DADOS EXEMPLO
data(marketing) #Base de dados exemplo contendo o impacto de três mídias publicitárias (youtube, facebook e jornal) nas vendas.

#MÉTODO LEAVE ONE OUT

cv.errors.loo <- numeric()
for(i in 1:nrow(marketing)){
  fit =lm(sales∼.,data=marketing[-i,])
  pred=predict(fit,marketing[i,])
  cv.errors.loo[i] = (marketing$sales[i]-pred)^2
}

mean(cv.errors.loo) #Erro Quadrático Médio Leave One Out



# MÉTODO K-FOLDS

k=10
set.seed(114)
folds=sample(1:k,nrow(marketing),replace =TRUE)#Amostra aleatória simples


cv.errors.kfolds <- numeric()
for(j in 1:k){
  fit =lm(sales∼.,data=marketing[j!=folds,])
  pred=predict(fit,marketing[j==folds,])
  cv.errors.kfolds[j] = mean( (marketing$sales[folds ==j]-pred)^2)
}

mean(cv.errors.kfolds) #Erro Quadrático Médio K-Folds

```

### Árvore de Decisão

Técnica utilizada para problemas de classificação e regressão que consiste em dividir o espaço preditor em regiões (${X|X_j>s}$ e ${X|X_j<s}$) que fornecem a maior redução de alguma medida de erro. O método tende a fornecer boa interpretabilidade mas baixo poder de previsão.

De maneira simplificada, o método pode ser entendido em duas etapas: dividir o espaço preditor (determinar possíveis valores para as variáveis preditoras $X_1,X_2,...,X_k$) em $p$ regiões distintas e não sobrepostas $R_1,R_2,...,R_p$;para cada observação que ficar na região $R_p$, fazemos a mesma previsão, que é simplesmente a média dos valores de resposta para as observações de treinamento em $R_p$ [@james2013introduction].

```{r, message=FALSE, warning=FALSE}
library(tree)

marketing <- marketing %>% mutate(sales.c = factor(ifelse(sales<15,0,1)))

tree.mkt =tree(sales.c∼.-sales ,marketing)

summary(tree.mkt)

plot(tree.mkt)
text(tree.mkt,pretty = 0)

```


### Bagging

   Procedimento de propósito geral para reduzir a variância de um método de ML. Utilizando conceitos de inferência estatística (dado $x_1,x_2,...,x_n$ $iid$ normais então $\overline{x} \sim N(\mu,\mathbf{\frac{\sigma^2}{n}})$, ou seja, variânvia de $\overline{x}$ é menos do que variância de $x$), aplica-se o método de ML em B amostras *Bootstrap* e utiliza-se a média dos resultados como previsão.
     $$
     \hat{f}(x) = \frac{1}{B}\sum_{b=1}^B \hat{f}^b(x).
     $$
     Aplicado em árvore de decisão fornece melhorias na precisão ao combinar centenas ou mesmo milhares de árvores em um único procedimento.
    
```{r, message=FALSE, warning=FALSE}

library (randomForest)
set.seed (1)

bag.mkt =randomForest(sales.c∼.-sales,data=marketing, mtry=3, importance =TRUE) #mtry = 3 considera todas as variáveis (Bagging)
bag.mkt 

```


### Floresta Aleatória 

O método de *Bagging* tende a utilizar as mesmas variáveis para construir as árvores de decisão em cada amostra *Bootstrap*, gerando árvores correlacionadas o que impacta a redução de variância.  As florestas aleatórias (FA) superam esse problema, forçando cada divisão a considerar apenas um subconjunto dos preditores.
  
   $$
  \hat{f}^{*}(x) = \frac{1}{B}\sum_{b=1}^B \hat{f}^{*}_b(x).
   $$

Em geral utiliza-se um grupo de $m=\sqrt{p}$ variáveis para construir cada árvore de decisão.

```{r, message=FALSE, warning=FALSE}

library (randomForest)
set.seed (1)

rf.mkt =randomForest(sales.c∼.-sales,data=marketing, mtry=2, importance =TRUE) 
rf.mkt 

```

### Floresta Aleatória em *Big* *Data*

A redução de correlação entre as árvores de decisão é a motivação para construção de FA. Entretanto o desempenho do método é dependente da performance de cada árvore de decisão, ou seja, se muitas árvores apresentarem baixo poder de previsão, o método irá apresentar estimativas ruins (ou classificações ruins).

No contexto de *Big Data*, uma grande proporção de preditores não são informativos para a análise e a utilização de amostragem aleatória simples tende a gerar árvores de decisão com baixo poder de previsão.
@ye2013stratified  apresentam uma metodologia que utiliza amostragem **estratificada** para construção da floresta aleatória.

 A ideia dos autores se baseia em dividir as $p$ variáveis em dois grupos: muito informativo e pouco informativo. Em seguida, selecionamos aleatoriamente variáveis de cada grupo, garantindo que temos características representativas de ambos. Esta abordagem garante que cada subespaço contenha informação suficiente para a finalidade.  
 
Considera-se uma função não negativa $\phi$ que capture o grau de explicação da variável $p_i$ com respeito a variável resposta $Y$. Normaliza-se $\phi_i$:

$$
\theta_i = \frac{\phi_i}{\sum_{k=1}^N \phi_k}
$$

As variáveis preditoras são ordenadas segundo $\theta_i$ e determina-se um limiar $\alpha$ que irá dividir as variáveis em dois grupos. O algoritmo proposto constrói um modelo de floresta aleatório considerando os estratos para produzir cada árvore de decisão.



### Referências
@article{Hansen1982,
author = {Hansen, Lars Peter},
doi = {10.2307/1912775},
file = {:home/joaogabrielsouza/{\'{A}}rea de Trabalho/PPGA/Doutorado{\_}Financas/Tese - Financas Bancarias/Artigos Tese/Ensaio 2/Hansen - Large Sample Properties of Generalized Method of Moments Estimators - 1982 - Econometrica.pdf:pdf},
issn = {00129682},
journal = {Econometrica},
month = {jul},
number = {4},
pages = {1029-1054},
title = {{Large Sample Properties of Generalized Method of Moments Estimators}},
url = {https://www.jstor.org/stable/1912775?origin=crossref},
volume = {50},
year = {1982}
}

@misc{Cochrane2017,
author = {Cochrane, John H},
file = {:home/joaogabrielsouza/{\'{A}}rea de Trabalho/PPGA/Artigos/Cochrane - GMM_notes.pdf:pdf},
pages = {210--231},
title = {{GMM Notes}},
year = {2017}
}

@article{Newey1987,
author = {Newey, Whitney K and West, Kenneth D},
file = {:home/joaogabrielsouza/.local/share/data/Mendeley Ltd./Mendeley Desktop/Downloaded/Newey, West - 1987 - A Simple, Positive Semi-Definite, Heteroskedasticity and Autocorrelation Consistent Covariance Matrix.pdf:pdf},
journal = {Econometrica},
number = {3},
pages = {703--708},
title = {{A Simple, Positive Semi-Definite, Heteroskedasticity and Autocorrelation Consistent Covariance Matrix}},
volume = {55},
year = {1987}
}

@article{Fama1993,
author = {Fama, Eugene F. and French, Kenneth R.},
doi = {10.1016/0304-405X(93)90023-5},
file = {:home/joaogabrielsouza/.local/share/data/Mendeley Ltd./Mendeley Desktop/Downloaded/Fama, French - 1993 - Common risk factors in the returns on stocks and bonds.pdf:pdf},
issn = {0304405X},
journal = {Journal of Financial Economics},
number = {1},
pages = {3--56},
pmid = {295138},
publisher = {Elsevier},
series = {Journal of Financial Economics},
title = {{Common risk factors in the returns on stocks and bonds}},
volume = {33},
year = {1993}
}

@article{mayer2010drawing,
  title={Drawing an elephant with four complex parameters},
  author={Mayer, J{\"u}rgen and Khairy, Khaled and Howard, Jonathon},
  journal={American Journal of Physics},
  volume={78},
  number={6},
  pages={648--649},
  year={2010},
  publisher={American Association of Physics Teachers}
}

@book{james2013introduction,
  title={An introduction to statistical learning},
  author={James, Gareth and Witten, Daniela and Hastie, Trevor and Tibshirani, Robert},
  volume={112},
  year={2013},
  publisher={Springer}
}
