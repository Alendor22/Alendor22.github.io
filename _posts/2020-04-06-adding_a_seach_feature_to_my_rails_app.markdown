---
layout: post
title:      "Adding a seach feature to my Rails App."
date:       2020-04-06 13:17:23 -0400
permalink:  adding_a_seach_feature_to_my_rails_app
---

     During my Assesment for my Rails App for the live coding portion I was asked to implement a search feature.  Unfortunately only had about 10 minutes to do so and I could not complete it in the time alotted, so I was given until the next day 10:30am to complete it.  The first thing I set out to do was create the form, for this I decided on using a form_tag, this was inserted into the index.html.erb view, see below for the code.
```
		 <%= form_tag user_emails_path, :method => "get", id: "search-form" do %>
    <%= label_tag(:Search, "Please enter a letter the subject begins with to initiate a search:") %><br>
    <%= text_field_tag :search, params[:search], placeholder: "Enter letter"  %><br>
    
    <%= submit_tag("Search", :name => nil) %>
  <% end %>
	```
     Next I decided to create the method in the model, I chose a scope method which is a class method that utlizes Active Record search query methods like "where", and "sort", see below for the scope method.

`scope :search, -> (subject) {where('subject LIKE ?', "%#{subject}%")}`

     After I had the scope and form I thought the next step would be to add some logic to the index controller action, the below code is how I would initiate a search via the controller.
```
def index
    if params[:search]
      @emails = Email.search(params[:search])
    else 
      @emails = Email.all.order('created_at DESC')
    end
end
```
	
Finally I needed a way to view the search results so I created the following in the index.html.erb file as well.
	```
	<p>Seach Results</p>
 <ul>
<% @emails.each do |email| %>
  <li><%= link_to email.subject, user_email_path(current_user, email) %></li>
<% end %>
</ul>
```
     With the search feature up and running, now it was time to see if it all worked, so I input a letter, eureka!  The search feature worked as it should and I was satisfied, however, as I ran into some issues while attempting to get the feature working I did not pass the review.  The first error I hit was the method in the form_tag, I had forgotten to ensure that the method was a "get" request as form_tag defaults to "post".  This error plauged me throughout the course getting the feature up and running.  I was under the impression that a simple refresh would pull the new code down onto the server, but I was mistaken.  A new request to the server was required via the URL bar to ensure the form data  was reloaded and the method was refreshed as well.  I had never seen this error, before and cost me the pass on the Rails Review.  With a review failure under my belt I felt like I hit a setback in my coding process, so I took the day off to regroup.  I am still frustrated with my performance, but I undeerstand that this is part of the learning process.  If you always do everything correctly how much exactly are you learning?  So with my re-assessment coming up I plan on being better preparred to ensure I pass this time around.
