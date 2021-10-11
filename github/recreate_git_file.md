# recreate git file 

sometime the .git file not work properly. you need re-create

### clear old .git files
```sh
rm -rf .git
```


### initiate new git file

```sh
git init
```

add all current file and folder
```sh
git add .
```

add commit message

```sh
git commit -m "first commit"
```

change to main branch
```sh
git branch -M main
```

### create repository on github.com

click new
![Screenshot from 2021-10-11 08-05-03](https://user-images.githubusercontent.com/21187699/136813331-171f0b1c-6e30-421d-a543-8a5c68a16f7b.png)


click create repository

![Screenshot from 2021-10-11 08-08-38](https://user-images.githubusercontent.com/21187699/136814744-7685c82f-b6c6-4d2b-afe1-b24bf8bdf3e6.png)

click code, copy Https 

![Screenshot from 2021-10-11 08-12-51](https://user-images.githubusercontent.com/21187699/136814541-628c5be8-0e09-46fa-8535-4a4974e1ec1a.png)


### add your remote repository 
```sh
git remote add origin https://github.com/harryji168/amazing_amazon.git
```

### force push the project
```sh
git push -u origin main --force
```