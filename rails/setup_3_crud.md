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
  question_params = params.require(:question).permit(:title, :body)
  Question.create question_params
  render text: "Question created successfully"
end
```
Note from the above that we use `params.require` then `permit` for security reasons to only allow certain parameters: title and body to be passed from the user. We then create the question using ActiveRecord's `create` method.

We want to be able to show the user errors if validations fails. Let's start by adding validations to our model:
```ruby
class Question < ActiveRecord::Base
  validates :title, presence: true
  validates :body, presence: true
end
```
The above ensures that user enters both the title and the body.

We then render the form again if validation fails:
```ruby
# in the QuestionsController
def create
  question_params = params.require(:question).permit(:title, :body)
  @question = Question.new question_params
  if @question.save
    redirect_to question_path(@question.id)
    render text: "Question created successfully"
  else
    render :new
  end
end
```
We instantiate a variable `@question` in here so that we can use it in the form again if errors occur. This way we have the option to show the form again with `render :new`. However, the form doesn't show the errors yet so we can add that to the app so let's add that to our view file `app/views/questions/new.html.erb`:
```erb
<h1>New Questions</h1>
<% if @question.errors.any? %>
  <ul>
    <% @question.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
  </ul>
<% end %>

<%= form_for @question do |f| %>
  <div>
  ....
```
When the questions is created successfully we only show text to the user which is not very user-friendly. Let's create a display page to show the question details. Let's implement the `show` action

## SHOW action
When we have a question in the database we want to be able to view the question details which we can do in the `show` action. Let's start by defining the route:
```ruby
get "/questions/:id" => "questions#show", as: :question
```
Note from the route above that we need to have a variable component `:id` as part of the url in order to identify the question. We can use that to fetch the question from the database. Let's do that in the `show` action:
```ruby
# in the QuestionsController
def show
  @question = Question.find params[:id]
end
```
Note that we defined the questions as an instance variable so we can access it in the view file. Let's create the view file `app/views/questions/show.html.erb` and add the following code to it:
```erb
<h1><%= @question.title %></h1>

<p><%= @question.body %></p>

<p><%= @question.created_at %></p>
```
Now we can make the `create` action redirect to the show page instead of just showing text:
```ruby
# in the QuestionsController
def create
  question_params = params.require(:question).permit(:title, :body)
  @question = Question.new question_params
  if @question.save
    flash[:notice] = "Question created successfully!"
    redirect_to question_path(@question)
  else
    render :new
  end
end
```
#not use  render text: "Question created successfully"  

## INDEX Action
Let's build the `index` action to list all the questions we have in our database. We start by adding the route:
```ruby
get "/questions" => "questions#index"
```
We then implement the `index` action to fetch all questions:
```ruby
def index
  @questions = Question.all
end
```
After that we implement the view file `app/views/questions/index.html.erb`:
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

## EDIT action
We use the `edit` action in order to display a form that enables us to edit a create already created. Let's start by defining the route:
```ruby
#get "/questions/:id/edit" => "questions#edit"
 get('/questions',{to: 'questions#edit', as: :edit_question })
```
We can then implement the action in the controller:
```ruby
def edit
  @question = Question.find params[:id]
end
```
Note that we need to find the question in order to pre-populate the form with its data. We can use the same form code we used in the `new.html.erb` template:
```erb
<h1>New Questions</h1>
<% if @question.errors.any? %>
  <ul>
    <% @question.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
  </ul>
<% end %>

<%= form_with(model: @question) do |form| %>
  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>
  <div>
    <%= form.label :body %>
    <%= form.text_area :body , cols: 50, rows: 5 %>
  </div>
  <%= form.submit %>
<% end %>
```
`form_for` is replaced with `form_with` 
The `form_for` helped method automatically sets the `HTTP` method to `PATCH` by including a hidden input field with name `_method` and value `PATCH`. It also automatically sets the action for the form to be `question_path(@question)`. It knows how to do this by checking whether `@question` is persisted or not. Persisted primarily means whether `@question` exists in the database or not and it knows this by looking at its `id` it also checks that `@question` wasn't just destroyed.

We can also add a link in the show page `app/views/questions/show.html.erb` to link to the edit page:
```erb
<h1><%= @question.title %></h1>

<p><%= @question.body %></p>

<p><%= @question.created_at %></p>

<%= link_to("Edit", edit_question_path(@questions)) %>
```

## UPDATE Action
After submitting the form that edits the question we will need to capture the new parameters and update the question in the database. We start by defining a route:
```ruby
patch "/questions/:id" => "questions#update"
```
Then we create the action `update`:
```ruby
def update
  question_params = params.require(:question).permit(:title, :body)
  question = Question.find params[:id]
  question.update question_params
  redirect_to question_path(question)
end
```
In the code above we use the `require` with `permit` technique to enforce using only the parameters we allow the user to change which are the `title` and `body`. We then find the question by its id and update it with the parameters using the ActiveRecord `update` method. After that, we redirect to show page.

In most cases we would want to show the edit page again if the update fail, for that we need to modify the `update` action to do so:
```ruby
def update
  question_params = params.require(:question).permit(:title, :body)
  @question       = Question.find params[:id]
  if @quesiton.update question_params
    redirect_to question_path(@question)
  else
    render :edit  
  end
