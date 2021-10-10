# CRUD in Rails Summary Notes
We will cover how to build a CRUD (Create / Read / Update / Delete) operations on a resource in Rails.

## The Model
We will work with a `Question` model. Let's start by creating the model:
```shell
rails g model question title body:text
```
Then we migrate the database:
```shell
rails db:migrate
```

## The Controller
We then generate the `QuestionsController` where we will put all the actions for the CRUD:
```shell
rails g controller questions
```

## NEW Action
We start by working on the `new` action which is what we need to display a page with a form to create the question. We start by defining the route:

```ruby
# in config/routes.rb
get('/questions/new',{to: 'questions#new'})
```
We then define the `new` action in the `QuestionsController`:

```ruby
# in app/controllers/questions_controller.rb
def new
  @question = Question.new
end
```
Note that we need to instantiate a variable `@question` to help us easily generate a form in the view file.

We then build the view file `app/views/questions/new.html.erb`:

```erb
<%= form_for @question do |f| %>
  <div>
    <%= f.label :title %>
    <%= f.text_field :title %>
  </div>
  <div>
    <%= f.label :body %>
    <%= f.text_area :body %>
  </div>
  <%= f.submit %>
<% end %>
```
Using `form_for` enables us to generate a form that will automatically submit to `/questions` for new questions so we can capture that with the `create` action to create new questions with the parameters submitted by the user.

## CREATE Action
The form that we created the `new` action above will need to submit to an action in order to create the question which is, in this case, the `create` action. We start by defining the route for it:
```ruby
# in config/routes.rb
post('/questions',{to: 'questions#create'})
```
We then build the `create` action:

```ruby
# in app/controllers/questions_controller.rb
def create
  # get the data from the form and add it into DB
  # User the  `require` on the params object to retrieve the nested hash of a key
  # usually corresponding the na-value pairs of a form
  # `permit` to specify all input names are  allowed to submit to the DB
  @question = Question.new(params.require(:question).permit(:title,:body))
  #@question = Question.new(question_params)
  if @question.save
      flash[:notice] = "Question created successfully!"
      redirect_to question_path(@question.id)
  else
      render :new
  end
end
```

Note from the above that we use `params.require` then `permit` for security reasons to only allow certain parameters: title and body to be passed from the user. We then create the question using ActiveRecord's `create` method.

We want to be able to show the user errors if validations fails. Let's start by adding validations to our model:

```ruby
# in app/models/question.rb
class Question < ActiveRecord::Base
  validates :title, presence: true
  validates :body, presence: true
end
```
The above ensures that user enters both the title and the body.
 
We instantiate a variable `@question` in here so that we can use it in the form again if errors occur. This way we have the option to show the form again with `render :new`. However, the form doesn't show the errors yet so we can add that to the app so let's add that to our view file 
### create file `app/views/questions/_form.html.erb`:
```erb
<% if @question.errors.any? %>
    <ul>
        <% @question.errors.full_messages.each do |msg|%>
            <li><%= msg %></li>
        <% end%>
    </ul>
<% end%>
<%= form_with(model: @question) do |form|%>

<div class="form-group">
    <%= form.label :title%>
    <br>
    <%= form.text_field :title ,class:"form-text" , autofocus: true   %>
</div>
<div class="form-group">
    <%= form.label :body%>
    <br>
    <%= form.text_area :body, cols: 50, rows: 5 ,class:"form-text" %>
</div>
<%= form.submit %>

<% end%>
```

### create file `app/views/questions/new.html.erb`:

```erb
<h1>New Questions</h1>

<%= render "form" %>  
```

## EDIT action
We use the `edit` action in order to display a form that enables us to edit a create already created. Let's start by defining the route:

```ruby
# in config/routes.rb
get('/questions/:id/edit',{to: 'questions#edit', as: :edit_question })
```

We can then implement the action in the controller:
```ruby
# in app/controllers/questions_controller.rb
def edit
end
```

### create file `app/views/questions/edit.html.erb`:
```erb
<h1>Question Edit Page</h1>
<%= render "form" %>  
```


## UPDATE Action
After submitting the form that edits the question we will need to capture the new parameters and update the question in the database. We start by defining a route:

```ruby
# in config/routes.rb
patch('/questions/:id',{to: 'questions#update'})
```

Then we create the action `update`:
```ruby
# in app/controllers/questions_controller.rb
def update
  question_params = params.require(:question).permit(:title, :body)
  question = Question.find params[:id]
   if question.update question_params
      redirect_to question_path(question)    
   else
      render :edit
   end 
end
```
 
In the code above we render the `edit` template again if the update fails by returning `false` and we redirect to the show page otherwise.


## SHOW action
When we have a question in the database we want to be able to view the question details which we can do in the `show` action. Let's start by defining the route:

```ruby
# in config/routes.rb
get('/questions/:id',{to: 'questions#show', as: :question })
```
Note from the route above that we need to have a variable component `:id` as part of the url in order to identify the question. We can use that to fetch the question from the database. Let's do that in the `show` action:

```ruby
# in app/controllers/questions_controller.rb
def show  
end
```

Note that we defined the questions as an instance variable so we can access it in the view file. Let's create the view file `app/views/questions/show.html.erb` and add the following code to it:

### create file `app/views/questions/show.html.erb`:
```erb
<h1><%= @question.title %></h1>

<p><%= @question.body %></p>

<p><%= time_ago_in_words(@question.created_at) %></p>
<p><%= time_ago_in_words(@question.updated_at) %></p>

<%= link_to("Delete", question_path(@question.id), method: :delete, data: {confirm: "Are you sure"}) %>
<%= link_to("Edit", edit_question_path(@question.id)) %>

```
  

## INDEX Action
Let's build the `index` action to list all the questions we have in our database. We start by adding the route:

```ruby
# in config/routes.rb
get('/questions', {to: 'questions#index'})
```
We then implement the `index` action to fetch all questions:
```ruby
def index 
# in app/controllers/questions_controller.rb
  @questions = Question.all
end
```
 

### create file `app/views/questions/index.html.erb`:
```erb
<h1>All Question</h1>

<% @questions.each do |question|  %>
  <div>
    <h2><%= link_to question.title, question_path(question) %></h2>
    <p><%= question.body %></p>
    <hr>
  </div>
<% end %>
```
Note that we iterate through the collection of questions and display the title and body for each.


## DESTROY action
Let's implement the `destroy` action that enables us to delete a question from the database. We start by implementing the route:

```ruby
# in config/routes.rb
delete('/questions/:id',{to: 'questions#destroy'})
```
 
Note that normal links in web page send only `GET` requests. Rails makes it possible to make a link send non-GET requests by using Javascript with jQuery. So the code above requires Javascript to be enabled and jQuery included (jQuery is included with all Rails projects by default). If you want the link to send a `DELETE` request without using Javascript you can switch the `link_to` to `button_to` which makes a form with one button and uses a hidden `input` field with name `_meothd` and value `DELETE` to mimic sending a `DELETE` HTTP request. The downside of using a form is that you may have to do extra work to style it consistently with other buttons on your website.
