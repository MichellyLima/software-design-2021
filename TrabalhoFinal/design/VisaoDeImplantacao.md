### Visão de Implantação

A Visão de Implantação do sistema pode ser ilustrada com o seguinte Diagrama Físico:

![image](https://user-images.githubusercontent.com/71414081/161967120-22d445af-492a-44ae-b9a0-4ae7ddbbc650.png)

Todos os componentes físicos do sistema se comunicam via HTTP, através da internet. O servidor da aplicação Sistema de Consulta de Códigos executa o backend, frontend, script de busca e atualização e o banco de dados. O script de busca e atualização adquire, periodica e seletivamente, os conjuntos de códigos FHIRPath no servidor da Rede Nacional de Dados em Saúde, armazenando-os em banco de dados MongoDB (NoSQL). Estes dados podem ser acessados por dispositivos diversos por usuários cadastrados (profissionais de saúde) ou pelo administrador do sistema através de navegadores web, conectando-se ao frontend da aplicação, desenvolvido utilizando Django. Aplicações terceiras podem, assim como o frontend da aplicação, se conectar diretamente ao backend da aplicação, uma API Python responsável por receber requisições HTTP e, após buscar os dados requisitados no banco de dados, as responder.