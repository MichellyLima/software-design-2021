### Visão Lógica

![DiagramaDeClasse-Atualizacao_base_de_dados2](https://user-images.githubusercontent.com/30759534/162078251-02696180-05d8-45a5-b258-9f1455d75e6c.png)

Temos aqui dois processos principais. O primeiro demonstrado no diagrama acima é relativo a **busca e atualização dos documentos** das várias fontes de dados que serão disponibilizadas para consulta pelos usuários. A busca e atualização será realizada por **scripts** que farão um varredura nos sites da Rede Nacional de Dados em Saúde. Cada script será regido por um **Regra de Atualização**, que pode ser acionada por periodicidade ou por algum evento.

Esses scripts trarão como resultados o **documentos em formato JSON** que serão então **indexados automaticamente** e então **persistidos no banco de dados NoSQL** orientado a documentos. A escolha desse tipo de banco é devido tanto pela sua velocidade quanto pelo fato de que cada documento possui sua especificidade, possuindo campos diferentes e únicos que tornam a utilização de um banco de dados relacional muito mais complexa. Concluido essa etapa passamos o processo da consulta pelo usuário.


![DiagramaDeClasse-Consulta_do_usuario](https://user-images.githubusercontent.com/45233540/161885884-9c584188-46c2-406b-bb97-ca262262ffd4.png)

O usuário poderá se cadastrar no sistema caso ele queira salvar um histórico de consultas mais recentes e registrar sinonimos para os códigos que ele buscar. Para isto ele fará login utilizando um e-mail e senha cadastrados.

O usuário poderá **realizar a pesquisa indicando qual fonte de dados ele quer procurar**, por padrão é indicado para pesquisar em todas. A pesquisa trará um ou mais resultados. **Cada resultado é relacionado a um dos documentos ou um campo específico deste.** Ao encontrar o resultado pretendido, o usuário, se estiver logado, poderá **registrar um sinônimo** para ele, que guardará a qual documento e campo deste ele se refere. Assim, ao realizar uma nova busca, utilizando como termo um dos sinônimos cadastrados, este será considerado para trazer um resultado ainda mais rápido. Também será mantido um registros dos últimos resultados obtidos para que o usuário possa acessar com mais facilidade aqueles resultados mais pretendidos.