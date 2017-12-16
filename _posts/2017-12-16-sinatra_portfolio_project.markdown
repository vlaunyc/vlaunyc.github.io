---
layout: post
title:      "Sinatra Portfolio Project "
date:       2017-12-16 18:07:32 +0000
permalink:  sinatra_portfolio_project
---


While coming up with ideas of what sort of app I wanted to create, I knew it would have something to do with traveling and bringing people together for adventures. Thus, I created [Travelbuddy](https://github.com/vlaunyc/travelbuddy)! 

Travelbuddy is an app were users can create an account/sign in, view what others have posted on their travels, and comment on their post. Or they can create their own post by selecting the destination(s), dates of travel, writing content of their travels, posting it and reply with comments on posts. A User can delete their own post only, and any comments under that post will be removed regardless of the author. Users can delete their own comment from a published post.

Never have I ever created something that was so satisfying when [kind of, almost, basically] finished. In a way it reminds me of an art form, where when painting, your painting is never actually finished, it’s left at a state but could be worked on and improved. Just as your program could always be better in some way. But unlike painting, the process of programming made me feel utterly stupid and cause so much frustration, confusion, and insecurity. I’m discovering that programers are indeed problem solvers, who are also digital masochists facing error messages and broken code, only to fix it and discover more errors elsewhere. Yet it’s so fun and satisfying! I guess I’m admitting that I’m a masochist.

I’ve learned a lot while working on this app for the past week. Starting from scratch, file structure, to database structure and table relationships… There were many, many struggles. There were a few nights where I would roll around in bed thinking about the project and at last just get up (at 3am) and work on it (since I wasn’t falling asleep anyways). It was fun and infuriatingly fun. I enjoyed piecing every part together to get the entire thing to function and work. Then I spent the past two days finessing the look of it and working on user experience and interface.

There were times where it should work in theory, but didn’t. For example I spent hours trying to figure out why my `flash[:message]` sometimes worked and sometimes didn’t. From reformatting the messages, looking line by line making comparison between the message that did flash and the message that didn’t flash, to deleting the database, bundle installing over and over… Wondering if it’s a failure to save the message problem, or a problem with bootstrap, or the universe telling me something… I finally asked for help and was told everything looks right and it ‘should’ work. I then went back to helplessly searching google. A while later, I discovered a post by another student back in the playlister lab. Where she switched to Sinatra::Flash instead of Rack::Flash and resolved her issues. It was an alternative that worked!

These moments of success are so satisfying that makes learning enjoyable. 

**Other highlights:**
* Found an awesome app [DB Browser for SQLite](http://sqlitebrowser.org/) to view database structure and browse the data.
* Discovering free themes for [Bootstrap](https://bootswatch.com/) that look great and are easy to install.
* Finding so many resources when stuck and improving googling skills. 

**Areas that could use improvement:**
* View pages contain Ruby, HTML, CSS, Jquery scrips, this is way too much and so confusing!  View files need to be cleaned up and separated out.
* Finding a different solution for present time instead of doing all the calculations.  Maybe a local time gem? 
* Improving user flow from page to page of the program. Looking at wire framing of an app before diving into coding it.
* Allow the user to have more control over thier account such as changing their own username and password or deleting their account. 

## Demo Video:
<iframe width="560" height="315" src="https://www.youtube.com/embed/ofpHecIz01c" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
