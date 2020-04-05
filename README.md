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
