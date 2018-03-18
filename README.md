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

@Credit to Richard
