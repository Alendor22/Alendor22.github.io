---
layout: post
title:      "Mod4 JavaScript Project Art-Gallery-App Challenges."
date:       2020-04-25 01:46:23 +0000
permalink:  mod4_javascript_project_art-gallery-app_challenges
---


     Here we are again at the end of another mod, and while I'd like to say I'm glad it's over, this mod had it's challenges and then there were hurdles, and further still more challenges.  I guess it's the expansiveness of JS as a language which can be intimidating.  What I mean by expansiveness is this - with javascript you can do things like sovle algorithmic problems as you can with other languages. With JS its ubiquity dominates this area, you can fetch, post, patch, and delete information on a website with JSON, and you can manipulate the DOM (Document Object Model).  Throughout this module I felt an undertow, sometimes subtle, sometimes not so subtle, As I began the module I described it like feeling as if I were in a boat on troubled waters without a paddle and with no land in site, in short - lost.
		 
		 So it would sufice to say that when it came to the mod4 project I felt equally as lost and over matched, but I refused to let JS get the best of me, I would have to learn this languge the hard way.  Through the bitter pill of experience, there's no better teacher, in my opinion.  I started project week having no clue what I was going to build, but I did not want to take what I thought was the easy way out of this project and build a blog.  There was plenty of video's and lectures doing just that I wanted to try a differnet tact.  so when I finally decided on what to build, it was only a matter of planning now.
		 
		 I decided I would build something sort of like an instagram clone, but for art.  I attempted to have the user upload their own files, but when it came to instantiating that behavior (associating a file to a user) the complexities became more than I could chew for the alotted timeframe.  So with that in mind I decided that active storage was the way to go to associate a user to an image to a user, again I was sadly mistaken.  Ruby on Rails in API mode doesn't work in the same way that Ruby on Rails does when not in API mode.  So that idea to was nixxed.  I finally came to the realization that the only way I was going to get images and associate them to a user in my case an artist was to pull them directly from the web.  I did this with "copy image address" when you right click the image with the mouse.  These images were saved as strings on my painting table and apply named URL.  With images in hand they were added to the JSON that was served from my back-end seed data.  
		 
		 When you start a project that you entend to use JS as the front-end language you setup the back-end first with Ruby on Rails in API mode, thats done by entering 
		 `rails new <app-name> --database=postgresql --api` 
--database=postgresql is used when you'd like to host your app on heroku.  then cd into the new folder and run `rail g scafffold` once this is done go to the Gemfile to uncomment `gem 'rack-cors'`, then navigate to `onfig/initializers/cors.rb` and uncomment the following section of code.

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

     After this setup your models by running `rails g model <model_name> name: string age:integer` and run `rails db:create` as postgresql is different from SQL as you have to create your database first, then run `rails db:migrate` after this is done run `rails db:seed` to seed your database.  I used name and age as examples above as the name would default to datatype of string anyway I just wanted to show what you could select, but any other datatype other than sting you have to specify.  It is highly engouraged that you use "namespacing" for the controllers.  This is done by first creating a folder usually called "API" then in that folder  create a folder called "v1" then you would put all of your controller files into v1.  The v1 stands for version 1, if you were planning on creating new features you would do so, in another folder apply called v1.1, so on and so forth.  after you create these folders you would now have to namespace the controller this is done like so `API::v1::AppController < ApplicationController`.  Once you are at this point take a look at the controller and all you have it should look something like this:
		 
		 ```
       class Api::V1::ArtistsController < ApplicationController
   
	          before_action :set_artist, only: [:show, :update, :destroy]

              def index
                @artists = Artist.all

                render json: @artists, include: [:paintings]
              end

              def show
                render json: @artist, include: [:paintings]
              end

              def create
                @artist = Artist.new(artist_params)

                 if @artist.save
                    render json: @artist, include: [:paintings], status: :created
                 else
                    render json: @artist.errors, status: :unprocessable_entity
                 end
              end

              def update
                if @artist.update(artist_params)
                  render json: @artist
                else
                  render json: @artist.errors, status: :unprocessable_entity
                end
              end

              def destroy
                if @artist.destroy
                  render json: {message: "Artist deleted!"}, status: 200
                else
                  render json: {message: "Artist failed to delete! "},  
				status: :unprocessable_entity
               end
		      end

             private
   
             def set_artist
               @artist = Artist.find(params[:id])
             end

              def artist_params
                params.require(:artist).permit(:name, :age, :gender)
              end
		
       end
```

     Once your API is set up its time to check your JSON, for that you start your server `rails s` and navigate to `localhost:3000/api/v1/artists`.  it should look like this:
		 
   ```
       // 20200424204939
// http://localhost:3000/api/v1/artists

[
  {
    "id": 1,
    "name": "Pablo Picaso",
    "age": "91",
    "gender": "M",
    "created_at": "2020-04-15T18:47:45.583Z",
    "updated_at": "2020-04-15T18:47:45.583Z",
    "paintings": [
      {
        }
	]
   ```
	 
	      Now that you have verified that you have your JavaScript Objects now its time to work on the front-end.  To do that you create another folder on the top level of your file structure named frontend, inside of this folder create a file called index.html, this is where all your bolierplate HTML will live.  Create another folder called javascripts inside of the frontend folder, inside the javascripts folder create a file called index.js, this is where you add
				
```
   document.addEventListener("DOMContentLoaded", function () {
				Painting.loadPaintings();
				Painting.listenForClick();
				Artist.loadArtists();
				Artist.addListenerToArtistForm();
	}
```

      This will wait for the page to load and execute the callbacks listed in the anonymous function.  which will load your page with the data from your back-end.  The main culprits that do the loading are `Painting.loadPaintings();` and `Artist.loadArtists();`, their methods look like this:
			
	```
	  static loadPaintings() {
    
    API.get('/v1/paintings')
     .then((paintings) => {
        paintings.forEach((data) => { 
        let painting = new Painting(data);
      
        Painting.renderPaintingFromTemplate(painting.paintingTemplate(painting));
        });
        Painting.deletePaintingAction();
      });
  }
	```
	
	and
	
	
	```
	static loadArtists() {
    API.get('/v1/artists')
    .then((artists) => {
      Artist.all = [];
      artists.forEach((data) => {
        let artist = new Artist(data);
        Artist.renderArtistFromTemplate(artist.artistTemplate(artist));
      });

      Artist.addArtistsToSelectDropDown();
      Artist.deleteArtistAction();
    });
    
  }
	```
	
	     Respectivly, they make use of another method that is located in the API class:
			 
	```
	static get(url) {
      return fetch(API.baseURL + url)
        .then(function (response) {
          if (response.status !== 200) {
            throw new Error(response.statusText)
          }
          return response.json();
        });
  }
	```
	
	     which is returning the response from the fetch to the back-end, which is a promise `.then` in JSON format.  We then, for lack of of better word, take that promise and pass it to another `.then` which is also a promise statement with the data that was returned, take that data interate over each artist and construct a new artist instance, pass that data to our `Artist.renderArtistFromTemplate` method and pass each artist to `(artist.artistTemplate(artist));`.  Then we call `Artist.addArtistToSelectDropDown();` and `Artist.deleteArtistAction()` to ensure each time we load an artist they are also loaded to the drop down menu, and have a delete button attached.  The only method we have not gone over is the post method and delete.  The post is slightly more complex and delete is slightly simpler.  But rest assured that a new artist can be created and deleted.  For now I'm signing off, stay motivated and keep coding!
