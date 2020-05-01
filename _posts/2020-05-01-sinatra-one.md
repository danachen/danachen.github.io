Sinatra is the first framework I was exposed to. Even though it's routed as a light-weight alternative to Rails, there's still enough nooks and crannies to make it tricky to work with for a beginner. I struggled for a long time to get to grips with the routing conventions as wel as the DSL (domain specific language). 

It was one thing to keep a logical data structure in mind while developing, keeping your eye on the session object (if you are developing without a database, which for beginners, is not a bad idea), it was another to trouble shoot your way through the DSL that interacts with the HTML, where one missing bracket or an equal sign could send the whole app to the deep end.

What I also ran into on an intermittent basis, which many do when working with any Ruby package managers, is the issue of gem version incompatibilities. Without warning, adding a gem and running a `bundle install` command is enough to trigger an avalanche of error messages.

After multiple attempts, I want to finally try to chronicle the process of Sinatra development for a newbie, and the myriad of issues that one comes across, and the attempts to debug and work through those issues. 

## The first project: A contact tracking app
