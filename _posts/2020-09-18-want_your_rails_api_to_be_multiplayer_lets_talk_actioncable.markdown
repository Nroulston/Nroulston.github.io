---
layout: post
title:      "Want your rails API to be multiplayer? Let's talk ActionCable."
date:       2020-09-18 14:59:10 -0400
permalink:  want_your_rails_api_to_be_multiplayer_lets_talk_actioncable
---


Multiplayer, realtime communication, and async fetches combined in one tuturial. We will be using WebPack.js v4.44.2(the latest as of this date), Rails 6, and Vanilla Javascript.

## The backend set up

The only thing I am going to assume is that you already know a few things about setting up a normal rails API. If you don't know what that is you can still follow along with this tutorial to learn the basics of how ActionCable works. First things first, create a rails API! I love this part.

```
rails new appName --api --no-test-framework
```

Thats is it! Your API has just been set up for you without all the unneccesary bloat code that a full rails app might use. We need to generate a few things to work with the rest of the tutorial. 

```
rails g channel Room

rails g scaffold Room name

rails g scaffold User name
```


Since we created our app in API mode the number of files that you just created is quite small. In the future you would probably get even leaner by not using scaffold and only generating the things you actually need. Don't forget to migrate.

```
rails db:migrate
```

## Configuring the backend

Uncomment out the gem 'rack-cors' in your Gemfile. Run `bundle install`, and proceed to uncommenting out this code in cors. rb. After you have uncommented make sure to put a star as the origins argument. This allows you to accept requests from all origin urls.

```
# test/config/initializers/cors.rb

Rails.application.config.middleware.insert_before 0, Rack::Cors do
     allow do
          origins 'example.com'  <- Change to '*'
          resource '*',
               headers: :any,
               methods: [:get, :post, :put, :patch, :delete, :options, :head]
     end
end
```

ActionCable needs it's own routes to operate. In your routes.rb file add the following to the routes that scaffolding already created

```
# app/config/routes.rb

mount ActionCable.server => '/cable'
```

ActionCable has its own security measures that you can modify per your needs. Since this tutorial runs on a local server you need to tell ActionCable that is where it should expect a WebSocket connection from. I added mine at the bottom of the file.

```
# testApp/config/environments/development.rb

config.action_cable.allowed_request_origins = ['file://']

```

Lastly you have to let your channel that you created know where it will be streaming information from. Add  `room_channel` as the argument for stream_from in your room_channel.rb

```
# app/channels/room_channel.rb

class ChatChannel < ApplicationCable::Channel
  def subscribed
    stream_from "room_channel"
  end
end
```


Thats it! Your API is now ready to work with a WebSocket connections via ActionCable Magic. Some call it automagical.

##  The Front End Set Up

Make a new directory outside of your Rails API's directory. I chose to nest both my front end directory and my backend directory in a parent directory. 

Once you have that directory its time to get Webpack.js installed on your local machine. The installation tutorial on their website is fantastic so I will be borrowing heavily from there.


First run
``` npm init -y```

then run one of the following. If a new release is out you will have to use the version option
```
npm install --save-dev webpack
# or specific version
npm install --save-dev webpack@<version>
```

then run
```
npm install --save-dev webpack-cli
```

lastly you need to get actioncable loaded into the frontend 

```
npm install actioncable --save
```

Lastly you may or may not have noticed that your backend now needs to have webpack installed as well. If you get an error, the error gives you explicit instructions on how to fix it in this case. Go Rails!

### Side note on WebPack.js

Webpack is used in this project on the front end. To properly import actioncable in a front end environment you need WebPack to work it's magic of compiling all of your import and export and require statements into a single file. This gets you up an running,  but also can open up a whole can of worms as to how you need to structure your files, write your code, and needing to make sure that you run npx webpack everytime that you change the frontend code and want the browser to know about the differences. Compiling in javascript... Yay. Don't worry though the following is just a basic use scenario and following the tutorial should have you up and running to play with the main star of this show. ActionCable.

## Frontend Configuration

### webpack config
Start at the scariest place. I have heard that some Webpack config files are monstrosities that cause stress for days. This will not be one of those files.
Make a webpack.config.js in the top level of your frontend directory and copy this into there.
```
// frontend/webpack.config.js

const path = require('path');

module.exports = {
  entry: {
    'main.js': './src/index.js',
  },
  output: {
    filename: '[name]',
    path: path.join(__dirname, 'dist'),
  },
	
  optimization: {
    minimize: false
},

};
```

If you don't want to do this it's ok. Webpack comes configured already to work out of the box. **Trust me you really want to do the above**. Webpack automatically minimizes javascript which means two things. One: Debugger isn't triggered after minimization. Two: variable names are changed so digging into your code in devtools is nearly impossible. Three days of madenning development flow and I tackled the research of how to configure Webpack to not minimize.