end
```
In the code above we render the `edit` template again if the update fails by returning `false` and we redirect to the show page otherwise.

## DESTROY action
Let's implement the `destroy` action that enables us to delete a question from the database. We start by implementing the route:
```ruby
delete "/questions/:id" => "questions#destroy"
```
We then implement the action in the controller:
```ruby
def destroy
  question = Question.find params[:id]
  question.destroy
  redirect_to questions_path
end
```
In the code above we find the question by its id and then destroy it using the ActiveRecord `destroy` method. After that we redirect the user to the question listing page.

We can create a link in the `show` template:
```erb
<h1><%= @question.title %></h1>

<p><%= @question.body %></p>

<p><%= @question.created_at %></p>

<%= link_to "Edit", edit_question_path(@questions) %>
<%= link_to "Delete", question_path(@questions), method: :delete, data: {confirm: "Are you sure"} %>
```
Note that normal links in web page send only `GET` requests. Rails makes it possible to make a link send non-GET requests by using Javascript with jQuery. So the code above requires Javascript to be enabled and jQuery included (jQuery is included with all Rails projects by default). If you want the link to send a `DELETE` request without using Javascript you can switch the `link_to` to `button_to` which makes a form with one button and uses a hidden `input` field with name `_meothd` and value `DELETE` to mimic sending a `DELETE` HTTP request. The downside of using a form is that you may have to do extra work to style it consistently with other buttons on your website.

## Refactoring
Let's do some improvements to our `CRUD` by refactoring a few parts of it.

### Using `resources` for Routes
We can simplify our Routes by using `resources` instead of using individual routes so you can have in your `routes.rb`:
```ruby
resources :questions
```
This will generate routes for all the actions we covered earlier: `new`, `create`, `show`, `index`, `edit`, `update` and `destroy`. So you can delete all the individual routes we added earlier. Note that we intentionally written the routes in a way to match the ones provided by Rails `resources` method. You can double check by running:
```shell
rails routes
```
You can also visit: `http://localhost:3000/rails/info` to see the generated routes.

### Refactoring `question_params`
We notice that the `create` and `update` actions both use similar code for handling parameters. In Ruby calling a local variable or local method is done the same way so we can simply refactor by removing the variable definition lines and defining a private method to handle that:
```ruby
# in QuestionsController
  # ...
  private

  def question_params
    params.require(:question).permit(:title, :body)
  end

end
```
We can now remove the `question_params` variable definition from the `create` action so it becomes:
```ruby
def create
  @question = Question.new question_params
  if @question.save
    redirect_to question_path(@question)
  else
    render :new
  end
end
```
We can do the same for the update action.

### Refactoring Finding a Question
We notice that the `show`, `edit`, `update` and `destroy` actions all use the same line of code to fetch a question from the database with its id. We can refactor that using a `before_action` which defines a method that gets called before the code within the action:
```ruby
class QuestionsController < ApplicationController
  before_action :find_question, only: [:show, :edit, :update, :destroy]

  # ... ACTIONS HERE

  private

  def find_question
    @question = Question.find params[:id]
  end
end
```
We can then remove the line `@question = Question.find params[:id]` from the actions. For instance the show action becomes:
```ruby
def show
end
```
We will just need to change the `destroy` action to use an instance variable instead of a local variable:
```ruby
def destroy
  @question.destroy
  redirect_to questions_path
end
```

### Refactoring `new` and `edit` Forms
We notice that the `new.html.erb` and `edit.html.erb` templates have the same form code. We can refactor using a partial so we create a partial file `app/views/questions/_form.html.erb` which contains:
```erb
<% if @question.errors.any? %>
  <ul>
    <% @question.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
  </ul>
<% end %>

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
Then we can simply use the partial in the `new.html.erb` template as in:
```erb
<h1>New Questions</h1>

<%= render "form" %>
```
And in the `edit.html.erb` template as in:
```erb
<h1>Edit Questions</h1>

<%= render "form" %>
```
Note that partials file names must start with `_` per Rails convention. Also, note that partials have access to instance variables the same way as templates.

## Displaying Flash Messages
You may want to display a temporary message for users right after the complete an action. For instance if the user created a question you can display "Question created successfully". You can do that using `flash` object in Rails which stores the message you give it in the session and then removes it as soon as it's displayed.

We start by displaying the flash messages in the `app/layouts/application.html.erb` layout file:
```erb
<!DOCTYPE html>
<html>
<head>
  <title>GitcastCrud</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= flash[:notice] || flash[:alert] %>

<%= yield %>

</body>
</html>
```
Note that we distinguish between two kinds of `flash` messages in the code above to help us style it later so we can show notices in green and alerts in red for instance. You can define others if you'd like.

Then you can set the flash message from within the action:
```ruby
def create
  @question = Question.new question_params
  if @question.save
    flash[:notice] = "Question created successfully"
    redirect_to question_path(@question)
  else
    render :new
  end
end
```
Or when doing a `redirect` you can use a shortcut:

```ruby
def create
  @question = Question.new question_params
  if @question.save
    redirect_to question_path(@question), notice: "Question created successfully"
  else
    render :new
  end
end
```
