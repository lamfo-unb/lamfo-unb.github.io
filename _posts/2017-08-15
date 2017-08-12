---
layout: post
title: Teste de Eventos
lang: pt
header-img: 
date: 2017-08-15 10:00:00
tags: [econometria, finanças]
author: Ana Julia Akaishi Padula
comments: true
---
# Teste de Eventos

Olá pessoal! Essa semana o LAMFO traz para vocês a metodologia conhecida como Teste de Eventos, com uma breve introdução, referencial teórico e um exercício prático utilizando R.

Durante o dia, não é incomum ouvirmos ou lermos alguma notícia sobre acontecimentos ou mudanças no país e no mundo. A todo momento recebemos atualizações, novas informações e opiniões a respeito dos mais diversos temas que refletem em nosso comportamento e até planejamento para o futuro, mas e o mercado? Será que ele também é afetado pela repercursão desses eventos?

Para responder essa pergunta, foi criado o Teste de Evento (*Event Study*).

## Metodologia

Os autores Campbell, Lo e Mackinley (1997) foram os primeiros a definirem e escreverem sobre esta metodologia. A idéia básica é observar se o valor de uma empresa é alterado com o aparecimento de um evento, ou seja, se o "retorno normal" esperado sofre alguma "anormalidade". O desenvolvimento do Teste de Eventos tem como base a Teoria dos Mercados Eficientes de Fama (1970), no qual acredita-se que o mercado absorve as informações públicas disponíveis e realiza o ajuste do preço dos ativos.

Apresentaremos quatro passos simples para começar nosso Teste de Eventos!

### Passo 1: Difinição do Evento

*Mas de que tipo de evento estamos falando?*

Podemos estudar qualquer tipo de evento, contanto que tenhamos dados o suficiente e que o evento seja publicamente conhecido. Temos por exemplo eventos como:
1. Distribuição de dividendos; 
2. Fusão e aquisições de outras empresas; 
3. Recompra de ações;
4. Novas leis e regulamentações;
5. Privatização ou estatização;
6. Escândalos políticos. 

Uma vez escolhido o evento, é preciso definir uma linha do tempo e as janelas de análise. 

