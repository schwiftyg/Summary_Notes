# Cancancan

## in `gemil file  add 
```ruby
gem 'cancancan', '~> 1.15'
```

## run bundle
```
bundle
```

## add cancan:ability

```
rails g cancan:ability
```

### edit ability.rb file
```
class Ability
  include CanCan::Ability

  def initialize(user)
    # Define abilities for the passed in user here. For example:
    #
    #   user ||= User.new # guest user (not logged in)
    #   if user.admin?
    #     can :manage, :all
    #   else
    #     can :read, :all
    #   end
    #
    # The first argument to `can` is the action you are giving the user
    # permission to do.
    # If you pass :manage it will apply to every action. Other common actions
    # here are :read, :create, :update and :destroy.
    #
    # The second argument is the resource the user can perform the action on.
    # If you pass :all it will apply to every resource. Otherwise pass a Ruby
    # class of the resource.
    #
    # The third argument is an optional hash of conditions to further filter the
    # objects.
    # For example, here the user can only update published articles.
    #
    #   can :update, Article, :published => true
    #
    # See the wiki for details:
    # https://github.com/CanCanCommunity/cancancan/wiki/Defining-Abilities

    alias_action(:create, :read, :update, :destroy, to: :crud)

    can(:crud, Question) do |question|
      user == question.user
    end

    can(:crud, Answer) do |answer|
      user == answer.user
    endk
    
  end
end
```
### modify `questions_controller.rb`

before add

```
 before_action :authorize_user!, only: [:update , :destroy ]
 ```

 after add 

```
def authorize_user!
      ## redirect_to root_path, alert: "Not Authorized!" unless can?([:update,:destroy], @question)

      redirect_to root_path, alert: "Not Authorized!" unless can?(:crud, @question)
   
end
```


### modify  `answer_controller.rb`

before add

```
 before_action :authorize_user!, only: [:update , :destroy ]
 ```

after add 

```
    private

    def find_question
        @question = Question.find params[:question_id]
    end    

    def find_answer
        @answer = Answer.find params[:id]
    end

    def answer_params
        params.require(:answer).permit(:body)
    end
    
    def authorize_user!
        redirect_to root_path, alert: 'Not Authorized' unless can?(:crud, @answer)
    end
```    




# Deploying to Heroku
Heroku is one of the easiest ways to take your Rails application live. Here are the steps to deploy your application to Heroku.

## Git
Heroku deploys with Git so make sure that you have added git to you project. You can follow a tutorial or previous notes to do that.

## Create an Account
Create an account on Heroku: [http://heroku.com/](http://heroku.com/)

## Download the Heroku Toolbelt
The toolbelt makes deploying to Heroku much easier: [http://toolbelt.heroku.com/](http://toolbelt.heroku.com/)

## Add the `rails_12factor` Gem
Adding the `rails_12factor` gem helps in some aspect of taking your application live: [https://github.com/heroku/rails_12factor](https://github.com/heroku/rails_12factor):

```ruby
gem 'rails_12factor', group: :production
```
Then run `bundle` to install the gem.

## Create Heroku Project
Once your ready and made your last commit. Start by creating an app on Heroku:
```shell
heroku create my-awesome-app
```
Note that the name of your app such as `my-awesome-app` must be a valid subdomain name and it must unique at Heroku. Your application URL will be `http://my-awesome-app.herokuapp.com`.

When you first use Heroku on a computer, it will ask you for your Heroku email and password. Heroku will usually set up public keys so you don't have to re-enter email/password every time.

## Run Migrations
Heroku doesn't automatically run migrations for you so run migrations there from you machine:
```shell
heroku run rake db:migrate
```
Now you should be able to visit your app at `http://my-awesome-app.herokuapp.com`.

## Logs
If you want to display logs live to help you debug you can run:
```shell
heroku logs --tail
```