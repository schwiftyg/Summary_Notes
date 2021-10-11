
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


### create database connection file
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

### edit package.json

add "start": "nodemon" under scripts

``` 
 "start": "nodemon",
```


