# React.learn

A React study note.

## Comunication Between Components

Example: ToDo List APP

React component and comunication between components

### Data Flows Down
Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class. This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it. A component may choose to pass its state down as props to its child components:

```javascript
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

This also works for user-defined components:
```javascript
<FormattedDate date={this.state.date} />
```

The FormattedDate component would receive the date in its props and wouldn’t know whether it came from the Clock’s state, from the Clock’s props, or was typed by hand:

```javascript
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree. 

If you imagine a component tree as a waterfall of props, each component’s state is like an additional water source that joins it at an arbitrary point but also flows down.

### Lift State Up
Once the state is changed by setState, component and it’s children’s virtual DOM will be re-rendered.

Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their lowest common ancestor. 

There should be a single “source of truth” for any data that changes in a React application. Usually, the state is first added to the component that needs it for rendering. Then, if other components also need it, you can lift it up to their lowest common ancestor. Instead of trying to sync the state between different components, you should rely on the top-down data flow.

### Callback Climbs Up
When we want to pass the data generated from child component to parent component for a state change. We can pass binded function down as props. This means that child components can trigger state changes in their parent components.

![structure](https://github.com/shuaizhang621/React.learn/raw/master/TodoList/WechatIMG51.jpeg)

## Component Life Cycle

When we build or update a component, it follows below procedure:

![lifeCycle](https://pbs.twimg.com/media/CbSfWgQW8AQSDJF.png:large)

**Mounting**

These methods are called when an instance of a component is being created and inserted into the DOM:
+ constructor()
+ componentWillMount()
+ render()
+ componentDidMount()

**Updating**

An update can be caused by changes to props or state. These methods are called when a component is being re-rendered:
+ componentWillReceiveProps()
+ shouldComponentUpdate()
+ componentWillUpdate()
+ render()
+ componentDidUpdate()

**Unmounting**

This method is called when a component is being removed from the DOM:
+ componentWillUnmount()

https://reactjs.org/docs/react-component.html

## Mili ..

### Promises

Old Calbacks
```JavaScript
function successCallback(result) {
  console.log("It succeeded with " + result);
}
function failureCallback(error) {
  console.log("It failed with " + error);
}
doSomething(successCallback, failureCallback);
```

Use promises to handle callback
```JavaScript
const promise = doSomething();
promise.then(successCallback, failureCallback);
// Or simply
doSomething().then(successCallback, failureCallback);
```

```JavaScript
function timeout(duration = 0) {
 return new Promise((resolve, reject) => {
   setTimeout(() => resolve(1), duration);
 })
}

// Then
timeout(1000).then((v) => {
 console.log('resolved with: ' + v);
}, (v) => {
 console.log('rejected with: ' + v);
})
// resolved with: 1

function timeout(duration = 0) {
 return new Promise((resolve, reject) => {
   setTimeout(() => reject(1), duration);
 })
}

// Then
timeout(1000).then((v) => {
 console.log('resolved with: ' + v);
}, (v) => {
 console.log('rejected with: ' + v);
})
// rejected with: 0

// Catch
timeout(1000).then((v) => {
 console.log('resolved with: ' + v);
 throw new Error("hmm");
}, (v) => {
 console.log('rejected with: ' + v);
}).catch((err) => {
 console.log(err);
})
// resolved with: 1     if resolved
// hmm


// Chain

function timeout(duration = 0) {
 return new Promise((resolve, reject) => {
   setTimeout(() => reject(2), duration);
   //setTimeout(() => resolved(2), duration);
 })
}

timeout(1000).then((v) => {
 console.log('resolved with: ' + v);
 return timeout(2000);
}, (v) => {
 console.log('rejected with: ' + v);
 return timeout(5000);
}).then((v) => {
 console.log('resolved with: ' + v);
 throw new Error("hmm");
}, (v) => {
 console.log('rejected with: ' + v);
}).catch((err) => {
 console.log(err);
})
// if resolved
// 2 sec later...
// resolved with 1
// 5 sec later...
// resolved with 1
//hmm

// if rejected
// 2 sec later...
// rejected with 2
// 5 sec later...
// rejected with 2
```
### Arrow functions in ES6+
```Javascript
// Manually bind, wherever you need to
class PostInfo extends React.Component {
  constructor(props) {
    super(props);
    // Manually bind this method to the component instance...
    this.handleOptionsButtonClick = this.handleOptionsButtonClick.bind(this);
  }
  handleOptionsButtonClick(e) {
    // ...to ensure that 'this' refers to the component instance here.
    this.setState({showOptionsModal: true});
  }
}
```
In ES6, you only need to
```JavaScript
class PostInfo extends React.Component {
  handleOptionsButtonClick = (e) => {
    this.setState({showOptionsModal: true});
  }
}
```
https://babeljs.io/blog/2015/06/07/react-on-es6-plus
