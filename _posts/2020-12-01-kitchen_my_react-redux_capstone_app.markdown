---
layout: post
title:      "Kitchen my react-redux capstone app"
date:       2020-12-01 13:40:56 -0500
permalink:  kitchen_my_react-redux_capstone_app
---




*** React from scratch ***


**jsx, components,props, state**

*app component -> produces jsx and handles user events
jsx -> set of instructions to tell react what content to render on the screen*

jsx tell react to show normal html or another component if its a component its gonna call the function and inspect all the jsx elements inside
react library is a reconciler 
react dom is a renderer
props are passed down from parent to child 
we make use of the state systrem whenever we want to make an update on the screen
class based componant can use lifecycle method keeps track of state 
rules for class components must be a javascript class, must extend subclass React.component
must define a render method that returns some jsx

rules of state: only usable with class components can be used with new hooks system in functional components 
state is a js object that contains data relevant to a component
updating state on a component cause the compoennt to rerender 
state must be initialized when component is created
state can only be updatted using the function setState

constructor functions are a very good place to initialize our state when the component is first created
example:
```
class App extends React.Component {
consturnctor(props) {
super(props};
this.state = { someporperty: null};
somefunction(
newproperty => {this.setState({someporperty: new prop value})
}
//here we can do this
render() {
return <div> someproperty: {this.state.someproperty}
}
```

constructor is automatically called with props object also a ceremonial thing is to do super(props) because how the inheritance system works so we are inherating from the react class the consturctor is overriding the set up for the original React.Component constructor but to inherit everything we must call super and pass in props
if we dont do this we get an error message saying super hasnt been called 
so under super we can initialize our state object with whatever property trhat we want and we can default it to a reasonable value wehter its a number text or what not or null if we dont know the value 
now to update our state we always call setState if we have some kind of data coming from a fetch request or an async request its best to use it when the state is initialized and use setState to set the value of the property like the example above the reason behind it is the async request takes time to fetch the data, we can use a lifecycle method would be better and i will show this next  


***app lifecycle walkthrough***

1. js file loaded by browser
2. instance of App component is created 
3.App component 'consturctor'  function gets called 
4.State object is created and assigned to the this.state property
5.we call api or get some data 
6.React calls the compoennts render mehtod 
7.App return jsx, gets rendered to page as html

remember with async request that the setState will not work until the promise is resolved that is why we call our api or fetch thru a callback so than that call back function will get called after the promise is resolved so after the reuslt comes back
we update our state object with a call to this.setState
React sees that we updated the state of a component 
so React calls the render method a second time 
Render method return some update jsx
React takes the jsx and updates content on the screen
conditanel rendering content for example if our api is having a problem we can put a condition where it says if someprop is null show a loading div or an error message if our api didnt fetch correctly

***lifecycle methods 
component lifecycle timeline *

constructor -> if we dont specify a consturctor will be created for us ->good to do one time setup
render -> optional its the only method required 
componentDidMount -> content visble on screen -> defined outside of the consturctor will be called one time used to do some inital data loading
render will be called here right before componentDidUpdate
componentDidUpdate -> sit and wait for update ->good for more data loading when state/props change
componentWillUnmount -> sit and wait until this component is no longer showing -> good place for cleanup
alternate state initialization

```
class App extends React.Component {
state = {someproperty: null}
...componentDidMount ... all the rest of the code than render here
}
```


the reason behind this is if we go to bablejs.io and enter this we can see the equivalent or transpiled version of this code which will translate to the consturnctor way we used above
passing State as Props

say we import some component that we made to our App component 
we can create an instance of that imported compoentn in our render method and pass in a prop that we want so in our render would look something like this keep in mind to import the component to where we need it 

```
render() {
// we are taking a property from the state on the app component and passing it as a prop in the 
the importedcompent(note this is a child component )
return <theimportedcompoent someproperty={this.state.someproperty}/>
}
so now when SetState is caled theimportedcomponent will rerender as well since we are passing in the new state change into it as a prop so the parent will rerender any children that its showing as well 
inside of our theimportedcompoennt will look as follows
import React from 'react' 
const theimportedcompoent = (props) => {
console.log(props.someproperty)
}
export...
```

