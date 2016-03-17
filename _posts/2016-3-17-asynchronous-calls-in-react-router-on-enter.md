---
layout: post
title: Asynchronous calls in React Router onEnter
---

Recently in an React/Redux app I am working, needed to do an authentication check when entering a route state. JWT tokens are being used for authentication, but I needed to do a network call to check the validity of the token just instead of checking the presence for it in local storage. 

So how do we wait till the asynchronous network call is done and then only route to the state ? This was a problem in older versions of react-router and solutions like an intermediary shim could be used. Thankfully in a recent version of react-router, one of the parameters passed to onEnter is a callback function.

To use this function with an async call just add it to the done function of the async call or fail if that is the case. This will let react-router know that it can now transition to the state. Pretty nifty thing !
