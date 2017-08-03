---
layout: post
title: Uma visão amigável do Teorema de Bayes
lang: pt
header-img: img/bayes/juskteez-vu-1041.jpg
date: 2017-07-31 10:00:00
tags: [estatística,finanças,probabilidade]
author: Igor Nascimento
comments: true
---
<link href='https://fonts.googleapis.com/css?family=Rock Salt' rel='stylesheet'>
<link href='https://fonts.googleapis.com/css?family=Sofia' rel='stylesheet'>


# Eventos, Probabilidade e Condicionalidade


No livro *The History of Probability* o autor Todhunter descreve cronologicamente sequenciada registros de alguns notáveis personagens da história com elementares considerações sobre a Teoria da Probabilidade. No entanto, os primórdios da probabilida está relatado nas correspondências entre Pierre De Fermat e Blaise Pascal sobre um problema de jogos em lançamento de dados, citados em *Games, Gods \& Gambling*.

Probabilidade é a área da matemática que mensura o que não é determinístico. O lançamento de um dado e uma moeda e a observação da face virada para cima são considerados **experimentos de probabilidade**, pois os resultados estão sujeitos ao acaso.


Considere as seguintes abordagens para o cálculo de probabilidades:

- **Clássica**: "Suponha que dentre $n$ resultados possíveis $m$ sejam favoráveis. A probabilidade de um evento favorável é $p = \frac{m}{n}$."  Essa abordagem é o resultado das correspondências entre Fermat e Pascal.

- **Frequentista**: "Foram realizados $n$ experimentos dos quais $m$ eram favoráveis. A frequência relativa da ocorrência de um evento favorável é $\frac{m}{n}$." Jakob Bernoulli, em seu livro *Ars Conjectandi*, provou a converência entre a frequência relativa de ocorrência e a probabilidade $p$ clássica a medida que aumenta o número de experimentos $n$.

Dito isso, vamos defir como $P(A)$ a probabilidade de ocorrer o evento $A$ em um determinado conjunto de valores possívels $\Omega$. Antes de tudo vamos aquecer o cérebro compreendendo os importantes conceitos de **independência** e **permutabilidade** para o estudo de probabilidade.

## Independência

Seja uma caixa contendo **3 bolas brancas** e **2 pretas**. Considere o experimento da retirada aleatória e observância da cor da bolas, com representação $X_1$ de eventos **Bernoulli**:

- $X_1 = 0$ caso a bola retirada seja branca

- $X_1 = 1$ caso a bola retirada seja preta

Pela abordagem clássica, a probabilidade da bola ser branca é $P(X_1 = 1) = \frac{3}{5}$ e de ser preta $P(X_1 = 0) = \frac{2}{5}$.

Após a primeira retirada, e sem reposição, você retira mais uma bola. Qual a probabilidade da segunda bola ser branca? 

A resposta mais intuitiva é **DEPENDE**. De fato, a probabilidade desse evento no segundo lançamento está **condicionada** ao resultado do primeiro lançamento. Perceba, caso a primeira seja branca, restam mais $1$ branca e as demais $3$ pretas, e, com isso, a probabilidade de da segunda retirada branca é $\frac{1}{4}$. Caso a primeira tenha sido preta, seria $\frac{2}{4}$.

A probabilidade do evento $B$ condicionada ao evento $A$ é representada por $P(B|A)$. Nesse caso, temos que:

- $P(X_2 = 1|X_1 = 1) = \frac{1}{4}$

- $P(X_2 = 1|X_1 = 0) = \frac{2}{4}$

Dessa forma, dizemos que o evento $X_2$ é **dependente** do evento $X_1$. 

A probabilidade conjunta dos eventos $A$ e $B$ é expressa pela regra do produto:
$$P( B \cap A)   = P(B|A) P(A)$$
que também pode ser expressa por $P( B \cap A) = P( B , A)$.

Considere agora um outro experimento, o lançamento de uma **moeda honesta** e a observância da face virada para cima. Considere a representação $X_1$ de eventos **Bernoulli**:

- $0$ caso o resultado do lançamento seja coroa

- $1$ caso o resultado do lançamento seja cara

Por ser justa, a probabilidade do resultado cara é $P(X_1 = 1) = \frac{1}{2}$ e o de coroa é $P(X_1 = 0) = \frac{1}{2}$.

Em seguida, a moeda é lançada novamente e o resultado representado por $X_2$. Perceba que nesse caso, a probabilidade da ocorrência de cara no segundo lançamento também é igual a $P(X_2 = 1) = \frac{1}{2}$ por não ser afetado pelo resultado do primeiro lançamento.

Dizemos que $X_2$ é **independente** de $X_1$, e nesse caso, temos que a regra do produto é:

