---
layout: post
title:      "My First Ruby Application - The Star Wars Planet Finder"
date:       2020-04-02 21:33:22 +0000
permalink:  my_first_ruby_application_-_the_star_wars_planet_finder
---


I have managed to pull off what felt like the impossible, I have completed my ruby cli project and I am actually satisfied with the results. After completing the lessons about scraping I decided I wanted to use a more modern way of collecting data from an external source and wanted to work with an API. An API, or Application Programming Interface is a much simpler way of collecting information from another page by making calls and requests defined by the API to the site. This made it much easier to choose what I wanted to do with my application. 

I started by brainstorming a few topics that I may be interested and quickly came to the conclusion of Star Wars. From there I went on to Google to seach for a Star Wars API to see what kind of data I could have access to and found one called SWAPI that was perfect! From there I went to make an application [flowchart](https://app.diagrams.net?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=planet_cli_flow_chart.drawio#R1Vxdm5o4FP41cznz8CGglx07%2FdhOu2592m4vI0SlReJCHLW%2FfhNIEBKEoIBMLzrkEAKc857PHLwzp5vD%2Bwhs15%2BRB4M7Q%2FMOd%2BbbO8Nwxjb5nxKOKcHQHT2lrCLfS2k5wtz%2FAxlRY9Sd78G4MBEjFGB%2FWyS6KAyhiws0EEVoX5y2REHxrluwghJh7oJApv7wPbxOqWPDOdE%2FQH%2B15nfW7Ul6ZgP4ZPYm8Rp4aJ8jmU935jRCCKdHm8MUBpR5nC%2Fpde%2FOnM0eLIIhVrngu2d%2BXz4eZv5q%2BTv%2BtDDmZvTPvZmu8gKCHXvhbzGMCOXrLozJnzfbLXt6fOQsIcsS7pPB437tYzjfApee2RMAENoabwIy0skhiLepSJb%2BAZKneJQfmb3FC4wwPORI7BXeQ7SBODqSKfwsBwYDlG6lw%2F1JODabsc7JhdMAg8MqW%2FjEMXLAmNaAgZbEQJlfofeGIpGM3ADEse8W2USYER3%2FJQPtweLDn3TIB28PhdGRjw4%2Bzl1GRj%2F5iuT4dBEd8GvOSiBGu8iF9UiBXkFfZDnlBGGVCILTIhgA7L8UtaxMOuwOM%2BSTJz7BwBRgoAkCTt%2BHXZXXCnGhkbCQISyEQbSCWFooAUv22pfjRzfOaaAfbneY6uA2ACFMjjBKLR32QzLZ0BCdR%2BWbnQtQTE%2BAoaltZs9z8JiUwGPSlZ7qoxYUNVM42zZzKnevPWi6U6l3dDCDkU9ehgi3gS56Xw7HL58c1xkvnvcvsQc%2Ff7DuDeZVU2wqoGsgWmtoRWUzRhdqbfZAXGtF3LSntaUC0CcSmubkloTyA0RUU2eJzpKDd37oUXUWoBbv%2FQ2dUgSYu%2FYD7xkc0Y7yPcbA%2Fc1HjxGMSXA042jRBdJnwHG3RpH%2Fh9gIEDACWSfCLLIyijPm9BZ1cFTXcrsolYms87pmlMDLrHLP7G5fiU0C4SqATW5nVmCZ3wsERCVDgOEj2oVe3AVc%2BDvn4DJ9%2FkgIU2pmFMFRIcazEqwGr7r1LhOkVsJbyXuqCVJ5fXH5dmVXHuDZjRxHiES5KUqFv9pATPVIsLAi51UttbjOvah%2BHcdXlhxfNRPfyelPKLMyp689OM64BZ9fCYbaqHxoDt4oSttsyb%2Ff9xyVG81QU5PWFZK6U453Jq1rCJHaGJBLcSAQMZ1RQbSi61c2LEZxHb0zhJSy35Rd%2BjZKHxCvqWvzAAYJt%2F7bwRiTPMrQFsfs7C4uCQoj6sRoxpXgoCZLWxLP%2Fw5s%2FIC%2B%2FwcYvEDsu4CdYGGCbrDxFAUoSm5iask%2FmuYF%2Fiqk4CU4o%2FapKfAahA%2BToqhM2d2P%2B6zZZIHtSXYg%2Fk0ILPNOUukk%2FaZ3v%2BMZODkIwQbyCauEskERTGYvUbQheoFCSazXp9styWGkl%2Fv0GjmMu5KDaclGlRipORuiCK%2FRCoUgeDpRH4tKcprzjNCWcfQXxPjIVADsiLBuYZdrXbdyEm%2BqRobKlvk67ZEDYm75Aj%2Bmf9CS2j%2BIgR%2FQXBgsUKJJTLtiGBC8JwaR69VQFcYW6xW3Vpgyp4M22xx3l0lNkBsvao20EB4SQ%2Bam1mmaIIWLLDNtXHb08rSkOFSpOEMzY60Gi8U9AL3nYNEcVrA4doqivjRYHE%2BK64h1x47TCVPeZqtCyPkklB%2FnPFaDrZ7r%2FJWyG%2BoJGkKqqYsJgDI2ajDWMTZGYwkbexi4KAk0U6NOLXZm5wsGfqgmWtidKwn4nRJkOF1ZaKuZ%2FtVZaKPRNm3bJlofD0oPa0s1yjWfcTloekroS5LCbBPnlZXpxa3srsv2je93gzK%2BLpvZiCzpw5ckoU%2BKMvn05bTpnhrcirJNV1a3%2BSa7YHZ7DIwrt6lzPHcjSMQcJ1nJnvJ68YswJ05Y64erlC9xkhyyAtqSOD7y583so8T4V1Iva97jdKaKfrN6mdzklNOd6qR%2Fd%2BukvzH3rcu431l6aciWqyp46Td5UO0e4xAaSNBii12El7aP2UL7mBjCdh20yKWHxFC%2B0ogly8uvi1iuaxSTa2zN1E1VdYxBaYTocy6ttOh29TodZ9OOJLx0m4YFEyzIsAPaUbUgjsle0aOvMMbTwIfJi4DQk2f8Nf%2F7Czk3o8FJdVDSlR%2Fjmq6%2B2zOsKqku51dVenWTKil7onwKXhnYDkV3a3ROOQMXtjv67rpoFujUtOhe0xNf6d1qY53RsAo0QllF7JRVL5MWzYk16RUb1kjGQp97xZ0YHGVI8epFrWVqf6%2F4TBeP2IAtBtBdN2id%2FXAp%2B2yipHFDBNAtv4gQ95bl75j6zTLlyKkX43vpx09nJVCfipqyLr2KnFUst4pYUM5ZhQ0vvbtKezlf5Y8nmkOtNcOsq6JhWDGfI6bOl6LBEdBg9owG0x6SY%2B%2FO7JiqQDNH%2FbhwXRd8eM%2BCH%2Bk3ETz3UfliqJqH0gtAuaBfsHWoKHf%2FX%2FsxiGAjDCWgEAMOjrlpWzohrnBwE7vkLme9GJt9tj%2BcPXPd%2BawlyBJAnj5%2Bu7ZOroHko9ak08MGGxp9hot4m4BIrERd0rp300B3ItTa70s%2BBe63X0%2F%2BFJh%2FNz3AnzsQmx1vvR01kqO3b2W74rdlW23tsyo0a7%2F%2FqI3KFndD9GtzoQPpYWTUeaNkpPwFen00PKyfg7CEzodLvyu3xmds1dUxERmefuolnX76wRzz6X8%3D)
 so I knew how to set up my file structure within my app so that they would all communicate with each other.
 
 After that was done it was time to get to work. I had my Idea, my API, and my application flowchart. All that was left was to set up my Gem within my local environment on VSCode. I decided to use a local environment instead of the IDE on Learn.co because of my prior familiarity with them. I was more comfortable working within my local environment knowing that all of my work would be saved properly and being able to issue commands with the Terminal made things easier for me. I created my directories using mkdir and my project name, then used 'bundle gem project_name' to create my gem that I then opened within my editor. This created all of the files that I need in order to get started and from there I really started to take off!

After some reading regarding setting up an app using an API, I decided to use RestClient in order to communicate with the API and get the data I needed for functionality. Then I created a simple get_data method within my API class that would call on the API and use Json to parse through the data and assign the data to my Planet variable based on the name of the planet. 


`def self.get_data`
`response = RestClient.get("https://swapi.co/api/planets/?page=2")`
        `planet_array = JSON.parse(response.body)["results"]`
        `planet_array.each do |planet|`
            `Planets.new(planet)`
        `end`
    `end`
		
After that was complete I went and set up my executable file and my environment file and made sure that the right assets were required in the right places. Within my bin/run file I added a ruby [shebang](https://gist.github.com/sanemat/802206) to tell the file to run as a ruby application. I also made sure to require_relative my environment file which in this case was planets_cli.rb. Over in that file I required both json and rest-client so that I could use those external tools with my API and then required_relative all of my other application files(api.rb, cli.rb, planets.rb, version.rb).

Next came the fun part! I started to set up my cli.rb file. This is the file that I have all my code that will interact with the user within. I started with a simple `start` method that would `puts` out a greeting and promt the user to interact with the app and choose to see a list of planets. The planets would be pulled from the API and they would be numbered. I would also later pick what data I wanted to display with the planets. For now I continued with the with CLI class. My next method would take the user input and decide what to do with it. I used an If Else statement in order to complete this action stating that if the user entered 'planets' they would be shown the planet_list, elsif they put 'exit' they would exit the application, else if they input anything else they would receive and error and be prompted to try again. After that came the planet_list method which needed to somehow take the information in my Planets class and create a numbered list of planets for the user to choose from. The simplest solution I could see was to implement the each_with_index enumerator while calling on the Planets class like this:

`Planets.all.each_with_index do |planet, index|` 
		
With this I was able to tell the app to puts out the index + 1 and the planet.name in order to get a numbered list that started at 1 instead of 0. With that finished I was ready to move on to my planet_picker method that would take the data received from the API and assign it to the correct planet and label it correctly for the user to view. This required me to stop and start coding out the attributes with the Planets class. There I set up setters and getters using attr_accessor for the name, diameter, climate, terrain, and population. Then I set up a @@all array and self.all method to save the information to be used within an array. All that was left after that was to give my app a way to find the name of the planet entered by the user. I did this by using the self.find_by_name method to take the user entered data and downcase all the characters and seach the API for an exact match. Now with the Planets Class taken care of I was able to jump back over to my CLI class and finish up by invoking the Planets class and the find_by_name method within it, assign them to a variable and display the information about the planet selected by the user! 

```
 world = Planets.find_by_name(planet)
        world.each do |p|
```

After that was all done, all that was left was cleaning up my application, refactoring that which was necessary, and adding in some cool messages to keep the user entertained while using. The end result ended up being a nifty little planetary look up that will tell you a list of facts about a small group of planets within the star wars universe!
Check out my project [here!](https://github.com/MisterAnthropy/planets_cli_project.git)

		
		
		

