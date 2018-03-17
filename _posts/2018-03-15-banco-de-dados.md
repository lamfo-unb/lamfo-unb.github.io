---
layout: post
title: Bancos de dados - Onde vivem, O que comem e Como se reproduzem
lang: pt
header-img: img/banco_de_dados/capa.jpg
date: 2018-03-15
tags: [bd, database, banco de dados, base de dados]
author: Paulo Vitor Moura Barros Henrique
comments: true
---
# Bancos de dados - Onde vivem, O que comem e Como se reproduzem

Esse post tem a intenção de fazer uma introdução no conceito de banco de dados estruturados e dos diferentes modelos de armazenamento de dados.

![Inter-relacionados](/img/banco_de_dados/relacionados.png)

Mas o que são bancos de dados? Bancos de dados nada mais são do que um conjunto de dados inter-relacionados, agrupados em coleções organizadas de forma que faça sentido; ou seja, são dados agrupados e organizados de forma que se possa extrair informações deles.

![SGBDs](/img/banco_de_dados/sgbds.jpg)

Mas se apenas isso constitui um banco de dados, o que são os bancos vendidos por grandes empresas como Oracle, Microsoft dentre outras? O que essas empresas vendem não são apenas arquivos TXT com os dados, mas sim programas que são conhecidos como SGBDs (Sistema de Gerenciamento de Banco de Dados) que tem como finalidade introduzir formas diferentes de tratamento e armazenamento dos dados visando aumentar a velocidade das consultas e das manipulações dos dados, garantir a consistência dos dados armazenados, bem como implementar modelos de armazenamento diferentes que podem se adequar melhor a algumas situações.

Modelos de armazenamento? Como assim “modelos de armazenamento”? Imagine o seguinte cenário, você tem uma tabela do departamento de RH de uma empresa, e a tabela descreve todos os usuários com seus dados e a hierarquia da empresa, até o momento não temos problemas. Então um dia uma diretora da empresa entra em contato com o RH para avisar que ela se casou e resolve acrescentar o sobrenome do seu marido. Como temos uma tabela para conter todos os dados então teríamos que alterar todos os funcionários abaixo dela na hierarquia para constar o seu novo nome, e se algum usuário ficasse com a atualização por fazer não conseguiríamos mais vinculá-lo a diretora que mudou o nome. Com cenários como esse percebemos que precisamos de modelos diferentes de armazenamento, modelos que definitivamente não são mais simples, mas que conseguem expor o conceito da relação dos dados de uma forma mais fácil de ser mapeada.

Modelos de armazenamento:

![Flat](/img/banco_de_dados/Flat_File_Model.svg.png)
  * **Modelo plano (Flat File Model):** consiste de uma matriz bidimensional, ou seja, é o famoso “tabelão” onde todos os dados estão em um lugar.

![Relacional](/img/banco_de_dados/Relational_Model.png)
  * **Modelo relacional (Relational Model):** é o modelo atualmente mais utilizado, no modelo relacional nos dividimos os dados em diversas tabelas e criamos vínculos entre os dados.

![Hierárquico](/img/banco_de_dados/Hierarchical_Model.svg.png)
  * **Modelo hierárquico (Hierarchical Model):** modelo que conecta as informações por meio de uma estrutura de dados em árvore, limitando o tipo de vínculo entre as informações a uma relação 1:n, ou seja, um registro está relacionado a n registros. Para contextualizar melhor, imagine a hierarquia convencional de uma empresa, o gerente tem muitos funcionários abaixo dele, porém os funcionários abaixo dele só tem um gerente.

![Rede](/img/banco_de_dados/Network_Model.svg.png)
  * **Modelo em rede (Network Model):** semelhante ao modelo hierárquico, com a diferença que os registros “filhos” pode ser ligado a mais de um registro “pai”.

![O-Objeto](/img/banco_de_dados/Object-Oriented_Model.svg.png)
  * **Modelo orientado a objeto (Object-Oriented Model):** modelo em que as informações são armazenadas na forma de objetos, visam facilitar a manipulação e comunicação com a aplicação.

A forma que os SGBDs utilizam para facilitar a implantação de modelos de dados diferentes é através das constraints, que são limitações de dados, além disso elas também servem para garantir a integridade dos dados. Imagine que você tem banco de dados contendo os dados de clientes e alguém, visando corromper o seu sistema, faça um cadastro utilizando um CPF pertencente a outro cliente, nesse caso as constraints, mais especificamente uma chave única iria garantir que o segundo cadastro não fosse concluído. Mas essa não é a única função delas, elas são também responsáveis, por exemplo, pela garantia de que dados inter-relacionados sejam interdependentes. Isso tudo poderia ser facilmente substituído por regras no código da aplicação, ou por bloqueios manuais se essa fosse a única vantagem da sua utilização, porém a forma que as constraints são feitas otimizam as consultas que as utilizam.

Por exemplo, quando você define o CPF como uma chave única, o SGBD cria uma tabela separada com apenas os CPF e uma referência a qual linha do banco ele pertence em ordem crescente pelo CPF, essa tabela não é visível ao usuário, mas com esse ambiente configurado, todas as consultas e operações que filtrarem pelo CPF vão utilizar essa tabela para fazer a consulta, por ser menor e estar ordenada as consultas serão bem mais rápidas. Usando a mesma ideia é possível criar índices para qualquer campo de uma tabela e até para múltiplos campos ao mesmo tempo, com isso qualquer consulta pode ser otimizada, porém ao fazer isso as inserções na tabela ficaram mais demoradas, pois toda vez que um novo registro for criado todas as tabelas de índice terão que ser atualizadas para manter a ordem.

Quando falamos de banco de dados também é importante falar sobre SQL (Structured Query Language, ou Linguagem de Consulta Estruturada), que é o conjunto de instruções para interação com os dados do modelo relacional, e como o modelo relacional é o mais utilizado, a linguagem SQL acabou que se tornando sinônimo de linguagem de banco de dados.

Dentro do conjunto de comandos que é conhecido como SQL temos algumas subdivisões de comandos que especificam o tipo de requisição que está sendo feita ao banco, são essas subdivisões:

  * **DML (Data Manipulation Language - Linguagem de manipulação de dados):** Comandos responsáveis pela inserção, atualização e limpeza dos dados.

  * **DDL (Data Definition Language - Linguagem de Definição de Dados):** Comandos responsáveis pela criação e manutenção das estruturas do banco de dados, como tabelas, índices e views.

  * **DCL (Data Control Language - Linguagem de Controle de Dados):** Comandos responsáveis pela manutenção de acessos do banco de dados.

  * **DTL (Data Transaction Language - Linguagem de transação de Dados):** Comandos responsáveis pela execução de outros comandos de forma segura, ou seja, comandos DTL criam uma área na memória para que os comandos executados dentro dessa área não tenham efeito até que se tenha certeza que a consulta deve ser executada.

  * **DQL (Data Query Language - Linguagem de Consulta de Dados):** Comando responsável pela leitura dos dados do banco.

Resumindo, bancos de dados não são apenas programas feitos para o armazenamento e manipulação de dados, mas sim qualquer forma em que os dados sejam armazenados de maneira que se possa extrair informações coerentes deles. Seus diferentes modelos podem se adequar a diversas situações. Os SGBDs são programas para facilitar modelagens, agilizar consultas e dar mais segurança aos dados, otimizando o uso da informação pelas organizações. 
