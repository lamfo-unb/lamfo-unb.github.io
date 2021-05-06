---

  layout: post
  title: "Autoencoder"
  lang: pt
  header-img: img/manipulacao_data.table/img_data.table.png
  date: 2021-11-21 23:59:07
  tags: [Autoencoder, Machine Learning, Statistics]
  author: Fernanda Amorim, Pedro Watuhã
  comments: true
---

# Autoencoder

*Autoencoder* é uma rede neural que utiliza aprendizado não-supervisionado para treinamento e tem como objetivo aprender reconstruções próximas aos dados de entrada. Um Autoencoder possui um codificador e um decodificador, que podem ser representados como:

$$Z=f_{\theta} =S(W X+b) (I) \\
Y′=f_{\theta}′=S(W′Z+b′) (II)\\
||X−Y′|| (III)$$

A primeira equação (I) é a equação do codificador Z que faz o mapeamento dos dados de entrada X por meio a função de ativação S, com a matriz de pesos W e o vetor de viés b. A segunda equação (II) é a equação do decodificador que mapeia Z de volta ao espaço de entrada original, como uma reconstrução feita pela mesma transformação do  codificador.  O  erro  de  reconstrução  é  calculado  pela  diferença  entre  os  dados  de entrada X e a reconstrução Y′.


O codificador reduz a dimensionalidade dos dados de entrada e o decodificador refaz os dados de entrada e o objetivo principal do algoritmo é minimizar o erro de reconstrução (LEJON; KYÖSTI; LINDSTRÖM, 2018; FAN et al., 2018).A figura abaixo mostra como funciona a estrutura do Autoenconder.


![](https://i.imgur.com/7BoQhex.png)
Fonte: FAN et al., 2018






A Figura  evidencia dois tipos de arquiteturas possíveis para algoritmo de *Autoencoder*. A primeira arquitetura apresenta uma estrutura sub completa onde $p < m$, e o modelo aprende uma representação compacta dos dados de entrada ($X$). Já a segunda arquitetura apresenta uma estrutura completa, onde $p > m$, o modelo, neste caso, aprende uma representação esparsa (FAN et al., 2018). De acordo com  Schölkopf, Platt e Hofmann (2007), representações esparsas viabilizam interpretações mais simples para os dados de entrada a partir de um pequeno número de ''partes'', dessa forma é extraída uma estrutura oculta dos dados. Um vantagem no uso deste método é devido a extração de recursos que são úteis para as interpretações e aprimoramento de recursos que não são úteis . Há várias possibilidades de arquitetura de algoritmos *Autoencoder*, o modelo mais simples é construído com 3 camadas conectadas (uma camada de entrada, uma camada oculta e uma camada de saída). Uma maneira de se melhorar a capacidade de reconstrução é adicionando novas camadas ocultas ou neurônios ocultos, entretanto é preciso um conhecimento específico dos dados para avaliar a melhor estrutura e testar quais geram melhores resultados (FAN et al., 2018).

# Aplicação

Dentre as diversas possíveis aplicações de *Autoencoder* está a redução de ruídos. Para demonstrar isso, utilizou-se a base de dados de textos em papeis amassados disponibilizada no [Kaggle](https://www.kaggle.com/c/denoising-dirty-documents/data) junto a estrutura base do *Autoencoder*. Inicialmente, 144 imagens foram usadas para compor o train set, que foram redimensionalisadas em uma matriz para se adequarem à arquitetura.

![](https://i.imgur.com/vwa60wZ.png).

Em seguida, elaborou-se a estrutura do *Autoencoder* utilizando camadas de Convoluções, de UpSampling e de MaxPooling de forma simétrica. Como otimizador, optou-se pela Adam, pois, mesmo agindo, por vezes, de forma agressiva, demandou menor capacidade do processador e apresentou resultados de forma mais rápida.

![](https://i.imgur.com/eCdBNXj.png)

Por fim, aplicando o *Autoencoder* elaborado à amostra de dados, usando um batch size de 8 e 50 epochs, observou-se os seguintes resultados.

![](https://i.imgur.com/MIdSPLe.png)

Que, quando aplicado às imagens de teste fornecidas, foi capaz de obter ótimos resultados. No último dos exemplos, pode-se observar a utilização de uma imagem real, que não obtem o resultado almejado, visto que a base de dados apresentou papéis em melhor estado que o fornecido e com características próprias, além da qualidade da foto tirada. Todavia, esperar-se-ia que, com uma base de dados maior e maior capacidade de processamento, o modelo seria capaz de limpá-la.

![](https://i.imgur.com/VlX7Adx.png)

![](https://i.imgur.com/rFsE0DM.png)

![](https://i.imgur.com/84J2FRZ.png)
