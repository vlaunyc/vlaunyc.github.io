---
layout: post
title:  "EVER WONDERED HOW TO CHANGE THE LOOK OF YOUR TERMINAL?"
date:   2017-07-24 20:41:24 -0400
---

(FOR MAC USERS) 
If your black and white terminal bothers you as much as it bothered me, here’s a few steps on how to change that!

**HOW TO EDIT .BASH_PROFILE**
**Step 1:** 
Open your Terminal.app 


**Step 2:**
Type `**nano .bash_profile**` (This command will open the .bash_profile document or create it if it doesn’t already exist) 


**Step 3**
Now you can make a add code to change to your terminal.

This is the layout and look I decided to go with: <br>
![](http://i.imgur.com/dMdazri.png)
<br>

To get the above, paste these lines of code to change your Terminal prompt. 

```
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\[\033[44m\]\[\033[37m\][\@]\[\033[00m\]\[\033[31m\] \$(parse_git_branch) \[\033[32m\] \W\[\033[37m\]
🐢 "
```

**Breaking it down:**
Let's first talk about color. The color tag is `\[\033[##m\]`

`##` = where you enter the color value. 

Here are the color values for foreground text:

* Black: 30
* Blue: 34
* Cyan: 36
* Green: 32
* Purple: 35
* Red: 31
* White: 37
* Yellow: 33

Here are the color values for background colors:

* Black background: 40
* Blue background: 44
* Cyan background: 46
* Green background: 42
* Purple background: 45
* Red background: 41
* White background: 47
* Yellow background: 43



**Here's a breakdown of the above PS1 and the properties I wanted:**
(PS1 stands for Promt String 1. When you open terminal, it will display the content defined in PS1 variable in your bash profile.)  

`\[\033[44m\]\[\033[37m\][\@]`   <-- this customizes the blue background color and foreground text color to white,  `\@` is 12-hour time in AM/PM format. You can also use `\t` for 24-hour HH:MM:SS format; `\T` for 12-hour HH:MM:SS format; or `\A` for 24-hour HH:MM format.

`\[\033[00m\]\[\033[31m\] \$(parse_git_branch)`  <-- this customizes the background color to nothing and foreground text color to red; and `parse_git_branch()` function extracts the branch name of your git repository.

`\[\033[32m\] \W`  <-- this customizes the foreground text color to green *(I then adjust it to aqua in my Preferences>Profiles>Text settings and adjusting ANSI Colors)* and `\W` is the current working directory base name (current folder only; if you want it to show all folders use lowercase  `\w`)  

`\[\033[37m\] 🐢 `  <-- this customizes the foreground text color to white and you can even use an emoji by going to Edit > Emoji & Symbols, and then selecting the Emoji you'd like to use (the turtle emjoi is my favorite.) 



**Step 4 - Save:**
Now save your changes by typing **ctrl + o**.  Hit **return** to save.  Then exit Nano by typing **ctrl + x**.
<br><br>
**HOW TO PERSONALIZE PROFILE COLORS**
In Terminal > Preferences > click the '+' to add a New settings.
Click on each color-chip and adjust to preferred color. See image below for reference:

![](http://i.imgur.com/PRKdrla.png)

You can also download a color scheme too. I based my colors on [Solarized - by Ethan Schoonover](http://ethanschoonover.com/solarized) and adjusted to my preference.
<br><br>
<hr />
**Resources:**<br><br>
https://natelandau.com/my-mac-osx-bash_profile/

https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/

https://coderwall.com/p/fasnya/add-git-branch-name-to-bash-prompt

http://misc.flogisoft.com/bash/tip_colors_and_formatting




