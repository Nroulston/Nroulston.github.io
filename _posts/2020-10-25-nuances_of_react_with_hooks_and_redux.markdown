---
layout: post
title:      "Nuances of React with hooks and Redux"
date:       2020-10-25 18:40:53 -0400
permalink:  nuances_of_react_with_hooks_and_redux
---

### A Flatiron students primer to working with hooks

## Everything changes with redux-toolkit

When implimenting Redux with class based React components a developer will end up with a lot of small files populating the programs architecture. Actions, State components, Pure components, reducers, combining reducers, etc... Say no more to boilerplate code. Well not say no more, there is still a bit of boilerplate to working with Redux. All of your earlier code in your stateful components, pure components, and the rest will now be taken care of with the redux-tool-kit and the bevy of hooks, tools, and ready made packages that you already integrated.



## When I say everything changes I mean it.

### 1. The file structure

If you don't believe me take the time to spin up a new create-react-app using the redux template. P.s. It is how the docs say things should be done so give it a shot. 

```npx create-react-app my-app --template redux```

If you take a look at the code structure you will notice there is no longer a components, action, reducer, or container files. This is all replaced with the features folder. This change ultimately is just structural, but it is a new way of thinking where to put your code.

### 2. How to connect to the store.

You no longer will be using connect at the bottom of your container components. Instead a global store file is created and you access it with hooks. 

```
// app/store.js

const store = configureStore({
  reducer: counter
})
```
All of your reducers will be imported into the store file as a key value pair and will be connected to with hooks. We will get to that later. After you create your store you include it at the top level of your app.

```
// index.js
ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
        <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

By nesting the App component inside of the provider with a props of store the rest of the application will now be able to use hooks to access the store from any component. 

### The slice file becomes your best friend

Actions, reducers, thunk calls, global state, global state defaults, complex selectors for specific global state you need are now all written inside one file. To quickly set up your structure follow the following. 

```

import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'


const initialState = {
  brewlogs: []
}
const localHost = 'http://localhost:3001'

export const fetchBrewlogs = createAsyncThunk('brewlogs/fetchBrewlogs', async() => {
  const response = await axios.get(`${localHost}/brewlogs`, {withCredentials:true})
  return response.data.brewlogs
})

const brewlogsSlice = createSlice({
  name: 'brewlogs',
  initialState,
  reducers: {
    brewlogAdded(state, action) {
      state.brewlogs.push(action.payload)
    }
  },
  extraReducers:{
    [fetchBrewlogs.fulfilled]: (state, action) => { state.brewlogs = action.payload}
  }
})

export const { brewlogAdded } = brewlogsSlice.actions

export default brewlogsSlice.reducer

export const selectBrewlogById = (state, brewlogId) => 
  state.brewlogs.brewlogs.find(brewlog => brewlog.id == brewlogId)
```
	
The above is the powerhouse of Redux interaction. You start with importing the tools you need from the redux toolkit. Create Slice allows you tell the redux store what global state it has for this slice name:brewlogs, what actions it will be listening for, and at the same time how it treat the action that is dispatched(reducer). The extra reducers inside of the slice deal with async Thunk calls. At the bottom of the file you export your actions, the reducer to include in your store.js file, and lastly any complex calls for state that you want to use in your components. 
	
### useState(), and dispatch() hooks
	
These two hooks allow you to work with Redux outside of your slice files.
	
useState is, simply put, a getter that retrieves information from the global state using a callback function. You can write this inline, or you can write it in your slice and export it.
	
dispatch is used to send out your actions which then get handled in your slice file.
	
As crazy as it sounds that is the basics of working with Redux using hooks. There is a bit more complexity to hooks in general when working with the entire React app, but that is saved for another blog. 
	
	
