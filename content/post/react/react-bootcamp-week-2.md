---
title: "React Bootcamp | Week 2 | Handlers & State"
date: 2020-03-24T23:44:12-06:00
draft: true
categories:
- development
- react
tags:
- react
- bootcamp
- javascript
---

# React Bootcamp with Tyler McGinnis | Weeek 2 | Handlers & State

Q: How to get a function down to the element?
A: You can pass it as a `prop`

Q: How do we hook up the event handler?
A: Add a button and for each button we can set the `onClick` to a reference of the function that we want to call. On the button element we need to pass the `onClick` an arrow function. `() => `. Wrapping it in the function makes it so that the function will not run the first time that we load the page and set up the element list. If the function you plan to call doesn't need to pass any arguments, then you do not need the arrow function because then you will only be passing the reference to the function.

## Let's talk about `this` in Javascript
> In order to know what `this` is referencing, you need to know where the function is being invoked. 

What is to the left of the `.` (dot) when the function is invoked? That is what `this` is referencing.

## `.bind()` What does it do?
`.bind()` is used to set the reference of `this` to the app component that you declare when calling bind and passing it `this`. You're telling it to bind the reference to the refenrence of `this` that you pass so that it always works. This is especially helpful when setting state.

## Adding New Friends to our list
`friends: currentState.Friends.concat([this.state.input])`
Q: Why use `concat` instead of `push`
A: We use concat and not push so that we don't modify the array, instead we create a new array. We don't want to modify the state directly.

## Challenges
Starting with the code from this [gist](https://gist.github.com/tylermcginnis/079338a043bffbc8431a077017af750a), do the following:
1. Don't allow submissions with empty input
2. Add a Clear All button
3. Separate friends into 2 lists, active and inactive.

### Don't allow submissions with empty input

### Add a Clear All button
Taking the state and reset the array of objects to a new empty array

### Separate friends into 2 lists, active and inactive.
Doing it with an array of objects makes more sense to me than multiple arrays as well.