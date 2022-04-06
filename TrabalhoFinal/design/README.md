## Design

O Design segue o modelo 4+1 Views, desenvolvido por Philippe Cruchten 
com o objetivo de descrever como é um sistema de software baseado em multiplas visões concorrentes.

Além disso, utilizaremos o Diagrama de Contexto, do modelo C4, para complementar o design da solução.

### Diagrama de Contexto
![DiagramaDeContexto](https://user-images.githubusercontent.com/43323869/161669345-6ea88bda-cedc-4e3a-9a5b-0a56eb2ccc98.png)

Os atores deste sistema serão os **Profissionais da Saúde** como usuários finais, a aplicação a ser desenvolvida - que está identificada como **Sistema de Consulta de Códigos** -, de outro lado, consumindo os serviços do sistema via API REST, estão os **Softwares de Terceiros**. Ainda teremos o papel do **Administador do Sistema** que ficará responsável por disparar o trigger, isto é, o script, que atualizará as informações das tabelas do Banco de Dados do Sistema, trazendo os Code Systems para serem consultados pela aplicação.

O **Profissional da Saúde** realiza uma consulta ao **Sistema de Consulta de Códigos**, que por sua vez fará consultas e processamentos internos para retornar as informações sobre o Código procurado, podendo vizualizá-las através de uma interface amigável para o usuário. Concorrentemente, os **Softwares de Terceiros** também podem enviar requisições de busca de código para o Sistema para utilizar os seus serviços e obter as mesmas informações.

### Visão Lógica
![DiagramaDeClasse-Atualizacao_base_de_dados](https://user-images.githubusercontent.com/45233540/161885829-d5fb9968-f8d2-4670-b37b-b355c0906b7f.png)

Temos aqui dois processos principais. O primeiro demonstrado no diagrama acima é relativo a **busca e atualização dos documentos** das várias fontes de dados que serão disponibilizadas para consulta pelos usuários. A busca e atualização será realizada por **scripts** que farão um varredura nos sites da Rede Nacional de Dados em Saúde. Cada script será regido por um **Regra de Atualização**, que pode ser acionada por periodicidade ou por algum evento.

Esses scripts trarão como resultados o **documentos em formato JSON** que serão então **indexados automaticamente** e então **persistidos no banco de dados NoSQL** orientado a documentos. A escolha desse tipo de banco é devido tanto pela sua velocidade quanto pelo fato de que cada documento possui sua especificidade, possuindo campos diferentes e únicos que tornam a utilização de um banco de dados relacional muito mais complexa. Concluido essa etapa passamos o processo da consulta pelo usuário.


![DiagramaDeClasse-Consulta_do_usuario](https://user-images.githubusercontent.com/45233540/161885884-9c584188-46c2-406b-bb97-ca262262ffd4.png)

O usuário poderá se cadastrar no sistema caso ele queira salvar um histórico de consultas mais recentes e registrar sinonimos para os códigos que ele buscar. Para isto ele fará login utilizando um e-mail e senha cadastrados.

O usuário poderá **realizar a pesquisa indicando qual fonte de dados ele quer procurar**, por padrão é indicado para pesquisar em todas. A pesquisa trará um ou mais resultados. **Cada resultado é relacionado a um dos documentos ou um campo específico deste.** Ao encontrar o resultado pretendido, o usuário, se estiver logado, poderá **registrar um sinônimo** para ele, que guardará a qual documento e campo deste ele se refere. Assim, ao realizar uma nova busca, utilizando como termo um dos sinônimos cadastrados, este será considerado para trazer um resultado ainda mais rápido. Também será mantido um registros dos últimos resultados obtidos para que o usuário possa acessar com mais facilidade aqueles resultados mais pretendidos.



### Visão do Desenvolvedor
Lorem

### Visão de Processo
Para fornecer a Visão de Processo, fizemos uso de um Diagrama de Sequência:
![image](https://user-images.githubusercontent.com/43323869/161885194-cb9ac5cf-3232-471c-8560-1f3b134571d9.png)

Neste diagrama podemos observar como ficará o fluxo de informações na aplicação, considerando a sequência dos métodos e ações ao pesquisar um Código utilizando um termo. O profissional de saúde acessa a Tela de Busca via Interface. Ao pesquisar por um termo, a Interface aciona o Controller, que por sua vez utiliza o serviço de Pesquisa, que conterá a implementação da busca de Código por termos. Esta, por fim, obterá o código utilizando o EnumBaseDeDados para obter os Code Systems nos quais irá pesquisar.

### Visão de Implantação
Lorem ipsum

### Visão de Caso de Uso
Lorem ipsum
