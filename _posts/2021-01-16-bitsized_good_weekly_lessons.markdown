---
layout: post
title:      "BitSized Good weekly lessons"
date:       2021-01-16 19:23:44 +0000
permalink:  bitsized_good_weekly_lessons
---


Guess what Designers might not know CSS and responsive design; I might not either. Webflow doesn't have a way to store secrets! Big uh oh moment when trying to work with external API's. 

### CSS and Responsive design working with UX/UI

This past week we have been working furiously at finishing up our Self Love Beauty project at BitSized Good. During that process we have figured out that a great designer, developer team needs to be able to speak in a common language; at least we figured out that it was something we needed for our team. Once we got on the same page things began to flow a little more smoothly. 

When I first say the handoff from the designers with Self Love Beauty I was so excited. There was a style guide, some components broken out, and a really great looking site. What we discovered is that the components that we thought we would need needed to be broken down even farther. To speed up development we needed to start talking about containers that held our components, images, frames, flex box, alignment, and more. If we could design CSS classes that took care of the majority of setup, and then are customized with addition of a few other classes the speed of development increased, and so did the continuity of our site. 

I consider myself a backend developer, so the land of CSS is new to me. It makes a lot of sense to me when you look at CSS classes as factories that spit out instances that contain all the methods that you want. When you use class inheritance in the backend you are giving yourself the ability to customize a new set of instances. That sounds a lot like having a parent CSS class that cascades it's styling down to nested elements, and being able to override the CSS with custom classes(methods) at each element level, or even inline css. 

I learned a lot of the above when working in Webflow to translate. I also begin to see the difficulty in great design that is responsive as well as aesthetically pleasing at the same time. The idea of being able to use padding, margin, and relative placement is great for a single size screen that isn't responsive, but it makes you have to do all kinds of work to make each breakpoint format correctly. So how do we communicate as a team that design needs to be thought of in terms of flexbox children and Grid sections. I found that horizontal alignment with relative placement, or using margin would create disastrous ripples downstream when changing the size of my screen. What was interesting is that vertical alignment didn't have as much of an effect when used with relative placement. What I ended up doing was formatting my containers in a way that was similar to how I have used Materialize CSS. You create a container that houses smaller containers. The first one delegates base alignment and each subsequent container using percentage size and flex or grid was able to be placed relatively closely to where the designer wanted it. Some of it to me seemed to be more complex than necessary, and makes me think that there is a reason why we have these design philosophies and that a lot of sites end up looking like bootstrap. The style guide can be a strong influencer as to where items are placed in relation to each other, and how the CSS will look overall.

As I said I prefer the backend so let me know if conceptually there are improvements to my conceptual knowledge and best practices for the teams of bootcamp grads that I get the pleasure to work with. 

### Webflow and its limit of custom code, results in microservice architecture.

Initially I got really excited when I heard that I would be able to write custom code for a tool that let me visually design the front end, and inject javascript when I needed it. Oh how little did I know. It is funny that concepts that you believe you know don't crop up until you experience them out of their usual context. Coming from a fullstack background you assume that security is going to be a part of the process for example you can write env secrets. Well what happens when you don't have access to the backend at all? Secrets now live in the public domain. So your original plan of using an S3 bucket to hold React components that are imported into the Webflow front end falls apart. The S3 bucket will expose that key if not properly locked down, and at the same time will leave that key sitting in the clients browser when it's imported. Talk about security risk! 

Well now we can't go with the 'simplicity' of S3 Buckets. Conceptually you have an app that needs to talk to other apps and do that securely. What do you do if you can't control the security of one of the apps? Well in this case you set up a gateway API. An example use case is using Webflows built in hook feature to send username and email information over to Constant Contact whenever a new record is made in the CMS. Well Constant Contact needs an API key unique to the account holder, and you have nowhere to store it. This sounds like the perfect situation for a gateway API. The question is how do we implement. 

So let's get to building it then. The first option was going with Ruby on Rails. It is quick, clean, and has a lot of security already baked in. Personally it smelt like over engineering to use RoR. Thus option 2 comes along. We can just use Sinatra, Rack, and Rack middleware to control the bloat and have a very specific application. To be honest I like this idea a lot, but here comes the kicker: do you build it and charge the cost of hosting to the client, do you go with a service like AWS Gateway API(What a cool service) and charge the client the AWS fees, or do you go with Zapier. 

I mean Zapier literally does exactly what we are asking it to do. It hooks into an app and grabs the information, and sends it into the next apps API endpoint. Are we going to build Zapier everytime a new client wants to integrate webflow data with another app? You know I really want to say yes we build it every single time, and eventually have a system in place that is plug and play with clients in a multi-tenancy sort of way. What I have to say in this case is Zapier could possibly be free forever for the client so why am I building something that they don't need. Now when they jump out of Zapiers free tier it absolutely is worth looking at how much AWS would cost, and if it would be cheaper to have a custom Gateway API built at the moment. 

What I have learned in the long run is that if BitSized Good is going to continue to work with Webflow that we are going to need to either charge the client for the hosting costs associated with the microservice architecture, or we are going to need to receive funding that supports hosting multi-tenancy API gateways and apps. How rad would it be if option two happened and we could develop solutions specifically for non-profits that brought the cost of their technological solutions down. I mean Zapier costs 17 dollars a month. A pittance to some, but for a small company 17 dollars can be hard to cough up.






