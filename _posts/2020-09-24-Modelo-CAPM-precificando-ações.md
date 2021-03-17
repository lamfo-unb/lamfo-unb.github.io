---
layout: post
title: Modelo CAPM: precificando ações
lang: pt
header-img: img/manipulacao_data.table/img_data.table.png
date: 2020-09-24 23:59:07
tags: [CAPM, Machine Learning, Finanças]
author: Neuremberg Matos, Sarah  e Alícia Isaias Macedo
comments: true
---

# Modelo CAPM: precificando ações

O CAPM (Capital Asset Pricing Model) é um modelo de precificação de ativos que relaciona o risco e retorno dos ativos. Esse modelo é relativamente novo, ao se comparar quando as ações surgiram.

O CAPM pode ser expresso por meio da equação abaixo:

$$
   R_i = R_f + \beta_i(R_m – R_f)
$$

Em que:

- $R_i$ = taxa  de retorno esperada do ativo.
- $R_m$ = taxa  de retorno esperada da carteira de mercado
- $R_f$ = taxa  de retorno de ativo sem risco
- $\beta_i$ =  coeficiente  beta  do  ativo

Assim, segundo o CAPM, o retorno esperado de ativo $i$ é taxa livre de risco $R_f$ mais um prêmio que é ponderado por um coeficiente $\beta_i$. Esse coeficiente incorpora uma medida de risco para o ativo $i$.

### Interpretando o $\beta$

De modo geral, o $\beta$ mede a sensibilidade do retorno do ativo $i$ a variações dos retornos da carteira de mercado. Isto é, o beta de um ativo é a variação percentual esperada no retorno do ativo, dado a variação de $1\%$ no retorno da carteira de mercado.

