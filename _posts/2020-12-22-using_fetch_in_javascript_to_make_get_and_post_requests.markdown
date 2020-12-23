---
layout: post
title:      "Using Fetch in Javascript to Make GET and POST Requests"
date:       2020-12-23 00:11:31 +0000
permalink:  using_fetch_in_javascript_to_make_get_and_post_requests
---


Fetch can be used for contacting and interaction with an API, usually to retrieve data from the back end of the application and manipulate it on the front end. 
Fetch is a function that accepts two arguements, the first is a stringified URL and the second is an optional arguement that will contain information about your request. There are 4 types of requests that can be made with fetch:

1.  **Get** - retrieves the informations from the API
2.  **Post** - appends the data to the API
3.  **Patch/Put** - Edit or Update information in the API
4. **Delete** - Delete information from the API 

In order to make a Get request we need to fetch the URL and then parse the response into something that Javascript can read using a .then statement:

```fetch(URL)
.then(res => res.json())```

Now that we have fetched our data we need to call the data so that we can see it like so:

```fetch(URL)
.then(res => res.json())
.then(data => {
console.log(data)
})```

Get requests are the most simple of the ways to use fetch. Now let's go over Post. 
Post requests will post new information to the API, in order to do that we need to include a second arguement that will be an object. The object will contain information on what to post to the backend, what kind of data we are posting, and the keys of the data we are posting too. For example:

`` const configObj = {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            "Accept": "application/json"
        },
        body: JSON.stringify({
            title: recipeInput.value
        })``

Here Post is called within the configObj(object) and tells the backend of the application to accept data in the form of JSON. Then within the body is our stringified form dat(the actual data we want to update the server with. 

After the config object is created we wrap it in a function, add an event listener so that we can have a submit button to submit our form with and then return the results. See Below for Example:


``recipeForm.addEventListener("submit", submitRecipe)
//fetch request
function submitRecipe() {
    event.preventDefault()
    const configObj = {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            "Accept": "application/json"
        },
        body: JSON.stringify({
            title: recipeInput.value
        })
    }
    fetch(recipeUrl, configObj)
    renderRecipe(recipeInput.value)
}``


