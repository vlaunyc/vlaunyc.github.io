---
layout: post
title:      "CLI Data Gem Project"
date:       2017-10-17 06:36:18 +0000
permalink:  cli_data_gem_project
---

**Hunger strikes. Pizza wins.**<br>
My initial project idea was scraping top 10 art galleries from different neighborhoods in New York City. After hours of frustration from the way the website was organized I started to get hungry… craving pizza. It was 2AM and I decided to toss my project, start fresh and select a pizza website to scrap from instead. Unfortunately all the best pizza places by me were closed, but I did go to bed dreaming of pizza. 

**Finding the right one <3**<br>
It took a while for me to find a website that was well organized and easier to scrape from. One of the first ones I selected saved the restaurants in multiple pages. For example, restaurants 1-10 on one page, and then another link for 11-20, and another link for 21-30… etc. That was hard to use, especially since information was not organized in the same way for each restaurant.

The next website I tried to work with, had a “more +” button which was inserted at random parts of each div. This was a span that blocked off the rest of the listed information I was tying to retrieve. Since the span was placed in different places on the website. I had trouble scraping the data. My code just got more and more complicated as I had to create variables and methods just to avoid the span. In the end, I realized that it wasn’t worth it to write so much code for something so simple. I decided to search again for another website that was better organized and well structured. After clicking on the 11th page of google, I found a website that worked well with the CLI I had already written. Woo-hoo!

**class BestPizza::CLI**<br>
My program runs in the CLI and displays a list of the 26 best pizza restaurants in NYC. The user types the number of the restaurant they want to get more information on (or list to view the list of restaurants again, or exit to end the program). The CLI then populates with the restaurant’s neighborhood and description. The user then has the choice to repeat and view another restaurant or exit.

**The CLI controller has 5 methods:**<br>
* **#call**<br>
This method takes no arguments and calls on BestPizza::Restaurant.scrape_pizza , #list_pizza, and #menu

* **#list_pizza**<br>
Puts the index number and name of the pizza restaurant 

* **#menu**<br>
Collects the users input of the index number and puts the pizza restaurants name, neighborhood area, and description. It can also accept ‘list’ and list the name of restaurants again, or ‘exit’ to exit the program. If the user inputs something else, that is out of index range, the program will puts "Not sure what you're looking for, type 1-26, 'list', or 'exit’”.

* **#repeat**<br>
Asks the user if they would like to view another restaurant. If input is y, it will go back to #list_pizza. Any other response would move to the #thanks method.

* **#thanks**<br>
Thanks the user and exits the program.

**class BestPizza::Restaurant**<br>
This class scrapes the restaurants name, area, and description from the website; then pushes the parsed elements into an empty array, and has a .find method.

```
class BestPizza::Restaurant

	attr_accessor :name, :description, :area

	@@pizza_restaurants = []

	def self.pizza_restaurants
		@@pizza_restaurants
	end

	def self.scrape_pizza
		doc = Nokogiri::HTML(open("https://www.thrillist.com/eat/new-york/the-best-pizza-in-new-york-city"))

		doc.css("section.save-venue.saveable-venue").collect do |info| 
			pizza = self.new
			pizza.name = info.css('h1').text.strip
			pizza.area = info.css('h2').text.strip
			pizza.description = info.css('p.save-venue__description').text.strip

			self.pizza_restaurants << pizza
		end
	end

	 def self.find(id)
    self.pizza_restaurants[id.to_i - 1]
  end
end
```

The Thrillist website was very easy to scrape, as the information was well organized in sections and by heading tags.


**Lessons learned**<br>
1. Scraping data from an unorganized website really, REALLY sucks. I now have a greater appreciation for well organized data/code/everything. 
2. Starting with an outline and framework of how you imagine your program to work is a great starting point! Small steps will lead you there. Building out the interface first and then making things real, helped to visualize how the program should flow and how to make it work.
3. Google is God. It’s challenging when you just don’t know how to do something. (Even harder when you don’t even know what you don’t know.) Thank goodness for Google! Figuring out what you don’t know and being able to search for an answer is part of problem solving and learning. It’s part of the process of learning to program. 