\begin{eqnarray}
P(X_2  , X_1 ) & = P(X_2|X_1) P(X_1) \nonumber \\
 & = P(X_2)P(X_1 ) \nonumber
\end{eqnarray}


## Permutabilidade


A permutabilidade é a propriedade de alterar a ordem de uma sequência de eventos sem que a probabilidade conjunta seja alterada. É importante notar que a independência dos eventes garante a permutabilidade, mas a permutabilidade não garante a independência.

O teorema de De Finetti utiliza a permutabilidade e a independência.


Dessa forma, a sequência de resultados **cara, coroa, cara, cara, coroa** pode ser representada pela sequência numérica $(1,0,0,1,0)$.


# Teorema de Bayes

O teorema de bayes estabelece a seguinte relação entre dois eventos A e B, com probabilidades, respectivamente, P(A) e P(B):

$$P(A|B) = \frac{P( B | A) }{P(B)}P(A)$$

Utilizando o conceito de permutabilidade definido no Teorema de Finetti, o termo $P(A \cap B)$ é igual a $P( B \cap A )$ e, dessa forma, pode ser escrita como:

$$P( B  \cap A ) = P( B | A)P(A)$$

#################################################
Acho que o teorema de De Finetti não é tão "amigável" para quem não tem já alguma bagagem estatística... tente ver um jeito mais noob de explicar que a interseção é comutativa, usando diagramas de conjuntos, talvez
#################################################

Por fim, tem-se a seguinte relação:
$$P(A|B) = \frac{P( B | A) }{P(B)}P(A)$$

Nesse caso, o probabilidade $P(A)$ é denominada probabilidade a *priori*, isto é, a informação sobre o evento $A$ antes que se soubesse algo sobre o evento $B$. Mais adiante, quando se tenha conhecimento sobre $B$, a probabilidade relacionada ao evento $A$ deve ser atualizada pela probabilidade do evento $B$. A probabilidade $P(A|B)$ é agora denominada probabilidade a *posteriori*. Sendo a razão  $\frac{P(B|A) }{P(B)}$ o fator de atualização das informações sobre o evento $A$.

Para compreender com mais detalhes o Teorema de Bayes é necessário entender a **regra da probabilidade total (RPT)**, que expressa a probabilidade total de um resultado por meio de vários eventos disjuntos. 

Inicialmente, considere o problema em encontrar o valor para a probabilidade do evento $A$, vide (a). Considere agora que seja possível particionar o espaço $\Omega$ em partes $B_i$ sem intersecções entre si, vide (b). A probabilidade $A$ pode ser determinada pela intersecção entre o evento $A$ e cada partição $B_i$, vide (c) e (d).


Nos espaços amostrais $\Omega$ formados pela união de partes $B_i$ disjuntas (mutuamente exclusivas) a probabilidade de qualquer evento de $\Omega$ é:

\begin{eqnarray}
P(A) = & P(A \cap B_1) + P(A \cap B_2) + ... + P(A \cap B_N) \nonumber \\
P(A) = & P(A | B_1)P(B_1)+P(A | B_2)P(B_2) + ... + P(A | B_N)P(B_N) 
\end{eqnarray}

Dessa forma, a probabilidade do evento $A$ pode ser representado por:

\begin{equation}
P(A) = \sum_{i=1}^N P(A | B_i)P(B_i)
\end{equation}


\textbf{Qual a importância desse resultado?} Esssa pergunta pergunta pode ser respondida com o seguinte problema...


Um amigo muito próximo lhe pediu $R\$ 1.000,00$ emprestado ($V_{emprestado}$) para solução financeira de uma emergência. Você é um investidor nato e não suporta a ideia de perder seu patrimônio conquistado. Você decide ajudar seu amigo e cobrará juros $T_{juros}$ sobre o montante inicial, mas está preocupado com o **risco** do não pagamento do empréstimo$V_{devolvido}$:

\begin{eqnarray}
V_{devolvido} = V_{emprestado} \times (1 + T_{juros}) \nonumber 
\end{eqnarray}


Você percebeu que o **valor devolvido** ($V_{devolvido}$) do seu "investimento" ao final do período de empréstimo está sujeito às "variações do mercado", que, nesse caso, estão relacionadas a um evento \textbf{incerto} do não pagamento da dívida. Com isso, você define o **valor esperado** ($V_{esperado}$) como o valor recebido ao final do período considerando tal incerteza.

Considere $A$ o evento indicativo do pagamento do seu amigo, então, o valor esperado  ($V_{esperado}$) ao final do período de empréstimo é a média ponderada entre as possibilidades de valores devolvidos, $V_{devolvido}$ e $0$, e suas respectivas probabilidades, $P(A)$ e $1 - P(A)$. Dessa forma, o $V_{esperado}$ do investimento é:

