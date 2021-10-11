
## What is Node?

Node.js is a server-side, javascript runtime environment. An application which runs on computer operating system in oppose to web browser or phone, it’s an application that looks into javascript and decides which javascript files or commands should be executed and when and then sends that javascript to an embedded javascript engine (V8) for execution on the computer. **_So, in a nutshell, V8 is the car’s engine while Node.js is everything else that makes the car, and as a node.js developer, you are the driver._** So, Node.js is a C++ application that embeds the V8 js engine.

### init project - using Super_Team_Ticker as sample

create and enter director
```shell
mkdir Super_Team_Picker && cd Super_Team_Picker
```

initiate node project, with option -y, then create file directly

```shell
npm init -y  
```
check the folder 
```shell
code .
```
you will get package.json like this

![Screenshot from 2021-10-10 18-22-55](https://user-images.githubusercontent.com/21187699/136720801-aca36ee5-9936-4636-a477-d1d95a663703.png)


### install neccessary modules

```shell  
npm i ejs express knex method-override pg
```
- i : install
- ejs:  html template file
- express:  node simple web sever
- knex: node database connection
- method-override: for edit, delete action
- pg :postgreSQL database

you also can do one by one like 
```shell  
npm i knex
npm i express
npm i pg
npm i method-override
npm i ejs
```
the package.json will like this

![Screenshot from 2021-10-10 18-34-53](https://user-images.githubusercontent.com/21187699/136721423-1cb5b928-8162-4e65-a003-f808495c22ef.png)

### install development modules with option -D

```shell  
npm i -D concurrently faker http-server morgan nodemon 
```
- faker : test date for seed
- morgan: for showing test data
- nodemon: keep runnig server for test  
- optional:  concurrently, http-server for inputing demo image 

the package.json will like this

![Screenshot from 2021-10-10 18-37-19](https://user-images.githubusercontent.com/21187699/136721527-e2469162-c31a-4047-ad4c-91bad2833b8e.png)


Verify install use 

```shell  
knex --version
```
you will get 

![Screenshot from 2021-10-10 18-39-24](https://user-images.githubusercontent.com/21187699/136721629-a794a736-a65b-4758-bff0-d57806260866.png)

### Create a .gitignore file  

```shell  
curl https://www.toptal.com/developers/gitignore/api/node,macos,windows,linux> .gitignore
```
click this file, you will get

![Screenshot from 2021-10-10 18-46-54](https://user-images.githubusercontent.com/21187699/136722044-c065ba7c-daf3-4ee6-ac3f-f12cc9586a23.png)

>.gitignore file just let different system ignore the incompatible files in this project


### create a database for this project

```shell  
createdb --echo Super_Team_Picker
```
> echo just show the create process
you will get in terminal 

![Screenshot from 2021-10-10 18-55-46](https://user-images.githubusercontent.com/21187699/136722693-013c8ad2-14bf-446a-bb39-ac185700a158.png)


### create database connection file knexfile.js
```shell  
knex init
```
you will get in terminal 
![Screenshot from 2021-10-10 19-02-08](https://user-images.githubusercontent.com/21187699/136723077-f6dd3722-cc06-4e8b-abd8-55bd836cbc41.png)


verify database, enter database terminal
```shell  
psql -d Super_Team_Picker
```
in database terminal 

```shell  
knex_db=# \d
```
show Did not find any relations.

exit database terminal
```shell  
knex_db=# \q
```
the process look like this  

![Screenshot from 2021-10-10 19-06-08](https://user-images.githubusercontent.com/21187699/136723332-c317fd0d-63ea-40b2-8204-d868537ebf15.png)


### edit database connection file  knexfile.js

remove staging, producting part, just keep development part

change  "client: 'sqlite3'," to 

```node
client: 'pg'
```

change connect to 
```node
 database: 'Super_Team_Picker',
```

for linux user, need add username and password
```node
username: harry
password:12345678
```

add migrations and seeds part
```node
 migrations: {
      directory: "./db/migrations"
    },
    seeds: {
      directory: "./db/seeds"
    }
```

final result like this: 
```node 
module.exports = {
  development: {
    client: 'pg',
    connection: {
      database: 'Super_Team_Picker',
      username:'harry',
      password:'12345678'
    },
    migrations: {
      directory: "./db/migrations"
    },
    seeds: {
      directory: "./db/seeds"
    }
  },
};
```

![Screenshot from 2021-10-10 19-22-38](https://user-images.githubusercontent.com/21187699/136724461-62d83a3c-2852-4b99-a6c4-b4dec80dde46.png)


### create migrations file for create tables 
``` 
knex migrate:make CreateCohorts
```
it will auto create folder "./db/migrations" and file "20211011023105_CreateCohorts.js"
>filename could different have timestampe_part.

like this 

![Screenshot from 2021-10-10 19-33-10](https://user-images.githubusercontent.com/21187699/136725258-7b07ae87-ff35-445b-b382-3c14dddb1d91.png)


### edit migration file for create tables 
add create table in exports.up ,add drop table in exports.down
final result like this
```node
exports.up = function(knex) {
    return knex.schema.createTable('cohorts', table => {
        table.bigIncrements('id');
        table.string('members');
        table.string('name');
        table.text('logoUrl');
        table.timestamp('createdAt').defaultTo(knex.fn.now());
    });
};

exports.down = function(knex) {
    return knex.schema.dropTable('cohorts');
};
```
- bigIncrements is auto add integer number
- string include charactor from 0-255
- text include unlimited charactor
- image just store image url instead of file
- timestamp is store time stamp, using defaulf knex function  knex.fn.now()


### run migration file 
run in terminal, it mean run the latest migration file
```shell
knex migrate:latest
```
it will show  Batch 1 run: 1 migrations

verify database

```shell  
psql -d Super_Team_Picker
```
in database terminal 

```shell  
knex_db=# \d
```
it will show database table files, like this

![Screenshot from 2021-10-10 19-46-45](https://user-images.githubusercontent.com/21187699/136726239-9f0d14b0-96a1-4861-b4e9-e40f1a121356.png)

```shell 
knex_db=# \d knex_migrations  
```
![Screenshot from 2021-10-10 19-52-05](https://user-images.githubusercontent.com/21187699/136726636-64bf4dcf-dd9c-4c7f-90e6-7b199c4c700e.png)

```shell 
knex_db=# \d cohorts
```
![Screenshot from 2021-10-10 19-53-38](https://user-images.githubusercontent.com/21187699/136726753-9af7c014-7fcc-40f5-8307-f27780f06ab6.png)


exit database terminal
```shell  
knex_db=# \q
```

### create client.js under db for connection

```node
const knex = require('knex');
const dbConfig = require('../knexfile');

const client = knex(dbConfig.development);
module.exports = client;
```
> it is in db folder, not in db/migrations folder

like this 

![Screenshot from 2021-10-10 19-58-53](https://user-images.githubusercontent.com/21187699/136727358-a4721ee8-457f-4f36-b703-c314aac8e072.png)






### edit package.json

add "start": "nodemon" under scripts add 
```   
    ,
 "start": "nodemon",
```

look like this 

![Screenshot from 2021-10-10 20-10-07](https://user-images.githubusercontent.com/21187699/136728112-6bce1e43-b90b-4b96-86d3-873e982a3f27.png)

### add routes file under routes folder

create routes folder
```shell
mkdir routes
```
add routes file call `cohorts.js`  
>could use project name as main route

```shell
code routes/cohorts.js
```
input code 

```shell
const express = require('express');
const knex = require('../db/client');
const router = express.Router();



module.exports = router;
```

![Screenshot from 2021-10-10 20-22-49](https://user-images.githubusercontent.com/21187699/136728866-e982d8a3-d5e2-4eee-a781-32341219e286.png)


### add index.js to top 

```node 
const express = require('express');
const logger = require('morgan');
const app = express();
const path = require('path');
const cohortsRouter = require('./routes/cohorts');
const methodOverride = require('method-override');

app.use(express.static(path.join(__dirname, 'public')));
app.use(express.urlencoded({
    extended: true
}));
app.use(methodOverride((req, res) => {
    if (req.body && req.body._method) {
        const method = req.body._method
        return method;
    }
}));

app.use(logger('dev'));
app.set('view engine', 'ejs');
app.use("/cohorts", cohortsRouter);

app.get('/', function (request, response) {   
    response.render('home', { pageTitle: "Super Team Picker Home" } );
});


const PORT = 5000;
const ADDRESS = '127.0.0.1';

app.listen(PORT, ADDRESS, () => {
    console.log(`EXPRESS server listening on ${ADDRESS}:${PORT}`);
});
```
look like this

![Screenshot from 2021-10-10 20-25-20](https://user-images.githubusercontent.com/21187699/136729007-59afdc75-e75d-4d20-a13b-70b966979c4e.png)



## try test project
Now the project construe is setup, you run the server for test 

in terminal
```node 
npm start
```

teminal will show npm start

![Screenshot from 2021-10-10 20-28-45](https://user-images.githubusercontent.com/21187699/136729205-2c0b3e44-a1fe-43c3-8a5d-c64b2e559947.png)

open browser

```sh
http://localhost:5000/
```

it will show "Error: Failed to lookup view "home" in views directory"
like this, so we could add webpage now.

![Screenshot from 2021-10-10 20-31-55](https://user-images.githubusercontent.com/21187699/136729434-3fa0276b-295e-4a20-8c6b-eff5e6e0176a.png)

use Ctrl+C stop server now 

### setup views struction

create views folder
```node 
mkdir views
```

create partials folder
```node 
mkdir views/partials
```

### create header file  `header.ejs`

```node 
code views/partials/header.ejs
```
add code
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title><%= pageTitle%> - [Homework 4]Super Team Picker</title>
    <link rel="stylesheet" href="/css/bootstrap.min.css"> 
  </head>
  <body class="mx-4 my-3">
    <div class="d-flex flex-row justify-content-between">
        <h5 class="text-primary align-self-center">Team Picker</h5>
        <ul class="nav justify-content-end">
            <li class="nav-item">
                <a class="nav-link" href="/">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/cohorts">Cohorts</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/cohorts/new">New Cohort</a>
            </li>
        </ul>
    </div> 

<div class="container">
```
look like
![Screenshot from 2021-10-10 20-40-37](https://user-images.githubusercontent.com/21187699/136730022-465d1065-3086-4c70-b2d2-f2b413c80f04.png)



### create footer file  `footer.ejs`

```node 
code views/partials/footer.ejs
```

add code
```
</div>
</body>
</html> 
```
like this

![Screenshot from 2021-10-10 20-43-31](https://user-images.githubusercontent.com/21187699/136730226-c9c82a1e-42a4-436a-be1e-acb54d356740.png)

### create home page  `home.ejs`

```node 
code views/home.ejs
```

add code
```
<%- include('./partials/header') %>
<p> 
<h1>Welcome to Super Team Picker</h1>
<p>
<p>Team Picker is old and is in need of a rebuild. Create an Express app with Postgres to store cohorts. Cohorts will have a list of members stored as comma separated names. They'll also have a name and a logo.
<p>
    When visiting a cohort's show page, you will be able to generate teams for that cohort. Below are mockups of every page that you need to build for the application. Use Bootstrap to build it. Try to match the mockups as accurately as possible.</p>

<p>
    <a href="/cohorts/new" class="btn btn-primary text-primary bg-light">Creating New Cohorts</a>

    <a href="/cohorts/" class="btn btn-primary text-primary bg-light">Index to List Cohorts</a>
    
<%- include('./partials/footer') %>
```

like this

![Screenshot from 2021-10-10 20-49-18](https://user-images.githubusercontent.com/21187699/136730650-8185ef45-f9dd-406b-8602-578c0049a9fe.png)



## test project again
Now the project construe is setup, you run the server for test 

in terminal
```node 
npm start
```
teminal will show npm start

open browser

```sh
http://localhost:5000/
```

now it will show  content like this:

![Screenshot from 2021-10-10 20-51-15](https://user-images.githubusercontent.com/21187699/136730803-025debd8-b969-4b6b-97e5-2837759de9a2.png)

use Ctrl+C stop server now 

## add stylesheet to beatiful page 


### setup folder

create public folder
```sh
mkdir public
```

create css folder
```sh
mkdir public/css
```

### get bootstrap css file

```sh
curl https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css > public/css/bootstrap.min.css
``` 
![Screenshot from 2021-10-10 21-02-12](https://user-images.githubusercontent.com/21187699/136731553-0b08068b-2fec-42ab-8940-5003fac75b2c.png)

click file like:

![Screenshot from 2021-10-10 21-03-07](https://user-images.githubusercontent.com/21187699/136731607-edd12be5-f07a-42df-863b-cea1fac9cd3d.png)



## test project again
Now you could run the server again  

in terminal
```node 
npm start
```
teminal will show npm start

open browser

```sh
http://localhost:5000/
```

now it will show  content like this: it much better than before

![Screenshot from 2021-10-10 21-06-34](https://user-images.githubusercontent.com/21187699/136731854-415b37fb-34e0-4a4e-9019-f19e2524a863.png)


use Ctrl+C stop server now 

## add New feathure 