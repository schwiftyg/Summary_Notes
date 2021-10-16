PM_Tool

# how to models: Project: title (required & unique)
# how to models:Task: title (required & unique within a project) and due_date

### https://boringrails.com/tips/rails-unique-scope

class Project < ApplicationRecord
  belongs_to :account

  has_many :tasks

  validates :name, presence: true, uniqueness: { scope: :account_id }
end


###  follow scaffols_crud?




### change ruby version 

## whereis ruby
```
whereis ruby
```
cd /home/harry/.rvm/rubies

## which ruby
/home/harry/.rvm/rubies/ruby-3.0.0/bin/ruby



## run command line file in rails
https://ruby-doc.org/core-2.0.0/IO.html#method-c-popen

in controller
```
IO.popen(["ls", "./", :err=>[:child, :out]]) {|ls_io|
            @ls_result_with_error = ls_io.read 
 }
 ```

 in erb 
``` 
<%= @ls_result_with_error %> 
```