\begin{eqnarray}
V_{esperado}  = &  V_{devolvido} \times P(A)                    & + 0 \times (1 - P(A)) \nonumber \\
 = &  [V_{emprestado} \times (1 + T_{juros})] \times P(A)  & + 0 \times (1 - P(A)) \nonumber \\
 = &  [V_{emprestado} \times (1 + T_{juros})] \times P(A)   
\end{eqnarray}

 Você decide que o valor dos juros  será determinado de maneira que o valor esperado seja igual ao investimento inicial, isto é, $V_{esperado}=V_{emprestado}$. Dessa forma, a taxa de juros utilizada será:

\begin{eqnarray}
 T_{juros} = & \frac{V_{esperado}}{V_{emprestado} \times P(A)} - 1\nonumber \\
 T_{juros} = & \frac{1000}{1000 \times P(A)} - 1 = \frac{1}{P(A)} - 1
 \label{eq:juros}
\end{eqnarray}

Você utilizará uma *proxy* o evento $A$ baseado no cadastro nacional de bom ou mal pagador. Infelizmente, você não tem acesso à esse cadastro. No entanto, você sabe que, assim como você, seu amigo possui conta no banco ABC, que regularmente publica informações agregadas sobre as operações com os clientes. 

Tal banco realizou um levantamento, e constatou que $1$ em cada $10$ clientes possuem registo ativo em um cadastro nacional de mal pagadores.  Dessa forma, a probabilidade do pagamento do seu amigo se concretizar é de $P(A)=\frac{9}{10} = 90\%$.

Dito isso, utilizando a taxa de juros que você deve adotar é:
\begin{eqnarray}
 T_{juros} = & \frac{1}{0.9} - 1 = 11.111\% \nonumber
\end{eqnarray}

Dessa forma, a *priori*, seu amigo deveria lhe pagar R\$ $1.111,11$ ao final do período para garantir que, em média e desconsiderando inflação, seu investimento inicial seja recuperado.

Nos informativos do banco também consta que $2$ em cada $4$ mal pagadores atrasam o pagamento do boleto, enquanto dentre os bons pagadores, $1$ a cada $20$ atrasam suas obrigações. 

Durante a conversa, seu amigo te informou que possui boletos atrasados nesse banco. Baseado nessa **nova informação**, qual a probabilidade do seu amigo ser mal pagador dado que atrasou o pagamento? Qual a nova taxa de juros que você deve adotar para proteger seu "investimento"?

O Teorema de Bayes responde diretamente essa pergunta. Antes disso, vamos modelar os eventos e identificar suas probabilidades. Considere o evento $A$ o cliente ser um bom pagador e o evento $B$ o atraso do pagamento de um boleto da obrigação financeira nesse banco específico. 

- Ser bom pagador: evento $A$. Sendo $P(A)=\frac{9}{10}$.
- Ser mal pagador: evento $A^{c}$. Sendo $P(A^{c})=1-P(A)=\frac{1}{10}$.
- Atraso no pagamento: evento $B$. Sendo $P(B)=$ não informado.
- Atraso no pagamento dos bons pagadores: evento $B|A$. Sendo $P(B|A)=\frac{1}{20}$.
- Atraso no pagamento dos mal pagadores: evento $B|A^{c}$. Sendo $P(B|A^{c})=\frac{2}{4}$.
- Probabilidade do seu amigo ser bom pagadores caso tenha atrasado o pagamento. $P(A|B)=$?.

Utilizando o Teorema de Bayes e a  regra da probabilidade total em $P(B)$, tem-se que:
\begin{eqnarray}
P(A|B) = &  \frac{P( B | A) }{P(B)}P(A) \nonumber \\
P(A|B) = & \left[ \frac{P( B | A) }{P(B | A)P(A)+P(B | A^{c})P(A^{c})}\right]P(A) \nonumber \\
P(A|B) = &  \left[ \frac{\frac{1}{20} }{\frac{1}{20}\frac{9}{10}+\frac{2}{4}\frac{1}{10}}\right]\frac{9}{10} \nonumber \\
P(A|B) = &  \left[ \frac{38}{20}\right]\frac{9}{10}  \nonumber \\
P(A|B) = &   47.36\%  \nonumber
\end{eqnarray}

Dessa forma, após saber que ele não pagou o boleto do banco, a probabilidade de ser bom pagador a *posteriori* reduz em quase a metade da *priori*. Dessa forma, a nova taxa de juros é $\frac{1}{0.4736} - 1 = 111.111\%$, fazendo com que o valor cobrado seja de R\$ $2.111,11$.

Esse é uma versão introdutória do Teorema de Bayes e serve para motivação para posts futuros que exigirão mais pré requisitos em teoria de probabilidades.

#################################################
Apresente algumas aplicações do teorema em situações mais complicadas, aonde mais pode se aplicar essa ideia
#################################################
