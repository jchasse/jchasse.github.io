---
layout: post
title:      "Ready, setState, Go!"
date:       2020-12-02 04:40:43 +0000
permalink:  ready_setstate_go
---


One of the things I had trouble grasping in this last module was the idea of using state with the react library. State is data. This data can be changed and updated inside of a function component or class component. State is differing from props because it can be changed. Props requires a parent to send information to it. State allows us to track and manage these changes to data.

**Initial State:**
Initial state is set by a constructor when the method runs first. *The constructor is only run one time so the initial state is controlled by the constructor* An example of setting the initial state is below:
```
    state = {
        edit: false
    }
```
And can also be written as:
```
    constructor({id, title, city, state, content, price, cashflow, link, votes}) {
        super()
        this.state = {
            id: (id ? id : ' '),
            title: (title ? title : ' '),
            city: (city ? city : ' '),
            state: (state ? state : ' '),
            content: (content ? content : ' '),
            votes: (votes ? votes : ' ')
        }
    }
```
The difference here is the the constructor term and inherited super() is understood when setting state, allowing us to code it shorthand with the latest updates to the React library.

**setState:**
Now that the initial state is set, the only way to change those values is to utilize the asynchronous setState function. The purpose of this method is very clear, update the state! 

An example to show state getting updated is below:

```
    editOrSubmit = () => {
        this.setState(prevState => ({edit: !prevState.edit}))
    }
```
Or another example:

```
    handleOnChange(event) {
        this.setState({
            [event.target.name]: event.target.value
        })
    }
```

IMPORTANT NOTE - setState is asynchronous, so in the case of below, if you check the value of your state in this debugger, you will see that the state *has not changed.* This is becuase the setState method runs asynchronous and updates the state at the appropriate time in the queue.

```
    handleOnSubmit(event) {
        event.preventDefault()
        this.setState({
            id: '',
            title: '',
            city: '',
            state: '',
            content: '',
            votes: 0
        })
				debugger
    }
```

**Why would I want to use state?**
State can be a powerful tool to manage data and the changes of data. It should be limited to appropriate situations. Ways that I utilized state are below:
* keep track of a boolean status to control flow when button are clicked
* keep track of form inputs when a user enters characters
* keep track of multiple form inputs before submission or returning to parent methods
* set initial values to be sent to the redux store if the user did not alter/update them

Events are often the trigger point to update state, as you have seen in the above snippets including the functions handleOnChange and handleOnSubmit

Additional ways state can be useful:
* tracking timestamps
* managing style settings
* storing arrays

Good luck coding!
