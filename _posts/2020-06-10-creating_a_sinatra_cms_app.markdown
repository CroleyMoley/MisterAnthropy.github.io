---
layout: post
title:      "Creating a Sinatra CMS App"
date:       2020-06-11 00:03:58 +0000
permalink:  creating_a_sinatra_cms_app
---


Creation of a Sinatra CMS (content management system) App was rather painless for me. Once I had a solid idea of what kind of CMS app I wanted to make and was able to connect the necessary associations, everything seemed to just fall in place. 

I started by making use of the corneal gem. This allowed me to use the command `corneal new gaming-project` to automatically create the file structure that I would need in order to get started and skip the painstaking process of manually putting it all together. 

Once inside the correct folder within my text editor and the repository is created, I was able to get started. The first step was to create some migrations and set up my tables. So I created a couple of files: `001_create_users.rb` and `002_create_games.rb` and within them I set up the following tables:
```
class CreateUsers < ActiveRecord::Migration
    def change 
        create_table :users do |t|
            t.string :username
            t.string :password_digest
            t.timestamps null: false
        end
    end
end
```
```
class CreateGames < ActiveRecord::Migration
    def change
        create_table :video_games do |t|
            t.string :title
            t.integer :user_id
            t.timestamps null: false
        end
    end
end
```

Easy enough, next up were the models. I can't very well set up tables without the model classes there to set my associations up. First I created two files: `user.rb` and `video_game.rb` and made the following associations within:

```
class User < ActiveRecord::Base
    has_secure_password

    has_many :video_games
end


class VideoGame < ActiveRecord::Base
    belongs_to :user
end

```

I also wanted to set up some users and games to test and play with once I ran my migrations so I set up a `seed.rb` file within my `db` folder and created a couple of users and some associated games. 

With this set up I was ready to run `rake db:migrate` and start getting my routes set up within my yet-to-be-created controllers. To get started I went to `/app/controllers` to create controllers for each of my models. I started with the Users Controller because the games belong to the user. 

I set up a simple login page to begin

```
class UsersController < ApplicationController


    #render the login page 
    get '/login' do 
        erb :login
    end
```

With each route that I created, I needed an associated file that will be displayed with the route is called and ran. Each time I created a route, I would also make an erb file with the same name. In order to get the login page to display I created `login.erb` within `/views` and wrote the following html code:

```
<h2>Welcome! Please Log in to Continue</h2>

<form class="" action="/login" method="POST">
    <label for="username">Username: </label>
    <input type="text" name="username" value="">
    <label for="password">Password: </label>
    <input type="text" name="password" value="">
    <input type="submit" value="Login">
</form>
```

This created a form for my users and vistors to interact with that will allow the user to fill out a username and password and submit that in order to access their profile. In order to test this out I also needed to `use` this file within my main application controller so that the app new what file to rely on. For my users to get that button to do anything at all I needed another route for that action within my controller and some helpers with the app controller. That's when I built these helper methods and the `POST` route to go with it:

```
get "/" do
    if logged_in?
      redirect "/users/#{current_user}"
    else
      erb :welcome
    end
  end

  helpers do 

    def logged_in?

      !!session[:user_id]
    end

    def current_user
      @current_user ||= User.find_by(id: session[:user_id])
    end

    def authorized?(game)
      game.user == current_user 
    end
```

This allowed me to use methods instead of retyping the same code over and over! This also allowed me to create the following post method: 

```
post '/login' do 
        @user = User.find_by(username: params[:username])
         if @user.authenticate(params[:password])
            session[:user_id] = @user.id
            puts session
            redirect "/users/#{@user.id}"
         else 
            redirect '/login'

         end
    end
```

Now that I have my method to get users logged in and a session started it was time to focus on allowing new users to sign up for my app! This required yet another set of get and post methods. Here is what I did:

```
get '/signup' do 
        erb :signup 
    end
    

    post '/users' do 
        if 
            params[:username] != "" && params[:password] != ""
            @user = User.create(params)
            session[:user_id] = @user.id
            redirect "/users/#{@user.id}"
        else
            #invalid input
            redirect '/signup'
        end


    end



    get '/users/:id' do 
        @user = User.find_by(id: params[:id])
        
        erb :'/users/show'
    end
```