Para uma compreensão mais profunda sobre o papel do $\beta$ é necessário relacioná-lo com os conceitos de risco sistêmico e risco idiossincrático. O risco de um ativo é dividido em duas partes: o idiossincrático, que é o risco associado a fatores que afetam apenas o ativo em questão; e o rico de sistêmico, que se devem a fatores que afetam todos ativos do mercado. Maiores detalhes podem ser encontrados nesse [post](https://lamfo-unb.github.io/2020/01/22/Markowitz-selecao-carteiras/).

Os investidores podem se livrar do risco idiossincrático através da diversificação, mas não podem se livrar do risco de mercado. Por essa razão, o risco idiossincrático e o risco sistêmico são conhecidos também como risco diversificável e risco não diversificável.

A frase que diz: "Quanto maior o risco, maior o retorno esperado" não está totalmente correta, uma vez que apenas o risco não diversificável é renumerado. Pois, caso o risco diversificável fosse renumerado, os agentes poderiam realizar arbitragem ao receberem a renumeração por esse tipo de risco e livrarem-se dele através da diversificação. Dessa forma, a frase mais correta seria: "Quanto maior o risco não diversificável, maior o retorno esperado".

A razão para falar disso se deve à definição de carteira de mercado. Como discutido nesse [post](https://lamfo-unb.github.io/2020/01/22/Markowitz-selecao-carteiras/), a carteira de mercado é uma carteira pertencente a fronteira eficiente, sendo a mais diversificável possível. Portanto, ela contém apenas risco não diversificável.

Considerando isso, o $\beta$ de um ativo pode ser interpretado como a sensibilidade do retorno do ativo ao risco de mercado. Portanto, quanto maior o $\beta$ maior a renumeração esperada para o ativo.

Formalmente, o $\beta$ é dado por:

$$
\beta_i = \frac{Cov(R_i, R_m)}{Var(R_m)}
$$

Colocando o $\beta$ em termos de variâncias e correlações:

$$
\beta_i = \frac{\sigma_i \sigma_m \rho_{im}}{\sigma_m^2}
$$

Portanto,

$$
\beta_i = \frac{\sigma_i \rho_{im}}{\sigma_m}
$$

Tal que, $\sigma_i$ é a variância dos retornos do  ativo $i$, $\sigma_m$ é a variância dos retornos da carteira de mercado e $\rho_{im}$ é a correlação entre os retornos do ativo $i$ e os retornos da carteira de mercado.

Na relação acima, o numerador $\sigma_i \rho_{im}$ é parte do risco do ativo que é comum com o risco de mercado e o denominador $\sigma_m$ é o risco de mercado. Assim, o $\beta_i$ é o risco sistemático do ativo $i$ ponderando pelo risco sistemático de todo o mercado $\sigma_m$. Desta forma, se $\beta_i > 1$, então o ativo $i$ possui mais risco de mercado que a carteira de mercado e, portanto, deve ter maior renumeração do que ela e vice-versa.

Além disso, o retorno do ativo tem relação linear com $\beta$. Podemos representar o retorno esperado como uma função linear do $\beta$:

<figure>
  <img src="https://i.imgur.com/EEGev1t.png"
      style="display: block; margin: auto; width: 70%; height: 70%;">
</figure>

A reta acima é conhecida como *Security Marketing Line (SML)*, ela mostra a relação entre $\beta$ e o retorno esperado. Para $\beta = 1$, o ativo correspondente tem mesmo risco de mercado que a carteira de mercado, portanto possui a mesma renumeração $R_m$. Além disso, a inclinação da reta é dada por $(R_m - R_f)$, que é a renumeração excedente em relação à taxa livre de risco $R_f$.

### Pressupostos do CAPM

O CAPM possui um conjunto fundamentos sobre a hipótese de mercados eficientes. Dentre eles, espera-se que os investidores sejam racionais e que estão dispostos a aceitar um prêmio pelo risco como medida de compensação. Além disso, são avessos ao risco, ou seja, desejam minimizar o risco, tudo mais constante.

Para que o mercado de capitais seja eficiente, espera-se que seja competitivo, exista um grande número de investidores que não têm poder para influenciar individualmente o mercado. E os preços reflitam todas as informações disponíveis e que os investidores tenham pleno acesso a essas informações. Ademais, presume-se que não há impostos sobre as transações, e se houver, que não gerem distorções.

O modelo também supõe que o retorno esperado do ativo e do mercado sejam previsíveis, ou calculáveis. Note que, na prática, é praticamente impossível atender todas essas premissas. Entretanto o modelo é uma ferramenta extensivamente utilizada e apresenta um referencial teórico de grande importância na avaliação de risco e retorno.


### Usos do modelo CAPM

O modelo *CAPM* possui várias aplicações, entretanto, ele é frequentemente usando em duas situações: precificar e comparar ativos e estimar o custo de capital próprio.

No primeiro caso, o modelo CAPM fornece uma taxa de retorno esperada para o ativo. Suponha que um investidor deseja escolher um conjunto de ações para investir, um critério de escolha é usar o retorno esperado das ações. O CAPM fornece um meio de estimar esses retornos, além disso, fornece uma medida de risco de mercado para ação.

Uma medida de risco frequentemente utilizado para comparar a performance entre ativos é o índice de *Sharpe*. Ele mede a taxa de retorno por unidade de risco, assim dado uma carteira *P*, o índice de sharpe é calculado como:

$$
I_s(P) = \frac{R_m - R_p}{\sigma_p}
$$

Assim, quanto maior o $I_s$, melhor o desempenho da carteira ponderado pelo o seu próprio risco. Entretanto, essa medida não é mais acurada, uma vez que ela considera risco não renumerável. Dessa forma, o $\beta$ acaba sendo uma medida melhor.

No segundo caso, o CAPM é utilizado para estimar o custo de capital próprio de uma empresa.

O custo de capital próprio de uma empresa pode ser definido como a taxa de retorno que os acionistas esperam ganhar em troca por investir os seus recursos na empresa. O custo de capital $(K)$ própria é estimado como:

$$
K = R_f + \beta(R_m - R_f)
$$

Sendo facilmente estimado quando a empresa possui ações listadas no mercado.

Além disso, a taxa $K$ fornece uma forma de avaliar a viabilidade de projetos com capital próprio na empresa. O valor presente líquido (VPL) é uma técnica largamente empregado para avaliar a viabilidade de projetos, o critério de avaliação é se $VPL > 0$ o projeto é viável. Essa técnica consiste em descontar o valor dos fluxos de caixa futuros por uma taxa apropriada, que reflete a risco do projeto e a taxa $K$ é justamente isso. De forma mais abrangente, o VPL pode ser usado para estimar valor total da empresa.

Um empecilho para a aplicação do CAPM é a necessidade de uma carteira de mercado, que relativamente difícil de ser calculada. Na prática, os investidores usam uma *proxy* para essa carteira. Na Bovespa, o índice Ibovespa é bastante utilizado como *proxy*.


### Aplicando modelo CAPM na *Bovespa*

O primeiro passo logo após importar as bibliotecas foi incluir os dados. As empresas escolhidas para essa demonstração didática foram:

- CMIG3 – Cemig (energia elétrica)
- ELET3 – Eletrobrás (energia elétrica)
- ITUB4 – Itaú Unibanco (setor bancário)
- LREN3 – Lojas Renner (Varejista de roupas)

Como carteira de mercado, será considerado o índice Bovespa (BVSP).


```python
#Importar as bibliotecas que serão utilizadas
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

#Criar os Dataframes dos ativos analisados
#Data frames de 01-07-2019 a 30-07-2020
#Criar os Dataframes dos ativos analisados
'Data frames de 01-07-2019 a 30-07-2020'
dir_data = "../data/"
BVSP_df = pd.read_csv(dir_data+'BVSP.csv')
CMIG_df = pd.read_csv(dir_data+'CMIG3.SA.csv')
ELET_df = pd.read_csv(dir_data+'ELET3.SA.csv')
ITUB_df = pd.read_csv(dir_data+'ITUB4.SA.csv')
LREN_df = pd.read_csv(dir_data+'LREN3.SA.csv')

#Grafico com evolução do preço BVSP
Dias=list(range(0,len(BVSP_df)))
Preco=plt.plot(Dias,BVSP_df['Adj Close'])
plt.title("Preço BVSP X Tempo (dias)")
```
![](https://i.imgur.com/U5f8xk3.png)

Os dados foram importados do Yahoo! Finanças, do período de 01/07/2019 a 30/07/2020. Como o arquivo não incluí o retorno em porcentagem, a seguir foi adicionada uma coluna de retornos, a qual utilizou como base a coluna “Adj Close” que é fornecida pelo arquivo do Yahoo. “Adj Close” é o preço de fechamento somado aos dividendos que podem ter sido distribuídos no dia por ação, incluindo então esta outra forma de rendimento.


```python
#Adicionar uma coluna de retornos que é (new_Adj close / old_ Adj close) -1
BVSP_df ['Returns'] = BVSP_df['Adj Close'].pct_change()
CMIG_df ['Returns'] = CMIG_df['Adj Close'].pct_change()
ELET_df ['Returns'] = ELET_df['Adj Close'].pct_change()
ITUB_df ['Returns'] = ITUB_df['Adj Close'].pct_change()
LREN_df ['Returns'] = LREN_df['Adj Close'].pct_change()
```

A fórmula do retorno foi:

$$\frac{Adj\ close_t}{Adj\ close_{t-1}} – 1$$

Como o cálculo possui o preço do dia anterior no divisor, a data mais antiga não terá um valor definido, e por isso será ignorada essa linha.


```python
#Ignorar os NaN
BVSP_returns = BVSP_df['Returns'].values[1:]
CMIG_returns = CMIG_df['Returns'].values[1:]
ELET_returns = ELET_df['Returns'].values[1:]
ITUB_returns = ITUB_df['Returns'].values[1:]
LREN_returns = LREN_df['Returns'].values[1:]
```

Para continuar, serão considerados apenas os retornos percentuais calculados anteriormente.

É possível visualizar no seguinte exemplo como ocorre a regressão linear entre os retornos do ativo e retornos do Bovespa:


```python
Retornos = [BVSP_returns, CMIG_returns, ELET_returns, ITUB_returns, LREN_returns]
#Usando SeaBorn para visualizar a regressão linear (Grafico de pontos)
regressao_linear=plt.figure(figsize=(16,6))
ax = sns.regplot(BVSP_returns, CMIG_returns, scatter_kws={"color":"blue"},
                 line_kws={"color":"orange"})
ax.tick_params(axis = 'x', color = 'black')
ax.set(xlabel="BVSP Retornos", ylabel = "CMIG Retornos", title = 'Retornos BVSP X CMIG',
       )
plt.show()
```


![](https://i.imgur.com/vLPUket.png)



A seguir, o cálculo da matriz de variância e covariância, que será utilizada para calcular o Beta de cada ativo.
Para este exemplo, foi considerado o valor de 0.0054% de retorno diária, tendo a taxa Selic como renda fixa.


```python
#CAPM: ERi  = RFR + Bi * (ERm - RFR)

"""Lembrar que:
    Bi = 0, o ativo não se relaciona com o mercado
    Bi = 1, ativo perfeitamente correlacionado com o mercado
    Bi > 1, ativo mais volátil que o mercado
    Bi <1, ativo menos volátil que o mercado  """

#Matriz de variancia e covariancia
vcov=np.cov(Retornos)

#Cálculo dos betas
Tamanho=5
variancias=np.zeros(Tamanho)
for n in range(0,Tamanho): #Captura as variancias pra calcular
    variancias[n]=vcov[n][n]
beta=np.zeros(Tamanho)
for n in range(0,Tamanho): #seleciona todos da primeira coluna de vcov
      beta[n]=vcov[n][0]/variancias[n]

rm = np.mean(BVSP_returns)

rf = 0.000054  #taxa selic atual
ri = np.zeros(Tamanho)
for n in range(0,Tamanho):
    ri[n] = (rf + beta[n] * (rm - rf))*100
```

Os resultados de retorno esperado são pequenos, porém é preciso lembrar que foi calculado para o período diário.Dado o nível de risco e considerando as relações dos ativos com o mercado, os resultados foram:


```python
# Cria um DataFrame com os retornos
df = pd.DataFrame(data=ri,columns=[ 'Retorno Esperado'])
dfnomes= pd.DataFrame(data=Nomes,columns=['Papel'])
df2= dfnomes.join(df,how='right')

#Gera um grafico de retornos esperados
retorno_esperado_graf=plt.figure()
retorno_esperado_graf=plt.bar(df2['Papel'],df2['Retorno Esperado'])
plt.title("Retorno esperado de cada papel")

print(df2)
```

      Papel  Retorno Esperado
    0  BVSP          0.049422
    1  CMIG          0.031297
    2  ELET          0.025346
    3  ITUB          0.040241
    4  LREN          0.031604

![](https://i.imgur.com/3oD63TJ.png)


## Referências

BERK, Jonathan B.;DEMARZO, Peter. Corporate finance. 3rd ed. Peason, 2014.
JORDAN, Ross W. Fundamentos de Administração Financeira. 9ª ed. AMGH, 2013.
