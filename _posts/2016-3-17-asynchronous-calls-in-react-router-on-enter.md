---
layout: post
title: Asynchronous calls in React Router onEnter
published: true
---


Recently in an React/Redux app I am working, needed to do an authentication check when entering a route state. [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token) tokens are being used for authentication, but I needed to do a network call to check the validity of the token instead of just checking the presence for it in local storage. I might write something like this in the route

	 <Route
      path='edit'
      component={EditComponent}
      onEnter={ _ => {
        checkToken()
      }}
    />

Where `checkToken` could be a async network call like

    const checkToken = _ =>
    makeXHR({type: TOKEN_REQUEST})
    .then(response) => {
      if (response === 'TOKEN_DOES_NOT_EXIST' ) {
        // No token, proceed to login screen
      }
      // else we are good to go
    })

So how do we wait till the asynchronous network call is done and then only route to the state ? This was a problem in older versions of react-router and solutions like an intermediary shim could be used. Thankfully in a recent version of react-router, one of the parameters passed to onEnter is a callback function.

To use this function with an async call just invoke it when you are done with the call. This will let react-router know that it can now transition to the state and it will wait till the `callback` function is called. Pretty nifty thing !

	 <Route
      path='edit'
      component={EditComponent}
      onEnter={ (nextState,replace,callback) => {
        checkToken(callback)
      }}
    />
    
   	const checkToken = (callback) =>
    makeXHR({type: TOKEN_REQUEST})
    .then(response) => {
      if (response === 'TOKEN_DOES_NOT_EXIST' ) {
        // No token, proceed to login screen
      }
      // else we are good to go and let react-router know
      callback()
    })
