
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

### install neccessary modules

```shell  
npm i ejs express knex method-override pg
```


### install development modules with option -D

```shell  
npm i -D concurrently faker http-server morgan nodemon 
```