The /signup route takes the new user to the sign up form page, the post /users route allows the user to interact with the form and tells the app to make sure they entered both a username and password. If the parameters are not met then they will be redirected to another page. The '/users/:id' route will take the successfully signed in user to their home page. 
After making and testing these routes, the only thing left was a simple logout button on the main layout of the app and a subsequential '/logout' route. Take a look below: 

Route:
```
get '/logout' do 
        session.clear
        redirect '/'
    end
```

HTML:
```
 <div class="navigations">
      <a href="/logout">Logout</a>
      <a href="/games">Video Game Collection</a>
      <a href="/games/new">Enter New Game</a>
    </div>
```

And just like that my Users portion of my application was complete. They can sign up with a username and password, logout, log back in with the same password, and had restrictions on how they can sign in or sign up. 
Next up is the Video Games Controller.

I started by created the class VideoGamesController and entering `use VideoGamesController` in my application controller. 
With this controller I wanted to create the following types of routes - a home page that displays all games, allow the games to be editable, allow for game creation, allow for game deletion, select games by name and display a page for each game, block users from editing the games created by other users. With that in mind, my routes would end up calling on the following files within the views folder: games/  edit.erb, index.erb, new.erb, and show.erb. Take a look at the routes below: 

```
get '/games' do 
        @games = VideoGame.all 
        erb :'/games/index'
    end

    get '/games/new' do 
        erb :'/games/new'
    end

    post '/games' do 
        if !logged_in?
            redirect '/'
        end
        if params[:title] != ""
            @game = VideoGame.create(title: params[:title], user_id: current_user.id)
            redirect "/games/#{@game.id}"
        else
            redirect '/games/new'
        end
    end
```

Here we are setting up routes to the index.erb file that will display a list of all the games. Then we set up a route for the games to be created by users via a form within the new.erb file. Once this is done and the user submits, the post /games route will check to make sure the user is logged in before creating the new game. If the user is not logged in, they will be redirected to the main `'/'` page where they can sign in. If they are logged in, the game will be created and they will be taken to the games page where they can edit or delete it. 

Creating the erb files for the edit page was easy enough. I wanted a page with a text box and submit button. The text box will be autopopulated with the current name of the game that you're editing, and finally I wanted the submit button to change the name of the game and take me back to the games/:id page. Take a look at the code below and I will break it down. 

```
 get '/games/:id' do
        set_game
        erb :'/games/show'
    end


    get '/games/:id/edit' do 
        set_game
        if logged_in?
            if authorized?(@game)
            erb :'/games/edit'
            #problem code below
            else 
                redirect "/users/#{current_user.id}"
            end
        else 
            redirect '/'
        end

    end

    post '/games/:id' do
        set_game
        if logged_in?
            if @game.user == current_user 
            @game.update(title: params[:title])
            redirect "/games/#{@game.id}"
            else
                redirect "users/#{current_user.id}"
            end
        else
            redirect '/'
        end

    end

    delete '/games/:id' do 
        set_game
        if authorized?(@game)
            @game.destroy
            redirect '/games'
        else    
            redirect '/games'
        end

    end

    def set_game
        @game = VideoGame.find_by(id: params[:id])
    end
```

Let's take it from the top. The first route uses the method `set_game` to display the game's main page after being selected from the /games list. Next route checks to make sure that the user is logged in and that the user created the selected game, if yes then the user is given an option to edit the selected game. If no, the user is not given an option to edit. When the user decided to edit the game and submit the form this next method will update the game with the new information and take you to the /games/:id page. The final route if for the delete button. This route will check to make sure that the game `belongs_to` the user and if it does then the user will be given an option to delete the game. When deleted, the user is redirected to the main /games list. 
 
This is a very basic sinatra cms app but shows how to properly associate between to models and classes while using ActiveRecord. Check out the app [here.(https://github.com/MisterAnthropy/gaming-project) Follow installation instructions in the Readme.md file for setup.
