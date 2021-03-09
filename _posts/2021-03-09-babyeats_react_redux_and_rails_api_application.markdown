---
layout: post
title:      "BabyEats – React/Redux and Rails API application"
date:       2021-03-09 19:08:19 +0000
permalink:  babyeats_react_redux_and_rails_api_application
---


 
 
Here I am, writing a blog about my final project for Flatiron school. With a lot of excitement as well as a bit of anxiety, I cannot believe that I am at the end of this journey.  
I created the BabyEats application which is coded with Rails API for the backend and React/Redux for the frontend. It is simple to use it. Users can create meals per day and by clicking on the calendar date, the meals for that day will render. 
The first step was creating the Rails API backend and that was an easier part of this project. Using Rails generators to create necessary files helped me to move faster to the frontend planning.  
Initially, I struggled with the organization of files and the implementation of Redux from the very beginning. Once the setup of React files and Redux were done it became easier to control the flow. 
I decided to use Redux for managing the application state. The beauty of having all data in one place and the easy access to the same made me go that route.   

The Redux flow looks like this : 
ACTION => REDUCER => UPDATED STATE 
 
The whole global state of the application is stored in one container- Redux store. To change the state object is to create an action, to be more specific, an object describing what happened, and dispatching it to the store. That can be done by writing a pure reducer function that will process a new state based on the old one and the action created. 
With Redux, we can focus more on presentation in our React components and use actions and reducers to handle the logic of organizing data. This allows us to keep many of our React components simple — no need for passing props through many nested components, no need to use the component state to keep track of all the data. 
When we need a single tiny piece of information in a React component, we need to connect to the store and in the function, `mapStateToProps` specify what part of the state we want to grab. We then refer to it with either `this.props.pieceOfInformationWeNeed` (in class component) or just `props.pieceOfInformationWeNeed` (in functional component). 
## Actions 
Actions are like events. They are the only way to send data to the store. The data can be gathered from user interactions, form submissions, or API calls.  
Actions are POJO (plain old JavaScript object). Each action must have a type and a payload.  
 
 
 
 
 
This is an example of an action creator from my application.  `days` is the action’s payload 
``` 
 export const setDays = days => { 
    
    return { 
        type: "SET_DAYS", 
        days 
    } 
} ```
## Reducer 
 
Once we create the action, it is sent to the reducer, which is just a pure function. It takes 2 arguments: the current state and an action. Using switch statement reducers go through the action types and describes how to change the state depending on the type. 
This is example of a reducer from my application. 
```
const days = (state = [], action) => { 
        switch (action.type) { 
      case "SET_DAYS": 
            return { 
                days: action.days 
            } 
      
  default: 
            return state 
   } 
  } 
  export default days ```
 
 
## Store 
 
Redux store is the object that holds the application’s state. The convention is to have one store in the application, and this is the process of creating the store: 
``` 
import { createStore } from 'redux' 
import { Provider } from 'react-redux' 
 
const store = createStore(reducer) 
ReactDOM.render( 
  <Provider store={store}> 
    <Router>  
      <App className="App"/> 
    </Router> 
  </Provider>, 
  document.getElementById('root') 
); ```
 
 
So what do we do with all of this?? 
The next step is to import `connect` in the React component in which we plan to get the state from the store or dispatch an action or both. 
This is the process: 
```
import { connect } from 'react-redux'; 
import { setDays } from './actions/days'; ```
 
`connect` accepts 4 arguments, but mostly 2 of them are only used. The first one is always `mapStateToProps` if we are not planning on using it we can place null instead of the function. The second one is `mapDispatchToProps`  
`mapStateToProps` is a function that returns a state object, and we can be as specific as needed with the piece of state we need for the component in React. 
`mapDispatchToProps` is dispatching the desired action to the store. 
 
And this is how it is done:  
 ```
const mapStateToProps = state => { 
 return { 
  items: state.items 
 } 
} 
  
const mapDispatchToProps = dispatch => { 
 return { 
   setDays: () => { dispatch(setDays()) } 
  } 
} 
  
export default connect(mapStateToProps, mapDispatchToProps)(App); ```
 
This is equivalent to: 
 
 ```
const mapStateToProps = state => { 
 return { 
  items: state.items 
 } 
} 
   
export default connect(mapStateToProps, {setDays})(App); ```
 
It is easy to love and hate Redux at the same time. It seems that there are too many steps to get it to work, but once you understand its purpose it is so convenient when storing all of the state data in one container. 
 
 
