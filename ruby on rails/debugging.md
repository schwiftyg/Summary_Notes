# Debugging

## byebug 

#### check byebug exisit?

```ruby
#Gemfile have this 
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
end
``` 
#### if no ,add the before lines in  Gemfile  
 After defining the gems you want to use, you will need to run:
 
```shell
bundle
```
####  how to use it 

add byebug in any ruby code
```ruby  
byebug
```

![Screenshot from 2021-10-10 14-35-13](https://user-images.githubusercontent.com/21187699/136713730-7e713e4e-4641-4c07-b5c5-10f2c50c8d65.png)


when run it, will stop this line in terminal

![Screenshot from 2021-10-10 14-35-40](https://user-images.githubusercontent.com/21187699/136713750-0dadce92-73e0-4556-87ed-25a0663a98d9.png)

you could test the values

![Screenshot from 2021-10-10 14-36-22](https://user-images.githubusercontent.com/21187699/136713759-e5641e5b-0aeb-46f8-a2fc-3c364d041566.png)

type continue and enter to run again
```ruby  
continue
```

![Screenshot from 2021-10-10 14-36-33](https://user-images.githubusercontent.com/21187699/136713765-39a48043-9163-40d2-9fd0-6cf9bbb432a4.png)

if don't want to run, just comment

![Screenshot from 2021-10-10 14-36-52](https://user-images.githubusercontent.com/21187699/136713794-7822fb18-6a47-4ffd-ad6d-fecbf180a774.png)



## Routing list  

type some wrong address in address bar, it will show all routings

```ruby  
http://localhost:3000/questionserret
```

![Screenshot from 2021-10-10 14-43-20](https://user-images.githubusercontent.com/21187699/136713846-86adccfe-663e-4c36-a194-7edaa1e3a67e.png)