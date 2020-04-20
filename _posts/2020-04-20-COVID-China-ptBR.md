---
layout: post
title: Análise estatística dos dados chineses do COVID-19 usando Lei de Benford e clusterização.
lang: pt
header-img: img/covid_capa.jpg
date: 2020-04-20
tags: [COVID-19, Coronavírus, crescimento exponencial, teste de hipótese]
author: Peng Yaohao e Mateus Hiro Nagata.
comments: true
---

### "真金不怕火煉" 
#### *(Dito chinês: "O ouro verdadeiro não teme o teste do fogo"*)


---

###### As opiniões emitidas neste trabalho são de inteira responsabilidade dos autores, não exprimindo, necessariamente, o ponto de vista das instituições às quais estão ou estiveram vinculados.

---

## Motivação

COVID-19 (SARS-CoV-2) é uma pandemia vigente que já infectou mais de 2 milhões de pessoas em todo o mundo e já fez mais de 160 mil vítimas fatais. Diferentemente de todas as outras pandemias já registradas na história, grande volume de dados e notícias sobre o COVID-19 são disponibilizadas com grande velocidade e abrangência, mobilizando estudiosos de variados campos do conhecimento para concentrar seus esforços em analisar esses dados e propor soluções.

Em dados de epidemias, é natural observar um crescimento exponencial do número de infectados, em especial nos estágios iniciais da doença. Conforme abordado em posts como [este](https://medium.com/@tomaspueyo/coronavirus-the-hammer-and-the-dance-be9337092b56), medidas de isolamento social buscam "achatar a curva", reduzindo o pico do número de infectados, mas prolongando a "onda" ao longo do tempo. Analogamente, o número de óbitos também segue uma tendência exponencial. 

Analisando os dados dos países afetados pela pandemia, é possível observar padrões que são comuns a todos. Apesar da existência de várias peculiaridades como extensão territorial, densidade populacional, temperatura, estação do ano, grau de subnotificação, disciplina social para acatar medidas de isolamento, etc., as quais diferem significativamente entre os diferentes países, o vírus (ainda) não sofreu mutações radicais desde seu surgimento na China, de modo que os parâmetros gerais de infectividade e letalidade são similares entre os países. No entanto, **os dados chineses são uma notória exceção**, apresentando um comportamento que difere dos demais -- apesar de ser o primeiro país afetado pela doença, o crescimento do número de infectados se manteve próximo de uma tendência linear em estágios iniciais, com <ins> poucos momentos que sofrem variação abrupta e prolongados períodos marcados pela ausência de variância</ins>, ambas tendências pouco usuais na natureza. Veja a seguir alguns gráficos:

![](/img/covid19/f1.png)
***Imagem 1**: Casos acumulados de COVID-19 na China, dados de 18/04/2020*

![](/img/covid19/f2.png)
***Imagem 2**: Igual à imagem 1, comparando a província de Hubei com todas as outras províncias*

![](/img/covid19/f3.png)
***Imagem 3**: Igual à imagem 2, com todas as províncias exceto Hubei, com escala ajustada para melhor visualização*


A **imagem 1** acima mostra o número acumulado de casos confirmados de COVID-19 na China (não inclui Taiwan). Note que o padrão exponencial aparece no início mas a concavidade da curva muda rapidamente, ao contrário do esperado e observado em praticamente todos os outros países. O crescimento acontece com "saltos" no início da série, seguido por uma tendência praticamente linear nos primeiros dias de fevereiro, um único dia de grande crescimento, e um longo período com cada vez menos casos novos por dia, até se tornar praticamente uma linha reta a partir do início de março.

Ademais, vendo a decomposição do dado agregado para nível de província na **imagem 2**, é possível notar que uma única província -- Hubei -- puxa praticamente toda a série da China, enquanto que todas as outras províncias (as quais também diferem bastante em área, densidade populacional, temperatura, distância com a cidade de origem do vírus, etc.) apresentam praticamente o mesmo padrão, resultando numa curva sigmóide praticamente cirúrgica.

A nível de província, o único local que apresentou algum grau de variância ao longo do tempo foi Hong Kong; em Shandong, observou-se uma grande variação num único dia -- 21/02/2020, quando foi notificado um surto do vírus na penitenciária de Rencheng da cidade de Jinin: nesse dia 203 pessoas foram adicionadas à estatística de confirmados, um ponto isolado numa curva "bem comportada" em todos os períodos além desse dia. Em Guangdong (no sul da China) e em Heilongjiang (no nordeste da China) a série voltou a apresentar crescimento no fim de março e no início de abril, respectivamente, ambos após um longo período praticamente sem variações. Veja o comportamento das séries dessas províncias na **imagem 3**.

Para efeito de comparação, veja as séries dos casos de COVID-19 em cada estado dos Estados Unidos:

![](/img/covid19/f4.png)
***Imagem 4**: Casos acumulados de COVID-19 por estado dos Estados Unidos, dados de 30/03/2020. Retirado de ["Coronavirus: Out of Many, One" por Tomas Pueyo](https://medium.com/@tomaspueyo/coronavirus-out-of-many-one-36b886af37e9)*

![](/img/covid19/f5.png)
***Imagem 5**: Igual à imagem 4, com todas as províncias exceto New York e New Jersey. Retirado de ["Coronavirus: Out of Many, One" por Tomas Pueyo](https://medium.com/@tomaspueyo/coronavirus-out-of-many-one-36b886af37e9)*


É bem sabido que <ins>a derivada de uma função exponencial também é uma exponencial</ins>, então é de se esperar que a variação diária do número de confirmados também siga uma exponencial. Os dados chineses, porém, mostraram algo bem diferente:

![](/img/covid19/f6.png)
***Imagem 6**: Variação diária de casos de COVID-19 na China, dados de 18/04/2020*

O segmento inicial da série mais se assemelha a uma reta do que uma exponencial considerando os dois picos em 28/01 e 02/02, enquanto que o número de novos casos diminuiu rapidamente e sem grandes variações -- exceto o pico de 15136 novos casos no dia 13/02/2020, que claramente destoa dos demais períodos. Desconsiderando esse ponto anômalo, é possível enxergar uma tendência praticamente linear entre 02/02 e 23/02, data a partir da qual a série praticamente vira uma linha reta -- novamente, um padrão raro em doenças contagiosas.

Veja abaixo o mesmo gráfico comparando a província de Hubei com todas as demais províncias:

![](/img/covid19/f7.png)
***Imagem 7**: Variação diária de casos de COVID-19 na China, dados de 18/04/2020, comparando Hubei com todas as outras províncias*

![](/img/covid19/f8.png)
***Imagem 8**: Igual à imagem 7, com todas as províncias exceto Hubei, com escala ajustada para melhor visualização*

Vale notar que, [no mesmo dia que houve esse pico, Jiang Chaoling e Ma Guoqiang foram exonerados](http://www.xinhuanet.com/renshi/2020-02/13/c_1125568253.htm): eles eram o Nº 1 e o Nº 2 na hierarquia de comando de Hubei (secretário-geral e vice secretário-geral do Partido na província, respectivamente).

**Os dados chineses de óbitos por COVID-19 são igualmente anti-intuitivos**, veja abaixo nas **imagens 9 a 11**. Note que a província com mais mortes registradas foi Henan, que é vizinha de Hubei, com apenas 22 óbitos. A variação diária de mortos é uma série praticamente estacionária, temperada com o bizarro "ajuste" de 1290 mortes em 17/04 após mais de um mês quase sem mortes oficiais:

![](/img/covid19/f9.png)
***Imagem 9**: Óbitos totais por COVID-19 na China, dados de 18/04/2020, comparando Hubei com todas as outras províncias*

![](/img/covid19/f10.png)
***Imagem 10**: Igual à imagem 9, com todas as províncias exceto Hubei, com escala ajustada para melhor visualização*

![](/img/covid19/f11.png)
***Imagem 11**: Variação diária de óbitos por COVID-19 na China, dados de 18/04/2020*

A adoção de medidas severas de isolamento social influenciam diretamente no formato das curvas, porém o padrão exponencial se mantém pelo menos nos estágios iniciais, e seus efeitos também demoram um certo tempo para se tornarem evidentes. Compare abaixo com dados de Taiwan, Singapura e Coreia do Sul, adotaram quarentena em estágios iniciais da doença, bem como da Itália, Espanha e Reino Unido, que agiram com maior intensidade em estágios mais avançados:

![](/img/covid19/f12.png)
***Imagem 12**: Casos totais de COVID-19 na Espanha, Itália, Reino Unido, China, Coreia do Sul, Japão e Singapura, dados de 18/04/2020*

![](/img/covid19/f13.png)
***Imagem 13**: Igual à imagem 12, com os países asiáticos exceto China. Note que Japão e Singapura também apresentaram um crescimento exponencial que começou mais tarde. Apenas a curva da Coreia do Sul apresentou um formato que se assemelha aos dados da China*

Com a análise visual acima é possível deduzir que existe algum "padrão" subjacente aos dados do COVID-19, mas que por alguma razão não aparece nos dados da China. Vamos fazer a seguir um exercício de tentar identificar esse padrão usando a **Lei de Benford**.


## Lei de Benford

A vida é misteriosa e padrões inesperados governam o mundo. Como dizem por aí, “a vida imita a arte”. Quando, entretanto, a arte tenta imitar a vida, podemos sentir algo estranho, como se a complexidade da vida não pudesse ser substituída pela ingênua engenhosidade humana.

Um desses padrões que parecem "emergir" na natureza é Lei de Benford, descoberta por [Newcomb (1881)](https://www.semanticscholar.org/paper/Note-on-the-Frequency-of-Use-of-the-Different-in-Newcomb/4136337f95c88d446a5577d9331c8fc0309c11af) e popularizada por [Benford (1938)](https://pt.scribd.com/document/209534421/The-Law-of-Anomalous-Numbers), muito usada para verificar fraudes em base de dados.

Benford mostrou que, para mais de 20 variáveis de contextos, tais como tamanhos de rio, população de cidades, constantes da física, taxa de mortalidade, etc., **a chance do primeiro dígito de um número ser 1 é a mais alta, chance que vai diminuindo progressivamente com os algarismos subsequentes**. Ou seja, é mais provável que o primeiro algarismo do número seja o número 1. Em seguida, o 2, e sucessivamente até o 9.

[Okhrimenko e Kopczewski (2019)](https://www.nbp.pl/badania/seminaria/8ii2019.pdf), dois economistas comportamentais, testaram a capacidade das pessoas de criar dados falsos, com o intuito de burlar a base tributária. Os autores encontraram clara evidência de que pelo critério da lei de Benford, o sistema facilmente identificaria os dados falsos. Outras aplicações da Lei de Benford para identificação de dados manipulados e detecção de fraudes incluem [Hales et al.(2008)](https://www.sciencedirect.com/science/article/abs/pii/S0377221706011702), [Abrantes-Metz et al. (2012)](https://www.sciencedirect.com/science/article/abs/pii/S0378426611002032) e [Nigrini (2012)](https://www.amazon.com/Benfords-Law-Applications-Accounting-Detection/dp/1118152859).

Como explicar intuitivamente essa regularidade aparentemente sem sentido? A resposta está relacionada a dois conceitos conhecidos: a função exponencial e a escala logarítmica. Vamos revisá-las pois elas estão por toda a parte, em especial durante a presente pandemia...

![](/img/covid19/expo.jpg)

Epidemias como a do Coronavirus, a qual estamos vivendo nesse momento, são clássicos exemplos para explicar a função exponencial. A modelagem acontece da seguinte forma: a quantidade de infectados amanhã $$I_1$$ é igual a uma constante $$\alpha$$ vezes a quantidade de infectados hoje $$I_0$$; ou seja, $$I_1 = \alpha \cdot I_0$$.


Supondo que a taxa seja a mesma para amanhã (podemos interpretar como sendo que nenhuma política ou mudança de hábitos da população tenha ocorrido), A quantidade de pessoas infectadas depois de amanhã, $$I_2$$, é uma proporção do que é amanhã ($$I_2=\alpha \cdot I_1$$), que por sua vez pode ser substituído por $$I_2=\alpha \cdot I_1 = \alpha \cdot \alpha \cdot I_0$$. Com perspicácia, percebemos que podemos generalizar essa fórmula para daqui a t dias. Sendo t qualquer número que quisermos. A generalização é $$I_t=\alpha^t \cdot I_0$$. O praticamente onipresente regime de juros compostos dos números financeiros também segue essa mesma lógica ($$F = P(1+i)^n$$).

Aqui está o segredo da lei de Benford. Vejamos a simulação de crescimento da epidemia exponencialmente, isso é, cada dia é um múltiplo fixo do dia anterior. Analisemos na escala padrão e na escala logarítmica ao longo do tempo (nesse caso usamos exponenciais de 2, mas poderia ser qualquer número-base). Veja que nos dois casos, cada gradação de azul é a área referente a um dígito. A primeira corresponde entre 10 e 20, a segunda entre 20 e 30, assim sucessivamente até o número 100. Veja que essa distância é diferente na escala padrão e na escala logarítmica.

![](/img/covid19/f14.png)
***Imagem 14**: Função exponencial e escala logarítmica*

Esse gráfico já nos dá uma pista de porque fenômenos exponenciais podem obedecer a lei de Benford. Quando olhamos pelas lentes logarítmicas, uma função exponencial se parece uma função que cresce linearmente, que cada observação é equidistante das observações antes e depois. Todavia, nessa mesma lente logarítmica, a área que existe entre 10 e 20 é menor a que existe entre 20 e 30, e assim succesivamente. Isso quer dizer que a probabilidade da variável cair nessa faixa é menor que nas faixas seguintes.

Usando um jargão técnico, <ins>**a mantissa dos logs é uniformemente distribuída**</ins>. Para o número $$x$$ que conseguimos da amostra, vamos aplicar o seu log -- por exemplo, apliquemos o log para o número 150:  $$log_{10} (150) \approx 2.176$$. Podemos decompor o resultado na parte inteira $$m = 2$$ e na parte decimal $$d = 0.176$$. A parte decimal é a que chamamos de *mantissa*.

### Mantissa e distribuição teórica $$D_T$$
Pelas propriedades do logaritmo, $$log_{10} (150) = log_{10}(100 \cdot 1.5) = log_{10}(100) + log_{10}(1.5) = 2 + 0.176$$. Observe que o log na base 10 de qualquer potência inteira de 10 (100, 1000, 10000...) vai resultar em um número inteiro. Nesse sentido, a parte inteira do log na base 10 retorna a quantidade de algarismos que o número original tem (já que o sistema numérico indo-arábico possui 10 algarismos). A mantissa, por outro lado, é o responsável por dizer qual é o primeiro algarismo.

Na casa das centenas, enquanto a mantissa estiver no intervalo $$[0, 0.301)$$, o número original estará entre $$[100, 200)$$. Fazendo o mesmo procedimento anterior, quando o número chegar em 200, o seu log resultará em $$log_{10}(200) = log_{10}(100 \cdot 2) = log_{10}(100) + log_{10}(2)$$ que resulta em $$2 + 0.301$$.

Como vimos que a mantissa é decisiva para saber o primeiro dígito, podemos finalmente entender a lei de Benford. A sua ideia é que a mantissa tem distribuição uniforme para os dígitos de 1 a 9. Assim, é mais fácil occorrer o dígito número 1 por que ele tem o maior intervalo da mantissa $$[0, 0.301)$$.

Podemos definir, de acordo com a lei de Benford, a probabilidade de um múnero possuir primeiro dígito $$d$$, dada por:

\begin{equation}
P(d)=\log_{10}\left(1+\frac{1}{d}\right), d = 1,...,9
\end{equation}

Para poupar tempo do leitor, calculamos a distribuição de probabilidade de cada dígito ser o primeiro de acordo com a Lei de Benford ($$D_T$$), bem como os respectivos intervalos na mantissa:

<center>

\begin{array}{|c|c|c|c|}
\hline d & P(d) & Probabilidade & Mantissa \\
\hline 1 & log_{10} (1+1) = 0.30103  & 30.1\% & [0, 0.301)\\
\hline 2 & \log _{10}\left(1+\frac{1}{2}\right) =  0.1760913  & 17.6\% & [0.301, 0.477)\\
\hline 3 & \log _{10}\left(1+\frac{1}{3}\right) = 0.1249387 & 12.5\% & [0.477, 0.602) \\
\hline 4 & \log _{10}\left(1+\frac{1}{4}\right) = 0.09691001 & 9.7\% & [0.602, 0.699)\\
\hline 5 & \log _{10}\left(1+\frac{1}{5}\right) = 0.07918125 & 7.9\% & [0.699, 0.778)\\
\hline 6 & \log _{10}\left(1+\frac{1}{6}\right) = 0.06694679 & 6.7\% & [0.778, 0.845)\\
\hline 7 & \log _{10}\left(1+\frac{1}{7}\right) = 0.05799195 & 5.8\% & [0.845, 0.912)\\
\hline 8 & \log _{10}\left(1+\frac{1}{8}\right) = 0.05115252 & 5.1\% & [0.912, 0.954)\\
\hline 9 & \log _{10}\left(1+\frac{1}{9}\right) = 0.04575749 & 4.6\% & [0.963, 1.00]\\
\hline
\end{array}

</center>
***Tabela 1** : Distribuição do primeiro dígito de acordo com a Lei de Benford*

Para mais detalhes sobre a lei de Benford e suas aplicações, dê uma olhada nesses links [[1](http://prorum.com/?qa=2157/existem-formas-de-detectar-fraudes-em-bases-de-dados); [2](https://epublications.marquette.edu/cgi/viewcontent.cgi?article=1031&context=account_fac); [3](https://towardsdatascience.com/what-is-benfords-law-and-why-is-it-important-for-data-science-312cb8b61048)]

## Distribuições empíricas $$D_E$$ dos dados do COVID-19

Para este exercício, escolhemos os <ins> países com mais de 10000 casos confirmados de COVID-19 em 18/04/2020</ins>, de acordo com os dados disponíveis [neste link](https://github.com/RamiKrispin/coronavirus-csv). Vejamos a seguir como ficou a distribuição do primeiro dígito para as séries temporais de casos e óbitos por COVID-19 dos 24 países selecionados:

![](/img/covid19/f15.png)
***Imagem 15**: Distribuição do primeiro dígito das séries de casos de COVID-19*

![](/img/covid19/f16.png)
***Imagem 16**: Distribuição do primeiro dígito das séries de óbitos por COVID-19*

**Nenhuma base de dados segue perfeitamente a Lei de Benford, mas as distribuições empíricas da China parecem ser particularmente diferentes das dos demais países**. Pela inspeção visual, os dados da China parecem destoar significativamente da Lei de Benford. Para maior robustez, vamos comparar as distribuições empíricas $$D_E$$ com a distribuição teórica $$D_T$$ que vem da Lei de Benford realizando alguns testes de hipóteses.

## Testes de hipóteses

Para este exercício, vamos realizar três testes de hipóteses:

* Teste qui-quadrado
* Teste Kolmogorov-Smirnov (bicaudal)
* Teste de Kuiper

Os três testes acima são parecidos, <ins>todos têm como hipótese nula a igualdade entre as distribuições empírica ($$D_E$$) e teórica ($$D_T$$)</ins>. O teste qui-quadrado é o mais comumente utilizado, porém tende a rejeitar a hipótese nula mais facilmente, enquanto que o teste KS é menos sensível a diferenças pontuais; o teste de Kuiper funciona da mesma forma que o teste KS, com a diferença que considera separadamente diferenças positivas e negativas entre as distribuições (o caso "E maior que T" é encarado como diferente do caso "T maior que E"). A tabela com os p-valores associados está abaixo:

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-1wig">País/Teste</th>
    <th class="tg-1wig">Qui-quadrado</th>
    <th class="tg-1wig">Kolmogorov-Smirnov</th>
    <th class="tg-1wig">Kuiper</th>
  </tr>
  <tr>
    <td class="tg-1wig">Áustria</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Bélgica</td>
    <td class="tg-1wig">0.0004</td>
    <td class="tg-1wig">0.0366</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brasil</td>
    <td class="tg-0lax">0.2685</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canadá</td>
    <td class="tg-1wig">0.0117</td>
    <td class="tg-0lax">1.0000</td>
    <td class="tg-0lax">1.0000</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-1wig">0.0086</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">França</td>
    <td class="tg-1wig">0.0363</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">1.0000</td>
  </tr>
  <tr>
    <td class="tg-1wig">Alemanha</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Índia</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irã</td>
    <td class="tg-0lax">0.1284</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irlanda</td>
    <td class="tg-0lax">0.8036</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.5245</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Itália</td>
    <td class="tg-0lax">0.0705</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.0521</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japão</td>
    <td class="tg-0lax">0.0509</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Coreia do Sul</td>
    <td class="tg-1wig">0.0002</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Países Baixos</td>
    <td class="tg-0lax">0.4804</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.9629</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.6247</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Rússia</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Espanha</td>
    <td class="tg-1wig">0.0027</td>
    <td class="tg-1wig">0.0366</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suécia</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suíça</td>
    <td class="tg-1wig">0.0111</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turquia</td>
    <td class="tg-0lax">0.6985</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Reino Unido</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Estados Unidos</td>
    <td class="tg-1wig">0.0156</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
</table>
</center>
***Tabela 2** : P-valores para os testes de hipótese para dados de casos de COVID-19, arredondado para quatro casas decimais. Valores significantes ao nível de confiança de 95% estão em negrito*

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-1wig">País/Teste</th>
    <th class="tg-1wig">Qui-quadrado</th>
    <th class="tg-1wig">Kolmogorov-Smirnov</th>
    <th class="tg-1wig">Kuiper</th>
  </tr>
  <tr>
    <td class="tg-1wig">Áustria</td>
    <td class="tg-0lax">0.2883</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Bélgica</td>
    <td class="tg-0lax">0.3746</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brasil</td>
    <td class="tg-0lax">0.9773</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canadá</td>
    <td class="tg-0lax">0.7868</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-1wig">0.0000</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">França</td>
    <td class="tg-1wig">0.0454</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Alemanha</td>
    <td class="tg-0lax">0.5473</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Índia</td>
    <td class="tg-0lax">0.3685</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irã</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irlanda</td>
    <td class="tg-0lax">0.7039</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.4175</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Itália</td>
    <td class="tg-0lax">0.2414</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japão</td>
    <td class="tg-1wig">0.0203</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Coreia do Sul</td>
    <td class="tg-0lax">0.1442</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Países Baixos</td>
    <td class="tg-0lax">0.4993</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.5246</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.3712</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.1797</td>
  </tr>
  <tr>
    <td class="tg-1wig">Rússia</td>
    <td class="tg-0lax">0.6750</td>
    <td class="tg-0lax">0.3364</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Espanha</td>
    <td class="tg-0lax">0.1228</td>
    <td class="tg-0lax">0.1243</td>
    <td class="tg-0lax">0.4969</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suécia</td>
    <td class="tg-0lax">0.7078</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suíça</td>
    <td class="tg-0lax">0.6034</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turquia</td>
    <td class="tg-0lax">0.5745</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Reino Unido</td>
    <td class="tg-0lax">0.8325</td>
    <td class="tg-0lax">0.9794</td>
    <td class="tg-0lax">0.7292</td>
  </tr>
  <tr>
    <td class="tg-1wig">Estados Unidos</td>
    <td class="tg-0lax">0.9284</td>
    <td class="tg-0lax">0.6994</td>
    <td class="tg-0lax">0.9761</td>
  </tr>
</table>

</center>
***Tabela 3** : P-valores para os testes de hipótese para dados de óbitos por COVID-19, arredondado para quatro casas decimais. Valores significantes ao nível de confiança de 95% estão em negrito*

Basicamente, quanto **menor** o p-valor, **menos** os dados do respectivo país parecem "obedecer" à Lei de Benford. <ins>Usando essa métrica de avaliação, os dados chineses claramente são anômalos em relação à Lei de Benford, ao passo que a grande maioria dos outros países parecem segui-la razoavelmente bem.</ins>

## Distância KL e clusterização por DBSCAN

Vamos ver agora o quão "parecidos" são os dados dos países entre si, utilizando uma métrica chamada **Distância de Kullback-Leibler** (doravante "distância KL"), a qual é uma <ins>medida de entropia relativa entre duas distribuições de probabilidade</ins>. Seu cálculo para distribuições discretas se dá mediante a seguinte expressão:

\begin{equation}
KL(D_E||D_T) = \sum\limits_{x \in \mathcal{P}}{D_E(x)\cdot log\left(\frac{D_E(x)}{D_T(x)}\right)}
\end{equation}

A distância KL fornece o valor esperado da diferença logarítmica entre duas distribuições (no nosso caso, $$D_E$$ e $$D_T$$) definidas no mesmo espaço de probabilidade $$\mathcal{P}$$; a discussão teórica da  está além dos escopos deste post, pois envolve conhecimentos de teoria da informação e teoria da medida. Em termos simplificados, a distância KL mede o quão diferentes são duas distribuições de probabilidade -- quanto mais próximo de zero, mais "parecidas" elas são. Como são 24 países, como resultado temos uma matriz 24x24 que mede a "diferença" entre os dados dos países considerados, matriz cuja diagonal principal é toda zero (a "diferença" de algo com ela mesma é igual a zero!).

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0lax"></th>
    <th class="tg-1wig">Áustria</th>
    <th class="tg-1wig">Bélgica</th>
    <th class="tg-1wig">Brasil</th>
    <th class="tg-1wig">Canadá</th>
    <th class="tg-1wig">China</th>
    <th class="tg-1wig">França</th>
    <th class="tg-1wig">Alemanha</th>
    <th class="tg-1wig">Índia</th>
    <th class="tg-1wig">Irã</th>
    <th class="tg-1wig">Irlanda</th>
    <th class="tg-1wig">Israel</th>
    <th class="tg-1wig">Itália</th>
    <th class="tg-1wig">Japão</th>
    <th class="tg-1wig">Coreia do Sul</th>
    <th class="tg-1wig">Países Baixos</th>
    <th class="tg-1wig">Peru</th>
    <th class="tg-1wig">Portugal</th>
    <th class="tg-1wig">Rússia</th>
    <th class="tg-1wig">Espanha</th>
    <th class="tg-1wig">Suécia</th>
    <th class="tg-1wig">Suíça</th>
    <th class="tg-1wig">Turquia</th>
    <th class="tg-1wig">Reino Unido</th>
    <th class="tg-1wig">Estados Unidos</th>
  </tr>
  <tr>
    <td class="tg-1wig">Áustria</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.0734</td>
    <td class="tg-0lax">0.1005</td>
    <td class="tg-0lax">0.5631</td>
    <td class="tg-0lax">0.0387</td>
    <td class="tg-0lax">0.0447</td>
    <td class="tg-0lax">0.1739</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.0371</td>
    <td class="tg-0lax">0.0395</td>
    <td class="tg-0lax">0.0595</td>
    <td class="tg-0lax">0.0921</td>
    <td class="tg-0lax">0.0778</td>
    <td class="tg-0lax">0.0313</td>
    <td class="tg-0lax">0.0268</td>
    <td class="tg-0lax">0.0146</td>
    <td class="tg-0lax">0.1832</td>
    <td class="tg-0lax">0.0362</td>
    <td class="tg-0lax">0.0214</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0826</td>
    <td class="tg-0lax">0.1109</td>
    <td class="tg-0lax">0.0410</td>
  </tr>
  <tr>
    <td class="tg-1wig">Bélgica</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0452</td>
    <td class="tg-0lax">0.1612</td>
    <td class="tg-0lax">0.7024</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0956</td>
    <td class="tg-0lax">0.1827</td>
    <td class="tg-0lax">0.1964</td>
    <td class="tg-0lax">0.0727</td>
    <td class="tg-0lax">0.0819</td>
    <td class="tg-0lax">0.0488</td>
    <td class="tg-0lax">0.1466</td>
    <td class="tg-0lax">0.1303</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0704</td>
    <td class="tg-0lax">0.0556</td>
    <td class="tg-0lax">0.1564</td>
    <td class="tg-0lax">0.0539</td>
    <td class="tg-0lax">0.0455</td>
    <td class="tg-0lax">0.0995</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1662</td>
    <td class="tg-0lax">0.1019</td>
  </tr>
  <tr>
    <td class="tg-1wig">Brasil</td>
    <td class="tg-0lax">0.0734</td>
    <td class="tg-0lax">0.0452</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0981</td>
    <td class="tg-0lax">0.5960</td>
    <td class="tg-0lax">0.1057</td>
    <td class="tg-0lax">0.1721</td>
    <td class="tg-0lax">0.1647</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0601</td>
    <td class="tg-0lax">0.0887</td>
    <td class="tg-0lax">0.0252</td>
    <td class="tg-0lax">0.0707</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0235</td>
    <td class="tg-0lax">0.0650</td>
    <td class="tg-0lax">0.0670</td>
    <td class="tg-0lax">0.0532</td>
    <td class="tg-0lax">0.0693</td>
    <td class="tg-0lax">0.1201</td>
    <td class="tg-0lax">0.0533</td>
    <td class="tg-0lax">0.1100</td>
    <td class="tg-0lax">0.0990</td>
    <td class="tg-0lax">0.1111</td>
  </tr>
  <tr>
    <td class="tg-1wig">Canadá</td>
    <td class="tg-0lax">0.1005</td>
    <td class="tg-0lax">0.1612</td>
    <td class="tg-0lax">0.0981</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.3342</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.1370</td>
    <td class="tg-0lax">0.2187</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.0376</td>
    <td class="tg-0lax">0.1266</td>
    <td class="tg-0lax">0.0310</td>
    <td class="tg-0lax">0.0433</td>
    <td class="tg-0lax">0.1267</td>
    <td class="tg-0lax">0.0698</td>
    <td class="tg-0lax">0.0839</td>
    <td class="tg-0lax">0.2046</td>
    <td class="tg-0lax">0.1453</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1335</td>
    <td class="tg-0lax">0.0830</td>
    <td class="tg-0lax">0.0824</td>
    <td class="tg-0lax">0.1181</td>
  </tr>
  <tr>
    <td class="tg-1wig">China</td>
    <td class="tg-0lax">0.5631</td>
    <td class="tg-0lax">0.7024</td>
    <td class="tg-0lax">0.5960</td>
    <td class="tg-0lax">0.3342</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.7432</td>
    <td class="tg-0lax">0.7605</td>
    <td class="tg-0lax">0.6701</td>
    <td class="tg-0lax">0.5725</td>
    <td class="tg-0lax">0.6681</td>
    <td class="tg-0lax">0.5067</td>
    <td class="tg-0lax">0.6763</td>
    <td class="tg-0lax">0.4182</td>
    <td class="tg-0lax">0.3305</td>
    <td class="tg-0lax">0.6036</td>
    <td class="tg-0lax">0.5808</td>
    <td class="tg-0lax">0.6312</td>
    <td class="tg-0lax">0.8489</td>
    <td class="tg-0lax">0.7006</td>
    <td class="tg-0lax">0.7566</td>
    <td class="tg-0lax">0.6211</td>
    <td class="tg-0lax">0.6705</td>
    <td class="tg-0lax">0.4789</td>
    <td class="tg-0lax">0.6572</td>
  </tr>
  <tr>
    <td class="tg-1wig">França</td>
    <td class="tg-0lax">0.0387</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.1057</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.7432</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.1576</td>
    <td class="tg-0lax">0.0771</td>
    <td class="tg-0lax">0.0246</td>
    <td class="tg-0lax">0.0361</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.1107</td>
    <td class="tg-0lax">0.1180</td>
    <td class="tg-0lax">0.0807</td>
    <td class="tg-0lax">0.0137</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.2226</td>
    <td class="tg-0lax">0.0879</td>
    <td class="tg-0lax">0.0524</td>
    <td class="tg-0lax">0.1224</td>
    <td class="tg-0lax">0.0256</td>
    <td class="tg-0lax">0.1278</td>
    <td class="tg-0lax">0.0244</td>
  </tr>
  <tr>
    <td class="tg-1wig">Alemanha</td>
    <td class="tg-0lax">0.0447</td>
    <td class="tg-0lax">0.0956</td>
    <td class="tg-0lax">0.1721</td>
    <td class="tg-0lax">0.1370</td>
    <td class="tg-0lax">0.7605</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.2417</td>
    <td class="tg-0lax">0.1751</td>
    <td class="tg-0lax">0.0690</td>
    <td class="tg-0lax">0.0461</td>
    <td class="tg-0lax">0.1260</td>
    <td class="tg-0lax">0.1635</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.1170</td>
    <td class="tg-0lax">0.0521</td>
    <td class="tg-0lax">0.0420</td>
    <td class="tg-0lax">0.2765</td>
    <td class="tg-0lax">0.0858</td>
    <td class="tg-0lax">0.0206</td>
    <td class="tg-0lax">0.1607</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.1935</td>
    <td class="tg-0lax">0.0691</td>
  </tr>
  <tr>
    <td class="tg-1wig">Índia</td>
    <td class="tg-0lax">0.1739</td>
    <td class="tg-0lax">0.1827</td>
    <td class="tg-0lax">0.1647</td>
    <td class="tg-0lax">0.2187</td>
    <td class="tg-0lax">0.6701</td>
    <td class="tg-0lax">0.1576</td>
    <td class="tg-0lax">0.2417</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.2459</td>
    <td class="tg-0lax">0.2386</td>
    <td class="tg-0lax">0.2244</td>
    <td class="tg-0lax">0.0706</td>
    <td class="tg-0lax">0.2892</td>
    <td class="tg-0lax">0.2965</td>
    <td class="tg-0lax">0.1888</td>
    <td class="tg-0lax">0.2027</td>
    <td class="tg-0lax">0.2205</td>
    <td class="tg-0lax">0.3063</td>
    <td class="tg-0lax">0.3947</td>
    <td class="tg-0lax">0.2911</td>
    <td class="tg-0lax">0.3474</td>
    <td class="tg-0lax">0.1517</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.2509</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irã</td>
    <td class="tg-0lax">0.1223</td>
    <td class="tg-0lax">0.1964</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0409</td>
    <td class="tg-0lax">0.5725</td>
    <td class="tg-0lax">0.0771</td>
    <td class="tg-0lax">0.1751</td>
    <td class="tg-0lax">0.2459</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0947</td>
    <td class="tg-0lax">0.0674</td>
    <td class="tg-0lax">0.1353</td>
    <td class="tg-0lax">0.0491</td>
    <td class="tg-0lax">0.1192</td>
    <td class="tg-0lax">0.1351</td>
    <td class="tg-0lax">0.0598</td>
    <td class="tg-0lax">0.1113</td>
    <td class="tg-0lax">0.2188</td>
    <td class="tg-0lax">0.1537</td>
    <td class="tg-0lax">0.1645</td>
    <td class="tg-0lax">0.1445</td>
    <td class="tg-0lax">0.0292</td>
    <td class="tg-0lax">0.1040</td>
    <td class="tg-0lax">0.0680</td>
  </tr>
  <tr>
    <td class="tg-1wig">Irlanda</td>
    <td class="tg-0lax">0.0371</td>
    <td class="tg-0lax">0.0727</td>
    <td class="tg-0lax">0.0601</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.6681</td>
    <td class="tg-0lax">0.0246</td>
    <td class="tg-0lax">0.0690</td>
    <td class="tg-0lax">0.2386</td>
    <td class="tg-0lax">0.0947</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0321</td>
    <td class="tg-0lax">0.0540</td>
    <td class="tg-0lax">0.0644</td>
    <td class="tg-0lax">0.0742</td>
    <td class="tg-0lax">0.0472</td>
    <td class="tg-0lax">0.0068</td>
    <td class="tg-0lax">0.0459</td>
    <td class="tg-0lax">0.1446</td>
    <td class="tg-0lax">0.0583</td>
    <td class="tg-0lax">0.0663</td>
    <td class="tg-0lax">0.0632</td>
    <td class="tg-0lax">0.0446</td>
    <td class="tg-0lax">0.0790</td>
    <td class="tg-0lax">0.0411</td>
  </tr>
  <tr>
    <td class="tg-1wig">Israel</td>
    <td class="tg-0lax">0.0395</td>
    <td class="tg-0lax">0.0819</td>
    <td class="tg-0lax">0.0887</td>
    <td class="tg-0lax">0.0376</td>
    <td class="tg-0lax">0.5067</td>
    <td class="tg-0lax">0.0361</td>
    <td class="tg-0lax">0.0461</td>
    <td class="tg-0lax">0.2244</td>
    <td class="tg-0lax">0.0674</td>
    <td class="tg-0lax">0.0321</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.0621</td>
    <td class="tg-0lax">0.0432</td>
    <td class="tg-0lax">0.0809</td>
    <td class="tg-0lax">0.0184</td>
    <td class="tg-0lax">0.0456</td>
    <td class="tg-0lax">0.2038</td>
    <td class="tg-0lax">0.0847</td>
    <td class="tg-0lax">0.0467</td>
    <td class="tg-0lax">0.1104</td>
    <td class="tg-0lax">0.0476</td>
    <td class="tg-0lax">0.0952</td>
    <td class="tg-0lax">0.0578</td>
  </tr>
  <tr>
    <td class="tg-1wig">Itália</td>
    <td class="tg-0lax">0.0595</td>
    <td class="tg-0lax">0.0488</td>
    <td class="tg-0lax">0.0252</td>
    <td class="tg-0lax">0.1266</td>
    <td class="tg-0lax">0.6763</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.1260</td>
    <td class="tg-0lax">0.0706</td>
    <td class="tg-0lax">0.1353</td>
    <td class="tg-0lax">0.0540</td>
    <td class="tg-0lax">0.0719</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1031</td>
    <td class="tg-0lax">0.1094</td>
    <td class="tg-0lax">0.0357</td>
    <td class="tg-0lax">0.0558</td>
    <td class="tg-0lax">0.0724</td>
    <td class="tg-0lax">0.1237</td>
    <td class="tg-0lax">0.1236</td>
    <td class="tg-0lax">0.1072</td>
    <td class="tg-0lax">0.1077</td>
    <td class="tg-0lax">0.0759</td>
    <td class="tg-0lax">0.0870</td>
    <td class="tg-0lax">0.1073</td>
  </tr>
  <tr>
    <td class="tg-1wig">Japão</td>
    <td class="tg-0lax">0.0921</td>
    <td class="tg-0lax">0.1466</td>
    <td class="tg-0lax">0.0707</td>
    <td class="tg-0lax">0.0310</td>
    <td class="tg-0lax">0.4182</td>
    <td class="tg-0lax">0.1107</td>
    <td class="tg-0lax">0.1635</td>
    <td class="tg-0lax">0.2892</td>
    <td class="tg-0lax">0.0491</td>
    <td class="tg-0lax">0.0644</td>
    <td class="tg-0lax">0.0621</td>
    <td class="tg-0lax">0.1031</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0537</td>
    <td class="tg-0lax">0.0528</td>
    <td class="tg-0lax">0.0651</td>
    <td class="tg-0lax">0.0831</td>
    <td class="tg-0lax">0.0927</td>
    <td class="tg-0lax">0.0679</td>
    <td class="tg-0lax">0.1534</td>
    <td class="tg-0lax">0.0386</td>
    <td class="tg-0lax">0.1049</td>
    <td class="tg-0lax">0.0596</td>
    <td class="tg-0lax">0.1089</td>
  </tr>
  <tr>
    <td class="tg-1wig">Coreia do Sul</td>
    <td class="tg-0lax">0.0778</td>
    <td class="tg-0lax">0.1303</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.0433</td>
    <td class="tg-0lax">0.3305</td>
    <td class="tg-0lax">0.1180</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.2965</td>
    <td class="tg-0lax">0.1192</td>
    <td class="tg-0lax">0.0742</td>
    <td class="tg-0lax">0.0432</td>
    <td class="tg-0lax">0.1094</td>
    <td class="tg-0lax">0.0537</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0883</td>
    <td class="tg-0lax">0.0726</td>
    <td class="tg-0lax">0.0863</td>
    <td class="tg-0lax">0.2317</td>
    <td class="tg-0lax">0.1205</td>
    <td class="tg-0lax">0.1145</td>
    <td class="tg-0lax">0.1041</td>
    <td class="tg-0lax">0.1254</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.1473</td>
  </tr>
  <tr>
    <td class="tg-1wig">Países Baixos</td>
    <td class="tg-0lax">0.0313</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0235</td>
    <td class="tg-0lax">0.1267</td>
    <td class="tg-0lax">0.6036</td>
    <td class="tg-0lax">0.0807</td>
    <td class="tg-0lax">0.1170</td>
    <td class="tg-0lax">0.1888</td>
    <td class="tg-0lax">0.1351</td>
    <td class="tg-0lax">0.0472</td>
    <td class="tg-0lax">0.0809</td>
    <td class="tg-0lax">0.0357</td>
    <td class="tg-0lax">0.0528</td>
    <td class="tg-0lax">0.0883</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0457</td>
    <td class="tg-0lax">0.0423</td>
    <td class="tg-0lax">0.0785</td>
    <td class="tg-0lax">0.0317</td>
    <td class="tg-0lax">0.0859</td>
    <td class="tg-0lax">0.0276</td>
    <td class="tg-0lax">0.1009</td>
    <td class="tg-0lax">0.0683</td>
    <td class="tg-0lax">0.0821</td>
  </tr>
  <tr>
    <td class="tg-1wig">Peru</td>
    <td class="tg-0lax">0.0268</td>
    <td class="tg-0lax">0.0704</td>
    <td class="tg-0lax">0.0650</td>
    <td class="tg-0lax">0.0698</td>
    <td class="tg-0lax">0.5808</td>
    <td class="tg-0lax">0.0137</td>
    <td class="tg-0lax">0.0521</td>
    <td class="tg-0lax">0.2027</td>
    <td class="tg-0lax">0.0598</td>
    <td class="tg-0lax">0.0068</td>
    <td class="tg-0lax">0.0184</td>
    <td class="tg-0lax">0.0558</td>
    <td class="tg-0lax">0.0651</td>
    <td class="tg-0lax">0.0726</td>
    <td class="tg-0lax">0.0457</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0328</td>
    <td class="tg-0lax">0.1594</td>
    <td class="tg-0lax">0.0626</td>
    <td class="tg-0lax">0.0530</td>
    <td class="tg-0lax">0.0767</td>
    <td class="tg-0lax">0.0290</td>
    <td class="tg-0lax">0.0820</td>
    <td class="tg-0lax">0.0257</td>
  </tr>
  <tr>
    <td class="tg-1wig">Portugal</td>
    <td class="tg-0lax">0.0146</td>
    <td class="tg-0lax">0.0556</td>
    <td class="tg-0lax">0.0670</td>
    <td class="tg-0lax">0.0839</td>
    <td class="tg-0lax">0.6312</td>
    <td class="tg-0lax">0.0647</td>
    <td class="tg-0lax">0.0420</td>
    <td class="tg-0lax">0.2205</td>
    <td class="tg-0lax">0.1113</td>
    <td class="tg-0lax">0.0459</td>
    <td class="tg-0lax">0.0456</td>
    <td class="tg-0lax">0.0724</td>
    <td class="tg-0lax">0.0831</td>
    <td class="tg-0lax">0.0863</td>
    <td class="tg-0lax">0.0423</td>
    <td class="tg-0lax">0.0328</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1745</td>
    <td class="tg-0lax">0.0625</td>
    <td class="tg-0lax">0.0453</td>
    <td class="tg-0lax">0.0908</td>
    <td class="tg-0lax">0.0611</td>
    <td class="tg-0lax">0.0931</td>
    <td class="tg-0lax">0.0314</td>
  </tr>
  <tr>
    <td class="tg-1wig">Rússia</td>
    <td class="tg-0lax">0.1832</td>
    <td class="tg-0lax">0.1564</td>
    <td class="tg-0lax">0.0532</td>
    <td class="tg-0lax">0.2046</td>
    <td class="tg-0lax">0.8489</td>
    <td class="tg-0lax">0.2226</td>
    <td class="tg-0lax">0.2765</td>
    <td class="tg-0lax">0.3063</td>
    <td class="tg-0lax">0.2188</td>
    <td class="tg-0lax">0.1446</td>
    <td class="tg-0lax">0.2038</td>
    <td class="tg-0lax">0.1237</td>
    <td class="tg-0lax">0.0927</td>
    <td class="tg-0lax">0.2317</td>
    <td class="tg-0lax">0.0785</td>
    <td class="tg-0lax">0.1594</td>
    <td class="tg-0lax">0.1745</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0920</td>
    <td class="tg-0lax">0.2830</td>
    <td class="tg-0lax">0.0342</td>
    <td class="tg-0lax">0.2555</td>
    <td class="tg-0lax">0.1539</td>
    <td class="tg-0lax">0.2438</td>
  </tr>
  <tr>
    <td class="tg-1wig">Espanha</td>
    <td class="tg-0lax">0.0362</td>
    <td class="tg-0lax">0.0539</td>
    <td class="tg-0lax">0.0693</td>
    <td class="tg-0lax">0.1453</td>
    <td class="tg-0lax">0.7006</td>
    <td class="tg-0lax">0.0879</td>
    <td class="tg-0lax">0.0858</td>
    <td class="tg-0lax">0.3947</td>
    <td class="tg-0lax">0.1537</td>
    <td class="tg-0lax">0.0583</td>
    <td class="tg-0lax">0.0847</td>
    <td class="tg-0lax">0.1236</td>
    <td class="tg-0lax">0.0679</td>
    <td class="tg-0lax">0.1205</td>
    <td class="tg-0lax">0.0317</td>
    <td class="tg-0lax">0.0626</td>
    <td class="tg-0lax">0.0625</td>
    <td class="tg-0lax">0.0920</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.0725</td>
    <td class="tg-0lax">0.0254</td>
    <td class="tg-0lax">0.1403</td>
    <td class="tg-0lax">0.1252</td>
    <td class="tg-0lax">0.0915</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suécia</td>
    <td class="tg-0lax">0.0214</td>
    <td class="tg-0lax">0.0455</td>
    <td class="tg-0lax">0.1201</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.7566</td>
    <td class="tg-0lax">0.0524</td>
    <td class="tg-0lax">0.0206</td>
    <td class="tg-0lax">0.2911</td>
    <td class="tg-0lax">0.1645</td>
    <td class="tg-0lax">0.0663</td>
    <td class="tg-0lax">0.0467</td>
    <td class="tg-0lax">0.1072</td>
    <td class="tg-0lax">0.1534</td>
    <td class="tg-0lax">0.1145</td>
    <td class="tg-0lax">0.0859</td>
    <td class="tg-0lax">0.0530</td>
    <td class="tg-0lax">0.0453</td>
    <td class="tg-0lax">0.2830</td>
    <td class="tg-0lax">0.0725</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1327</td>
    <td class="tg-0lax">0.1115</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.0723</td>
  </tr>
  <tr>
    <td class="tg-1wig">Suíça</td>
    <td class="tg-0lax">0.0812</td>
    <td class="tg-0lax">0.0995</td>
    <td class="tg-0lax">0.0533</td>
    <td class="tg-0lax">0.1335</td>
    <td class="tg-0lax">0.6211</td>
    <td class="tg-0lax">0.1224</td>
    <td class="tg-0lax">0.1607</td>
    <td class="tg-0lax">0.3474</td>
    <td class="tg-0lax">0.1445</td>
    <td class="tg-0lax">0.0632</td>
    <td class="tg-0lax">0.1104</td>
    <td class="tg-0lax">0.1077</td>
    <td class="tg-0lax">0.0386</td>
    <td class="tg-0lax">0.1041</td>
    <td class="tg-0lax">0.0276</td>
    <td class="tg-0lax">0.0767</td>
    <td class="tg-0lax">0.0908</td>
    <td class="tg-0lax">0.0342</td>
    <td class="tg-0lax">0.0254</td>
    <td class="tg-0lax">0.1327</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1652</td>
    <td class="tg-0lax">0.0918</td>
    <td class="tg-0lax">0.1397</td>
  </tr>
  <tr>
    <td class="tg-1wig">Turquia</td>
    <td class="tg-0lax">0.0826</td>
    <td class="tg-0lax">0.1418</td>
    <td class="tg-0lax">0.1100</td>
    <td class="tg-0lax">0.0830</td>
    <td class="tg-0lax">0.6705</td>
    <td class="tg-0lax">0.0256</td>
    <td class="tg-0lax">0.1151</td>
    <td class="tg-0lax">0.1517</td>
    <td class="tg-0lax">0.0292</td>
    <td class="tg-0lax">0.0446</td>
    <td class="tg-0lax">0.0476</td>
    <td class="tg-0lax">0.0759</td>
    <td class="tg-0lax">0.1049</td>
    <td class="tg-0lax">0.1254</td>
    <td class="tg-0lax">0.1009</td>
    <td class="tg-0lax">0.0290</td>
    <td class="tg-0lax">0.0611</td>
    <td class="tg-0lax">0.2555</td>
    <td class="tg-0lax">0.1403</td>
    <td class="tg-0lax">0.1115</td>
    <td class="tg-0lax">0.1652</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1053</td>
    <td class="tg-0lax">0.0319</td>
  </tr>
  <tr>
    <td class="tg-1wig">Reino Unido</td>
    <td class="tg-0lax">0.1109</td>
    <td class="tg-0lax">0.1662</td>
    <td class="tg-0lax">0.0990</td>
    <td class="tg-0lax">0.0824</td>
    <td class="tg-0lax">0.4789</td>
    <td class="tg-0lax">0.1278</td>
    <td class="tg-0lax">0.1935</td>
    <td class="tg-0lax">0.2098</td>
    <td class="tg-0lax">0.1040</td>
    <td class="tg-0lax">0.0790</td>
    <td class="tg-0lax">0.0952</td>
    <td class="tg-0lax">0.0870</td>
    <td class="tg-0lax">0.0596</td>
    <td class="tg-0lax">0.0397</td>
    <td class="tg-0lax">0.0683</td>
    <td class="tg-0lax">0.0820</td>
    <td class="tg-0lax">0.0931</td>
    <td class="tg-0lax">0.1539</td>
    <td class="tg-0lax">0.1252</td>
    <td class="tg-0lax">0.1644</td>
    <td class="tg-0lax">0.0918</td>
    <td class="tg-0lax">0.1053</td>
    <td class="tg-0lax">0.0000</td>
    <td class="tg-0lax">0.1764</td>
  </tr>
  <tr>
    <td class="tg-1wig">Estados Unidos</td>
    <td class="tg-0lax">0.0410</td>
    <td class="tg-0lax">0.1019</td>
    <td class="tg-0lax">0.1111</td>
    <td class="tg-0lax">0.1181</td>
    <td class="tg-0lax">0.6572</td>
    <td class="tg-0lax">0.0244</td>
    <td class="tg-0lax">0.0691</td>
    <td class="tg-0lax">0.2509</td>
    <td class="tg-0lax">0.0680</td>
    <td class="tg-0lax">0.0411</td>
    <td class="tg-0lax">0.0578</td>
    <td class="tg-0lax">0.1073</td>
    <td class="tg-0lax">0.1089</td>
    <td class="tg-0lax">0.1473</td>
    <td class="tg-0lax">0.0821</td>
    <td class="tg-0lax">0.0257</td>
    <td class="tg-0lax">0.0314</td>
    <td class="tg-0lax">0.2438</td>
    <td class="tg-0lax">0.0915</td>
    <td class="tg-0lax">0.0723</td>
    <td class="tg-0lax">0.1397</td>
    <td class="tg-0lax">0.0319</td>
    <td class="tg-0lax">0.1764</td>
    <td class="tg-0lax">0.0000</td>
  </tr>
</table>

</center>
***Tabela 4**: Matriz de distâncias KL entre as distribuições do primeiro dígito do número de casos de COVID-19 dos países analisados, arredondado para quatro casas decimais*

A matriz acima não tem uma interpretação prática muito imediata, então aplicamos um algoritmo de clusterização para definir quais são os países que se parecem mais entre si -- pares de países com grande distância KL são menos parecidos entre si que pares de países com baixa distância KL. O algoritmo escolhido foi o DBSCAN, que cria *clusters* para cada ponto da amostra com base no número mínimo de pontos em cada *cluster* ($$mp$$) e na distância máxima que um ponto pode estar em relação a outro ponto do mesmo *cluster* ($$\varepsilon$$). <ins>Pontos que não possui pelo menos $$mp$$ pontos dentro do raio de $$\varepsilon$$ são classificados como *outliers* sem *cluster*</ins>. Um bom material introdutório sobre o DBSCAN pode ser encontrado em [aqui](https://medium.com/@elutins/dbscan-what-is-it-when-to-use-it-how-to-use-it-8bd506293818).


Uma das vantagens do DBSCAN é o fato de o número de *cluster* ser definido automaticamente em vez de escolhido pelo usuário, tornando-o um bom instrumento para a detecção de anomalias. Para este exercício, utilizamos $$mp=3$$ e $$\varepsilon$$ como sendo a média das distâncias KL entre os países acrescida de três desvios-padrão amostrais. O resultado dessa clusterização é mais fácil de ser interpretada:

<center>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">País</th>
    <th class="tg-7btt">Cluster</th>
  </tr>
  <tr>
    <td class="tg-c3ow">Áustria</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Bélgica</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Brasil</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Canadá</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">China</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">França</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Alemanha</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Índia</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Irã</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Irlanda</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Israel</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Itália</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Japão</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Coreia do Sul</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Países Baixos</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Peru</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Portugal</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Rússia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Espanha</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Suécia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Suíça</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Turquia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Reino Unido</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Estados Unidos</td>
    <td class="tg-c3ow">1</td>
  </tr>
</table>

</center>
***Tabela 5**: Clusterização por DBSCAN das distâncias KL entre as distribuições do primeiro dígito do número de casos de COVID-19 dos países analisados*


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">País</th>
    <th class="tg-7btt">Cluster</th>
  </tr>
  <tr>
    <td class="tg-c3ow">Áustria</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Bélgica</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Brasil</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Canadá</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">China</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">França</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Alemanha</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Índia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Irã</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Irlanda</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Israel</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Itália</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Japão</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Coreia do Sul</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Países Baixos</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Peru</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Portugal</td>
    <td class="tg-7btt">Outlier</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Rússia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Espanha</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Suécia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Suíça</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Turquia</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Reino Unido</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Estados Unidos</td>
    <td class="tg-c3ow">1</td>
  </tr>
</table>
***Tabela 6**: Clusterização por DBSCAN das distâncias KL entre as distribuições do primeiro dígito do número de óbitos por COVID-19 dos países analisados*

Novamente, assim como sugerido pela inspeção visual e pelos testes de hipóteses supramencionados, os resultados indicam que os dados da China apresentam padrões distintos da grande parte dos outros países mais afetados pela pandemia, os quais mostraram padrões similares de infectividade e letalidade: o algoritmo retornou apenas um *cluster*, classificando China como "*outlier*" tanto para os dados de casos quanto para os dados de óbitos. De fato, os países categorizados como "*cluster* 1" parecem seguir melhor a distribuição da Lei de Benford.

Apesar de a China ser o local de origem da doença, dada a grande divergência entre seus dados e os dos demais locais, os dados chineses devem ser utilizados com especial cautela para análises como a estimação dos parâmetros médicos (*basic reproducing number*, *serial interval* e *case-fatality ratio*, por exemplo), a modelagem da dispersão geográfica do patógeno, diagnósticos da eficácia de cenários de intervenção, etc.

## Considerações finais

A medida que à pandemia atinge mais e mais pessoas e traz impactos cada vez mais profundos nas atividades econômicas e na vida social a nível global, discutir a subnotificação dos dados do COVID-19 se torna especialmente relevante, tanto para a avaliação da severidade do cenário quanto para a proposição de soluções e meios de enfrentamento da crise. Dado que acadêmicos, pesquisadores e formuladores de políticas públicas do mundo todo estão se dedicando a essa causa, ter em mãos dados precisos e confiáveis é de fundamental importância, pois a qualidade dos dados condiciona diretamente na qualidade de todas as análises que deles derivam. Como diz o ditado: **"*Garbage in, garbage out!*"**


---

**Às vezes, "coincidência" é apenas um jeito conveniente de alguém se esquivar dos fatos que ele não conseguiu explicar.**

![tsuru](https://cdn.shopify.com/s/files/1/1218/4290/products/GOLD13391_46faec9e-ca27-48f7-997d-e152c18f4349_800x.JPG?v=1515022654)