![](https://i.imgur.com/QJzEnPH.png)


**Janela de estimação:** representa o período em que os retornos normais do ativo serão padronizados, de $$T_0$$ a $$T_1$$. Dessa forma, teremos uma base de comparação de retornos que não foram "contaminados" pelo aparecimento evento.

**Janela do Evento:** intervalo de tempo em que o evento em questão apareceu. A data que o evento ocorreu é determinada como momento zero ($$0$$) e a partir desse marco é estabelecido um "intervalo de segurança" para verificar se houve vazamento de informações privilegiadas antes do acontecimento, $$T_1$$ a $$0$$, e para englobar o período de absorção do evento pelo mercado, de $$0$$ a $$T_2$$.

Um detalhe importante é que não existe um número fixo para esta janela. Podemos por exemplo ter três meses antes e depois do evento ou até mesmo quinze dias antes e depois para compor a janela, uma saída seria realizar uma modelagem e avaliar o erro estatístico.

**Janela Pós-Evento:** momento após o acontecimento do evento e eventuais movimentações do mercado. É possível observar o impacto a longo prazo do acontecimento.

### Passo 2: Empresas

Com as datas determinadas, agora precisamos selecionar quais empresas farão parte do estudo e porque. Normalmente o primeiro filtro é se a empresa que se deseja estudar possui ações na bolsa de valores, uma vez que utilizamos o retorno para nossos calculos. 

Uma ideia para o segundo filtro seria o setor de empresas, por exemplo se usassemos o Teste de Eventos para verificar o impacto de novas leis ambientais, seria interessante selecionar empresas do setor de mineração e petróleo.

Agora é preciso coletar os dados dos retornos da empresa ou empresas selecionadas. É importante ressaltar que a unidade temporal dos dados deve ser a mesma, por exemplo se o evento estudado for registrado em um dia, as oberservações de retorno também devem ser diárias. 

### Passo 3: Estabelecendo Retornos Normais e Anormais

Como estabelecido na linha do tempo, primeiro estimamos o retorno normal do ativo para depois analisarmos a anormalidade.

Uma base de dado com os retornos da empresa escolhida é o suficiente para iniciar a composição da base de retornos normais. O importante é que a unidade temporal seja a mesma e que as observações se iniciem em $$T_0$$ e terminem em, pelo menos, em $$T_2$$.

Para verificar se existem ou não retornos anormais, usaremos a seguinte equação base:

$$Ra_{i,t}=R_{i,t} - \mathbb E[R_i|X_t ]$$ 

Sendo $$Ra_{i,t}$$ o retorno anormal, uma vez que será a diferença do retorno real em t ($$R_{i,t}$$) e do retorno normal esperado em determinado período ($$E[R_i|X_t ]$$).

Como já teríamos a base com os retornos da empresa de $$T_0$$ a $$T_2$$, precisamos escolher qual o melhor modelo para se estabelecer o retorno padrão:
1. Retorno Ajustado a Média
2. Retorno Ajustado ao Mercado
3. Modelo de Mercado
4. Modelos Econômicos

:::info
Retorno Ajustado a Média
:::

O retorno normal esperado aqui será definido pela média simples dos retornos reais da janela de estimação ($$T_0$$ a $$T_1$$).

$$Ra_{i,t}=R_{i,t} - \overline R_i$$ 

Uma crítica a esse modelo é que se assume que os retornos terão retornos médios constantes ao longo do tempo. Dependendo da unidade temporal escolhida, este pressuposto por si só já apresenta uma grande margem de erro.

:::info
Retorno Ajustado ao Mercado
:::

Neste caso, o retorno normal será definido como o retorno da cateira de mercado ($$Rm$$). Neste método será necessário obter uma base de dado com os retornos na mesma unidade temporal do ativo referência do mercado (*benchmark*).

$$Ra_{i,t}=R_{i,t} -  Rm_i$$ 

:::info
Modelo de Mercado
:::
A abordagem do Modelo de Mercado possui sua base na estatística, aprimorando as estimativas realizadas anteriormente pelos demais modelos.

Diferente do Modelo de Retorno Ajustado ao Mercado, o Modelo de Mercado não utiliza o retorno de uma carteira referência, mas sim índices como S&P500 para análises norte-americanas ou IBOVESPA para ações nacionais, sendo definido por:

$$Ra_{i,t}=R_{i,t} - \widehat \alpha_i - \widehat \beta_iR_{m,t}$$ 

No qual o retorno anormal ($$Ra_{i,t}$$) é definido pelo retorno real do ativo em um período ***t*** menos os os parâmetros alpha ($$\alpha$$) e beta ($$\beta$$) estimados por uma regressão linear da janela de estimação para o índice de mercado.

Por utilizar regressão linear, o ganho e previsão do modelo depende do R$$^2$$. Quanto maior for este indicador, menor será a variância do do retorno anormal, garantindo uma maior estabilidade ao modelo.

:::info
Modelos Econômicos
:::

Com base nas práticas do mercado, se desejamos estimar um valor para o retorno futuro de um ativo existem duas metodologias utilizadas (1) *Capital Asset Pricing Model* (CAPM) e (2) *Arbitrage Pricing Theory* (APT). Ambos podem ser utilizados para estimar o retorno normal, porém é preciso ficar atento para os requisitos de cada um assim como o período da janela de estimação.


1. *Capital Asset Princing Model* (CAPM)

$$Ra_{i,t} = R_i - Rf_i - \beta (Rm_i - Rf_i)$$

No qual o retorno anormal é identificado pela diferença do retorno real com o retorno estipulado considerando a sensibilidade do ativo e elementos como retorno de mercado ($$Rm$$) e de ativos livre de risco ($$Rf$$).

2. *Arbitrage Pricing Theory* (APT)

$$Ra_{i,t} = R_i - R_f + \beta_1 R_1 + \beta_2 R_2 + \beta_n R_n$$

Neste modelo temos um prêmio pelo risco para fatores diferentes, considerando a sensibilidade do ativo ao fator específico. Para adquirir o Beta ($$\beta_n$$) será preciso realizar uma regressão linear para cada fator e o ativo.

### Passo 4: Mensuração e Análise dos Retornos Anormais

Com o modelo definido, agora devemos agora calcular o comportamento dos retornos. Normalmente, usamos duas métricas para os retornos anormais, o retorno anormal médio da amostra e o retorno anormal acumulado.

Para aceitarmos os resultados, é preciso que o P-Valor do modelo esteja dentro da margem de aceitação ou que o $$R^2$$ seja o maior possível.

## Aplicando o modelo

A aplicação do Teste de Eventos será realizada utilizando o R, com adicionais de alguns pacotes como:
* quantmod : coletar os dados de retorno das empresas escolhidas.

A fim de demonstrar o modelo padrão, usaremos o Modelo de Mercado para mensurar os retornor anormais para um evento econômico e um evento político. 

*Existe disponível no R um pacote para realizar o Teste de Eventos, o EventStudy. Para fins didáticos, escreveremos o código passo a passo para entender o que é feito.*

::: info
Estouro da Bolha Imobiliaria nos EUA (2008)
:::
