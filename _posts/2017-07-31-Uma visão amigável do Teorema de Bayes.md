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

# Teorema de Bayes

O teorema de bayes estabelece a seguinte relação entre dois eventos A e B, com probabilidades, respectivamente, P(A) e P(B):

$$P(A|B) = \frac{P(A \cap B) }{P(B)}$$



Utilizando o conceito de permutabilidade definido no Teorema de Finetti, o termo $P(A \cap B)$ é igual a $P( B \cap A )$ e, dessa forma, pode ser escrita como:

$$P( B  \cap A ) = P( B | A)P(A)$$


Por fim, tem-se a seguinte relação:
$$P(A|B) = \frac{P( B | A) }{P(B)}P(A)$$

Nesse caso, o probabilidade $P(A)$ é denominada probabilidade a *priori*, isto é, a informação sobre o evento $A$ antes que se soubesse algo sobre o evento $B$. Mais adiante, quando se tenha conhecimento sobre $B$, a probabilidade relacionada ao evento $A$ deve ser atualizada pela probabilidade do evento $B$. A probabilidade $P(A|B)$ é agora denominada probabilidade a *posteriori*. Sendo a razão  $\frac{P(B|A) }{P(B)}$ o fator de atualização das informações sobre o evento $A$.

Para compreender com mais detalhes o Teorema de Bayes é necessário entender a **regra da probabilidade total (RPT)**, que expressa a probabilidade total de um resultado por meio de vários eventos disjuntos. 

Inicialmente, considere o problema em encontrar o valor para a probabilidade do evento $A$, vide (a). Considere agora que seja possível particionar o espaço $\Omega$ em partes $B_i$ sem intersecções entre si, vide (b). A probabilidade $A$ pode ser determina, pela intersecção entre o evento $A$ e cada partição $B_i$, vide (c) e (d).


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


Você percebeu que o **valor devolvido** ($V_{devolvido}$) do seu "investimento" ao final do período de empréstimo está sujeito às ``variações do mercado", que, nesse caso, estão relacionadas a um evento \textbf{incerto} do não pagamento da dívida. Com isso, você define o **valor esperado** ($V_{esperado}$) como o valor recebido ao final do período considerando tal incerteza.

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

Dessa forma, após saber que ele não pagou o boleto do banco, a probabilidade de ser bom pagador a *posteriori* reduz em quase a metade da *priori*. Dessa forma, a nova taxa de juros é $\frac{1}{0.4736} - 1 = 111.111\%$, fazendo com que o valor cobrado seja de R\$$2.111,11$.

Esse é uma versão introdutória do Teorema de Bayes e serve para motivação para posts futuros que exigirão mais pré requisitos em teoria de probabilidades.

