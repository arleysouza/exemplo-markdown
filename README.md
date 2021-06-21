### Aplicação React e NodeJS no mesmo app do Heroku

O código possui uma aplicação React (front-end) e Node (back-end) que executa no mesmo app do Heroku. O back-end persiste os dados no SGBD PostgreSQL disponível através do complemento Heroku Postgres.

![](https://github.com/arleysouza/prova-sub-scripts/blob/master/images/ilustracao.png)

#### Passos para configurar a aplicação no Heroku

1. Crie um app no Heroku. Neste exemplo ele terá o nome de `prova-sub`;

2. Como o objetivo é manter o SGBD PostgreSQL no aplicativo `prova-sub`, então iremos adicionar o add-on Heroku Postgres no app. Assim como é mostrado a seguir, acesse a aba `Resources` e digite parte do nome do add-on `Heroku Postgres`:
![](https://github.com/arleysouza/prova-sub-scripts/blob/master/images/figura1.png)

3. Será criada automaticamente a variável de ambiente `DATABASE_URL` ao adicionar o add-on `Heroku Postgres`. O problema é que não conseguimos editar essa variável para adicionar o parâmetro `sslmode`, então recomenda-se adicionar uma variável de ambiente. Como exemplo, foi adicionada a variável `BD_URL` e copiamos a URL que está na variável `DATABASE_URL` e adicionamos o parâmetro `sslmode=no-verify`, assim como é mostrado a seguir:
![](https://github.com/arleysouza/prova-sub-scripts/blob/master/images/figura2.png)

Observação: a conexão com o Heroku Postgres requer esse parâmetro.

4. O próximo passo é criar uma tabela de nome tbregistro no SGBD. O problema é que o Heroku não provê uma GUI de acesso ao SGBD PostgreSQL, então recomenda-se usar (https://datazenit.com/heroku-data-explorer.html). Acesse a interface do BD e utilize a cláusula a seguir para criar a tabela:
```sql
create table tbregistro(
  idregistro serial not null primary key,
  nro integer not null
);
```
Use a cláusula a seguir para testar o insert na tabela:
```sql
insert into tbregistro(nro) values(2),(5),(3),(5);
```
Use a cláusula a seguir para listar os números cadastrados e suas quantidades:
```sql
select nro, count(*) as "quant" from tbregistro group by nro order by nro;
```
Ao final desse passo a configuração do SGBD estará pronta no Heroku e podemos ir para o VS Code para criar a aplicação.


#### Passos para criar a aplicação no VS Code

5. Como teremos de colocar o front-end React e o back-end Node no mesmo projeto, então primeiramente teremos de criar o aplicativo React. Como exemplo, aqui utilizaremos a pasta `codigo` e criaremos um  aplicativo de nome `app`:
```
D:\pessoal\codigo> npx create-react-app app
```
Após terminar a criação recomenda-se remover os arquivos que não serão utilizados e rodar o projeto para testar.

6.	Excluir o arquivo `yarn.lock` ou `package-lock.json` do projeto, porque o Heroku irá acusar sobreposição ao fazer o deploy;

7.	O projeto ainda não tem funcionalidades, então faremos um deploy no Heroku para testar. Obtenha no Heroku os dados de deploy e execute os seguintes comandos no VS Code:
```
D:\pessoal\codigo\app> git init
D:\pessoal\codigo\app> heroku git:remote -a prova-sub 
D:\pessoal\codigo\app> git add .
D:\pessoal\codigo\app> git commit -am "inicio"
D:\pessoal\codigo\app> git push heroku master
```
Lembrando que o seu app possui um nome diferente de `prova-sub`. O correto é a aplicação fazer o deploy no Heroku e ela estar disponível numa URL do Heroku.

8. Crie uma pasta de nome `servidor` no `app`. Nessa pasta colocaremos o servidor Node, ou seja, o Node estará numa subpasta do projeto React:

![](https://github.com/arleysouza/prova-sub-scripts/blob/master/images/figura3.png)

9.	Inicialize um projeto Node na pasta `servidor`:
```
D:\pessoal\codigo\app\servidor> npm init -y
``` 
Observe que será criado apenas o arquivo `package.json`.

10.	Adicione os pacotes `express`, `pg` e `dotenv` no `servidor`:
 ```
D:\pessoal\codigo\app\servidor> npm i express pg dotenv
``` 
Dotenv é um modulo usado para carregar as variáveis de ambiente de um arquivo `.env` para `process.env`. 

11.	Crie o arquivo `.env`, na pasta `servidor`, e coloque nele a variável de ambiente `BD_URL` com o mesmo valor que colocamos no Passo 3:
```
BD_URL = 'postgres://roowzkzzvpoffw:bca09c54171e20229ccf@ec2-52-4-111-46.compute-1.amazonaws.com:5432/d4php93jva02bk?sslmode=no-verify'
```

12.	Crie o arquivo `server.js`, na pasta `servidor`, e coloque nele o código a seguir para testarmos o servidor:
```javascript
const express = require("express");
const app = express();
const path = require('path');
require("dotenv").config();

// para conversão de application/x-www-form-urlencoded
app.use(express.urlencoded({ extended: true }));

// usar a variável de ambiente PORT
const PORT = process.env.PORT || 3101;

app.listen(PORT, () => {
  console.log(`Rodando na porta ${PORT}...`);
});

app.get("/select", async (req, res) => {
    res.send("rodando select");
});

app.get("/insert/:nro", async (req, res) => {
    res.send("rodando insert");
});

// rota para os arquivos da pasta build do app
const buildPath = path.join(__dirname, '..', 'build');
app.use(express.static(buildPath));
```
