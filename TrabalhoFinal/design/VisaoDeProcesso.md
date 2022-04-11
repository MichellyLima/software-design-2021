### Visão de Processo
Para fornecer a Visão de Processo, fizemos uso de um Diagrama de Sequência:
![image](https://user-images.githubusercontent.com/43323869/161888270-88158772-e6b6-40b5-875c-245954e93086.png)


Neste diagrama podemos observar como ficará o fluxo de informações na aplicação, considerando a sequência dos métodos e ações ao pesquisar um Código utilizando um termo. O profissional de saúde acessa a Tela de Busca via Interface. Ao pesquisar por um termo, a Interface aciona o Controller, que por sua vez utiliza o serviço de Pesquisa, que conterá a implementação da busca de Código por termos. Esta, por fim, obterá o código utilizando o EnumBaseDeDados para obter os Code Systems nos quais irá pesquisar.