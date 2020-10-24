---
layout: post
title:      "Using Omniauth to Log in with Google"
date:       2020-08-09 15:50:27 -0400
permalink:  using_omniauth_to_log_in_with_google
---


In order to ingrate your web application with other sites and apps for the authenication process, we need to use a gem called Omniauth. There are several different omniauth gems that are called within our gemfile in order to integrate with sites like, google, facebook, github, twitter, etc. Let's take a look below to see how I integrated Google sign in with my Video Game Review app.

First I started with the gemfile and added the following gems: 
```
#omniauth gems
gem 'omniauth'
gem 'omniauth-google-oauth2'
gem 'dotenv-rails'
```
All of these gems do different things but all are necessary components when working with omniauth. Let's break them down:

1. Omniauth gem lays the foundation that we need in order to use the next two gems with our application. 
2. Omniauth-google-oauth2 tells omniauth which site we want to authenticate with
3. dotenv-rails gem allows us to use .env files within our app and store our session keys and secrets within them without uploading them to github or wherever you choose to store your application. 

Once these are added to your gemfile, go ahead and run 'bundle install' to bundle it all up and make sure in installs properly. 

Next we need to head over to the [Google API Console Credentials page](https://console.developers.google.com/apis/credentials?project=music-project-285801&folder=&organizationId=) to create a new project and obtain credentials for login. 

This is a very straight forward process. First create a new project, select web application for the type, and click create. 
Next click on Create Credentials and then OAuth client ID. You may have to "configure consent", when finished click create and the page that appears should have your OAuth Client ID and Client Secret. It should look like this:
                       ![](https://developers.google.com/ads/images/oauth2-client-id-secret.png)
											 
Now that this is done we can head over to our application within our text editior and create a file called .env
This file will not be uploaded to your repository because of the protections within Omniauth. This is the file that we are going to use to store our client id and client secret that we use to log in the user. It is very important that if your ID or Secret were to be compromised you need to create a new one and replace the old with it. Attackers can use this data to log in to your accounts and steal personal information. Inside your .env file should look like this: 
```
GOOGLE_CLIENT_ID=id_goes_here
GOOGLE_CLIENT_SECRET=secret_goes_here
```

Now go back to the Google Credentials page and copy and paste your Client ID and Secret in the appropriate places leaving no spaces.
Great! We are about halfway to getting our users to be able to sign in and authenticate using Google! Let's go into our config/initializers folder and create a file called omniauth.rb and within the file we are going to place the following code:
```
Rails.application.config.middleware.use OmniAuth::Builder do
    provider :google_oauth2, ENV["GOOGLE_CLIENT_ID"],ENV["GOOGLE_CLIENT_SECRET"], skip_jwt: true
  end 
```

This sets a provider as google.oauth2 so that when the user wants to sign in through google, the application knows where to look for authentication. This also leaves room to set up more than one integration with an external app or site. Next we need to go into our config/routes.rb file and create a route for our application to go to when the user wants to use google to sign in. I used the following route that I set up when I created the credentials for the my application on the Google Credentials page. 
```
get '/auth/:provider/callback' => 'sessions#omniauth'
```

Now with the routing done it is time to set up a link on our home page, next to our signup and login links, that our users can click to login to our application with their Google account. This was simple enough as I already had a view for the homepage and links for the other methods of signing in so that's where I decided to put this link as well.
I went over to app/views/sessions/index.html.erb and put the following line to create my link and tell it to follow the same path that I set up in my routes. 
```
<p>
    <%= link_to "Login", login_path %>
    OR
    <%= link_to "Signup", signup_path %>
    OR
    <%= link_to "Sign in with Google", "/auth/google_oauth2" %>
</p>
```

Everything is really coming together now! If you have your rails server running and were to go to the home page you should now see a link to sign in using google although if you click it you will get an error and won't be able to sign in. That is because we have not updated our Sessions Controller to accept the new routes and arguements so let's fix that!

Here is what we have in our controller prior to updating our application to include Omniauth: 
```
def index
end

  def new
  end

  def create
    @user = User.find_by(username: params[:user][:username])
    if @user.try(:authenticate, params[:user][:password])
      session[:user_id] = @user.id
      redirect_to user_path(@user)
    else
      flash[:error] = "Login Credentials are Invalid. Please Try Again."
      redirect_to login_path
    end
  end
	
	def destroy
    session.delete(:user_id)
    redirect_to '/'
  end
end
```

This is a pretty simple controller. The create method finds the created user by their username and authenticates the username with the password that was given. If both are correct the user is logged in and take to their home page. If it either is incorrect the user is redirected to login again and an error message is displayed.
In order to use Omniauth, we need to integrate it into our controller and our User model. Before we change the controller let's add this helper method to our user model:
```
def self.google_creation(auth)
    @user = User.find_or_create_by(username: auth[:info][:email]) do |u|
      u.password = SecureRandom.hex
    end
  end
```

This helper method will be called upon within the create method in the sessions controller to find the users google information and authenticate their login using their email and password. We create this as a helper method within our users model so that we can use omniauth with other sites other than just google. 
Now we will go back to our Sessions Controller and start to make those changes necessary to integrate. Let's start by adding two methods to our controller:
```
def omniauth
        
end
    
    private
		
    def auth
        request.env['omniauth.auth']
    end 
```
The first we will leave blank for now and will be later used to call upon our helper method to log our user in using OAuth2. 
The second method is a simple request to our .env file to use the client ID and secret to log our user in. Notice the use of the auth method within the helper method in our User model. We are able to call on the auth method due to the use of 'self' at the beginning of the google_creation method so our method knows where to look.
Next we will add code to the omniauth method in our controller so that this method calls on our helper to log in the user and then puts the user in a session and redirects. Take a look: 
```
def omniauth
        @user = User.google_creation(auth)
        
        session[:user_id] = @user.id
        redirect_to user_path(@user)
end
```
Now with that done we just need to update the create method in our sessions controller to accept new params that include google_oauth2. We will also need to keep the same logic and code so that our user is able to sign up or log in via our web application without omniauth. Take a look below to see how I did that: 
```
def create
        if params[:provider] == 'google_oauth2'
            @user = User.google_creation_omniauth(auth)
            session[:user_id] = @user.id
            redirect_to user_path(@user)
        else 
            @user = User.find_by(username: params[:user][:username])
            if @user.try(:authenticate, params[:user][:password])
                session[:user_id] = @user.id
                redirect_to user_path(@user)
            else
            flash[:error] = "Login Credentials are Invalid. Please Try Again."
            redirect_to login_path
            end
        end
    end
```

Our new code calls on our provider which is google_oauth2 and authenticates using our google_creation helper method and puts the user in a session if the credentials provided are correct. If not, it will redirect the user to the login path and display an error message asking the user to please try again. 
These are the basics of using the amazing gem Omniauth to log your users in using a third party app or site. 


