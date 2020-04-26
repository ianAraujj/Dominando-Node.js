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

2 - ```yarn init -y```
Cria o arquivo 'package.json', nesse arquivo contém todas as dependências do projeto. 

3 - ```yarn add express```
Se o projeto for compartilhado por outra pessoa, ela não precisa instalar as dependências novamente. Basta fazer um 'yarn' que todas as dependências do arquivo 'package.json' serão instaladas. As dependências são instaladas dentro da pastas 'node_modules'.

4 - Para executar: node index.js

## Auto Reloading
1 - Instalar essa dependência como DEV, por isso essa flag ```-D``` no final, quer dizer que em Produção essa dependência não existe: yarn add nodemon -D

2 - Agora, execute no terminal: yarn nodemon index.js

3 - No index.js, é necessário lembrar de definir a porta padrão

## Middleware
* É um Design Pattern presente no Node.js

* Podemos definir um Middleware global em que todas as requisições devem passar por ele. Um middleware receb os parâmetros (req, res, next)

* Se no middleware global, eu tenho um 'return next();', quer dizer que a requisição foi mandada para o próximo middleware. Pelo que eu entendi, cada rota que nós criamos é um middleware

* O User pode ser usado para fazer verificações nas requisições. É possível criar vário middlewares para fazer as verificações e adicioná-los em nossas rotas.

## Organização de código
* O que antes tinha apenas o ```index.js```, agora podemos dividir em ```app.js```, ```server.js```, ```routes.js```. Tudo isso dentro da pasta ```src```.

* O ideal é termos uma class para cada funcionalidade da aplicação

## Repositórios
* Desafio 1: https://github.com/ianAraujj/Conceitos_de_NodeJS

## Nodemon & Sucrase
* O Sucrase permite que agt possa usar 'imports' nos módulos da aplicação e 'export default'. Instalação do sucrase ```yarn add sucrase -D```.

* Lembrando que o Nodemon é usado por causa do auto-reloading, para usá-lo junto com o Sucrase precisamos configurar nosso package.json. A ideia é que ele execute Nodemon, mas antes executar o Sucrase para que os 'imports' sejam bem interpretados. Vamos configurar um "scripts" dentro do package.json: 

```
  "scripts": {
    "dev": "nodemon src/server.js"
  },
```

* Depois criar um novo arquivo chamado ```nodemon.json```, aqui o sucrase é executado. 

```
{
  "execMap": {
    "js": "node -r sucrase/register"
  }
}
```

* Para executar: yarn dev

## Sequelize & MVC:
* O Sequelize é uma ORM, ou seja, é uma abstração do banco de dados usando a arquitetura MVC. Na arquitetura MVC, para as tabelas users e projects, os models seriam os arquivos ```users.js``` e ```projects.js```. O Sequelize permite que você use JS em vez de SQL.

* Para criar um usuário, seria assim:
```
User.create({
  name: 'Ian Luccas',
  email: 'ianlucas2503@gmail.com'
})
```

* Para buscar UM usuário:
```
User.findOne({
  "where": {
      name: 'Ian Luccas'
  }
})
```

* Outra coisa que temos que ter é as ```MIGRAÇÕES```, é necessário para que nossa base de dados fique conscistente. As migrações mantém a base de dados atualizadas entre todos os desenvolvedores do time e também no ambiente de produção. Uma migration já criada e repassada NÃO pode ser editada, ou seja, crie uma nova migration para realizar as alterações que vc precisa.

* É possível desfazer uma migração se errarmos em algo enquanto tivermos desenvolvendo a feature, isso é possível graçar a operação de ROLLBACK ou DOWN como está explícito lá no arquivo migration.

* Será utilizado a arquitetura MVC. O controller na verdade é uma classe que retorna um JSON sendo que um controller não pode chamar outro controller ou ele mesmo. Cada model tem o seu controller.

* Um novo controller pode ter no máximo 5 métodos:

```index()```: listagem

```show()```: exibir um único item

```store()```: cadastrar

```update()```: alterar

```delete()```: deletar

## Padronização do Código
* ESLint, Prettier & EditorConfig

## Configuração do Sequelize
* Estrutura de Pastas:
```
├───app
│   ├───controllers
│   └───models
├───config
|----------database.js
└───database
    └───migrations
```

* Ìnstalar o ```yarn add sequelize``` e também ```yarn add sequelize-cli -D```. O CLI vai facilitar na criação de migrations e outros comandos no terminal. 

* Criar um arquivo no diretório principal: ```.sequelizerc``` e mudá-lo para a linguagem de JS. Esse arquivo vai ser responsável por exportar todos os arquivos de configuração que criamos acima.

```
const {resolve} = require('path');

module.exports = {
  config: resolve(__dirname, 'src', 'config', 'database.js'),
  'models-path': resolve(__dirname, 'src', 'app', 'models'),
  'migrations-path': resolve(__dirname, 'src', 'database', 'migrations'),
  'seeders-path': resolve(__dirname, 'src', 'database', 'seeds')
}
```

* No arquivo ```database.js```, temos:

```
module.exports = {
  dialect: 'postgres',
  host: 'localhost',
  username: 'postgre',
  password: 'postgre',
  database: 'postgres',
  define: {
    timestamps: true,
    underscored: true,
    underscoredAll: true
  }
}
```
No caso do 'postgres', nós temos que instalar a depedência ``` yarn add pg pg-hstore```. Sendo o 'timestamps' responsável por criar em cada registro uma informação para a data da adição e a data da última modificação. O 'underscored' é usado para definir o nome das tabelas e colunas, seguindo o padrão: 'data_de_nascimento', com caixa baixa e usando underline.

## Criando as migrations
Podemos usar o cli do sequelize em que instalamos pra nos ajudar a criar nossas migrations.

* Criando uma migration: yarn sequelize migration:create --name=create-users

* Adicionando a migration: yarn sequelize db:migrate

* Removendo uma migration: yarn sequelize db:migrate:undo

* Removendo todas as migrations: yarn sequelize db:migrate:undo:all

```
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('users',
      {
        id: {
          type: Sequelize.INTEGER,
          allowNull: false,
          autoIncrement: true,
          primaryKey: true
        },
        name: {
          type: Sequelize.STRING,
          allowNull: false,
        },
        email: {
          type: Sequelize.STRING,
          allowNull: false,
          unique: true
        },
        password_hash: {
          type: Sequelize.STRING,
          allowNull: false,
        },
        provider:{
          type: Sequelize.BOOLEAN,
          allowNull: false,
          defaultValue: false
        },
        created_at:{
          type: Sequelize.DATE,
          allowNull: false
        },
        update_at:{
          type: Sequelize.DATE,
          allowNull: false
        }
      }
    );
  },

  down: (queryInterface) => {
    return queryInterface.dropTable('users');

  }
};

```

* Depois disso, crie o Model User. Vale lembrar que ainda não temos uma conexão com o BD. Para criar o Model User, temos que receber por parâmetro um 'sequelize', ou seja, uma conexão. Essa conexão será criada em ```index.js``` dentro da pasta migrations. Depois disso, esse arquivo com as conexões precisa ser chamado em algum lugar, isso acontece no arquivo ```app.js```

* Para usar os Models nas rotas é o seguinte: vc importa o Model e depois vc define a função da rota como assíncrona (async), com isso é possível usar o 'await'. 
