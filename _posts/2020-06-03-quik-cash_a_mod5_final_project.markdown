---
layout: post
title:      "Quik-Cash a Mod5 Final project"
date:       2020-06-03 07:38:29 +0000
permalink:  quik-cash_a_mod5_final_project
---


   Well this is it the mod5 final project.  I made it to the finish line.  This as with all the other projects was difficult to say the least.  I went into this project thinking "wait what, what is going on here".  Through the entire module I was dumbfounded by the way you code a react project, I could not see the flow, I did not quite get how all the pieces fit together, and on top of that the use of JSX was down right wierd as well.  Much of the logic of how the application works if very abstract, but once you see the patern it becomes just a tab less difficult and you may just come to realize the power of using React. Redux on the hand was a different story, I understand the concept of the global store, it's your front end source of truth, like the database on the back end is.  Incorporating redux into this project was a bit overkill but it was necessary to make the state available to all the components, and I get it.  I was stuck at 30 percent for 3 days with a way to login and out and sign-up, but that was it.

   After being stuck for days I began to feel that old imposter syndrome kick in big time.  Do I have this, do I have what it takes, have I been posing all this time?  After that subsided a bit, I was able to think of where, when and what I wanted to happen inside of my application.  Understanding the flow of your application is critical to MVP, how do you want a user to eventually use this application, what would make them comeback, what features add to MVP?  From there you can begin to branch out once you have MVP, for me it was getting my listings to render to the page, as this app is all about the listings, the ability to create a listing, the ability to buy a listing, and of course listing out the index of the listings themselves.  Once I was able to render my listings index I was over the first incline of the roller coaster, and we all know that the first incline it the tallest, it provides all the momentum for the rest of the ride.  Just like a roller coster the process of coding reactfully with redux has its ups and downs, it throws you for a loop every now and then.  If you can get yourself to calm down long enogh to think about the logic it becomes managable, that is writing the code in your components.
	 
	 Enough of the prolouge, lets get into some code, I mean that's what we all came for, is it not.  The patern for React is this: create a compenent, trigger an event, that event will most likely call a function that will dispatch an action creator that returns a function (if you used thunk, but of course you did it was a requirement) a fetch. The fetch then dispatches the action with the responce that was returned by the fetch itself.  Let see this in action shall we.
	 
```
export const loadListingsIndex = (listings) => dispatch => {
  const config = {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
      body: JSON.stringify(listings)
   };
    fetch(BASE_URL + '/listings', config)
    .then(r => r.json())
    .then(data => {
      dispatch(getListingsAction(data));
    });
};
```
   Here we are exporting loadListingsIndex as a named component not a default export, so were ever it is imported it must be wrapped in culry braces.  The loadListingsIndex function take an argument of listings the loadListingsIndex function will return a function to be dispatched the fetch which then dispatches the action that is the response.  The response is your payload that is in the action creator, below is an example of the action creator for the loadListingsIndex function.

```
export const getListingsAction = (listingIndex) => ({
  type: 'GET_LISTINGS',
  payload: listingIndex
});
```
   I am also exporting this function as a named export not a default.  the getListingsAction will take the response as an argument and pass it(the response) as the payload to the combined reducer of type 'GET_LISTINGS' the type has to be a constant per the documents for redux.  Actions are the only way to send data from the application to the redux store to update the state.  How the store is updated is via case statements for an example see the below code of the GET_LISTINGS case statement.

```
case "GET_LISTINGS":
      return {...state, listings: payload}; 
```

   This code is handling the update to state for the listingsIndex component, when a new listing is created and added to the the DOM, the component will re-render with the new state for that component.  This is the patern in React\Redux applications, rinse, repeat, recyle.
	 
	 This concludes this blog entry, and hopefully my time at Flatiron School (for software engineering at least, I may be back for the new cyber security course) while this is an end of sorts it is also the beginning, the beginning of a new chapter in my life.  A chapter where I can chose my destiny, or create it if you look at it in terms of code.  So with that being said, till we meet again, stay motivated and keep coding!
