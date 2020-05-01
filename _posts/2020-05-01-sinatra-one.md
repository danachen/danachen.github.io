Sinatra is the first framework I was exposed to. Even though it's routed as a light-weight alternative to Rails, there's still enough nooks and crannies to make it tricky to work with for a beginner. I struggled for a long time to get to grips with the routing conventions as wel as the DSL (domain specific language). 

It was one thing to keep a logical data structure in mind while developing, keeping your eye on the session object (if you are developing without a database, which for beginners, is not a bad idea), it was another to trouble shoot your way through the DSL that interacts with the HTML, where one missing bracket or an equal sign could send the whole app to the deep end.

What I also ran into on an intermittent basis, which many do when working with any Ruby package managers, is the issue of gem version incompatibilities. Without warning, adding a gem and running a `bundle install` command is enough to trigger an avalanche of error messages.

After multiple attempts, I want to finally try to chronicle the process of Sinatra development for a newbie, and the myriad of issues that one comes across, and the attempts to debug and work through those issues. 

## The first project: A contact tracking app

### What the app should do: 
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
1. Set up the development environment
2. Add some template elements
3. Set up debugging
4. Get used to basic erb (our DSL of choice here) templating language
5. Add basic `get` and post `routes`
6. Pattern for data validation

### Setting up the development environment
Before working with Sinatra, it's important to make sure the basic Ruby development environment isn't going to give you a lot of issues. I have encounter various error messages detailing either gem conflicts, unsupported version, etc. With enough Googling and patience, most are solvable.
1. Google the error message to see if the error is a well-known one. In most cases, running a `gem update gem_name` might just solve the issue.
2. The `bundler` itself might need an update, in which case `Bundler update` might do the trick.
3. There's also the case where the 

