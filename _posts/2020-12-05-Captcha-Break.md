---
layout: post
title: "Introdução ao reconhecimento de imagens"
lang: pt
header-img: img/manipulacao_data.table/img_data.table.png
date: 2020-12-05 23:59:07
tags: [captcha, imagens, Machine Learning]
author: Marcius Lima, Eduardo Rubik, Rafael Morais
comments: true
---

# Introdução ao reconhecimento de imagens

## Como o computador “vê” uma imagem?

Para um computador a imagem é representada como um array numérico de 3 dimensões. Cada número que compõe o array varia entre 0 (preto) e 255 (branco). Na imagem do gato abaixo, ela apresenta uma largura de 248 pixels e uma altura de 400 pixels. Também possui 3 canais de cor Vermelho, Verde e Azul (RedGreenBlue - RGB). Portanto, multiplicando a largura pela altura e pela quantidade de canais de cor, temos 248 x 400 x 3 = 297.600 números que representam a imagem.


![](https://i.imgur.com/8IdE5tW.png)
**Imagem 1** Gato | Stanford University

A pergunta então é: como ensinar o computador a interpretar esses números como um gato?

Um ponto importante a ser observado é que do ponto de vista de um algoritmo, pequenos detalhes que muitas vezes são de fácil discernimento para o olho humano trazem desafios para o computador. Por exemplo, um objeto pode estar em diferentes ângulos com relação à câmera (variação de ângulo), o tamanho do objeto pode variar de imagem para imagem (escala), o objeto pode estar deformado (deformação), parte do objeto pode estar cortado ou ocultado na imagem (oclusão), a iluminação pode mudar (iluminação), o ambiente atrás do objeto pode se misturar com o mesmo entre outros.

Para que possamos classificar de forma satisfatória uma imagem, o classificador precisa manter a consistência no resultado sujeito às diferentes combinações desses fatores.

![](https://i.imgur.com/FwE4A4Y.jpg)
**Imagem 2** Desafios | Stanfod University

Uma observação importante é que em Aprendizado de Máquina, utiliza-se a variação de pixel (0-255) das imagens normalizada. Dessa forma, como estamos lidando com imagens, computa-se uma imagem média do conjunto de treino  e depois ela é subtraída de todas as imagens. O resultado são imagens com pixels que variam entre -127 a 127 aproximadamente. Pode-se então, dependendo do caso, mudar a escala desses valores, fazendo-os variar entre -1 e 1.

## Tratamento de Imagens

Tratar imagens significa manipulá-las de modo a facilitar sua “visualização” pelo computador. Podemos girá-las, torná-las mais escuras ou mais claras, acentuar determinadas cores, “pixelar” a imagem, distorcer, entre vários outros métodos. Os tratamentos a serem aplicados dependem do objetivo ao qual se quer chegar. Citaremos alguns exemplos de aplicações utilizando o pacote OpenCV em Python.

Podemos filtrar imagens com diversos objetivos como, por exemplo, remover ruídos, embaçar imagens ou acentuar vértices de objetos. Para isso, utilizamos o que se chama de low-pass filters (LPF) e high-pass filters (HPF). Em nosso exemplo, LPFs ajudam a remover ruídos e embaçar imagens, enquanto HPFs acentuam vértices de objetos dentro da imagem.

A partir de agora utilizaremos um termo chamado de Convolução. A convolução é uma operação em que um operador linear utilizando-se de dois sinais gera um terceiro sinal. Portanto para fazer tal operação utilizaremos um kernel ou uma matriz de convolução.

### O Kernel e suas funções

Um kernel é um filtro que utilizaremos em nossas imagens. Ele passará por toda a imagem, da esquerda para a direita, de cima para baixo, aplicando o produto da convolução. No final, teremos uma imagem filtrada.

![](https://i.imgur.com/nvORWfF.png)
**Imagem 3** Convolução | Data Science Stack Exchange


O kernel não precisa ter sua altura e largura de tamanhos iguais, isto é, não é necessário que seja um quadrado. A depender do sinal que se deseja analisar, pode-se alterar o formato do kernel para melhor o ajustar ao problema. Na prática, geralmente utiliza-se números ímpares para definir a altura e largura do kernel, com o intuito de deixar o output simétrico (ter pixel central).

Importante notar que geralmente o número de parâmetros cresce com o tamanho do kernel, assim como o tempo computacional. Isso acontece porque grande parte dos parâmetros de treinamento de uma camada convolucional está nos kernels.

#### Passo (Stride)

O movimento do kernel pelos pixels da imagem pode ser alterado. Geralmente o kernel se move da esquerda para a direita, de baixo para cima. Para alterar como o kernel se movimenta, alteramos seu “passo” (stride). Um stride de (1,5) significa um movimento do filtro de 5 por 5 pela horizontal e 1 por 1 pela vertical. O resultado disso é uma redução da dimensão em 5 vezes.

Quanto maior o stride, menor o tempo computacional.

#### Preenchimento (Padding)

Em algumas ocasiões, o tamanho do output precisa ser do tamanho do input. Temos que lembrar que o kernel diminui o tamanho do input. O preenchimento (ou padding) é a ferramenta para adicionar (preencher) um determinado número de pixels nas laterais da imagem. Desse modo, o output fica com o mesmo tamanho que o input. Geralmente o padding é 0, não havendo a necessidade de igualar os tamanhos.

O tempo computacional aumenta com o aumento do preenchimento, apesar de na maioria das vezes o incremento ser marginal.

#### Dilatação (Dilation)

A dilatação (dilation) pode ser entendida como a largura do núcleo do kernel. Não há muito impacto em tempo computacional, sendo como no Padding, marginal o incremento.

#### Grupos (Groups)

A ideia de grupos é basicamente tratar dados diferentes independentemente e no final o output de cada processamento ser concatenado. Importante salientar que o número de grupos tem que ser divisor comum entre o número de inputs e o número de outputs.


## Rede Neural Convolucional


Um modelo de deep learning muito utilizado para visão computacional é a Rede Neural Convolucional (CNN). Podemos pensar na CNN como uma rede neural que utiliza diversas cópias de um mesmo neurônio, de modo a ter uma capacidade de complexidade grande, mantendo um número reduzido de parâmetros.

### Estrutura de uma CNN

De acordo com o Deep Learning Book Brasil, "a arquitetura de uma ConvNet é análoga àquela do padrão de conectividade de neurônios no cérebro humano e foi inspirada na organização do Visual Cortex. Os neurônios individuais respondem a estímulos apenas em uma região restrita do campo visual conhecida como Campo Receptivo. Uma coleção desses campos se sobrepõe para cobrir toda a área visual". De forma prática, uma Rede Neural Convolucional segue a seguinte estrutura.

![](https://i.imgur.com/Ag1UKhk.png)
**Imagem 4** Estrutura | A Beginner's Guide To Understanding Convolutional Neural Networks - Adit Deshpande

#### Convolution

Convoluções são filtos que têm como objetivo identificar padrões. A convolução, do ponto de vista matemático, é uma operação linear entre duas funções.

#### Pooling Layers

A camada chamada de "pooling layer" tem como objetivo reduzir a quantidade de variáveis por meio de amostragem. O método mais comum é conhecido como MaxPooling, que pega o maior valor dentro de um mapa de variáveis, permitindo a redução de variáveis, conforme demonstrado na figura abaixo. Essa etapa ajuda a reduzir a quantidade de informação da rede.

![](https://i.imgur.com/HWQF4dO.png)
**Imagem 5** Max pooling | Andrew Ng

#### Fully connected layers

Fully conneted layers são camadas comuns em redes neurais, onde todos os inputs de uma camada estão conectados com as unidades da próxima camada. Em geral, sãos as últimas camadas de uma rede neural.

## Aplicação

A Rede Neural Convolucional pode ser aplicada utilizando-se o Framework TensorFlow. Um explicação simples e direta por ser encontrada [aqui](https://www.tensorflow.org/tutorials/images/cnn).

## Referências e links interessantes

[Convolutional Neural Networks for Visual Recognition, Stanford](https://cs231n.github.io/)

[Open CV](https://docs.opencv.org/master/d4/d13/tutorial_py_filtering.html)

[Resolvendo Captchas com Redes Neurais Convolucionais - Matheus Facure](https://matheusfacure.github.io/2017/03/12/cnn-captcha/)

[Definição de Pooling Layer](https://machinelearningmastery.com/pooling-layers-for-convolutional-neural-networks/)

[Capítulo sobre Redes Neurais Convolucionais](http://deeplearningbook.com.br/introducao-as-redes-neurais-convolucionais/)
