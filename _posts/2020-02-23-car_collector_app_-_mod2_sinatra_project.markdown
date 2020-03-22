---
layout: post
title:      "Car Collector App - Mod2 Sinatra Project"
date:       2020-02-23 22:48:18 -0500
permalink:  car_collector_app_-_mod2_sinatra_project
---

     This project was really challenging, moreso than the mod1 project, but who am I kidding they were equally difficult in their out right for different reasons.  Lets dive in shall we, some of what I found to be most challenging was passing data between the view and the controller.  Creating the session_controller was the easiest part in my opinion.  Going between the views and the controllers while maintaining that the current user was the same as the @user was something that got me tripped up for a while, while it seems trivial doing that proved to be a fomidable task.  While see the creation of the website was intresting and fun, the process was grueling.  This is however also the process of learning and that in of itself can be painful.  If you're not making mistakes you're not learn, and on this project the mistakes came early and often.  The issues I had the most problems with were:

```
patch '/users/:id' do
      find_user
      if logged_in?
        @user.update(params["user"])
        redirect to "/users/#{@user.id}"
      end
    end
```
		
		and
		
```
delete '/users/:id' do
      find_user
      if current_user.id != @user.id
        redirect to "/users/index"
      else
        @user.destroy
        session.clear
        redirect "/users"
      end
    end
```

after i figured out find_user I turned it into a help method and put it in the application controller, here is what it looks like:
		

```
def find_user
      @user = User.find_by(id: params[:id])
    end
```

     The project became easier once that was sorted out and I could accually start enjoying the process.  There were alot of moving parts to juggle with this project and that is part of the developement process from what I am learning, If I had to do this project again it will be easier I'm sure, but for now it was a painful learning process and I'll leave it at that.  Till the next time, same place, same blog, keep coding, and stay motivated.
