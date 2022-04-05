## Design

O Design segue o modelo 4+1 Views, desenvolvido por Philippe Cruchten 
com o objetivo de descrever como é um sistema de software baseado em multiplas visões concorrentes.

Além disso, utilizaremos o Diagrama de Contexto, do modelo C4, para complementar o design da solução.

### Diagrama de Contexto
![DiagramaDeContexto](https://user-images.githubusercontent.com/43323869/161669345-6ea88bda-cedc-4e3a-9a5b-0a56eb2ccc98.png)

Os atores deste sistema serão os **Profissionais da Saúde** como usuários finais, a aplicação a ser desenvolvida - que está identificada como **Sistema de Consulta de Códigos** -, de outro lado, consumindo os serviços do sistema via API REST, estão os **Softwares de Terceiros**. Ainda teremos o papel do **Administador do Sistema** que ficará responsável por disparar o trigger, isto é, o script, que atualizará as informações das tabelas do Banco de Dados do Sistema, trazendo os Code Systems para serem consultados pela aplicação.

O **Profissional da Saúde** realiza uma consulta ao **Sistema de Consulta de Códigos**, que por sua vez fará consultas e processamentos internos para retornar as informações sobre o Código procurado, podendo vizualizá-las através de uma interface amigável para o usuário. Concorrentemente, os **Softwares de Terceiros** também podem enviar requisições de busca de código para o Sistema para utilizar os seus serviços e obter as mesmas informações.

### Visão Lógica
Lorem ipsum

### Visão do Desenvolvedor
Lorem

### Visão de Processo
Lorem ipsum

### Visão de Implantação
Lorem ipsum

### Visão de Caso de Uso
Lorem ipsum
