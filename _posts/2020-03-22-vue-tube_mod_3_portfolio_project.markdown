---
layout: post
title:      "Vue-Tube Mod 3 Portfolio project"
date:       2020-03-22 01:13:48 -0400
permalink:  vue-tube_mod_3_portfolio_project
---


I've heard through the grapevine that your mod 3 portfolio project should be one that really stands out among your repertoire of project during your time in the software engineering program at Flatiron School.  With this in mind I set out to make an analog to Youtube, but then as I delved deeper into the concept I soon figured out that this would be no simple task like plugging in an API to my application.  First thing I did was install the devise gem and followed the docs on how to get authorization and authentication up and running very quickly, for those that don't know what the devise gem does it basically does the work to get authorization and authentication up and running very quickly.  After that was sorted out a requirement of this project was to sign users in with an external API, you had to use a major Oauth (Omni Authorization) provider like Google, Facebook, GitHub, etc... I chose Google, naturally.  Getting Oauth up and running in my app was would prove to be a very docs oriented process, what I mean by that is if you didn't follow the docs provided forgetaboutit.  Each step of the project was made slightly easier by the Ruby on Rails framework, but it still was a tedious process, there is quite a bit of back and forth between controllers and views then to your local host, then back again.  An sample of the Oauth code is below.


```
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

  def google_oauth2
    @user = User.from_omniauth(request.env['omniauth.auth'])

    if @user.persisted?
      flash[:notice] = I18n.t 'devise.omniauth_callbacks.success', kind: 'Google'
      sign_in_and_redirect @user, event: :authentication
    else
     # session['devise.google_data'] = request.env['omniauth.auth'].except(:extra) # Removing extra as it can overflow some session stores
      redirect_to new_user_registration_url, alert: @user.errors.full_messages.join("\n")
    end
  end

end
```


 At first glance at the requirements you think, whoa that's a bit of a heavy lift, bit then you start tackling them individually and they seem to become smaller and smaller lifts.  It has to do with separtion of concerns, once you separate each part and break them down to even smaller parts it becomes less difficult.  As I started this project I orignally began to code with the theory of replicating Youtube, that came crashing down after I began to understand what that would entail.  First there naming your app on the Google Developers Console, then you select which API you'd like to work with a link to the console is below.
		 
	
[https://console.developers.google.com/apis/library]

 
Then you have to get the Oauth2 keys, this is where I had some trouble, I though I needed to select a scope for the Youtube api to work, however I didn't need a scope after all.  Once I got the Oauth key from Google it was off to the races, or so I thought.  I changed my domain 3 days into project week after examining the requirement carefully and considering that I had no scopes from Youtube, I was limited to what could be done with the Youtube api.  So I chose User and Email, and made a email client.  A sample of my User class is below.


```
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable

  devise :database_authenticatable, :registerable, :recoverable, :rememberable, :validatable, :omniauthable, omniauth_providers: [:google_oauth2]

  has_many :sent_emails, class_name: "Email", foreign_key: :sender_id
  has_many :received_emails, class_name: "Email", foreign_key: :receiver_id
  has_many :to_contacts, -> { distinct }, through: :sent_emails, source: :receiver
  has_many :from_contacts, -> { distinct }, through: :received_emails, source: :sender

  validates :name, presence: true


        def self.from_omniauth(access_token)

          data = access_token.info
          user = User.find_by(email: data['email'])
          
          # Uncomment the section below if you want users to be created if they don't exist
          unless user
              user = User.create(
                name: data["name"],
                email: data['email'],
                password: Devise.friendly_token[0,20]
              )
          end
          user
        end


        # def destroy_email(email_id)
        #   if email = sent_emails.find_by_id(email_id)
        #     email.destroy
        #   elsif email = received_emails.find_by_id(email_id)
        #     email.destroy
        #   end
        # end
  
end
```


The relationships are polymorpic meaning that a model can belong to more than one other model, on a single association.  It was conceptually difficult working with these relationships, but once I understood them better, the product was facsinating.  This project in my opinion does show how far I have come since mod1 and how much further I can go, having an understanding of a programing language like Ruby is a good place to start to understand other langauges and I cant wait to learn another, JavaScript anyone?
