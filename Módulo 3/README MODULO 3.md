README: Transformações e Integração Power BI com MySQL no Azure

Introdução

Este projeto foi desenvolvido como parte de um desafio, cujo objetivo era criar relatórios no Power BI a partir de uma base de dados armazenada em um servidor MySQL no Azure. Ao longo deste processo, foram realizadas diversas transformações e modelagens de dados para preparar a base para visualizações eficazes. O foco dos relatórios foi na análise de departamentos e projetos, permitindo insights sobre a gestão e operações da empresa.

Descrição do Projeto

Este desafio teve como objetivo integrar o Power BI com um banco de dados MySQL hospedado na Azure para criar relatórios que ofereçam insights relevantes para a gestão da empresa.

Ferramentas Utilizadas

Azure: Para criar e hospedar o banco de dados MySQL.
MySQL Workbench: Para gerenciar e manipular a base de dados no Azure.
Power BI: Para a criação de relatórios e visualizações baseadas nos dados extraídos do MySQL.

PASSO 1 - Criação de uma instância na Azure para MySQL
PASSO 2 - Criar o Banco de Dados com Base Disponível no GitHub
PASSO 3 - Integração do Power BI com MySQL no Azure
PASSO 4 - Verificar Problemas na Base a Fim de Realizar a Transformação dos Dados

Segui esse script 

Instancia Azure - MySQL

Processo de ativar a instância 

1º Entrar no link da Azure - conta Power BI Lorenz Analytics
https://portal.azure.com/?feature.tokencaching=true&feature.internalgraphapiversion=true#home

2º Serviços Azure - Banco de Dados SQL - MySQL
Servidor Flexível +Criar
Detalhes do projeto
Assinatura - Azure subscription 1 Free
Grupo de recursos - Criar novo - desafio  
Detalhes do servidor
Nome do servidor - desafiodiolorenz 
Conta do ADM
Nome de usuário adm - company
Senha - *******
Confirmar senha - *******
Rede - Marcar Regras de Firewall - Permitir acesso publico 
Segurança - Rótulos - deixar padrão
Revisar +Criar - Criar

3º Revisar a documentação para fazer o tutorial - Gerenciar um servidor flexível do bando de dados do Azure para mySQL usando o portal do azure
Depois de fazer o tutorial e carregar a instancia, vai procurar na aba lateral a opção Conectar - Cloud Shell utilizando a linha de comando

4º Cloud Shell - Bash Criar armazenamento na assinatura atual
linha de comando:
copiar o link  da instancia em Conectar por navegador ou localmente.
senha para criar a instancia e vai logar na instancia pelo Shell; 
show databases;
create database/schema if not exists azure_company; criar o BD database ou schema não tem diferença para criar a raiz do BD
use azure_company; usar o BD como raiz
show tables; mostrar se possui alguma informação no BD

4.1 Não consegui criar o banco de dados via linha de comando, para todos os comandos apresentava bash: mysql: command not found isso era para todos.
Tive que partir para o querido e amado MySQL Workbench e por lá conectei com o banco de dados no azure e criei o banco de dados e inseri os dados via Workbench.

