---
layout: post
title:      "Redirect Using History with React"
date:       2021-03-08 18:25:28 +0000
permalink:  redirect_using_history_with_react
---


While creating a single page application using React and a Rails API you may come across the issue of how to redirect a user to a specific page or location after the user has interacted with the app. For me it was when my user would log in or sign up to use my app. I certainly would not want the user to stay on the login or sign up page, what kind of user experience would that be? Instead of forcing the user to redirect themselves, we can use history to push the user to a different location within the app easily. 

First things first, you will want to use history upon an action the the user interacts with. For example a within a fetch request is the perfect place for a history object to be called. When used within a fetch request, history will push the user to a new route on the app. To use history, make sure you are using the 'react-router-dom' api and set up your fetch request the following way: 

```
export const createArticle = ( articleData, history ) => {
    return dispatch => {
        const sendableArticleData = {
            subreddit: articleData.subreddit,
            title: articleData.title,
            url: articleData.url,
            content: articleData.content,
            userId: articleData.userId
        }

        return fetch("http://localhost:3001/api/v1/articles", {
            credentials: "include",
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(sendableArticleData)
        })
```


Here I have set up a function sendableArticleData, to be parsed and returned from my rails backend as JSON data. 
This is a very simple fetch request. Let's add the error responses and the dispatch requests. 

```
.then(r => r.json())
      .then(resp => {
        if (resp.error) {
          alert(resp.error)
        } else {
          dispatch(addArticle(resp.data))
          dispatch(resetNewArticleForm())
          history.push(`/articles`)
        }
    })
        
    }
```

Here I have handled the error response in case the fetch request fails I can get something back that tells me I am hitting the request. Down at the bottom I am calling my dispatch requests, addArticle (action creator for adding a new article),
resetNewArticleForm (resets form after submission), and history.push('/articles'). After passing history throught the initial createArticle function, I am able to call it and push the user to the route '/articles' where they will see all articles posted and the new article they just created. 

Another example of when to use history.push is within a handleSubmit function. When using history this way, a user will be pushed to a specified location in the app upon submission. Here is how I set it up: 

First create the Login Function
```
const Login = ({ loginFormData, updateLoginForm, login, history }) => {
}
```

Within this function we are going to pass through everything we want the user to be able to update or interact with, as well as history so we can use it later on. 

Next comes the handleInputChange:
```
const handleInputChange = event => {
        const {name, value} = event.target
        const updatedFormData ={
            ...loginFormData,
            [name]: value
        }
        updateLoginForm(updatedFormData)
    }
```

With this function we are able to allow the user to type in the input fields of our login form and have the form be updated with each keystroke. 

Next we handle submit:
```
const handleSubmit = event => {
        event.preventDefault()
        login(loginFormData, history)
    }
```

With this we are able to allow the user to submit the data that was input within the form fields and use history to push to a location specified within the fetch request that the user is making upon submission of the form. Let's have a look at the fetch request.

```
export const login = (creds, history) => {
    console.log("creds are", creds)
    return dispatch => {
        return fetch("http://localhost:3001/api/v1/login", {
            creds: "include",
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(creds)
        })
        .then(res => res.json())
        .then(response => {
            if (response.error) {
                alert(response.error)
            } else {
                dispatch(setCurrentUser(response.data))
                dispatch(getArticles())
                dispatch(resetLoginForm())
                history.push('/')
                
            }
        })
            .catch(console.log)
    }
}
```

As we can see, this fetch request is almost identical to the request sent for articles. It makes the same post request and passes the credentials(creds) to be returned as JSON. After the alert for any errors that may occur, dispatch is called to return data from our store and display it for the user. This is including history that pushes the user back to the home page ('/') after the user successfully logs in. 

Thanks for reading and I hope you gained a more thorough understanding of how to use history to redirect a user in a single page application. Check out my code [here!](https://github.com/CroleyMoley/reddit-clone-app-frontend)
