Sinatra is the first framework I was exposed to. Even though it's routed as a light-weight alternative to Rails, there's still enough nooks and crannies to make it tricky to work with for a beginner. I struggled for a long time to get to grips with the routing conventions as wel as the DSL (domain specific language). 

It was one thing to keep a logical data structure in mind while developing, keeping your eye on the session object (if you are developing without a database, which for beginners, is not a bad idea), it was another to trouble shoot your way through the DSL that interacts with the HTML, where one missing bracket or an equal sign could send the whole app to the deep end.

What I also ran into on an intermittent basis, which many do when working with any Ruby package managers, is the issue of gem version incompatibilities. Without warning, adding a gem and running a `bundle install` command is enough to trigger an avalanche of error messages.

After multiple attempts, I want to finally try to chronicle the process of Sinatra development for a newbie, and the myriad of issues that one comes across, and the attempts to debug and work through those issues. 

# The first project: A contact tracking app

## What the app should do: 
This is the most basic of the CRUD app, and we aren't doing anything fancy with the first one.
The app should provide the interface to input and display the following information:
- A unique identifier for each contact name (not visible to the user)
- Contact name
- Contact address
- Contact postal address
- Contact phone
- Contact email
- Time of contact added

We are going to work on the app in stages
1. [Set up the development environment](##setting-up-the-development-environment)
2. [Add some template elements](##adding-template-codes)
3. [Filling up the templates with some baseline code](##some-baseline-code)
4. [Set up debugging](##setting-up-debugging)
5. [Get used to basic erb (our DSL of choice here) templating language]
6. [Add basic `get` and post `routes`]
7. [Pattern for data validation]

<a name="setting-up-the-development-environment">
## Setting up the development environment
</a>
Before working with Sinatra, it's important to make sure the basic Ruby development environment isn't going to give you a lot of issues. I have encounter various error messages detailing either gem conflicts, unsupported version, etc. With enough Googling and patience, most are solvable.
1. Google the error message to see if the error is a well-known one. In most cases, running a `gem update gem_name` might just solve the issue.
2. The `bundler` itself might need an update, in which case `Bundler update` might do the trick.
3. There's also one time where I mistakenly intalled both RVM and RBENV on the system, and that raised some serious havoc. The only way out was to delete one of the package manager directories altogether (in my case, `rvm implode`).
4. There might also be compatibility issues withe specific versions of a gem. If no amount of updates fixes the issue, I usually just go ahead and `gem uninstall problem_gem`. If needed again, I install it again.

In short, issues with package managers is common and happens to everyone. One thing that set my mind to rest was when a software engineer from Github told me that this is such a headache that the company has a small team of support engineers whose sole purpose was to set up problem-free development environments for their engineer team. So there.

## Adding template codes
A Sinatra app typically requires at minimum, the following components to work.
```
├── Gemfile
├── Gemfile.lock
├── main.rb
└── views
    ├── add_contact_form.erb
    ├── contacts.erb
    └── layout.erb
```
Here's what they do:
- `Gemfile` tracks all required gems and ensures they are all downloaded from the source
- `Gemfile.lock` is an auto-generated file that lists all the gems that the required gems also depend on. When `bundle install` is ran, this file is regenerated
- `main.rb` is the main Ruby file that houses the program logic. When ran locally, this is also the file ran to run the program with command `ruby main.rb`
- `views` is the holder that controls how the logic is displayed. Typically, erb (or another DSL) works together with Ruby code to provide the front end experience

What's not displayed but also necessary later on
- There's typically a `public` folder that houses the necessary `jss`, `html`, and `css` that are responsible for more intricate styling and interactions
- There might also be one or more folders that house additional assets (images), or data files
- `config.ru` and `Procfile` are also necessary when the code is deployed

## Some baseline code
Let's say all the files listed above are empty at the moment. It's good to fill up some of them with some baseline code, since most of these are on a template level and should be required at the start of every project.

1. The `main.rb` file should have the following three blocks of code. 
- Require a number of gems needed during development, including the `sinatra` gem, the `sinatra-reloader` gem if the code is in development - this will ensure the page on the local environment is continuously reloading based on a change in code.
- Require `tilt/erubis` which is a gem that ensures the templating functionalities work. We will have a `layout.erb` that propagates across the whole site. The gem `sinatra/content_for` supports the `tilt` gem in **`yielding` content** across the various view pages.
- `pry` is a great debugging gem that will come in very handy when you start wrestling with the session variable

```ruby
# main.rb
require "sinatra"
require "sinatra/reloader" if development?
require "tilt/erubis"
require "sinatra/content_for"
require "pry"

configure do
  enable :sessions
  set :session_secret, 'secret'
  set :erb, escape_html: true
end

before do
  session[:contacts] ||=[]
  @contacts = session[:contacts]
end
```
The code block `configure do` does something to make up for the lack of a database for our application. We enable a Sinatra 
session, which is a way for our app to set a cookie that persists an identifier across multiple HTTP requests. This way, information persists across multiple HTTP requests in an app.

The first line in the `configure do` block turns the sessions on. The next line is an encryption key that will be used to store a `session_id`. The `session_id` is a unique alphanumeric value to a given user's session, and is stored in the browser cookie.

In reality, the `session_secret` should be kept in an environment variable and not part of the code. We need to specify it here, since if no `secret` is specified, every time Sinatra starts or stop, the `secret` will be reset, and any existing session will be invalidated. To prevent it from randomly generating a new `secret` every time, we set it here.

The next line we'll talk about at a later point. 

Next, since the app will make use of a variable that tracks contacts as the central data structure, we need to have this data structure made available. We can equal the variable to whatever data gets entered through the app, but what happens to the variable before any data is available? The variable will cause an error when it returns `nil`. We want to give the list a default value of an empty array.

Now, we can declare this variable in every route, or more sensibly, we place it in a `before` block. `||=` just means the `session[:list]` variable is either itself (if it exists), or an empty array.

One more sideline before we move on from the `before` block. This block is invoked before any route is executed. Typically, those blocks are used to check the validity of a session, or in this case, ensure the session variable exists.

## Setting up debugging
The last thing I like to do before starting to write routing code is setting up debugging. I use the `pry` gem to dig through problematic code, and it's simple to use. Just `require 'pry'` on top of the `main.rb` file, and add `gem 'pry'` to the `Gemfile`, execute `bundle install`, and you are done.

If the need arises for debugging, just insert `binding pry` under the code that you want to see executed, and you can access all variables within scope at that point. To get out of a global variable, press `:q`, and to get out of the session altogether, press `!!!`.

At this point, the code repository looks like [this](https://github.com/danachen/contact-sinatra/tree/734301510925cfb725882a9c6ddfe2a3386503e6).

## Setting up the first routes

