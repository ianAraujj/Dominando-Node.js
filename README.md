# Dominando-Node.js

## Gerenciadores de pacotes:
* NPM
* YARN (é mais rápido)

## API REST
* O servidor responde apenas a estrutura de dados
* É diferente do modelo MVC. O modelo MVC, o servidor já retorna o html pronto com os dados já renderizados

## Benefícios da API REST
* Múltiplios clientes (front-end) com o mesmo back-end
* Protocolo de comunicação padronizada: mesma estrutura para web / mobile / API pública

## Conteúdo da requisição

GET http://api.com/company/1/users?page=2

Temos a rota ('company' e 'users'), o parâmetro da rota ('1', ou seja, esse parâmetro não tem nome) e temos o query params (parâmetros nomeados que ficam no final do endereço)

POST http://api.com/company/1/users

Em uma requisição POST, nós utilizamos o body da requisição para enviar os dados. O body é utilizado apenas em requisições POST e PUT. Temos também o cabeçalho da requisição, aqui podemos definir parâmetros como a linguagem, essa informação pode ser útil já que o front-end pode informar ao back-end a linguagem do cliente

## Iniciando
1 - Crie e entre na pasta do projeto

2 - yarn init -y
Cria o arquivo 'package.json', nesse arquivo contém todas as dependências do projeto. 

3 - yarn add express
Se o projeto for compartilhado por outra pessoa, ela não precisa instalar as dependências novamente. Basta fazer um 'yarn' que todas as dependências do arquivo 'package.json' serão instaladas. As dependências são instaladas dentro da pastas 'node_modules'.

4 - Para executar: node index.js

## Auto Reloading
1 - Instalar essa dependência como DEV, por isso essa flag no final, quer dizer que em Produção essa dependência não existe: yarn add nodemon -D

2 - Agora, execute no terminal: yarn nodemon index.js
