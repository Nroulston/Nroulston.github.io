---
layout: post
title:      "Code Challenge Wisdom, or Folly"
date:       2021-01-28 14:07:12 -0500
permalink:  code_challenge_wisdom_or_folly
---


## Is it really a code challenge if you don't learn something new? 

I personally think Wisdom and Folly are two sides of a coin that go hand in hand. During my first coding challenge I decided to learn something new. Actually I decided to learn a lot of new things, but it started with using Server Side Rendered(SSR) React and Stimulus.js together in a Rails application. 

The code challenges requirements are as follows.

1. Has a single button in the center called "Current Weather"
2.  Upon click, it uses the built-in browser Geolocation API to get the user's location
3.  The application then grabs the current weather for that location from OpenWeatherMap and displays it on the page
4.  Deploy your project somewhere so it's accessible from the web. Heroku or a static host if you're going that route are perfectly fine.

You can use any tech stack that you want to, but the company uses Rails, SSR React, and Stimulus.js

## So much learning
#### I am going to break this down by problems I encountered, insights I gained, and solutions.

### Problem 1: Using too much technology for a simple problem

The challenge requirements are simple, and the solution could be completely written in Ruby making async calls with HTTParty or some other HTTP client gem. The challenge itself is to use the tools you are being asked to use to solve a problem.

It struck me as I was developing that there are times that you will be required to use a third party API,  write code structured a certain way, or in generally do something that you don't think is the best way to do it. This is my job though; to work with my team and client an integrate with what they think is the best way to do things. By opening my eyes to that I will be able to grow as a developer, because there is one thing I know: I don't know everything. 

### Problem 2: What is `this` in Stimulus, and why did it just go away?

Stimulus is an amazing piece of code. Yet for hours I struggled with `this` disappearing on me. Part of the struggle was writing a callback function and passing it to the GeoLocation API's wrapper. You don't write your own calls to an endpoint, so thus lose control of `this` once your callback function in essence disappears inside of the API's function. 

I tried binding my function, using .call and .apply to pass `this` in, and many other efforts. What was interesting is that Stimulus throws an error when you attempt to bind this with .call and .apply. It looks like they bind the event to get passed into the controller actions, and don't let you pass it along to another action. Learning that changed the way I thought about using Stimulus, it really is meant to be a sprinkle effect. I was coming into it with the experience of using StimulusReflex and CableReady and being able to broadcast from the backend directly to the html element I wanted to update. Ultimately I landed on writing an anonymous arrow function for the callback. It means there is a lot of code in one function, breaking the Single Responsibility rule, but it didn't bind this instead it uses the parent execution context when being called inside of the API's function. 

Is this the most ellegent solution; No. Was it the right way to use the tool; No. In the future I would either want to refactor this to React, or better yet use a sprinkling of Stimulus to call methods written in Ruby to do the AJAX calls, render the views, and pass that back to the Stimulus controller to update the DOM. 

### Problem 3: Running HTTPS on a local environment

The  GEOLocation API docs explicitly state that it is only available in a secure context. As a new developer I had no idea that this would affect my local environment. For a few hours on my first attempt I wrestled with code that just wasn't working, and without seeing any errors. I was learning how Stimulus worked so I thought it was another idiosyncrasy that I had to deal with. 

Once I realized I needed to be starting my dev server with HTTPS I tried out writing my own self signed certificate via open-ssl. This process worked great, but Google hated it, Safari accepted it, and FireFox for some reason didn't like the mutationObserver wrapper that Stimulus was using. I was able to open the application on Safari once or twice, and then it stopped working. I used another method to write the self signed certificate and got the same results; it worked for a bit and then stopped. Puma-Dev came to the rescue on this one. Google accepted the certificates that it generates on connection, and development commenced. 

Security is hard, and sometimes you need to use pre-made solutions because it is the better option. I had heard about puma-dev when I first looked into running the dev server on HTTPS, but didn't want to add another dependency. 

## Would I do this differently given the chance
I would love the opportunity to write the code in a way that makes sense to me, but at the same time if I was given the chance to continue writing it with SSR React and Stimulus I would. There is a lot to learn by using new technologies, and I don't think I scratched the surface of what those two tools can use. 

The one thing that I would change about my process no matter what is using TDD/BDD to guide the code that I am writing. Testing for the outcomes that I want is agnostic of the tools that I would use. I think that in general testing is the area that will have the most impact on how I develop in the future, and I look forward to learning it.

For those of you who read this. Thank you. I am always looking to learn so if you have any feedback let me know.

Repo: https://github.com/Nroulston/LoadUp-Engineering-Coding-Challenge
Email: Nicolas.roulston@gmail.com






