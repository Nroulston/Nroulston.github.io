---
layout: post
title:      "AWS S3 + React = Plugin you can include on any website"
date:       2021-01-02 20:09:40 +0000
permalink:  aws_s3_react_plugin_you_can_include_on_any_website
---


The following is the basics to get started. At the end I postulate on state management with Redux and potential architecture decision if building out a web app with multiple plugins that are included or not included. I also have note explanations at the bottom to expand on things I found interesting, but you don't need to know to do this.

## Set up the plugin 
We aren't going to be using ```npx create-react-app my-app``` to build out the plugin. We aren't building out an entire React App so the idea is to only have the dependecies that you need, and to keep the payload sent over the wire as small as possible. This requires a few extra steps as we will have to manually scaffold out our project, but fear not. Just copy and paste ;).
### Load up react
```
npm init

npm install react react-dom

# side note 1. 
```
### Create the file structure

-dist
    -index.html
-src
    - index.js
-.babelrc  // don't forget the period at the front of the filename
-webpack.config.js

### Build stock react component and html

```
// index.html

<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>React Plugin</title>
</head>
<body>
 <div id="react-plugin"></div>
 <script src="bundle.js"></script>
</body>
</html>
```

Index.html will be used for local development. The div with the id of react-plugin is where we are going to render whatever react plugin that you will build. I would suggest using something less generic if you plan on including multiple plugins in a webpage. 

```
// index.js

import React, {Component} from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
 render() {
   return (
     <div>Hello World</div>
  )
}
}

ReactDOM.render(
React.createElement(App, {}, null),
document.getElementById('react-plugin')
);
```

### Configure Webpack
Webpack is what allows us to write in multiple files in our development environement and then send over the wire a single packet: in this case it will be bundle.js.

Install webpack
```
npm install --save-dev webpack webpack-dev-server webpack-cli
#Note 1.
```

Add the following to package.json in the "scripts" key. This allows you to run a local version using `npm start` in the command line.
```
 "start": "webpack serve --config ./webpack.config.js --mode development --progress  --port 3000"
```

Configure webpack 
```
//webpack.config.js

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      },
       { test: /\.css$/, use: ["style-loader"," css-loader"] },
       {
         test: /\.(pdf|jpg|png|gif|svg|ico)$/,
         use: [
           {
             loader: 'url-loader'
           },
         ]
       },
       {  
         test: /\.(woff|woff2|eot|ttf|otf)$/,
         loader: "file-loader"
       }
    ]
  },
  resolve: {
    extensions: ['*', '.js', '.jsx']
  },
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist'
  }
  };
	```

### Configure Babel

Install Babel 
```
npm install --save-dev @babel/core @babel/preset-env babel-loader @babel/preset-react @babel/plugin-proposal-class-properties
```


```
// .babelrc

{
"presets": [
  "@babel/preset-env",
  "@babel/preset-react"
]
}
```

Woop Woop! Scaffolding is complete. Fire up the local with `npm start` and check out your local host to make sure it works.

## Set up S3 bucket for deployment 





1. Create an S3 bucket 
      1. enter in a unique name
      2. Uncheck block all public access // note 2
      3. Click Create Bucket
2. Configure Cors 
```
[
    {
        "AllowedHeaders": [],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```



### Build and deploy

Create your build script in package.json `"build": "webpack",`
Create your deploy script in package.json `"deploy": "aws s3 cp ./dist/bundle.js s3://BUCKETNAME/ --acl public-read"`
Build and deploy `npm run build && npm run deploy`

### Include your plugin on the page you want

```
<script src="https://unpkg.com/react@16.4.1/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@16.4.1/umd/react-dom.production.min.js"></script>
<script src="https://<YOUR_S3_REGION>.amazonaws.com/<YOUR_BUCKET>/bundle.js"></script>
```

Don't forget to have the target div set have the same id as your local html file.


### Musings on why you would want to do this and where to take it.

In reality why would you want to create a react plugin? In my case I am building it for someone who wants to use Webflow, and have some custom functionality that Webflow doesn't provide. What is interesting to me is that a non dev can build out their entire website, and then include plugins (can anyone say Wordpress with React). The no code movement is gaining strength and there are powerful tools out there, but what happens when you need something that goes beyond the scope of the no code builder. My thoughts are you can go two directions: 1. Build microservices and manage everything through API calls, or 2. Write small single function plugins that do exactly what you expect when loaded into the app.

In essence I think the question that needs to be answered is how do you build an ecosystem of plugins that can use state management  inside of a host website without writing crazy spaghetti code. I think it is very possible to write these plugins to access a global state management component. As each plugin is added they write to the global state, and will update to that global state. If any changes effect other plugins those plugins will get notified of the global state change and react accordingly. 

In the end it sounds like the react version of Wordpress, but hey nobody said that the solution you build is the solution you had in mind. 

An interesting thought can be found here. https://stackoverflow.com/questions/53067433/how-to-implement-plugin-based-architecture-using-redux

###  Notes

1. There is a difference between npm install, npm --save, and npm install --save-dev. 

In older code examples you might see `npm install --save`, but that has been folded into the `npm save` command. What this does is save the installation to your project so that the people who clone your project will install the depenencies that they need. If you are building something that will become an npm package or locks people into using your packages unnecesarrily you use `--save-dev`. This allows you to use the set up that you like, but doesn't require another user to use those same packages. 

2. Check out the warning and know that you are opening access to the world https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html#block-public-access-static-site




