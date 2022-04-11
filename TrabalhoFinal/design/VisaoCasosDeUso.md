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
