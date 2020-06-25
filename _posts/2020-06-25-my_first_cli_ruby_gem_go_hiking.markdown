---
layout: post
title:      "My First CLI Ruby Gem go_hiking!"
date:       2020-06-25 23:17:26 +0000
permalink:  my_first_cli_ruby_gem_go_hiking
---


   I finished my very first Flatiron School project. It is hard to describe how satisfying that is.
   This project was to create a Command Line Interface (CLI) Ruby Gem. Only month and a half ago did I just learn what variable is and now I am about to create a Ruby Gem that will allow users to access some data. Sounds intimidating, right?? Well… It is.  
	
   ** So what is a CLI?** CLI (Command Line Interface) is a program on our computer that allows us to create and delete files, run programs, and navigate through folders and files.
	
   ** And what is a Ruby gem?** The software package is called a “gem” which contains a packaged Ruby application or library. Gems can be used to extend or modify functionality in Ruby applications.
 
 
                                                                   **How did I create Ruby gem?**
																																	 
 The hardest part was to start. Up to this moment, all of the labs would come with specs, README.md, tests to pass, the environment would be set up and I needed to focus on nothing but code. Now, it had to be created from scratch. 
     Before I decided what my gem’s functionality will be, I knew I wanted to use an API vs.  Web Scraping to get data I will use for this project. It seems to me that APIs are more common practice nowadays and  most companies offer APIs that will contain data that programmers will use in their applications. That is why I chose that route. 
		 
 Deciding on a theme was kind of easy. I live in Colorado, the state that offers an amazing outdoors’s life. My goal was to offer users (mainly Denver visitors) a way to quickly decide on a hike trail they want to go on.
    This website (https://www.hikingproject.com) has a nice list of great hikes. To get an API from them, I had to sign up and I received my API key that I used to get needed data. 
		
 To create a gem, I needed to type bundle gem <file name> in my terminal. This command pre-populated a basic directory with:
		
* •	Bin
* •	Lib
* •	.gitignore
* •	.gemspec
* •	Gemfile
* •	License.txt
* •	Rakefile
* •	README.md

Pre-populated directories saved me some headaches.

 Bin directory has a console file that is very important for the program. I created a separate file `run` that I will use for running my code. This file is as simple as in the picture below, but every line is crucial for evoking the app.
 
 
```
#!/usr/bin/env ruby

require_relative "../lib/environment"

puts "Hello!"

GoHiking::CLI.new.start

```



  The first line `#!/usr/bin/env` ruby is called `shebang`. This line provides the shell with the absolute path to the Ruby interpreter. If I was writing my code in a different language, I would just use its name instead of `ruby` in this line of code. Having this code, my users only need to type bin/run in terminal and they will evoke the app. 
   Lib directory was installed with go_hiking folder which contains a version.rb file. As its name says this file shows the version of my gem. Also, go_hiking.rb file was created. I renamed it to environment.rb. This file handles all of the `requiring`. 

```
require_relative './go_hiking/version.rb'
require "pry"
require "httparty"

require_relative "./api_manager"
require_relative "./hike"
require_relative "./cli"

module GoHiking
  class Error < StandardError; end
  # Your code goes here...
end

```








   Lib folder will be responsible for holding the other files I created when building classes. For this app, I thought it would be enough to have only 3 different files:
	 
* •	api_manager.rb – This file contains class ApiManager which will be getting data from HikingProject API.
* •	cli.rb – This file contains class CLI that is my controller class. I have #start method that will evoke other methods which will help me to show the list of trails when users use a command bin/run in their terminal.
* •	hike.rb – This file contains class Hike, which is the model class. Methods in this class are responsible for instantiating and creating new Hike objects. Each object will have attributes such as name, location, summary, difficulty, URL, hike_length.

    I created the ApiManager class first because I wanted to have real data from the beginning. It is easier for me to debug if I see the real results. I have been using `binding.pry` in labs  before, but to be honest, working on this project I finally understand how helpful `pry` gem is. 
		
   I initially started a class method and used ` puts “Hello!”` as my code.  `puts` can be helpful as `binding.pry`. When I ran `bin/run`, I got an error. Permission denied, it said. I froze. The fear of breaking the app was so scary. Right then I learned about executable files and what it means. The first thing I did was to run `ls –lah` command which listed files and if it didn’t end with ‘x’ than that file was not executable. To change that I needed to run `chmod +x <file name>` and voila! My program output “Hello!”
	 
 All of my files were linked properly and I was able to get to actual coding.  
 
                                                            **What did I learn working on this project?**
* •	I learned how to create a gem, link files necessary for running the program. 
* •	I feel comfortable using gem `pry` properly. 
* •	I found out about gem `HTTParty`, which can be used to query web services and examine the resulting output. By default it will output the response as a pretty-printed Ruby object. This can also be overridden to output formatted XML or JSON.
* •	`Heredoc` was useful in two of my methods. A heredoc is a way to define a multiline string, while maintaining the original indentation & formatting. I started with the symbol <<-, then a word that represents the name for this heredoc, the heredoc contents, then I closed the heredoc with that same word on its own line.


```
def display_instructions
        puts <<-INST

Choose a trail by number or type "exit" to exit the program!

        INST
    end

```



  I won’t write much about the actual coding process. I feel it is covered very well through the Flatiron curriculum. All the other aspects of this project were more stressful and I wanted to focus on them.
All in all, working on this project was proof that I chose the right  carrier path. I am aware of having a long run before calling myself a `full stack programmer`. But hey, I created a code that works!
You can check my gem here:   https://github.com/ValentinaPanic/go_hiking