### Frontend file structure
Webpack works by looking at a file that you are pointing at and following all the import,exports to require the modules you will need to be using with Object Oriented Javascipt. It then compiles that code into a single file and outputs it to where you specified in the config file. 

When you set up the config file you were telling it that it was looking for a starting file index.js inside your src file. You then output to the file dist/main.js. Below is a simplified file structure that will get you started.

```
-FrontEndDirectory
    -dist/
	      -main.js
				-index.html
    -src/
		    -index.js
```

You will see some package-lock and package json files in the top level. We won't be touching those. 

### Setting up a single connection for all clients to use

I say setting up a single connection because it is possible to set up multiple connections per client in a single session. P.s. ActionCable uses the term consumer instead of client. I will be using it from here on out. Each of your clients needs to open up a WebSocket connection; a persistent connection to allow duplex(two way) communication in real time. Sounds complicated, because it is. Thankfully DHH has said that the only reason to use WebSockets is if it is easy. Insert the following into your index.js file

```
// FrontEnd/src/index.js

ActionCable = require('actioncable')
 
var cable = ActionCable.createConsumer('ws://localhost:3000/cable')
 
function establishActionCableConnection() {
    cable.subscriptions.create('GameRoomChannel', {
      connected() {
       console.log('connected to the room')
      },
  
      disconnected() {
       
      },
  
      received(data) {
       console.log(data)
        
      },
      
    });
  }
	
	establishActionCableConnection()
```

DHH said it should be easy, and the above should log the message connected to the room when you open up index.html. 

**If you want to test your code** You need to do a few things. Navigate to your backend directory and start your Rails server `rails s` in the terminal. Navigate to your front end directory and run `npx webpack` then `open dist/index.html` 

There are a lot of callback methods that the ActionCable module utilizes. Connected and disconnected are two of them that allow you to run code when a channel and consumer succesfully connect, and or are disconnected. The received method is our listener. When a JSON formatted string is sent over from the server the received method snags it and converts it into valid JSON, and you then you get to play with it.

### Working with sending and receiving data

In development ActionCable is set to work asynchronously, in production it switches to a Redis server which uses a Pub/sub method to stack and send data as it is added to the que. Since I made this project for 4th project at Flatiron I had a few constraints and the potential for my reviewer to not understand how ActionCable worked. So we will be communicating with our controller via Fetches and then sending information to be broadcasted. In the future you can skip using fetch and send the data directly to the channel from the client.

#### send over your first user to be broadcast.

In index.js add the following code.

```

 fetch('http://127.0.0.1:3000/users', {
      method: 'POST',
      headers: HEADERS,
      body: JSON.stringify(
			    {
			    user: { 
					    name: "TestName" 
							}
			    })
      })
      .then(response => response.json())
      .then(json => { 
       // if you want you can handle data sent back from your api here. The problem with 
			 // this is you will only be updating the client that sent the post fetch, and all other
			 // clients won't have those changes.
        
       
      })
```

You can do one of two things with this fetch when you send it. You can create an Object in JSON format that has all the data you want to send, or you can send over a javascript object that matches the backend model. The benefit to sending an object is if you have relationships built out the object on the client side will have all the information of it's belongs_to, or has_many objects too. 

### Receive the fetch in your controller and send it to all consumers

Now it's time to catch your post fetch. It will be sending data to the action create in your Users controller. How you handle that data is up to you. We won't be persisting it to the database or manipulating it in this tutorial. So delete everything inside the action and paste the following. 

```
# backend/app/controller/users_controller.rb

def create
  ActionCable.server.broadcast('room_channel', {name: user.name})
end
```

ActionCable uses broadcast to send the stringified second argument `{name: user.name}` to the channel `room_channel` and on out back to your client.

### receive the data in your client

Remember the received function in the consumers index.js file. That method we set up early has been waiting for us to send it something to work with. 


```
// frontend/src/index.js
received(data) {
       console.log(data)
      },
```

If everything worked out correctly you can open your console and see an object there. Go ahead and play around with dot notation and see what the name of the property is of the object. The real fun is opening up some incognito tabs and seeing that the received is triggered in all of them and each of them will console log the message that was sent from the backend. From here the world is your oyster. Make a game, make a chat app, have realtime notifications pop up for consumers, make a blog that updates comments in real time. The list goes on.

## Food for thought

There is so much more that you can do with ActionCable. I highly recommend reading the docs to get a better grasp as to what is happening. That being said StackOverFlow and blogs will be your best friend in understanding some of the more nuanced ways that ActionCable Works. 

Keep an eye out for a multi channel version, and deeper dive into how to work with ActionCable.

You can check out the current project [repo here](https://github.com/Nroulston/ActionCableCohortGame)

You can also see a [video demonstration here ](https://youtu.be/EYSuR9ryIc8)




