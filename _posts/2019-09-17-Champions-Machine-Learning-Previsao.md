layout: post
title: Prevendo a Champions League através de Machine Learning
lang: pt
header-img: 
date: 2019-09-17 23:38:07
tags: [statistics,variance]
author: Alixandro Werneck Leite, Carlos Aragon, Matheus Kempa Severino
comments: true


# Prevendo a Champions League através de Machine Learning.md

![](https://www.ol.fr/-/media/project/olg/olweb/articles/first-team/2018/12/16/une_1200x721championsleague2.jpg?h=721&la=en&w=1200&rev=d82760722f3c4f49b5f68d42fc5f705d)


**Table of Contents**

[TOCM]

[TOC]
# Champions League e FIFA
## Introdução
UEFA Champions League, quem nunca ouviu falar dessa competição? Qual jogador nunca sonhou em participar dela ou até mesmo conquistá-la? 

Estamos colocando em pauta a maior competição de clubes a nível mundial. Uma competição que paga até € 2 bilhões de euros aos times participantes e orienta todas as contratações da pré temporada, afinal, qual time que a disputa sem pretenção de ganhá-la?

Quanto a nós, fãs do futebol, somente apreciamos e torcemos, até mesmo quando tudo parece perdido, quando os comentaristas já decretaram a vitória para o time adversário e parece não haver tempo, ainda seguimos com a esperança. Esperança essa, capaz de alterar o estado de sobriedade do ser humano a ponto de levá-lo à bolões ou desafios, criando nele a certeza da sua capacidade de prever o imprevisível, ou a confiança de que seus instintos e conhecimentos sobre o futebol são inigualáveis. Talvez por essa razão a **Rainha do Reino Unido**, Elizabeth II, desferiu ao futebol a seguinte frase:

![](https://ogimg.infoglobo.com.br/in/23764499-e07-126/FT1086A/652/x83265122_TOPSHOTBritains-Queen-Elizabeth-II-arrives-by-carriage-on-day-two-of-the-Royal-Ascot-ho-1.jpg.pagespeed.ic.d9H8Fa4kk_.jpg)

> "O futebol é um **negócio difícil**!”- Elizabeth II 

Não sabemos ao certo o que ela almejava transmistir com essa frase, mas ela estava certa, o futebol é sim difícil. Mas talvez mais difícil ainda seja prevê-lo. 

Muitas acontecimentos hoje em dia são previsíveis, como por exemplo a desfecho de uma novela ou série, porém o futebol **dispensa** tais **características**. Aleatoriedades e reviravoltas estão na essência do futebol e por isso nossa tarefa aqui **não** é uma tarefa qualquer.

## FIFA e Histórico

O futebol é um celeiro de variáveis, prevê-lo então será um grande desafio. Ninguém que se atreva a fazer tal tarefa ignorará os fatos ou realidade dos times que estão inseridos e por isso é importante que tenhamos o máximo de variáveis conosco.

Sendo assim nesse post utilizaremos  a nosso favor todos os dados jogo do FIFA afim de levantarmos um maior número de variáveis que possam futuramente orientar da melhor maneira nossas máquinas.

![](https://supertabthemes.com/wp-content/uploads/2019/08/1-2-1024x576.jpg)

Será necessário também o retrospecto real de cada time envolvido na Champions League. Só assim teremos dados suficientes para enfim treinarmos nossa máquina, validarmos e enfim colocá-la em funcionamento.

O intuito aqui então fica claro, uma combinação de dados reais e dados fictícios, mas inspirados na realidade, serão capazes de orientar a nossa máquina a fim de nos levarmos a uma previsão mais acertiva? Será que dessa forma conseguiremos prever aleatoriedades, ou superarmos analistas esportivos?

# Processo de Modelagem de Dados


##Seleção de Variáveis

Para essa missão elencamos as variáveis indispensáveis para o melhor resultado final. Por isso pensamos em diversos **problemas** que poderiam interferir no ramo de qualquer confronto. Alguns que valem a pena serem melhor explicados.

#### Variável - Momento

Todos que acompanham futebol, sabem da existência da variável **fase** ou melhor, do **momento** em que o time vive. Na Champions do ano de 2019 evidenciamos um pouco disso, quando o Ajax eliminou os favoritos Real Madrid e Juventus, pois vivia uma fase excepcional.

Pensando nisso, nosso modelo conterá 3 colunas com o resultado dos três últimos jogos. 
Para cada coluna os seguintes resultados:

	1 = Vitória 
	2 = Derrota
	3 = Empate.

#### Variáveis - Expulsões, Lesões e Suspensões

Expulsões são um assunto sério, principalmente quando se tem como titular jogadores como por exemplo, Sérgio Ramos, que é atualmente o jogador com mais expulsões em todas as ligas européias (20). E para calcularmos isso, somaremos todas as expulsões dos anos anteriores e calcularemos a Probabilidade de termos algum jogador do time ser expulso.

Sendo assim teríamos duas colunas, cada uma contendo a probabilidade de um dos times a ter algum expulso.

Já para as lesões pretendemos utilizar os dados que o Fifa no disponibiliza, pois o FIFA já faz uma análise dos jogadores mais propensos a lesão. Com isso contaremos o número de jogadores que cada um dos times que se enquadram nessa classificação - *propensos  lesão*.

Para as suspensões, compilaremos todos os cartões vermelhos e em nosso modelo caso exista algum jogador com cartão vermelho no jogo anterior, então será contabilizado uma suspensão para aquele time. Portanto existirão duas colunas com suspensões, uma para cada time.

#### Variável - Evolução

É bem possível que ao longo da temporada um time evolua, ou até mesmo tenha uma decadência, em virtude disso em nosso modelo, os índices dos jogadores serão constantemente atualizados pelas bases semanais que a próprio EA - FIFA divulga, contendo também os melhores de cada semana.

#### Variável - Histórico

Sabem daqueles casos em que o time frequentemente perde quando enfrenta determinado adversário? É essa aleatóriedade que pretendemos captar. Com isso teremos 3 colunas, cada coluna contendo o retrospecto de vitórias, empates e derrotas, do time da casa para o visitante.

#### Variável - Overall Time Titular e Reserva - FIFA

Todos que jogam o jogo do FIFA sabem que existem milhões de dados sobre todos os jogadores ali datados. Como estaremos avaliando e prevendo cada partida, onde uma linha singifica um confronto, nosso modelo contará com a diferença do overall geral do time titular e do time reserva para cada um dos times. Ou seja

	Coluna 1) Overall Do Time Titular Casa (Difenca) #Simulando A Diferenca Do Time Da Casa E Visitante
	Coluna 2) Overall Do Time Reserva Casa (Difenca)
	Coluna 3) Overall Ataque Titular (Difenca)
	Coluna 4) Overall Meio Campo Titular (Difenca)
	Coluna 5) Overall Defesa Titular (Difenca)
# Modelos
# Resultados
