---
title: "React_Application"
date: 2019-11-04T21:55:58-08:00
draft: false
---

- - -
# Introduction
## Import libraries
ES2015 Modules: import </br>
CommonJS Modules: require

## Component 
A component is a function or class that produces HTML to show the user and handles feedback from the user

# Building Content with JSX
* Babel: javascript complier([babejs.io](babeljs.io))
* JSX vs HTML: need to convert style to JSX format
* Class vs ClassName

# Communicating with Props
faker.js

semantic ui

Extracting JSX to New Components

import and export

React's Props System: System for passing data form a parent component to a child component. Goal is to customize or configure a child component

passing one prop vs. passing multiple props

component reuse: ApprovalCard. Only contents of ApprovalCard are different. A component can be children. Use props.children

# Structuring Apps with Class-Based Components
functional components: good for simple content. Cannot handle async function

class components: good for just about everything elese

1. must be a js class
2. must extend React.Component 
3. Must define a 'render' method that returns some amount of JSX

Get location api: 
```
window.navigator.geolocation.getCurrentPosition(
        (position) => console.log(position),
        (err) => console.log(err)
    );
```


# State in React Components
Rules of State
1. Only usable with class components
2. you will confuse props with state
3. 'state' is a JS object that contains data relevant to a component 
4. updating 'state' on a component causes the component to almost instantly rerender
5. state must be initialized when a component is created
6. state can only be updated using the function 'setState'






















