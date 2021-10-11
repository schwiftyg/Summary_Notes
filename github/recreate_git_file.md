# recreate git file 

sometime the .git file not work properly. you need re-create

clear old .git files
```sh
rm -rf .git
```


initiate new git file

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


add your remote repository 
```sh
git remote add origin https://github.com/harryji168/Quiz.git
```

force push the project
```sh
git push -u origin main --force
```