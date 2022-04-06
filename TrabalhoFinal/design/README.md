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
![image](https://user-images.githubusercontent.com/43323869/162075965-d42a2f63-ac92-4409-b351-395a33525762.png)

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


### Decisões Arquiteturais
Com base nas visões fornecidas pelos Diagramas acima, realizamos algumas decisões arquiteturais:

Decisão Arquitetural | Justificativa
----- | ----------
O estilo arquitetural adotado será **Cliente-Servidor** com **MVC**, estando as Views no Cliente e os Controllers, Models e serviços no Servidor| Visto que será necessária uma interface para utilização do sistema, e ainda comunicação REST com outros sistemas, o Cliente-Servidor conseguirá atender de forma satisfatória, combinado ao MVC para trazer segurança, manutenibilidade e escalabilidade.
A aplicação será desenvolvida utilizando **Django**, um framework Web para Python | A linguagem Python possui uma série de recursos a disposição para tratamento de dados e busca em data sets, o que poderá ser bem útil para o desenvolvimento da aplicação com alta performance. A linguagem também oferece suporte para que possam ser implementados Scripts para ler os documentos obtidos dos Data Systems e armazená-los no Banco de Dados da aplicação.
O banco de dados utilizado será o **Mongo DB** (NoSQL) | Um SGBD NoSQL permitirá guardar tipos de dados de documentos, além de fornecer suporte para armazenar os relacionamento necessários entre as tabelas.
