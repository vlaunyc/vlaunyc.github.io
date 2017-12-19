---
layout: post
title:      "Sinatra Portfolio Project "
date:       2017-12-16 13:07:33 -0500
permalink:  sinatra_portfolio_project
---


While coming up with ideas of what sort of app I wanted to create, I knew it would have something to do with traveling and bringing people together for adventures. Thus, I created [Travelbuddy](https://github.com/vlaunyc/travelbuddy)! 

Travelbuddy is an app where users can create an account or sign in, view what others have posted, comment on their posts, and create a new post. They can create their own post by selecting destination tags, inputting travel dates, posting it and replying with comments. A user can only delete their own comments or posts. However, a deleted post with comments will be removed regardless of the author.

Never, have I created something that was so satisfying when finished. In a way it reminds me of a piece of art, where when painting, your painting is never actually finished, it’s left at a state but could be worked on and improved. Similarly, your program could always be better in some way. But unlike painting, the process of programming made me feel utterly stupid and caused so much frustration, confusion, and insecurity. I’m discovering that programmers are indeed problem solvers, who are also digital masochists facing error messages and broken code, only to fix it and discover more errors elsewhere. Yet, it’s so fun and satisfying! I guess I’m admitting that I’m a masochist.

I’ve learned a lot while working on this app for the past week. Starting from scratch, file structure, database structure, and table relationships. There were many, many struggles. Some nights I would roll around in bed thinking about the project and eventually find myself (at 3am)  working on it (since I wasn’t falling asleep anyway). It was fun, infuriatingly fun. I enjoyed piecing every part together to get the entire program to function and work. Then I spent the past two days finessing the look of it and working on user experience and interface.

There were times where it should have worked in theory, but didn’t. For example, I spent hours trying to figure out why my flash[:message] sometimes worked and sometimes didn’t. From reformatting the messages, looking line-by-line, making comparisons between the message that flashed and the message that didn’t flash, to deleting the database, bundle installing over and over… Wondering if it’s a failure to save the message problem, or a problem with bootstrap, or the universe telling me something… I finally asked for help and I was told everything looks right and it ‘should’ work. I then went back to helplessly searching google. Awhile later, I discovered a post by another student back in the playlister lab. Where she switched to Sinatra::Flash instead of Rack::Flash and resolved her issues. It was an alternative that worked!

**Other highlights:**
* Found an awesome app [DB Browser for SQLite](http://sqlitebrowser.org/) to view database structure and browse the data.
* Discovering free themes for [Bootstrap](https://bootswatch.com/) that look great and are easy to install.
* Finding so many resources when stuck and improving Googling skills. 

**Areas that could use improvement:**
* View pages contain Ruby, HTML, CSS, Jquery scripts, this is way too much and so confusing!  View files need to be cleaned up and separated out.
* Finding a different solution for present time instead of doing all the calculations.  Maybe a local time gem? 
* Improving user flow from page to page of the program. Looking at wire framing of an app before diving into coding it.
* Allow the user to have more control over their account such as changing their own username and password or deleting their account. 

**Demo Video:**
<iframe width="560" height="315" src="https://www.youtube.com/embed/ofpHecIz01c" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
