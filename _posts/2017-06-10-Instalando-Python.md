---
layout: post
title: Instalando Python para Aprendizado de Máquina
lang: pt
header-img: 
date: 2017-06-10 10:19:07
tags: [python,tensorflow]
author: Matheus Facure
comments: true
---

UPDATE: 18/06/2017

## Conteúdo

0. [Requisitos](#req)
1. [Instalando Python](#python)
2. [Instalando TensorFlow](#tf)
3. [Instalando Git](#git)
4. [Testando](#test)
5. [Considerações Finais](#fim)

## Requisitos computacionais <a name="req"></a>

Para seguir com esse tutorial de instalação, você precisa de um computador com Windows 64 bits e pelo menos 4 GB de RAM. Mas mais do que isso, se você planeja seguir adiante com estudos ou aplicações de aprendizado de máquina, é recomendável ter pelo menos 8 GB de RAM e um processador minimamente descente, como um Intel i5 ou outro processador com pelo menos quatro núcleos (*quad core*).

## Instalando Python <a name="python"></a>

<img src="/img/python.png" alt="python" class="img-responsive thumbnail pull-right" style="margin-left:3%; width:45%;">

No momento, Python a linguagem de programação preferida pelos cientistas de dados, especialmente para aprendizado de máquina. Mas mais do que isso, Python tem uma sintaxe extremamente simples e legível, o que te permite focar mais no conteúdo do seu programa do que em como escrevê-lo. Aqui, vamos ensinar como instalar a distribuição [Anaconda](https://www.continuum.io/downloads), um pacote de programas que inclui o Python mais uma série de extensões utilizadas em ciência de dados e aprendizado de máquina (como Numpy, Pandas, Jupyter Notebooks e Scikit Learn).

Antes de mais nada, [certifique-se de que seu sistema operacional é 64 bits](https://support.microsoft.com/pt-br/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64-bit-version-of-the-windows-operating-system). Infelizmente, o tutorial a seguir só funciona na versão 64 bits do Windows, então uma vez que você tenha confirmado essa versão, podemos prosseguir com a instalação. Agora, vá em [https://www.continuum.io/downloads](https://www.continuum.io/downloads) e baixe o Anaconda para Windows, versão 64-bit. Ao clicar no botão do download, um arquivo executável será baixado. 

Clique no arquivo executável para inicializar a instalação. Prossiga com as opções padrões, mas certifique-se de que a caixa com a opção `Add Anaconda to PATH` esteja selecionada. Esse processo de instalação pode demorar um pouco. Quando a instalação concluir, podemos testar se tudo deu certo.

Abra a prompt de comando. Antes de prosseguir, vamos nos familiarizar com ela um pouco. A propt de comando é uma forma alternativa de interagir com o seu computador. Da mesma forma que você navega pelas pastas e arquivos clicando com o mouse, você pode fazer isso na linha de comando. Por padrão, a linha de comando abre no seu `Home`, o local onde ficam suas pastas de imagens, documentos, downloads, etc. Após abrir o prompt de comando, digite `ls` para ver o que está na sua pasta, naquele local. Você verá escrito as pastas de arquivos do seu `Home`. O comando `ls` lista o que está na sua pasta (ou diretório) corrente.

Agora, digite `cd Desktop` para navegar ao Desktop do seu computador. Se quiser, digite `ls` para mostrar quais arquivos estão no seu Desktop. O comando `cd` (change directory) troca o diretório (ou pasta) corrente. O comando `cd ..` sobe um nível nas pastas do seu computador. Se você digitar isso na prompt de comando enquanto estiver no Desktop, ele te levará de volta ao `Home`, que é a o diretório um nível acima do `Desktop`. Volte para o `HOME`.

No `HOME`, digite `python` na prompt de comando. Isso iniciará o modo interativa do Python. Execute algumas operações matemáticas como `2+2` e veja o resultado sendo calculado pelo Python. Quando terminar, digite `quit()` para sair da versão interativa do Python.

Para finalizar, digite `jupyter notebook` na linha de comando. Isso inicializará o Jupyter, um programa que executa códigos em Python. Uma nova aba será aberta no seu browser (OBS: funciona apenas com Chrome ou Firefox) e nela você verá a interface do Jupyter. Caso essa aba não abra, na prompt de comando, procure um link que deve começar com `http://localhost:8889/`. Copie e cole esse link em uma nova aba do seu browser para abrir a interface do Jupyter. Nessa interface, você poderá escrever *notebooks* que intercalam código em Python intercalado com comentários, se assim desejar (veja um [exemplo](https://github.com/matheusfacure/Tutoriais-de-AM/blob/master/Redes%20Neurais%20Artificiais/DeepANN.ipynb)). Para sair do notebook, feche a aba no seu browser, volte a prompt de comando e aperte CTRL+C, seguido de y e enter. CTRL+C é um comando para fechar programas que estão rodando na sua prompt de comando; você terá que confirmar o fechamento com y (*yes*), seguido de enter.

Para aprender mais sobre a linha de comando, sugiro este [link](https://learnpythonthehardway.org/book/appendixa.html). Para aprender mais sobre o Jupyter, sugiro este [link](http://jupyter.readthedocs.io/en/latest/content-quickstart.html).

## Instalando TensorFlow <a name="tf"></a>

<img src="/img/tf.jpg" alt="python" class="img-responsive thumbnail pull-right" style="margin-left:3%; width:45%;">

O [TensorFlow](https://www.tensorflow.org/) é um programa de computação numérica, que tem sua versão principal em Python. Nós usamos o TensorFlow para programas de *deep learning*, devido a sua eficiência computacional e abundância de comandos facilitadores para construção e treinamento de redes neurais. Antes de instalar o TensorFlow, vamos criar um ambiente Python. Nesse ambiente, todas as extensões (ou pacotes) que vem no Anaconda estarão presentes, mas o que instalarmos nele não será acessível de fora dele. Isso evita que a instalação de um pacote novo possa prejudicar o funcionamento dos pacotes já instalados. 

Para criar um ambiente novo, digite `conda create -n tensorflow` na linha de comando. Confirme a criação do ambiente digitando y (*yes*), seguido de enter. Agora, digite `activate tensorflow` para entrar nesse ambiente. Note que a sua linha de comando mudará para incluir `(tensorflow)` antes do nome do seu diretório corrente. Para sair do ambiente, basta digitar `deactivate`.

Finalmente, para instalar o TensorFlow, entre no ambiente recém criado e digite:

```bash
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-1.2.0-cp36-cp36m-win_amd64.whl
```
Dica: CTRL+C e CTRL+V não funciona na linha de comando, mas você pode copiar e colar comandos clicando com o botão direito do mouse. Em alguns computadores, na linha de comando, podemos usar CTRL+SHIFT+C para copiar e CTRL+SHITF+V para colar.

Na instalação do TensorFlow, usaremos `pip`, que é o gerenciador de pacotes do Python, para instalar o TensorFlow 1.2 para Windows, com suporte para CPU apenas. A instalação com suporte para GPU é mais complicada e varia muito. Caso queira instalá-la, siga as [instruções no site do TensorFlow](https://www.tensorflow.org/install/install_windows). Teste agora sua instalação. Na linha de comando, digite `python` para iniciar o modo iterativo do Python. Digite `import tensorflow as tf`. Se tudo ocorreu bem até aqui, você não verá nenhum erro.

## Instalando Git <a name="git"></a>

Git é um programa de controle de versões. Ele permite que você volte a uma versão anterior do seu código caso escreva algum bug ou alguma coisa que pare o funcionamento do seu programa. Em outras palavras, o Git te permite experimentar novas coisas sem medo de ferrar com o que já foi feito, afinal você sempre poderá restaurar versões passadas do seu código. Além disso, o Git tem uma plataforma online, o [GitHub](https://github.com/), que permite colaboração entre programadores, principalmente para códigos abertos. O GitHub também funciona como um currículo do cientista de dados. Por meio dele, é possível compartilhar seus trabalhos de forma que eles sejam completamente reprodutíveis, permitindo que qualquer um possa certificar-se das suas habilidades.

Para baixar o Git, va em [https://git-scm.com/downloads](https://git-scm.com/downloads) e clique na versão para Windows. Isso iniciará o download de um executável. Quando o download terminar, clique no executável para iniciar a instalação. Aceite todas as configurações padrão, **exceto** a Configuração de Final de Linha. Nesse caso, mude para `Checkout as-is, commit Unix-style line endings`. Isso facilitará muito no momento que você estiver contribuindo com outros projetos via GitHub.

Recomendo fortemente que você faça [este pequeno curso online](https://br.udacity.com/course/how-to-use-git-and-github--ud775/) (e gratuito) sobre como usar controle de versões, Git e GitHub. Não deve te tomar muito tempo.

## Testando <a name="test"></a>
Vamos fazer um último teste para ver se tudo está funcionando. Abra a linha de comando do Git e digite 

```bash
git clone https://github.com/matheusfacure/DeepArt.git
```
Isso baixará um repositório meu com uma implementação do [algoritmo de DeepDream](https://en.wikipedia.org/wiki/DeepDream). Por padrão, o a linha de comando do Git também abre no `HOME`. Se não for o caso, navega até o Home com `cd` e então clone o repositório com o comando acima. Voltando à linha de comando do Windows, você deve estar no Home também. Digite `ls` para ver a pasta `DeepArt` que baixamos com o Git. 

Na linha de comando do Windows, digite agora `cd DeepArt/` para entrar no diretório baixado. Por fim, digite `python deep_dream.py` para executar o programa que fiz. Isso baixará uma rede neural (pode levar alguns minutos) e a utilizará para realizar o algoritmo de DeepDream. Usando o navegador padrão do seu sistema operacional (clicando com o mouse). Vá até o diretório (ou pasta) de DeepArt que acabamos de baixar e veja a imagem criada pelo algoritmo de DeepDream. Caso não saiba onde está essa pasta, na linha de comando do Git, digite `pwd` (print working directory) para mostrar o endereço da pasta.

## Considerações Finais <a name="fim"></a>

Eu testei a execução dos passos desse tutorial algumas vezes e tudo ocorreu sem problemas. No entanto, é possível que alguém encontre alguma dificuldade ou erro no meio do caminho. Se for o caso, por favor comente a sua dificuldade ou erro neste post. Além disso, qualquer sugestão é bem vinda e estarei sempre atualizando este tutorial, tanto para abarcar a resolução dos erros frequentemente encontrados quanto para torná-lo mais compreensível. 