Seguindo o script_bd_company teremos que preencher as tabelas com as informações
CREATE TABLE employee(;
alter table employee;
desc employee; Seguindo no Workbench quando cheguei nesta parte eu apanhei com os erros que me vinha a cada linha de comando que eu copiava do banco de dados fornecido para o desafio era um erro diferente atras do outro e depois acabava inserindo dados errados e vinha mais erros tive que deletar um punhado de vez para poder conseguir chegar na ultima linha de comando, mas consegui depois de muitos erros e acertos

5º Após concluir toda a configuração do script_bd_company, será a vez de inserir os dados do outro script insercao_de_dados

6º Criando Regra no Firewall - Permitir a conexão do IP da Azure com o banco de dados 
Rede - Permitir o acesso público de qualquer serviço
+ Adicionar 0.0.0.0 - 255.255.255.255

7º Integrando Power BI com MySQL na Azure
Obter dados - Banco de dados MySQL
Servidor - Copiar o Nome do servidor que é o link do projeto
Banco de Dados - company

OBS: Este foram os passos que segui para chegar na parte 4 e seguir com as transformações, teve algumas coisas diferentes pq foram varias tentativas de erros até o acerto =D

*******************************************************************

1ª Verifique os cabeçalhos e tipos de dados
Realizei as mudanças nos nomes nas colunas e alterei os tipos de dados (Data, Decimal, Texto)

Tabela: departament

Dname -> Nome do Departamento - 		Texto
Dnumber -> Nº Departamento - 			Número Inteiro
Mgr_ssn -> Nº Seguro Social do Gerente - 	Texto
Mgr_start_date -> Data de Início do Gerente - 	Data
Dept_create_date -> Data de Criação - 		Data

Tabela: dependent

Essn -> Nº Seguro Social do Empregado - 	Texto
Dependent_name -> Nome do Dependente - 		Texto
Sex -> Sexo do Dependente - 			Texto
Bdate -> Data de Nascimento do Dependente - 	Data
Relationship -> Parentesco - 			Texto

Tabela: dept_locations

Dnumber -> Nº Departamento - 			Número Inteiro
Dlocation -> Localização - 			Texto

Tabela: employee

Fname -> Primeiro Nome - 			Texto
Minit -> Inicial do Meio - 			Texto
Lname -> Sobrenome - 				Texto
Ssn -> Nº Seguro Social - 			Texto
Bdate -> Data de Nascimento - 			Data
Address -> Endereço - 				Texto
Sex -> Sexo - 					Texto
Salary -> Salário - 				Decimal Fixo
Super_ssn -> Nº Seguro Social do Gerente - 	Texto
Dno -> Nº Departamento - 			Número Inteiro

Tabela: project

Pname -> Nome do Projeto - 			Texto
Pnumber -> Nº Projeto - 			Número Inteiro
Plocation -> Localização do Projeto - 		Texto
Dnum -> Nº Departamento - 			Número Inteiro

Tabela: works_on

Essn -> Nº Seguro Social do Empregado - 	Texto
Pno -> Nº Projeto - 				Número Inteiro
Hours -> Horas Trabalhadas - 			Decimal

Com isso no passo 2.

2ª Modifique os valores monetários para o tipo double preciso
 
Tive que pesquisar para saber o que era double preciso =/

Colunas alteradas
Salary - Decimal Fixo
Bdate, Mgr_start_date, e Dept_create_date - Data
Ssn, Super_ssn, Essn, Pno, ,  - Texto 
Dnumber, Pnumber - Número Inteiro

Conforme alteração realizada e descrita no passo anterior deixei todas as colunas com seu tipo de dados


3ª Verifique a existência dos nulos e analise a remoção 
Fiz as consultas nas tabelas e onde tinha informação nula sem nada fiz a remoção e alguns casos tive que substituir as informações, o único que possui valor nulo é o gerente.

4ª Os employees com nulos em Super_ssn podem ser os gerentes. Verifique se há algum colaborador sem gerente

Conforme verificação e remoção dos nulos o único que possui valor nulo é o gerente James Borg.

5ª Verifique se há algum departamento sem gerente
Todas os departamentos possui um gerente

6ª Se houver departamento sem gerente, suponha que você possui os dados e preencha as lacunas 
Todas os departamentos possui um gerente

7ª Verifique o número de horas dos projetos
Não possui horas negativa na tabela. O funcionário James trabalhou 0 horas, ele é o gerente

8ª Separar colunas complexas
Foram separadas a coluna endereço em 4 colunas ( Número, Rua, Cidade, Estado ) Dividi a coluna por delimitador - hífen 

9ª Mesclar consultas employee e departament para criar uma tabela employee com o nome dos departamentos associados aos colaboradores. A mescla terá como base a tabela employee. Fique atento, essa informação influencia no tipo de junção

Mesclar consultas como Novas ( selecionei a tabela employee, fui em Mesclar consultas como Novas selecionei as colunas 'Nº Departamento' na employee e 'Nº Departamento' na departamento, coloquei o tipo de junção Externa esquerda, com isso conclui a mescla e criei uma nova tabela employee_departament.


10ª Neste processo elimine as colunas desnecessárias.

Fiz a remoção de diversas colunas com este prefixo azure_company. o nome da tabela.

Tabela employee_departament fiz a exclusão da coluna 'Nº Departamento' e 'endereço'. Devido que essas informações se encontrava em outra coluna

11ª Realize a junção dos colaboradores e respectivos nomes dos gerentes . Isso pode ser feito com consulta SQL ou pela mescla de tabelas com Power BI. Caso utilize SQL, especifique no README a query utilizada no processo.

Fiz a mesclagem das tabelas employee e employee_departament, fazendo a mescla da coluna Nº Departamento com a Nº Departamento tipo de junção esquerda
Expandi a coluna na tabela employee_departament e selecionei as colunas Nº Seguro Social do Gerente e Nome do Departamento.

Fiz a mesclagem das tabelas departament e employee, fazendo a mescla da coluna Nº Seguro Social do Gerente com a Nº Seguro Social tipo de junção esquerda 
Expandi a coluna mesclada da employee na tabela departament e selecionei as colunas Primeiro Nome, Inicial do Meio e Sobrenome criei uma nova coluna com nome completo do Colaborador e renomeado para (Nome Completo do Gerente).

12ª Mescle as colunas de Nome e Sobrenome para ter apenas uma coluna definindo os nomes dos colaboradores

Mesclei utilizando este comando [Primeiro Nome] & " " & [Inicial do Meio] & " " & [Sobrenome] e renomeando a coluna para Nome Completo do Colaborador na tabela employee, apaguei as colunas Primeiro Nome, Inicial do Meio e Sobrenome na tabela employee e employee_departament

Com  mescla realizado entre employee_departament e departament, consegui deixar a tabela employee_departament com as colunas nomes dos colaboradores e coluna com nomes dos gerentes responsáveis

13ª Mescle os nomes de departamentos e localização. Isso fará que cada combinação departamento-local seja único. Isso irá auxiliar na criação do modelo estrela em um módulo futuro. 

Mesclar consultas entre as tabelas departament e dept_locations colunas Nº Departamento em ambas, utilizei junção interna, garantir que apenas as combinações que existem nas duas tabelas sejam incluídas.
Expandi e adicionei a coluna Localização na tabela departament, com isso adicionei mais uma coluna personalizada agora, para atribuir o nome do departamento-localização na mesma coluna.

Adicionei uma coluna personalizada e coloquei o nome "Departamento e Localização" e coloquei a formula - "[Nome do Departamento] & " - " & [Localização]"

14ª Explique por que, neste caso supracitado, podemos apenas utilizar o mesclar e não o atribuir. 

Diferença entre Mesclar e Atribuir
Mesclar: Combina dados de duas tabelas com base em colunas comuns, adicionando informações complementares. No projeto, mesclamos departament e dept_locations pela coluna Nº Departamento para garantir combinações únicas de departamento e localização.

Atribuir: Une dados de duas tabelas com a mesma estrutura, empilhando registros semelhantes. Como as tabelas departament e dept_locations possuem dados diferentes, utilizamos mesclar, não atribuir.

15ª Agrupe os dados a fim de saber quantos colaboradores existem por gerente

Dupliquei a tabela employee_departament para criar o agrupamento das colunas, a tabela employee_departament foi renomeada para employee_total_de_colaboradores para realizar o agrupamento nela, com isso fiz o processo:

Pagina Inicial - Agrupar por - Avançadas - Nº Seguro Social do Gerente - Nome Completo do Gerente - Nome da Coluna - Total de Colaboradores - Operação - Contar linhas

Fiquei com a tabela com 3 colunas onde um é o Nome Completo do Gerente, a outra é o Nº Seguro Social do Gerente contendo os ID dos gerentes e a outra coluna é o Total de Colaboradores que cada gerente tem.

16ª Elimine as colunas desnecessárias, que não serão usadas no relatório, de cada tabela 


"Não eliminei nenhuma coluna a mais neste passo não achei necessário. Relatórios que vou executar!"

Na verdade achei que não iria apagar colunas, mas acabei tomando a decisão obrigatória de realizar algumas mudanças para poder finalizar a transformação sem dar erro na hora de aplicar.

"Tive que remover linhas duplicadas na tabela departament devido a um erro de muitos para muitos e o numero 5 se repetia diversas vezes e devido a isso para poder finalizar a transformação precisou remover as linhas." 
Precisei realizar o agrupamento de colunas para poder deixar o numero do departamento com o nome do departamento agrupado na tabela dept_locations, devido a duplicidade na tabela com o N Departamento que se repetia o 5 3x na coluna.
Realizei o mesmo processo na tabela employee_departament.

Realizei um processo de concatenação na tabela dept_locations para chegar a um resultado de agrupamento, onde fiz o agrupar por coluna Nº Departamento, nomeie a nova coluna como Localizações Agrupadas e tipo de operação todas as linhas.
Criei uma coluna personalizada com a seguinte formula 
" Text.Combine(Table.Column([Localizações Agrupadas], "Localização"), ", ") "

Com isso consegui agrupar todos os departamentos de numero 5, os nomes das cidades ficaram na mesma linha e com isso não terá duplicidade informação.

Apaguei as colunas Primeiro Nome, Inicial do Meio e Sobrenome  
Nº Departamento da tabela employee_departament

Página de Departamentos
Página de Projetos
Página de Dependentes
Página de Localização
Página de Resultados Gerais

Utilizando ícones para as paginas especificas.


Desenvolvimento de Relatórios

Página de Departamentos:

Utilizei os seguintes visuais:

Cartão: Utilizei para realizar a contagem de Gerentes e Departamentos

Tabela: Localização e Nº Departamento

Tabela: Exibir Nome do Departamento, Nome Completo do Gerente

Tabela: Exibir Nome Completo do Gerente e Nome do Departamento

Gráfico de Pizza: Distribuição dos departamentos

Mapa: Localização dos departamentos


Botão - Adicionado botão para o icone que representa os departamentos



Página de Projetos

Matriz: Nome do Projeto, Nº do Projeto, Nº Departamento, contagem Horas Trabalhadas

Tabela: Nome do Projeto, Localização do Projeto, Nº Departamento, Horas Trabalhadas.

Gráfico de Pizza: Nome do Projeto, Localização do Projeto

Cartão: Soma Nº do Projeto

Cartão: Soma Horas Trabalhadas









Conclusão

Este desafio proporcionou uma grande oportunidade para aprofundar o conhecimento na integração do Power BI com bancos de dados em nuvem, além de desenvolver habilidades em transformação de dados. Durante o processo, enfrentei desafios, mas consegui superá-los com pesquisas e práticas. Entrego este projeto com dois relatórios detalhados e documentados, prontos para serem utilizados pela empresa em suas análises estratégicas.