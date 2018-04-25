---
layout: post
title:      "Making Skincare - My Rails App now with a jQuery Front End!"
date:       2018-04-25 06:12:56 +0000
permalink:  making_skincare_-_my_rails_app_now_with_a_jquery_front_end
---

This project expands upon the rails assessment that I did previously (you can read more [here](http://vicky-lau.com/rails_portfolio_project_making_skincare)). The goal was to add dynamic features through jQuery and a JSON API. Some of the requirements involved rendering an index and show page using jQuery and JSON, creating a resource and rendering the response without a page refresh, a has-many relationship in the JSON, etc..
<br><br>

## **DEMO OF MY RAILS APP WITH JQUERY FRONT END** 
<iframe width="600" height="338"  src="https://www.youtube.com/embed/sVbDJ5y8Q-M?rel=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


## **PROJECT REQUIREMENTS AND HOW I IMPLEMENTED THEM** 

##### [x] Use jQuery for implementing new requirements

##### [x] Include a show resource rendered using jQuery and an Active Model Serialization JSON backend.
- /skinconcerns/1 have "next" and "previous" buttons that navigate to the following skin concern show page by alphebetical order of skin concern name.

- /categories/1 have "next" and "previous" buttons that navigate to the following category show page by numerical order of category name

- /users/1 have a "view formulas" button that appends all of the users formulas to the page with out a page refresh.

##### [x] Include an index resource rendered using jQuery and an Active Model Serialization JSON backend.
- /formulas index page has 'see description' link under each formula. On click it appends the formulas description.

##### [x] Include at least one has_many relationship in information rendered via JSON and appended to the DOM.
- Category `has_many` Formulas - renders next and previous category on button click

- Formula `has_many` Comments - renders new comment in div after submitting a form

- User `has_many` Formulas - renders users formulas on "view formulas" button click

- Skinconcerns `has_many` (and belongs to many) Formulas - renders next and previous skin concern on button click

- Formula `has_many` Ingredients - renders new form field for ingredient on click of 'Add Ingredient' button

###### [x] Use your Rails API and a form to create a resource and render the response without a page refresh.
- When a user adds a comment to a formula, the comment is serialized and submitted via a (AJAX) POST request with the response being the new object in JSON and then appending the new comment to the DOM without a page refresh.

##### [x] Translate JSON responses into JS model objects.
- Upon submission of a comment; the id, user, and content of new comment was used to create a JS comment object.

- Formula's date on users show page is taken from JSON response and turned into a JS date object  and appended to the page.

##### [x] At least one of the JS model objects must have at least one method added by your code to the prototype.
- `Comment.prototype.renderComments `-- The data of a comment is passed into the `renderComments()` function and appended to the DOM.


## **DYNAMIC INGREDIENT INPUT ON FORMULAS FORM** 
Previously I created dynamic form fields with just rails. But it would refresh the page on every 'add ingredient'. This time with javascript I was able to append the field without a page refresh. Creating dynamic forms with Javascript was a bit tricky. But here is how I implemented it.
```
//formula.js

//Below is code for formulas form, adding ingredient

//when the document is loaded
$(document).ready(function(){

	//and the user clicks on the addNewIngredient button
	$('#addNewIngredient').click(function(){    
	
		//create date object
		var date = new Date();
		
		//get number of milliseconds since midnight Jan 1, 1970  and use it for address key 
		var mSec = date.getTime();
		
		//Replace 0 with milliseconds 
		idAttributeIngredient = "formula_ingredients_attributes_0_name".replace("0", mSec);
		nameAttributeIngredient = "formula[ingredients_attributes][0][name]".replace("0", mSec);
		
		//Append the div for ingredient input with the unique attribute (gives each ingredient a unique id)
		$('div#ingredientsSet').append(
			"<div class='ingredientsForm input-group mb-3'>" +
			"<input class='form-control' type='text' name='"+ nameAttributeIngredient+"' id='"+idAttributeIngredient+"'>" +
				"<div class='input-group-append'>" +
					"<span class='input-group-text'>" +
						"<button type='button' class='removeIngredient close'> ×</button>" +
					"</span>" +
				"</div>" +
			"</div>"
		);
	});
  
	//when removeIngredent button is clicked
	$("div#ingredientsSet").on('click', '.removeIngredient', function(){
		//closest goes up thru the DOM, looking for the first ancestor with the class ingredientsForm and removes it
		$(this).closest('.ingredientsForm').remove();
	});
});
```

## **CREATING A COMMENT OBJECT IN JAVASCRIPT**
When a user adds a comment to a formula, the comment is serialized and submitted via an .ajax POST request.
```
$(function(){
  $("#new_comment").on("submit", function(e) {
    $.ajax({
      type: "POST",
      url: this.action, // this refers to whatever triggered the action
      data: $(this).serialize(), // takes our form data and serializes it
      success: function(response) {
        // update the DOM
        var comment = new Comment(response);
        comment.renderComments();
        $(".commentBox").val("");
      }
    });
    e.preventDefault();
  })
});

```
In the formula serializer, I created a custom attribute :comment_list
```
class FormulaSerializer < ActiveModel::Serializer
  attributes :id, :title, :description, :direction, :created_at, :updated_at, :image_file_name, :image_url, :comment_list
  has_one :user
  has_one :category

  def comment_list
    object.comments.map do |comment|
      {
        id: comment.id,
        user: {
          id: comment.user_id,
          username: User.find(comment.user_id).username
        },
        content: comment.content
      }
    end
  end
end
```
which returns JSON that is structured as below:
```
comment_list: [
	{
	id: 1,
	user: {
		id: 9,
		username: "organicisbest"
		},
	content: "hey this great! thanks for sharing!"
	}
]
```
On success, a new Comment is created with the response in the form of data
``` 
// here we set the data attributes to this
function Comment(data) {
  this.id = data.id;
  this.content = data.content;
  this.user = data.user;
};
```
and then we call 
```
comment.renderComments()
```
which brings us to this function:
```
Comment.prototype.renderComments = function() {
  var html = "" ;
  html += 
  "<br>" +
  "<div class=\'card' id=\'comment-" + this.id + "'>" +
    "<div class=\'card-body'>" +
      "<h6 class=\'card-subtitle mb-2 text-muted'>Posted by: "  + 
        "<a href='/users/" + this.user.id + "'>" + this.user.username + "</a>" + 
      "</h6>" + 
      "<p class=\'card-text'>" + this.content + "</p>"
    "</div>"
  "</div>";

  $("#submitted-comments").append(html);
};
```
Here we add the new comment's id, user.id, username, and content  in the html and append it to the page.  And then we call this line of code
```
$(".commentBox").val("");
```
Where we empty the comment box, so the user can add another comment. And that concludes the function for new comment.


## **CREATING A DATE OBJECT IN JAVASCRIPT**
This is a little easier to follow since it's all in one function.
```
// users.js

function loadUserFormula(data) {
  // only gives the array of users formulas.
  var formulas = data["formulas"]; 

  // set the div in DOM to variable usersFormulasDiv
  var usersFormulasDiv = $(".usersFormulas");

  // empty the div first
  usersFormulasDiv.empty();

  // set json to variables
  var userID = data["id"];
  var username = data["username"];

  // iterate over each formula in the formulas array
  $.each (formulas, function(formula) {
    // This is the date as saved in DB, returned in JSON
    var savedDate = formulas[formula].created_at; 

    // Here we create a new date object in JS
    var creationDate = new Date(savedDate);

    // options is needed by toLocaleDateString. This is how I formated the created at date for each formula
    var options = {month: 'short', day: 'numeric', year: 'numeric',  hour:'numeric', minute:'numeric', second:'numeric'};

    // This returns date in the format of: Apr 19, 2018 07:50:31 PM
    var localDate = creationDate.toLocaleDateString('en-US', options); 

    // Here we append the html with formulas info and the localDate which is a result of the new JS Date object
    usersFormulasDiv.append(
      "<div class='row'>" +
        "<div class='col-sm-8'>" +
          "<h5><a href='/formulas/"+ formulas[formula].id + "'>" + formulas[formula].title + "</a></h5>" + 
          "<h6>By: <a href='/users/" + userID + "'>" + username +"</a>, Created on "+ localDate +"</h6>" +
          "<p class='text-muted'>"+formulas[formula].description+"</p>" +
        "</div>" +

        "<div class='col-sm-4'>" +
          "<a href='/formulas/"+ formulas[formula].id + "'>" +
          "<img src='"+formulas[formula].image_url+ "'>" +
          "</a>" +
        "</div>" +
      "</div><hr>"
    )
  }); //end of $.each
}
```

## **CONCLUSION**
Overall I'm happy with the way my app turned out and learned a lot in the process. There are a few features I would like to add such as a user's ability to delete a comment that is theirs. Also channging the ingredients model to be a many to many relationship with formulas, creating a 3 column formula_ingredients tabel to save the id's and amt_of_ingredient. That way a user can look up formulas by an ingredient. But I will have to work on that at a later date. I'm excited to move forward onto React!

<hr>
## **RESOURCES**
Here are some resources that were helpful during my process

- Date.prototype.toLocaleDateString(): <br>
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString

- Adding paperclip image to JSON: <br>
https://stackoverflow.com/questions/5588185/how-can-i-get-url-for-paperclip-image-in-to-json

- The Detailed Guide on How Ajax Works With Ruby on Rails: <br>
https://launchschool.com/blog/the-detailed-guide-on-how-ajax-works-with-ruby-on-rails

- :first-child Selector <br>
https://api.jquery.com/first-child-selector/

- Appending input feilds: <br>
http://jsfiddle.net/8L8Zx/#run

- Adding unique ids to input feilds that were appended: <br>
http://jyrkis-blogs.blogspot.com/2014/06/adding-fields-on-fly-with-ruby-on-rails.html

- Sort Javascript Object Array By Date: <br>
https://stackoverflow.com/questions/10123953/sort-javascript-object-array-by-date

- Bootswatch style: Journal <br>
https://bootswatch.com/journal/



