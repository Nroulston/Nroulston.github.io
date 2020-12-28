---
layout: post
title:      "Get your own experience, this is mine."
date:       2020-12-26 13:20:46 -0500
permalink:  get_your_own_experience_this_is_mine
---


Clickbait title for a clickbait theme, perhaps not. You decide if you can get your own experience after hearing my story.

Bootcamp graduates do not have enough experience to get jobs. This seems to be a common theme based off the algorithmic feed of LinkedIn posts containing messages about long job searches, or recruiters saying it's time to give the inexperienced a shot. Take away the algorithm noise and the signal is still clear. 

1. My cohort mates have received numerous rejection letters before the initial interview: the explanation is always not enough experience for an entry level role. 
2.  Nonprofits are springing up to take care of this experience gap by organizing groups of the inexperienced across disciplines to work on technical projects for nonprofits around the world.
3.  For profit organizations are popping up willing to train you for an extended period of time and then place you for close to half of the average bootcamp graduates initial pay. (Honestly not a bad option as a last resort, and if you have the ability to move anywhere in the country with short notice)

Before I graduated from bootcamp I was receiving this signal loud and clear. So I decided to solve my own problem, and by doing so hopefully create a solution that others could implement themselves. I was going to volunteer my services to work with cross disciplinary teams to work on websites for friends, open source projects, and for nonprofits. 

## If you are here on your journey hit the brakes and learn from my mistakes

Scope your mission down, and iterate on the feedback you are getting. I had a vision in mind when I first started down this path. The vision is still the same, but how we are accomplishing it has changed. 

 I had identified three areas that I could potentially find project work for myself and others. There were open source projects for XR with React, InterPlanetary File System gems for easy local hosting, and building out virtual training for Boston Dynamics robot dogs. Friends and colleagues had websites that we could improve, and by doing so gain a recurring revenue stream from their new sales. Our first nonprofit project went from a basic redesign to a custom learning management system(LMS) in our first meeting. While all of these are extremely exciting projects to work on, most of the projects were too large for groups of people learning to work together without a framework as to how to work efficiently. 

### This is when it struck me

We needed small projects that were relatively simple in nature that could be completed in a relatively short amount of time; 1 to 2 months. So large scale projects were out. This left us with the choice of working on friends web pages, and nonprofit web pages. As an organization that is asking people to do work for free, having some members potentially get paid just didn’t sit right. We also have the opportunity to help make the world a better place. Ultimately if I was going to spend my time trying to build an organization to help people it should do two things. 1. Help people around the world. 2. Help a single person get experience while helping people around the world. Cue inception music here. Helping people to help people help people. 

#### BitSized Good is created and can get work, or can it?

Great! Now we have scoped down our mission and what we are going to do. We are going to work with nonprofits and provide a website redesign that will have a direct impact on the nonprofits goals. We will start with UX discovery and end the project when the developers hand it off to the nonprofit. 

What seems like a simple task of going from point A to point B is not. We have this idealistic goal of being able to come in with our new super powers and change the world. Hubris? I dare say I had far too much of it. We wanted to come in and build bespoke solutions from scratch and thought we would be able to walk away from them after handoff. Our clients quickly pointed out that they wanted to use something like Wordpress, because after we were gone who was going to maintain the site. Aghast… We hadn’t even thought of that. At this point our clients knew more about their situation than we did, yet we were trying to tell them what the best solution was for them. It was time to go back to the drawing board and learn about what the first problem was for a nonprofit. 

As it turns out nonprofits when first starting out are just like small businesses that are just starting out. Nonprofits potentially have a small budget, and the founder will be doing many of the technical tasks that larger organizations will generally have the budget to pay for. One of those outsourced items is paying for a website to be built and maintained by a third party. So you see Wordpress is a known solution for building out websites that supposedly is maintenance free. 

While we have found a solution that most nonprofits are comfortable with due to name brand recognition we have run into more problems.

1. At first Wordpress seems to be the easiest solution, but the costs of maintaining a Wordpress site can quickly stack up. There are a lot of plugins that make it easy to implement a solution, but Wordpress is notorious for having plugins break due to incompatibility. It is then the job of the site maintainer, in this case the nonprofit founder, to update those plugins and make them compatible again. If the plugin incompatibility isn’t noticed for awhile the website may be broken for an extended period of time. This cost isn’t necessarily financial, but it does cost the nonprofit their time. If they decide to pay for developer support it will cost them money. There are a number of costs that also come along with plugins as all are not free. 
2. The developer bootcamp graduates of Flatiron and numerous other technology bootcamps don’t come out with PHP experience, and don’t have the desire to learn it.
3. The UX/UI bootcamp graduates don’t know how to translate their designs to Wordpress and feel like site builders like Wix are too simple. 

Once again we have to iterate on our solutions model keeping in mind our original vision of helping both parties involved. What we know is that we need to have an easily maintainable cms and ecommerce website which has the ability to be visually edited by the client, and can have plugin-like features developed in Javascript and React. Once the website is handed off there should not be any need for the client to worry about their webpage breaking due to incompatibility, and they can update the content and/or the design without the help of a developer.

### We have arrived at Webflow

Webflow addresses each of the problems we defined above. 
UX/UI is able to design inside of Figma and pass off those designs to a developer who is able to use a drag and drop interface/ custom CSS to build out the visual nature of the page. 
Developers can build out solutions, using Javascript and React, not already provided by Webflow, and deploy to AWS S3 and then import via scripts into Webflow. 
Nonprofits have the ability to edit the webpage two ways. They can use the editor to update copy, images, and general aesthetics, or they can use the designer to go in and use the same interface the developer used to build the webpage. 

### It isn’t perfect - What now?

Ultimately this isn’t a perfect solution. We can still improve on the process by building out our own website builder using open source tools like GrapeJS. We also need to figure out a sustainable solution to scale nonprofits web presence without involving huge costs. How do we get a custom dynamic page hosted with only the cost of the domain? Some solutions may be a mix of Netlify and Firebase. How do we bring the cost of accepting donations down? Ultimately in my mind this could be a lifelong pursuit of building out a framework like Spree or Solideous that allow the speed of making these decisions a lot faster. 

We also haven’t addressed the ideas of making it easier for developers to learn to work together yet. Is there a way to TDD a merge request, and teach best practices for working together? How do we get mentors, and leads who will give their time to do good? Is this the best tool to teach teamwork in the first place? I imagine having students work on known project solutions that are mostly built out and just a plugin needs to be added might be a good direction to start with. 

There are so many questions, but what is great about trying to be agile is that we can move forward with our current solution since it is the best one we have, and at the same time we can continue to research if this is actually the best solution. 

### Hopefully this gave you value

As a new developer we have the ability to change the world. Remember you can scope down that idealistic goal and make bitsized changes to how things are done. Come join us at BitSized Good to take advantage of the research we have already done, or go your own way and help a local nonprofit. I highly recommend that you start with a small goal and learn from your experience. 

If you want to help or have ideas/feedback you can always contact me [Email](nicolas.roulston@gmail.com), or [LinkedIn](https://www.linkedin.com/in/nico-roulston/). 

