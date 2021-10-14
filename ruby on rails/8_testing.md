# TDD & MiniTest

## Test Driven Development Process

1. Write a test
2. Run all tests and see if any fails
3. Write code to make a test pass
4. Re-run tests
5. If tests pass, refactor code
6. Go to 1.

Often referred to as Red-Green-Refactor. When a test fails, it shows red. When you make a test pass, it shows green. Then, you refactor.

## Given-When-Then

Given-When-Then is a methodology of writing tests. Its approach is to divide writng a test into 3 steps:

- **Given** represents the state of the program before the test begins.
- **When** is the behaviour that we're specifying.
- **Then** represents the state of the program after the test ends.


## Testing with Minitest

You can use MiniTest to write simple tests in Ruby. It can be used with Rails as well.

## Setting Up
To set up MiniTest, simply install the gem:
```shell
gem install minitest
```

![Screenshot from 2021-10-14 09-49-13](https://user-images.githubusercontent.com/21187699/137361731-0e9dcbb9-f7a3-432f-a95e-b6d098237deb.png)

```
mkdir minitest
cd minitest
```


### make file  `AwesomeTest.rb`
``` ruby 
equire "minitest/autorun"


# To create a grouping of tests with MiniTest,
# create a class whose name ends in `Test` and inherits from MiniTest::Test
class AwesomeTest < MiniTest::Test

    def test_something
        
    end
    

end

```

```run test
ruby AwesomeTest.rb
```
![Screenshot from 2021-10-14 10-01-57](https://user-images.githubusercontent.com/21187699/137363444-cbac237a-d7e7-49fa-92a7-a406b1a26856.png)

change 
```
 assert_equal(3, 1+1)
``` 
it should error message 

![Screenshot from 2021-10-14 10-04-39](https://user-images.githubusercontent.com/21187699/137363828-23461bf1-0373-4b7b-bf7e-3d75a46d3a96.png)


### make another file Vector.rb
```ruby 
class Vector
    attr_accessor(:x, :y)
    def initialize(x, y)
        @x = x 
        @y = y
    end
     
    def length
        Math.sqrt(@x ** 2 + @y ** 2)
    end
end
```

### make test this class  VectorTest.rb

```ruby
require "minitest/autorun"
require "./Vector.rb"

class VectorTest < MiniTest::Test
    def test_length
        # GIVEN - the initial state of our program
        # a vector of (3, 4)
        vector = Vector.new(3 ,4)

        # WHEN - an action is triggered
        # Then length method is called
        length_of_vector = vector.length

        # THEN - we verify the final state
        # The length should equal 5
        assert_equal(5, length_of_vector)
        
        # When using assert_equal, the argument order is as follows
        # 1 the value we expect or want
        # 2 the actual value our code returned

    end
    
end
```

run test

```
ruby VectorTest.rb
```
![Screenshot from 2021-10-14 10-23-17](https://user-images.githubusercontent.com/21187699/137366327-8d04204d-dafd-4db6-97d3-738c65e25455.png)


## modify Vector.rb add 
```
def to_s
        "Vector (#{@x}, #{@y})"
end
```
### modify VectorTest.rb add 

```
def test_to_s
        # GIVEN 
        vector = Vector.new(1, 1)
        # WHEN
        to_s = vector.to_s
        # THEN
        assert_equal("Vector (1, 1)", to_s)        
    end
```    

```
ruby VectorTest.rb
``` 
![Screenshot from 2021-10-14 10-34-51](https://user-images.githubusercontent.com/21187699/137367857-205d154b-914b-465a-8a43-a6accd555ff4.png)



Break



## Writing Your first test
Let's say you have a `Cookie` class in file `cookie.rb`:
```ruby
class Cookie

  def initialize(sugar, flour)
    @sugar, @flour = sugar, flour
  end

  def calorie_count
    @sugar * 5 + @flour * 4
  end

end
```
We Can start by writing the test in a file `cookie_test.rb` that looks like:
```ruby
require "./cookie.rb"
require "minitest/autorun"

class CookieTest < Minitest::Test

end
```
Note in the file above that we require `minitest/autorun` and then we inherit from  `Minitest::Test`. Requiring `minitest/autorun` makes it easy to run our tests.

### Writing the first test
We can then define methods in the above class as:
```ruby
def test_requiring_sugar_and_flour
  assert_raises(ArgumentError) { Cookie.new }
end
```
In the example above we're testing to make sure that when we call `Cookie.new` we get an exception `ArgumentError` because we expect that when we call `.new`, two arguments are passed for the sugar and flour amounts.

Note that for this to work with MiniTest autorun you must start the method name with `test_`.

Also, you must make sure that the test fails at least once. This way if you've already written the code in your `Cookie` class, make sure to comment it out and run your test again to make sure that the test fails under some conditions. Otherwise, you may have written the test in a way that always passes which defeats the purpose of having the test all together.

### Using `assert_equal`
The most common way to write tests is to use `assert_equal` as in:
```ruby
def test_calorie_count_calculation
  c     = Cookie.new(6, 10)
  count = c.calorie_count
  assert_equal(70, count)
end
```
The above code makes sure that the `calorie_count` method returns `70`. There are many more other types of assertions. Find a list of [Minitest assertions here.](http://docs.seattlerb.org/minitest/Minitest/Assertions.html)

## Running Tests
To run tests run the following in your terminal:
```shell
ruby cookie_test.rb
```
This will give you the following output:
```shell
# Running:

..

Finished in 0.001136s, 1761.0641 runs/s, 1761.0641 assertions/s.

2 runs, 2 assertions, 0 failures, 0 errors, 0 skips
```
If tests fail, you will see messages describing that in the outcome.
