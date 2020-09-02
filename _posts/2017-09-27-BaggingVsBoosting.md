---
layout: post
title: Classificadores Ensemble&#58; Bagging e Boosting
lang: pt
header-img: img/0026.png
date: 2017-09-27 14:15:07
tags: [statistics, ML, ensemble]
author: Maisa Aniceto
comments: true
---

## Classificadores *Ensemble*


Estudos recentes têm mostrado que métodos de classificadores *ensemble* possuem performance melhor do que técnicas de inteligência artificial sozinhas.

Mas você sabe o que é um classificador *ensemble*? 
Um classificador *ensemble* (também chamado de comitê de *learners*, mistura de especialistas ou sistema de classificadores múltiplo), consiste em um conjunto de classificadores treinados individualmente, classificadores de base, cujas decisões são de alguma forma combinadas (Marques et al., 2012).

Nesse post apresentarei dois algoritmos *ensemble* tradicionais, o *Bagging* e o *Boosting*; e mais especificamente, o *AdaBoost*.

![](https://i.imgur.com/88aeJst.jpg)

### Bagging

O *Bagging (Bootstrap Aggregating)*, um método proposto por Breiman em 1996, gera um conjunto de dados por amostragem *bootstrap* dos dados originais (Já falamos de *bootstrap* aqui: https://lamfo-unb.github.io/2017/06/28/Bootstrap/). O conjunto de dados gera um conjunto de modelos utilizando um algoritmo de aprendizagem simples por meio da combinação por votos para classificação. O seu uso é particularmente atraente quando a informação disponível é de tamanho limitado.

No *Bagging* os classificadores são treinados de forma independente por diferentes conjuntos de treinamento através do método de inicialização. Para construí-los é necessário montar *k* conjuntos de treinamento idênticos e replicar esses dados de treinamento de forma aleatória para construir *k* redes independentes por re-amostragem com reposição. Em seguida, deve-se agregar as *k* redes através de um método de combinação apropriada, tal como a maioria de votos (Breiman, 1996).

Para garantir que há amostras de treinamento suficientes em cada subconjunto, grandes porções de amostras (75-100%) são colocadas em cada subconjunto. Com isso, os subconjuntos individuais de formação se sobrepõem de forma significativa, com muitos casos fazendo parte da maioria dos subconjuntos e podendo até mesmo aparecer várias vezes num mesmo subconjunto. A fim de assegurar a diversidade de situações, um *learner* de base relativamente instável é usado para que limites de decisão diferentes possam ser obtidos, considerando-se pequenas perturbações em diferentes amostras de treinamento (Wang, 2011).

O resumo do pseudo código do *Bagging*, proposto por Breiman, é o seguinte:


| *Bagging* | 
| -------- |
|   Entrada: Dataset $$    D=\{(z_1,y_1),(x_2y_2),...,(x_m,y_m)\}: $$ 
| Número de rounds de aprendizagem $$T$$.
| Processo: Para $$t=1,2,...,T:$$ 
| (a.) Forma conjuntos bootstrap de dados $$S_t$$ selecionando $$m$$ exemplos aleatórios do conjunto de treinamento com substituição e (b.) Deixa $$h_t$$ ser o resultado da base de treinamento do algoritmo baseado em $$S_t$$
| fim.			
| Saída: 		Classificador combinado: 	$$ H(x)=maioria(h_{1}(x),...,h_{T}(x)) $$
 
 

### *Boosting* e *AdaBoost*

No *Boosting*, de forma semelhante ao *Bagging*, cada classificador é treinado usando um conjunto de treinamento diferente. A abordagem por *Boosting* original foi proposta por Schapire em 1990. A principal diferença em relação ao *Bagging* é que os conjuntos de dados re-amostrados são construídos especificamente para gerar aprendizados complementares e a importância do voto é ponderado com base no desempenho de cada modelo, em vez da atribuição de mesmo peso para todos os votos. 
Essencialmente, esse procedimento permite aumentar o desempenho de um limiar arbitrário simplesmente adicionando *learners* mais fracos. Dada a utilidade desse achado, *Boosting* é considerado uma das descobertas mais significativas em aprendizado de máquina (LANTZ, 2013).

Já o **AdaBoost**, "*Adaptive Boosting*", é uma combinação das ideias de *Bagging* e *Boosting* e não exige um grande conjunto de treinamento como o *Boosting*. Inicialmente, cada exemplo de formação de um determinado conjunto de treinamento tem o mesmo peso.

Para treinar o $$k−esimo$$ classificador como um “modelo de aprendizagem fraca”, $n$ conjuntos de amostras de treinamento (n &lt;m) entre $$S$$ são usadas para treinar o $$k−esimo$$ classificador. Em seguida, o classificador treinado é avaliado por $$S$$ para identificar os exemplos de treinamento que não foram classificados corretamente (TSAI, 2014). A rede $$k+1$$ é então treinada por um conjunto treinado modificado que reforça a importância desses exemplos classificados incorretamente.

Este procedimento de amostragem será repetido até que $k$ amostras de treinamento sejam construídas para a construção da $$k−esima$$ rede. Portanto, a decisão final baseia-se na votação ponderada dos classificadores individuais. 
O pseudo código do *Adaboost*, segundo Wang (2011), é o seguinte:
       
       

| *Adaboost* | 
| -------- | 
| Entrada:  Dataset $$D=\{(z_1,y_1),(x_2,y_2),...,(x_m,y_m)\}$$;		
|Algoritmo de base de aprendizagem $$L$$; Número de rounds de aprendizagem $$T$$.
|Processo: $$ D_1(i) = 1/m.  $$ \% Inicializa a distribuição de pesos. 
|Para $$t=1,2,...,T$$: $$ h_1= L(D,D_t); $$
|Treina a base de aprendizado $$h_t$$ para D usando a distribuição $$D_t$$ 		
|$$ \epsilon_i= Pr_{i \cong D_i} [h_t(x_i \neq y_i)]; $$ 
|Mede o erro de $$h_t$$ 				
|$$\alpha_t = \frac{1}{2}\ln \frac{1 - \epsilon_t}{\epsilon_t} 	$$
|Determina o peso de $$h_t$$								
|$$ D_{t+1}(i)=\frac{D_{t}(i)}{Z_t}  \left\{\begin{array}{rll}				exp({-\alpha_t}) & \hbox{se} & h_{t}(x_{i})= y_i 	exp({\alpha_t}) & \hbox{se} & h_{t}(x_{i}) \neq y_i						\end{array}\right.$$
|Atualiza a distribuição
|$$    =\frac{D_{t}(i)exp(-\alpha_{t}y_{i}h_{t}(x_{i}))}{Z_{t}}      $$ Fator de normalização que permite $D_{t+1}$ ser uma distribuição
|			fim.			
|Saída:  $$  H(x)=sign\left( \sum_{t=1}^{T}\alpha_{t}h_{t}(x)\right) $$   | 
       
       

O pacote ipred do *software* R, oferece uma aplicação clássica *Bagging* em *Decision Tree*. Para treinar o modelo é utilizada a função "bagging()". No caso do *Adaboost*, pode ser utilizado o pacote C50.


Nesse post: https://lamfo-unb.github.io/2017/08/17/Modelos-Compostos/ você encontra uma aplicação com modelos regressivos compostos para estimativas de preço. É mostrado como uma combinação simples entre dois modelos podem impactar significantemente na sua predição.

Em um próximo post apresentaremos outros exemplos práticos de como utilizar esses classificadores *ensemble*. 


#### Referências

[Breiman(1996)] Breiman, L., 1996. Bagging Predictors. Machine Learning 24 (2), 123–140.

[Lantz(2013)] Lantz, B., 2013. Machine Learning with R. Packt Publishing Ltd.

[Marques et al.(2012)] Marques, A., Garcia, V., Sanchez, J., Sep 2012. Exploring the behaviour of base classifiers in credit scoring ensembles. Expert Systems with Applications 39 (11), 10244–10250.

[Schapire(1990)] Schapire, R. E., Jun 1990. http://dx.doi.org/10.1007/BF00116037 The strength
of weak learnability. Mach Learn 5 (2), 197–227.

[Tsai et al.(2014) Tsai, Hsu, and Yen] Tsai, C.-F., Hsu, Y.-F., Yen, D. C., Nov 2014. A comparative study of classifier ensembles for bankruptcy prediction. Applied Soft Computing 24, 977–984.

[Wang et al.(2011) Wang, Hao, Ma, and Jiang] Wang, G., Hao, J., Ma, J., Jiang, H., Jan 2011. A comparative assessment of ensemble learning for credit scoring. Expert Systems with Applications 38 (1), 223–230.
