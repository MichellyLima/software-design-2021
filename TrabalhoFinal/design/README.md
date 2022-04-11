## Design

O Design segue o modelo 4+1 Views, desenvolvido por Philippe Cruchten 
com o objetivo de descrever como é um sistema de software baseado em multiplas visões concorrentes.

Além disso, utilizaremos o Diagrama de Contexto, do modelo C4, para complementar o design da solução.

### Diagrama de Contexto
![DiagramaDeContexto](https://user-images.githubusercontent.com/43323869/161669345-6ea88bda-cedc-4e3a-9a5b-0a56eb2ccc98.png)

Os atores deste sistema serão os **Profissionais da Saúde** como usuários finais, a aplicação a ser desenvolvida - que está identificada como **Sistema de Consulta de Códigos** -, de outro lado, consumindo os serviços do sistema via API REST, estão os **Softwares de Terceiros**. Ainda teremos o papel do **Administador do Sistema** que ficará responsável por disparar o trigger, isto é, o script, que atualizará as informações das tabelas do Banco de Dados do Sistema, trazendo os Code Systems para serem consultados pela aplicação.

O **Profissional da Saúde** realiza uma consulta ao **Sistema de Consulta de Códigos**, que por sua vez fará consultas e processamentos internos para retornar as informações sobre o Código procurado, podendo vizualizá-las através de uma interface amigável para o usuário. Concorrentemente, os **Softwares de Terceiros** também podem enviar requisições de busca de código para o Sistema para utilizar os seus serviços e obter as mesmas informações.

### Visões do Design

1. [Visão de Casos de Uso](VisaoCasosDeUso.md)
2. [Visão do Desenvolvedor](VisaoDesenvolvedor.md)
3. [Visão de Processo](VisaoDeProcesso.md)
4. [Visão Lógica](VisaoLogica.md)
5. [Visão de Implantação](VisaoDeImplantacao.md)
### Decisões Arquiteturais
Com base nas visões fornecidas pelos Diagramas acima, realizamos algumas decisões arquiteturais:

Decisão Arquitetural | Justificativa
----- | ----------
O estilo arquitetural adotado será **Cliente-Servidor** com **MVC**, estando as Views no Cliente e os Controllers, Models e serviços no Servidor| Visto que será necessária uma interface para utilização do sistema, e ainda comunicação REST com outros sistemas, o Cliente-Servidor conseguirá atender de forma satisfatória, combinado ao MVC para trazer segurança, manutenibilidade e escalabilidade.
A aplicação será desenvolvida utilizando **Django**, um framework Web para Python | A linguagem Python possui uma série de recursos a disposição para tratamento de dados e busca em data sets, o que poderá ser bem útil para o desenvolvimento da aplicação com alta performance. A linguagem também oferece suporte para que possam ser implementados Scripts para ler os documentos obtidos dos Data Systems e armazená-los no Banco de Dados da aplicação.
O banco de dados utilizado será o **Mongo DB** (NoSQL) | Um SGBD NoSQL permitirá guardar tipos de dados de documentos, além de fornecer suporte para armazenar os relacionamento necessários entre as tabelas.
