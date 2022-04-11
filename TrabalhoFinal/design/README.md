## Design

O Design segue o modelo 4+1 Views, desenvolvido por Philippe Cruchten 
com o objetivo de descrever como é um sistema de software baseado em multiplas visões concorrentes.

Além disso, utilizaremos o Diagrama de Contexto, do modelo C4, para complementar o design da solução.

### Diagrama de Contexto
![DiagramaDeContexto](https://user-images.githubusercontent.com/43323869/161669345-6ea88bda-cedc-4e3a-9a5b-0a56eb2ccc98.png)

Os atores deste sistema serão os **Profissionais da Saúde** como usuários finais, a aplicação a ser desenvolvida - que está identificada como **Sistema de Consulta de Códigos** -, de outro lado, consumindo os serviços do sistema via API REST, estão os **Softwares de Terceiros**. Ainda teremos o papel do **Administador do Sistema** que ficará responsável por disparar o trigger, isto é, o script, que atualizará as informações das tabelas do Banco de Dados do Sistema, trazendo os Code Systems para serem consultados pela aplicação.

O **Profissional da Saúde** realiza uma consulta ao **Sistema de Consulta de Códigos**, que por sua vez fará consultas e processamentos internos para retornar as informações sobre o Código procurado, podendo vizualizá-las através de uma interface amigável para o usuário. Concorrentemente, os **Softwares de Terceiros** também podem enviar requisições de busca de código para o Sistema para utilizar os seus serviços e obter as mesmas informações.

### Visão Lógica

![DiagramaDeClasse-Atualizacao_base_de_dados2](https://user-images.githubusercontent.com/30759534/162078251-02696180-05d8-45a5-b258-9f1455d75e6c.png)

Temos aqui dois processos principais. O primeiro demonstrado no diagrama acima é relativo a **busca e atualização dos documentos** das várias fontes de dados que serão disponibilizadas para consulta pelos usuários. A busca e atualização será realizada por **scripts** que farão um varredura nos sites da Rede Nacional de Dados em Saúde. Cada script será regido por um **Regra de Atualização**, que pode ser acionada por periodicidade ou por algum evento.

Esses scripts trarão como resultados o **documentos em formato JSON** que serão então **indexados automaticamente** e então **persistidos no banco de dados NoSQL** orientado a documentos. A escolha desse tipo de banco é devido tanto pela sua velocidade quanto pelo fato de que cada documento possui sua especificidade, possuindo campos diferentes e únicos que tornam a utilização de um banco de dados relacional muito mais complexa. Concluido essa etapa passamos o processo da consulta pelo usuário.


![DiagramaDeClasse-Consulta_do_usuario](https://user-images.githubusercontent.com/45233540/161885884-9c584188-46c2-406b-bb97-ca262262ffd4.png)

O usuário poderá se cadastrar no sistema caso ele queira salvar um histórico de consultas mais recentes e registrar sinonimos para os códigos que ele buscar. Para isto ele fará login utilizando um e-mail e senha cadastrados.

O usuário poderá **realizar a pesquisa indicando qual fonte de dados ele quer procurar**, por padrão é indicado para pesquisar em todas. A pesquisa trará um ou mais resultados. **Cada resultado é relacionado a um dos documentos ou um campo específico deste.** Ao encontrar o resultado pretendido, o usuário, se estiver logado, poderá **registrar um sinônimo** para ele, que guardará a qual documento e campo deste ele se refere. Assim, ao realizar uma nova busca, utilizando como termo um dos sinônimos cadastrados, este será considerado para trazer um resultado ainda mais rápido. Também será mantido um registros dos últimos resultados obtidos para que o usuário possa acessar com mais facilidade aqueles resultados mais pretendidos.



### Visão do Desenvolvedor
![image](https://user-images.githubusercontent.com/43323869/162840122-27f1a518-5b9a-4f9d-bc4a-7a503ec5bdea.png)

O diagrama mostra os diversos componentes que são considerados pela aplicação e como eles estarão relacionados entre si, em que as Consultas e os Cadastros enviarão códigos ao sistema que realizará a consulta ou atualização dos registros no SGBD através da API.

### Visão de Processo
Para fornecer a Visão de Processo, fizemos uso de um Diagrama de Sequência:
![image](https://user-images.githubusercontent.com/43323869/161888270-88158772-e6b6-40b5-875c-245954e93086.png)


Neste diagrama podemos observar como ficará o fluxo de informações na aplicação, considerando a sequência dos métodos e ações ao pesquisar um Código utilizando um termo. O profissional de saúde acessa a Tela de Busca via Interface. Ao pesquisar por um termo, a Interface aciona o Controller, que por sua vez utiliza o serviço de Pesquisa, que conterá a implementação da busca de Código por termos. Esta, por fim, obterá o código utilizando o EnumBaseDeDados para obter os Code Systems nos quais irá pesquisar.

### Visão de Implantação
A Visão de Implantação do sistema pode ser ilustrada com o seguinte Diagrama Físico:

![image](https://user-images.githubusercontent.com/71414081/161967120-22d445af-492a-44ae-b9a0-4ae7ddbbc650.png)

Todos os componentes físicos do sistema se comunicam via HTTP, através da internet. O servidor da aplicação Sistema de Consulta de Códigos executa o backend, frontend, script de busca e atualização e o banco de dados. O script de busca e atualização adquire, periodica e seletivamente, os conjuntos de códigos FHIRPath no servidor da Rede Nacional de Dados em Saúde, armazenando-os em banco de dados MongoDB (NoSQL). Estes dados podem ser acessados por dispositivos diversos por usuários cadastrados (profissionais de saúde) ou pelo administrador do sistema através de navegadores web, conectando-se ao frontend da aplicação, desenvolvido utilizando Django. Aplicações terceiras podem, assim como o frontend da aplicação, se conectar diretamente ao backend da aplicação, uma API Python responsável por receber requisições HTTP e, após buscar os dados requisitados no banco de dados, as responder.

### Visão de Caso de Uso

#### **Atores**
Esse tópico descreve os atores, juntamente com as suas responsabilidades para com o Sistema. 

|**Papel**|**Descrição**|**Responsabilidade**|
| :- | :- | :- |
|Usuário|Usuário que realiza o próprio cadastro e que possui menor nível de acesso.|Loga e desloga do sistema, realiza consulta de códigos da saúde e adiciona sinônimos equivalentes para códigos|
|Administrador|Usuário que possui maior nível de acesso entre todos.|Loga e desloga do sistema, realiza consulta de códigos da saúde e executa Script de atualização do BD|
|CodAplic|Outro sistema que consome serviço do nosso sistema (Códigos Saúde).|Consulta de códigos da saúde|
|Convidado|Usuário sem identificação que utiliza o sistema via web.|Consulta de códigos da saúde|

#### **Diagrama de Caso de Uso**

![CasosDeUso 02 drawio](https://user-images.githubusercontent.com/30759534/162077306-9f55bc98-cae8-44aa-b238-cd3ecf7187e1.png)

#### **Detalhamento de Casos de Uso**

##### **Manter Cadastro**
###### UC001 - Cadastrar

|Descrição:|O caso de uso trata da realização do próprio cadastro no sistema como usuário.|
| :- | :- |
|Atores:|Usuário|
|Pré-condições:|<p>1. Possuir acesso à internet com navegador</p><p>2. Possuir email</p>|
|Pós-condições:|1. Usuário possui cadastro no sistema|

**Fluxo Principal**

1. Acessa a Tela de boas vindas
2. Seleciona o botão de cadastrar
3. Informa os valores necessários para cadastro (nome, email e senha) e confirma
4. Recebe o e-mail para ativação da conta
5. Clica no botão de confirmar conta no email
6. Direcionado para página de "conta confirmada"
7. Direcionado para página de login

**Fluxo Exceção**

*Usuário já cadastrado*

1. Acessa a Tela de boas vindas
2. Seleciona o botão de cadastrar
3. Informa os valores necessários para cadastro (nome, email e senha) e confirma
4. É identificado que já possuí cadastro
5. Informado "email já cadastrado"
6. Sistema direciona para tela de login

###### UC002 - Logar no Sistema

|Descrição:|O caso de uso descreve o ato de se autenticar no sistema|
| :- | :- |
|Atores:|Usuário e Administrador|
|Pré- condições:|1. Possuir Cadastro no Sistema (UC001)|
|Pós- condições:|1. Estar autenticado no Sistema|

**Fluxo Principal**

1. Acessar a Tela de Login
2. Informar Email e Senha cadastrados
3. Clica no botão de Login
4. Sistema autentica o acesso
5. Direcionado para página inicial logado

**Fluxo Exceção**

*Usuário e/ou Senha inválido*

1. Acessar a Tela de Login
2. Informar Email e Senha inválidos
3. Clica no botão de Login
4. Sistema mostra a mensagem: "Usuário e/ou Senha Inválidos"
5. Direcionado para página de Login


###### UC003 - Deslogar do Sistema

|Descrição:|O caso de uso descreve o ato de se desconectar do sistema|
| :- | :- |
|Atores:|Usuário e Administrador|
|Pré- condições:|1. Estar autenticado no Sistema|
|Pós- cond ições:|1. Não estar autenticado no Sistema|

**Fluxo Principal**

1. Selecionada a opção "Desconectar"
2. Confirmar o logout do sistema
3. Sistema realiza a finalização da sessão
4. Direcionado para a página de login

**Fluxo Exceção**

1. N/A

##### **Manter Etiqueta de Sinônimo para Código da Saúde**
###### UC004 - Cadastrar Etiqueta de Sinônimo para Código da Saúde

|Descrição:|O caso de uso trata da realização do cadastro de etiquetas para um Código de Saúde|
| :- | :- |
|Atores:|Usuário, Administrador|
|Pré- condições:|<p>1. Estar logado no sistema</p><p>2. Ter consultado um Código de Saúde</p>|
|Pós- condições:|1. Etiqueta Cadastrada para o Código de Saúde consultado|

**Fluxo Principal**

1. Informar o Nome da Nova Etiqueta
2. Confirmar
3. Etiqueta adicionada ao Código de Saúde consultado
4. Sistema permanece na página

**Fluxo Exceção**

*Etiqueta já cadastrada*

1. Informar o Nome da Nova Etiqueta
2. Confirmar
3. Sistema mostra a mensagem: “Etiqueta com mesmo nome já cadastrada”
4. Sistema permanece na página


###### UC005 - Editar Etiqueta de Sinônimo para Código da Saúde

|Descrição:|O caso de uso trata da realização da edição de etiquetas em um Código de Saúde|
| :- | :- |
|Atores:|Usuário, Administrador|
|Pré- condições:|<p>1. Estar logado no sistema</p><p>2. Ter consultado um Código de Saúde</p>|
|Pós- condições:|1. Etiqueta alterada para o Código de Saúde consultado|

**Fluxo Principal**

1. Selecionar uma Etiqueta da lista de etiquetas
2. Editar o Nome da Etiqueta
3. Confirmar
4. Etiqueta é alterada para o novo Nome informado
5. Sistema permanece na página

**Fluxo Exceção**

*Etiqueta já cadastrada*

1. Selecionar uma Etiqueta da lista de etiquetas
2. Editar o Nome da Etiqueta
3. Confirmar
4. Sistema mostra a mensagem: “Etiqueta com mesmo nome já cadastrada”
5. Sistema permanece na página

###### UC006 - Excluir Etiqueta de Sinônimo para Código da Saúde

|Descrição:|O caso de uso trata da realização da remoção de etiquetas em um Código de Saúde|
| :- | :- |
|Atores:|Usuário, Administrador|
|Pré- condições:|<p>1. Estar logado no sistema</p><p>2. Ter consultado um Código de Saúde</p>|
|Pós- condições:|1. Etiqueta alterada para o Código de Saúde consultado|

**Fluxo Principal**

1. Selecionar uma Etiqueta da lista de etiquetas
2. Selecionar a opção de Excluir a Etiqueta
3. Confirmar a Exclusão
4. Etiqueta é removida do Código de Saúde
5. Sistema permanece na página

**Fluxo Exceção**

1. N/A


##### **Consultar ao Código**
###### UC007 - Consultar Código de Saúde

|Descrição:|O caso de uso trata da realização de consulta de Códigos de Saúde no sistema|
| :- | :- |
|Atores:|Usuário, codAplic, Convidado e Administrador|
|Pré- condições:|1. Ter acesso à Internet|
|Pós- condições:|` `2. Ter um código de saúde consultado|

**Fluxo Principal**

1. Acessa a tela de consulta
2. Informa o nome ou parte do nome do Código de Saúde no campo de pesquisa
3. É gerada uma lista de Códigos de Saúde que contém o Nome informado
4. Seleciona um Código de Saúde da lista
5. São mostradas as informações referentes ao Código de Saúde selecionado



**Fluxo Exceção**

*Código de Saúde Inexistente*

1. Acessa a tela de consulta
2. Informa o nome ou parte do nome do Código de Saúde no campo de pesquisa
3. Sistema mostra na tela: “Não existem registros para os parâmetros informados”
4. Sistema permanece na página


##### **Tabela de Códigos**
###### UC008 - Executar Script de Atualização de BD

|Descrição:|O caso de uso descreve a execução de Scripts para a Atualização do Banco de Dados das tabelas dos códigos de saúde|
| :- | :- |
|Atores:|Administrador|
|Pré-condições:|1. Estar logado no sistema com privilégios de administrador|
|Pós-condições:|1. Banco de Dados Atualizado|

**Fluxo Principal**

1. Abrir a Aba de Atualização
2. Selecionar quais Scripts deverão ser executados (de acordo com a tabela correspondente)
3. Confirmar a Execução
4. O Sistema solicita uma nova confirmação
5. Confirma a Execução novamente
6. Sistema Executa e retorna uma lista dos Scripts com sucesso

**Fluxo Exceção**

*Falha na Execução do Script*

1. Abrir a Aba de Atualização
2. Selecionar quais Scripts deverão ser executados (de acordo com a tabela correspondente)
3. Confirmar a Execução
4. O Sistema solicita uma nova confirmação
5. Confirma a Execução novamente
6. Sistema Executa e retorna uma lista dos Scripts com falha
7. Enviado no email cadastrado detalhes da falha do Script.

### Decisões Arquiteturais
Com base nas visões fornecidas pelos Diagramas acima, realizamos algumas decisões arquiteturais:

Decisão Arquitetural | Justificativa
----- | ----------
O estilo arquitetural adotado será **Cliente-Servidor** com **MVC**, estando as Views no Cliente e os Controllers, Models e serviços no Servidor| Visto que será necessária uma interface para utilização do sistema, e ainda comunicação REST com outros sistemas, o Cliente-Servidor conseguirá atender de forma satisfatória, combinado ao MVC para trazer segurança, manutenibilidade e escalabilidade.
A aplicação será desenvolvida utilizando **Django**, um framework Web para Python | A linguagem Python possui uma série de recursos a disposição para tratamento de dados e busca em data sets, o que poderá ser bem útil para o desenvolvimento da aplicação com alta performance. A linguagem também oferece suporte para que possam ser implementados Scripts para ler os documentos obtidos dos Data Systems e armazená-los no Banco de Dados da aplicação.
O banco de dados utilizado será o **Mongo DB** (NoSQL) | Um SGBD NoSQL permitirá guardar tipos de dados de documentos, além de fornecer suporte para armazenar os relacionamento necessários entre as tabelas.
