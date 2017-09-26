---
layout: post
title: Classificadores Ensemble: Bagging e Boosting
lang: pt
header-img: img/0026.png
date: 2017-09-27 23:59:07
tags: [statistics,variance]
author: Maisa Aniceto
comments: true
---


\section{Classificadores \textit{Ensemble}}

%Ao contrario das técnicas estatísticas tradicionais, os métodos de aprendizado de máquina não exigem o conhecimento das relações entre as variáveis de entrada e de saída dos modelos. 

O problema de classificação no contexto da concessão de crédito é importante, envolve grandes quantias de dinheiro, e deve ser cada vez mais estudado. Atualmente, toda  melhoria sobre os métodos já utilizados gera uma enorme economia para as instituições envolvidas.

Estudos recentes tem mostrado que métodos de classificadores \textit{ensemble} possuem performance melhor do que técnicas de inteligência artificial sozinhas.

Mas você sabe o que é um classificador \textit{ensemble}?
Um classificador \textit{ensemble} (também chamado de comitê de \textit{learners}, mistura de especialistas e sistema de classificadores múltiplo), consiste em um conjunto de classificadores treinados individualmente, classificadores de base, cujas decisões são de alguma forma combinadas \cite{Marques2012b}.

Nesse post apresentarei dois algoritmos \textit{ensemble} tradicionais o \textit{Bagging} e o \textit{Boosting}, mais especificamente, o \textit{AdaBoost}. 

%Nesse artigo, \cite{Breiman1996} mostrou que a variância do preditor combinado é menor ou igual a variância de qualquer um dos preditores individuais envolvidos na combinação.



