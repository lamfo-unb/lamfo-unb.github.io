---
layout: post
title: Restricted Boltzmann Machine - RBM
lang: pt
header-img: img/home-bg.jpg
date: 2020-07-29 23:59:07
tags: [Restricted Boltzmann Machine, Probabilidade, Machine Learning]
author: Eduardo Rubik, Lucas Santana
comments: true
---

# Restricted Boltzmann Machine - RBM

## Motivação

Em 2009, um grupo de pesquisadores conseguiu aprimorar o sistema de recomendações de filmes da Netflix em 10,05%, utilizando a Máquina de Boltzmann restrita como um dos algoritmos.

## Termodinâmica

O nome Boltzmann, presente no nome do algoritmo, vem do físico austríaco Ludwig Eduard Boltzmann (1844-1906), que estabeleceu os principais conceitos da física clássica estatística, de onde a ideia geral do modelo foi retirada.

A Distribuição de Boltzmann descreve a probabilidade de se encontrar moléculas de um gás em um determinado estado energético. Essa distribuição, a menos de constantes, é dada por:

\begin{equation}
    \frac{N_i}{N}=\frac{g_i e^{\frac{E_i}{KT}}}{Z(T)}
\end{equation}

A Máquina de Boltzmann leva esse nome, pois a distribuição de probabilidades utilizada é a Distribuição de Boltzmann, representada na figura abaixo:

![](https://i.imgur.com/AiYzKhF.png)


## Máquina de Boltzmann

Máquinas de Boltzmann são modelos probabilísticos (ou geradores) não supervisionados, baseados em energia, representados por um sistema totalmente conectado.

![](https://i.imgur.com/U0bGPhL.png)

Para cada configuração do sistema, atribui-se um valor de energia e uma probabilidade associada a essa energia. Sendo assim, tem-se como objetivo, ajustar o modelo para que pouca energia represente alta probabilidade e muita energia represente baixa probabilidade.

### Máquina de Boltzmann Restrita

Smolensky (1986), propôs alterações práticas na máquina de Boltzmann, a partir de restrições na sua topologia, forçando a inexistência de conexões intra-camada, conforme esquema abaixo.

![](https://i.imgur.com/MDtJAVB.png)

Além disso, de forma a viabilizar o processamento computacional do modelo, Hinton (2002), propôs um modelo de aprendizado eficiente e que vem sendo utilizado amplamente na academia e na indústria.


#### Estrutura de uma RBM

As RBM são formadas conjuntos de unidades visíveis e ocultas. A Figura abaixo, extraída de Borin (2016), fornece a representação gráfica de uma RBM, contendo duas unidades visíveis e três unidades ocultas.

![](https://i.imgur.com/vZ6SoFx.png)

De acordo com Borin (2016), "as linhas ligando parâmetros a unidades indicam que esses estão associados, de forma que os círculos representam as unidades e os pequenos quadrados, os parâmetros do modelo."

##### Comparação com uma RNA

As RBM são frequentemente comparados à Redes Neurais Clássicas, e de acordo com Borion (2016), a diferença está nos neurônios.

Perceba que as RNA são formadas por neurônios que possuem como saída valores reais.

![](https://i.imgur.com/56L2QmZ.png)

Já nas RBM, conforme se observa na figura abaixo, os neurônios são estocásticos.

![](https://i.imgur.com/eYD5iEO.png)

## Formalização Matemática

### Energia Global

De acordo com Borin (2016), as RBM fazem parte de uma classe de modelos probabilísticos que são baseados em energia.

A energia total é calculada por meio da equação:

\begin{equation}
    E(v,h)=-\sum_{j=1}^{n_h}b_jh_j-\sum_{i=1}^{n_v}c_iv_i-\sum_{i=1}^{n_v}\sum_{j=1}^{n_h}h_jw_{ji}v_i
\end{equation}

E a distribuição de probabilidades pela equação:

\begin{equation}
    P(v,h)=\frac{e^{-E(v,h)}}{Z}
\end{equation}

Ou seja, uma RBM fica totalmente especificada pelos seus vieses e
pesos de conexão, pelo conjunto de parâmetros **W**, **b**, **c**.

### Aprendizado

Por meio de uma abordagem não supervisionada, o modelo é treinado com o Método do Gradiente Descendente Estocástico - SGD, aplicado na seguinte Função Perda.

\begin{equation}
\mathcal{L}\left(\boldsymbol{\theta} ; \mathbf{v}^{(t)}\right)=-\ln P\left(\mathbf{v}^{(t)}\right)
\end{equation}

Sendo o gradiente definido pela equação:

\begin{equation}\frac{\partial \mathcal{L}\left(\theta; \mathbf{v}^{(t)}\right)}{\partial \boldsymbol{\theta}}=\mathbb{E}_{P(\mathbf{h} | \mathbf{v})}\left[\frac{\partial E\left(\mathbf{v}^{(t)}, \mathbf{H}\right)}{\partial \theta} | \mathbf{v}^{(t)}\right]-\mathbb{E}_{p(\mathbf{v}, \mathbf{h})}\left[\frac{\partial E(\mathbf{V}, \mathbf{H})}{\partial \boldsymbol{\theta}}\right]
\end{equation}

A partir do gradiente calculado, os parâmetros são atualizados a cada rodada de treinos, respeitando a seguinte regra:

\begin{equation}\boldsymbol{\theta} \leftarrow \boldsymbol{\theta}-\lambda\left(\frac{\partial \mathcal{F}\left(\mathbf{v}^{(t)}\right)}{\partial \boldsymbol{\theta}}-\frac{\partial \mathcal{F}(\tilde{\mathbf{v}})}{\partial \boldsymbol{\theta}}\right)\end{equation}

Para acompanhar a evolução do treinamento, geralmente se utiliza
uma medida de erro de reconstrução:

\begin{equation}\varepsilon_{r}=\left\|\tilde{\mathbf{v}}-\mathbf{v}^{(t)}\right\|^{2}\end{equation}

## Aplicação

Uma aplicação interessante da RBM, utilizando TensorFlow, pode ser encontrada [aqui](https://www.linkedin.com/pulse/criando-um-sistema-de-recomenda%C3%A7%C3%A3o-com-tensorflow-e-rbm-ricardo-ramos/).

## Referências

Borin, R. G. (2016). Detecção de atividade vocal empregando máquinas de Boltzmann restritas (Doctoral dissertation, Universidade de São Paulo).

Smolensky, P. (1986). Information processing in dynamical systems: Foundations of harmony theory. Colorado Univ at Boulder Dept of Computer Science.

Hinton, G. E. (2002). Training products of experts by minimizing contrastive divergence. Neural computation, 14(8), 1771-1800.