Handeling user input with forms and events
so to handle user input we need a form lets take a small example 
say we want to show a search bar so after the initial set up we pass in the searchbar to our app component in our searchbar component it will look as follows


```
class SearchBar extends React.component {
render() {
return (
<div>
	<form onSubmit={this.handleSubmit}>>
// inside of onchange we will pass a call to handle change 
		<input type="text" value={this.state.value} onChange={this.handleChange} />
// another way to writing the event handelerwithout defining an outside function would be to like the code below
	<input type="text" value={this.state.value} onChange={(event) => 
    this.setState({value: event.target.value})} />
<input type="submit" value="Submit" />
	</form>
</div>
}
our app component will import searchbar and show a functional component 
const app = () => {
return (
<div>
	<SearchBar />
</div>
)
}
export default App
``` 

important => we do not put pranethese whenever we pass a callback function to an event handeler like onChange or onSubmit
handleCahnge and handleSubmit take an event argument they will look like the following 


```
 handleChange(event) {
console.log(event.traget.value)
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('search was submitted: ' + this.state.value);
    event.preventDefault();
  }
```
	
	
properties for event handelers 

onChange -> user changes text in an input
onClick -> user clicks on something
onSubmit -> user submits a form
https://reactjs.org/docs/events.html#mouse-events there are a lot of props for event handelers
as it sits our seachbar component is an uncontrolled component we want a controlled component
in a controlled element we have to set the value inside the input to this.state.whatevervalue we initialized our state with so basically our seachr bar component will look as follows
import React from 'react';

```
class SearchBar extends React.Component {

    state = { term: '' };

    onInputChange = (event) => {
        this.setState({term: event.target.value});
    };
    
    onFormSubmit = (event) => {
    event.preventDefault();
        // todo make sure we callback from parent component
        this.props.onFormSubmit(this.state.term);
    }


    render() {
        return (

            <div className="search-bar ui segment">
            <form onSubmit={this.onFormSubmit} className="ui container">
                <div>
                    <h1><label>  Recipe Search  </label></h1>
                    <input 
                    type="text"
                    value={this.state.term}
                    onChange={this.onInputChange}
                    />
                </div>
                   <div className="item">
                <button className="ui yellow button ">Search</button>
                </div>
            </form>
          </div>


        ) 
    }
}


export default SearchBar;
```


***controlled component steps***

1. user types in input
2. callbacks get invoked
3. we call setState with the new value
4. component rerenders
5. input is told what its value is (coming from state)
the difference in the code is that we want to centralize the value of the input inside of our react component and not our html element/ store data in our react app not the dom 
if i wanna answer what is the value of the input right now i can instead of looking at the dom input value reach in to the state properties that i set state: {term: 'hi there'} vs <input value='hi there'/>

***`Making API requests with react`***

axios vs fetch 
fetch is a function build into modern browesers 
axios is a 3rd party package

```
onSearchSubmit(term) {
fetch('https://example.com/posts?query=${term}', {
  method: 'GET',
params: {query:  term},
headers: {
				"Content-Type": "application/json",
				"Accept": "application/json"
			},})
.then(response => response.json())
.then(result => {
  console.log('Success:', result);
})
.catch(error => {
  console.error('Error:', error);
});
}
thats a lot of code instead we can use axios

onSearchSubmit(term) {
axios.get('https... ,{
params: {query:  term}
headers: {
				"Content-Type": "application/json",
				"Accept": "application/json"
		}
	}).then(res => console.log(res.data) // comes already parsed less code 
}

using the async await syntax 
 onTermSubmit = async (term) => {
        
      const response =  await  axios.get("https...", {
	
            params: {
... the params we want 
                q: term
            }
headers:{
headers depending on the https
}
        });

        this.setState({ recipes: response.data.feed})
        
};
```


the promise based syntax uses .then statement that will be called  anytime the request will be completed 
alternetivaly we can use the async await syntax far easier and less messy to look at
the async await syntax we can just asign to a variable and work with that variable later on avoiding all the .then mess
with axios we can also create client to differentiate API that we are using make a file and import axios thanw e can call axis.create and export that file

***binding callbacks***

dont forget to use arrow syntax or the bind(this)  if we are calling this inside of a callback 
usually the error is something like the following: this.setState is not a function
instead of async onSearchSubmit(term) we say  onTermSubmit = async (term) => {}
would be the easiest solution if we run into this problem 