\subsection{Bagging}
O \textit{Bagging (Bootstrap Aggregating)}, um método  proposto por \cite{Breiman1996}, gera um conjunto de dados por amostragem \textit{bootstrap} (Já falamos dela aqui: https://lamfo-unb.github.io/2017/06/28/Bootstrap/) dos dados originais. O conjunto de dados gera um conjunto de modelos utilizando um algoritmo de aprendizagem simples que são combinados por votação simples para classificação.

Bagging é particularmente atraente quando a informação disponível é de tamanho limitado. Nessa técnica, classificadores são treinados de forma independente por diferentes conjuntos de treinamento através do método de inicialização. 

Para construí-los é necessário montar $k$ conjuntos de treinamento idênticos, replicar esses dados de treinamento de forma aleatória para construir $k$ redes independentes por re-amostragem com reposição. Em seguida, deve-se agregar as $k$ redes através de um método de combinação apropriada, tal como a maioria de votos \cite{Breiman1996}.

Para garantir que há amostras de treinamento suficientes em cada subconjunto, grandes porções de amostras (75-100\%) são colocadas em cada subconjunto. Com isso, os subconjuntos individuais de formação se sobrepõem de forma significativa, com muitos casos fazendo parte da maioria dos subconjuntos e podendo até mesmo aparecer várias vezes num mesmo subconjunto. 

A fim de assegurar a diversidade de situações, um \textit{learner} de base relativamente instável é usado para que limites de decisão diferentes possam ser obtidos, considerando-se pequenas perturbações em diferentes amostras de treinamento \cite{Wang2011}.

O resumo do pseudo código do Bagging, proposto por Breiman, é o seguinte:


\begin{center}
\fbox{
\begin{minipage}{\textwidth}
          
					Entrada: Dataset $D=\{(z_1,y_1),(x_2y_2),...,(x_m,y_m)\}$;
					
					Número de rounds de aprendizagem $T$.
					
					Processo: \\
					Para $t = 1,2,..., T $: 
					
					a. Forma conjuntos bootstrap de dados $S_t$ selecionando $m$ exemplos aleatórios do conjunto de treinamento com substituição
					
					b. Deixa $h_t$ ser o resultado da base de treinamento do algoritmo baseado em $S_t$
								
										
					fim.			
			
		Saída:
		
  Classificador combinado:
	$$
  H(x)=maioria(h_{1}(x),...,h_{T}(x)) 
  $$
			
    
\end{minipage}
}
\end{center}


\subsection{Boosting e AdaBoost}

No Boosting, de forma semelhante ao Bagging, cada classificador é treinado usando um conjunto de treinamento diferente. A abordagem por Boosting original foi proposta por \cite{Schapire1990}. 

Segundo \cite{Lantz2013}, a principal diferença em relação ao Bagging é que os conjuntos de dados re-amostrados em Boosting são construídos especificamente para gerar aprendizados complementares e a importância do voto é ponderado com base no desempenho de cada modelo, em vez de atribuição de mesmo peso para todos os votos. 

Essencialmente, esse procedimento permite aumentar o desempenho de um limiar arbitrário simplesmente adicionando \textit{learners} mais fracos. 

Dada a utilidade desse achado, Boosting é considerado uma das descobertas mais significativas em aprendizado de máquina \cite{Lantz2013}.

Já o \textbf{AdaBoost} é uma combinação das ideias de Bagging e Boosting  e não exige um grande conjunto de treinamento como o Boosting. Inicialmente, cada exemplo de formação de um determinado conjunto de treinamento tem o mesmo peso  \cite{Tsai2014}.

Para treinar o $k-esimo$ classificador como um "modelo de aprendizagem fraca", $n$ conjuntos de amostras de treinamento $(n < m)$ entre $S$ são usadas para treinar o $k-esimo$ classificador.
Em seguida, o classificador treinado é avaliado por $S$ para identificar os exemplos de treinamento que não foram classificados corretamente. A rede $k + 1$ é então treinada por um conjunto de treinamento modificado que reforça a importância desses exemplos classificados incorretamente \cite{Tsai2014}.

Este procedimento de amostragem será repetido até que $k$ amostras de treinamento sejam construídas para a construção da $k-esima$ rede. Portanto, a decisão final baseia-se na votação ponderada dos classificadores individuais. %\cite{Tsai2014}.

O pseudo código do Adaboost, segundo \cite{Wang2011}, é o seguinte:


 \begin{center}
\fbox{
\begin{minipage}{\textwidth}

                
					Entrada: Dataset $D=\{(z_1,y_1),(x_2,y_2),...,(x_m,y_m)\}$;
					
					Algoritmo de base de aprendizagem $L$; \\
					Número de rounds de aprendizagem $T$.
					
					Processo: \\
					$$ D_1(i) = 1/m.  $$ \% Inicializa a distribuição de pesos
					
					Para $t = 1,2,..., T $: 
					
					$$ h_1= L(D,D_t); $$
					\% Treina a base de aprendizado $h_t$ para D usando a distribuição $D_t$ 
					
					$$ \epsilon_i= Pr_{i \cong D_i} [h_t(x_i \neq y_i)]; $$ 
					\% Mede o erro de $h_t$ \\
						
									
			$$
			\alpha_t = \frac{1}{2}\ln \frac{1 - \epsilon_t}{\epsilon_t} 	$$
			\% Determina o peso de $h_t$
		
											
			$$ 
			D_{t+1}(i)=\frac{D_{t}(i)}{Z_t}  \left\{\begin{array}{rll}
																			exp({-\alpha_t}) & \hbox{se} & h_{t}(x_{i})= y_i \\
																		exp({\alpha_t}) & \hbox{se} & h_{t}(x_{i}) \neq y_i
																			\end{array}\right.$$

		\% Atualiza a distribuição
			
			
					
			$$
   =\frac{D_{t}(i)exp(-\alpha_{t}y_{i}h_{t}(x_{i}))}{Z_{t}}
      $$
		\% fator de normalização que permite $D_{t+1}$ ser uma distribuição
						
			fim.			
			
		Saída:
  $$
  H(x)=sign\left( \sum_{t=1}^{T}\alpha_{t}h_{t}(x)\right) 
  $$
		
				
   
\end{minipage}
}
\end{center}


%No algoritmo AdaBoost são construídas várias Decision Trees e então a melhor classe para cada exemplo é escolhida. 


O pacote ipred do \textit{software} R, oferece uma aplicação clássica Bagging em Decision Tree. Para treinar o modelo é utilizada a função bagging(). %\cite{Lantz2013}.
Já o Adaboost pode ser calculado utilizando o pacote C50.
Em próximo post apresentaremos exemplos práticos de como utilizar esses classificadores \textit{ensemble}. 
