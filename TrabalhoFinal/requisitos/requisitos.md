## Requisitos

Profissionais de saúde usam códigos e informações pertinentes para o registro de várias informações em saúde de forma precisa. 

Alguns deles são códigos das cidades brasileiras, CNES, CBO, CID-10 e CIAP-2. Ainda há dezenas [CodeSystems](https://simplifier.net/redenacionaldedadosemsaude/~resources?category=CodeSystem) 
e dezenas de [ValueSets](https://simplifier.net/redenacionaldedadosemsaude/~resources?category=ValueSet) criados pela RNDS (Rede Nacional de Dados em Saúde). 
A definição  do padrão FHIR, adotado pelo Brasil, inclui 202 perfis (resources) e 63 tipos de dados.

Tendo em vista que é necessário o emprego de tais códigos, dentre outros, o objetivo é disponibilizar um cliente para o profissional de saúde realizar consultas e obter os códigos desejados. Adicionalmente, deve ser oferecido acesso ao mecanismo de busca também para outros sistemas (outros clientes). Ou seja, é desejado um cliente para uso imediato por profissionais de saúde e um serviço a ser empregado por sistemas clientes.

Espera-se que a busca possa ser realizada por qualquer subconjunto dos códigos, por exemplo, apenas CBO, ou por CBO e CIAP-2, ou por todos, naturalmente. Também é desejado que sinônimos possam ser associados aos códigos de tal forma que a busca inclua os sinônimos, por exemplo, o município de Araguapaz (GO) também é conhecido por “Cavalo Queimado” e, dessa forma, a busca pelo termo “queimado” quando o subconjunto dos municípios for considerado, e este “sinônimo” estiver associado ao município em questão, o resultado da busca deve incluir este município. 