lists of records and using the Map statements 
```
const  numbers = [4, 9, 16, 25];
const  x = numbers.map(Math.sqrt)
```
this will return a brand new array instead of overriding the original array the map statement is the way to go when we want to go over an array of records to display them or do some logic 

keys in lists

this error message warning each child in (file) an array or iterator should have a unique "key" prop. check the render method ... 
basically for lists we need keys because if we have duplications of values than react is not going to render the new list item since its already in the dom if we have keys that are unique than react will use that information instead of checking the value
after mapping over we can use <div key={array.id}> or whatever unique id is provided in the api 

communicating from child to parent 

assume App pass down props to list component and list pass down props to item component 
the way to do this is basically pass in a callback function that takes an item argument from app to our child component list and from list pass down the callback to item so now every item has a prop with that callback function than we can pass the item as an argument in that call back function
App : passes downporps(items -> array  [items,...] , onitemselect -> function () => {} to list
list passes porps(items -> array  [items,...] , onitemselect -> function () => {} to item
item has props porps(items -> item:  item , onitemselect -> function (item) => {} 
onitemselect is a function takes an argument item wil invoke a method on the app component with the item object so than app can update its state and say the selected item is the item that the user selected 
this will take a deeply nested callback in order to be achieved

***React-Router***

*without react router *

we can use window.location.pathname === '/whatever pathname we want(login...orwhateveru wanna name it)'  
for example 
const showDropdown = () => {
if(window.location.pathname === '/dropdown') {
return <Dropdown/> 
}} 
now make sure you have a Dropdown component and u have it exported 
and export your main file say we can name it navigation or whatever you want 

*with react-outer-dom*

first of all we need to npm install or yarn add the library because it is independent from react and react dom

usually sits in app dont forget the import statement 
```
import {
    Router,
    Switch,
    Route
  } from "react-router-dom";
 return (
        <div>
        <Router history={history}>
        <Header />
            <Switch>
            <Route path="/" exact component={yourcomponentnamehere}>
         ...
            </Switch>
           
        </Router>
     
        </div>
      );
    }
```

if you have a nav bar and u wanna add links use <Link/> component from react-router-dom because it prevent page refresher instead of an anchor tag `<a>` that has an html default behavior 

```
import { Link } from 'react-router-dom'
than `<Link to="/login" >  <Login/></link>`
```

# ***What is Redux***

***redux cycle ***

action creator-> action->dispatch->reducers->state

redux is a state managment library, react-redux gets react and redux to work together
redux was not designed to only work with react, so we can use it with other frameworks
react-redux gives us access to provider and connect connect communicates with the provider 
the provider is rendered at the top of the hiarchy the store passes information to the provider which intern passes the info to app than from app to the children 
the children are wrapped with the connect component so it can communicate changes with the provider through something called the context system that we have not learned about in this module 

 store -> provider -> app ->connect->somelist
 action creators -> connect <--> provider 
 
*behind the scenes*

so the flow is as follows create the provider pass it a refrence to our redux store than anytime that we have a component that needs to interact with the redux store we can wrap it with the connect component, we configure the coinnect function by telling it what peices of state we want out of our store and what different action creators we  want to have wired up 
the connect function does all the magic and makes sure that all the data shows up in our component as props

action creators always need a type but the payload can be optional, the common way to make a react redux app is to make a actions folder reducer folder and use import export where its needed.
if we have multiple reducers we need the combinedreducer component from redux in order to combine all the reducers 
export default combineReducers({
//we can name the key whatever we want
key: reducer
})

***wiring up the provider***

inside of the index.js where we import the App component our goal is to get provider in the top of the hiarchy and pass it in the store so we have to import provider from react-redux and createStore from redux 
also we need to import reducers that are combined 
so we wrap the app with provider like below also make sure we create the store

```

store= createStore(reducers)
ReactDOM.render(
    <Provider store={store}>
<App />
</Provider>,
document.querySelector('#root')
)
```

so now the connect function can communicate with all the data in our redux store 

the connect function
so for the connect function to communicate it needs some sort of configuration in order to get data from the store. first we have to import connect from react-redux 
connect has strange syntax looks somethng like this 
export default connect()(somecomponent)
essentially the connect function is written like this:

```
function connect() {
return function() {
return "hello";
 	}
}
connect()()
```

configuring connect to map state to props & my thoughts on map dispatch to props
The connect() function connects a React component to a Redux store.

It provides its connected component with the pieces of the data it needs from the store, and the functions it can use to dispatch actions to the store.

It does not modify the component class passed to it; instead, it returns a new, connected component class that wraps the component you passed in.
connect accepts four different parameters, all optional. By convention, they are called:

mapStateToProps?: Function
mapDispatchToProps?: Function | Object
mergeProps?: Function
options?: Object

```
const mapStateToProps = (state) => {
console.log(state)
return {key: state.values}
```

// so the object that we return from this function will show up as props inside of our component
//so this.props === {key: state.values} this is what is happening here 
}
export default connect(mapStateToProps)(...)
we can pass our action creator into the connect function 
export default connect(mapStateToProps, { actioncreator, actioncreator...} (...) 
this is basically saying that connect will call the dispatch function on those action creators essentially we can put those in a mapDispatchToProps function like so

```
const mDTP = (dispatch) => { // give access to dispatch() frm store
  return {
    getToysWithDispatch: () => dispatch(getToys())
  }
}
export default connect(mSTP, mDTP)(App)
```


//THE SECOND ARGUMENT IS CONVENTIONALLY NAMED MAPDISPATCHTOPROPS, BUT IF YOU DON'T PROVIDE IT, CONNECT WILL AUTOMATICALLY PUT THE DISPATCH FUNCTION IN PROPS FOR YOU TO USE
//MAPDISPATCH TO PROPS IS A FUNCTION THAT WILL RECEIVE DISPATCH AS AN ARGUMENT, AND IT WILL ALLOW US TO COMPOSE AN OBJECT THAT WILL BE MAPPED TO PROPS WITH OUR ACTION CREATORS WRAPPED IN A DISPATCH INVOCATION

Maybe mapDispatchToProps isn’t a good name at this point. Maybe you’d rather call it “actionCreators”, or skip naming it entirely and just pass an object literal into connect. Either way, this is what it looks like:

```
import React from "react";
import { connect } from "react-redux";
import { increment, decrement, reset } from "./actions";

function Counter({ count, increment, decrement, reset }) {
  return (
    <div>
      <button onClick={decrement}>-</button>
      <span>{count}</span>
      <button onClick={increment}>+</button>
      <button onClick={reset}>reset</button>
    </div>
  );
}

const mapStateToProps = state => ({
  count: state.count
});
```

// Look how simple this is now!
const mapDispatchToProps = {
  decrement,
  increment,
  reset
};

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter);

Customize The mapDispatchToProps Object
If your actions do need arguments, then you can just wrap those specific action creators in a function, like so:

const mapDispatchToProps = {
  decrement,
  increment: () => increment(42),
  reset
};
And, of course, you can always fall back to passing a function as mapDispatchToProps. But for the rest of the time, save yourself some typing and use the object form 👍

my googleauthentication and some rails stuff...
really the hard part building my app was getting auth to work i did use a guide for that, anyhow the part that was hard is that there was a bug that my user was being created over and over and i tried to solve that from the front end but ended up figuring out i need to manipulate rails in order to find that user if he was already created, the bread and butter of the auth system i built was a one click user authentication from google where i save that users information in my rails backend and generate an id for them of course thats a rails automagic thing so all i needed to do in order to prevent my users from being created over and over again is use the find or create by method in rails so than if they already exist by their email because thats basically another unique parameter than i can just find them instead of creating a new record here is a snippet of the code if anyone is having the same sort of issue i was having 

 ```
def create
      @user = User.find_or_create_by(user_params)
      if @user.valid?
          render json: @user
      else
          render json: { errors: @user.errors.full_messages }, status: :unprocessable_entity
      end
  end
```

finding by the params basically works here is the snippet of my user params

 ```
def user_params
      params.require(:user).permit(:name, :email)
  end
```

this was a fun huge blog review i hope if anyone read this or come accross it, helps them understanding react and redux i know i personally had a hard time wrapping my head around thigns at first but with practice things got a lot easier 
cheers!!!




