# Connect {#connect}

Reduxigen’s `connect` simplifies mapping state and dispatch to props. It does this by creating the `mapStateToProps` and `mapDispatchToProps` functions for you, then calling `react-redux`'s `connect` method. Below is a simplified example:

```js
import * as actions from "./booking-actions";
import connect from "reduxigen/connect";

// Your component here.

// The stateMap array contains a list of property names.
const stateMap = ["pickup", "dropoff", "details.pickupDate", "details.time", "cars", "cars_loading"];
export default connect(stateMap, actions)(Home);
```

`connect` supports nested properties. There are two ways to work with them. By default, `connect` will map nested propeties to nested properties. For example:

```js
const store = {
  test: {
    nested: {
      val: expected
    }
  }
};

const sampleComponent = props => (
  <h1>{props.test.nested.val}</h1>
);

export default connect(["test.nested.val"])(sampleComponent);
```

`connect` uses [`lodash/get`](https://lodash.com/docs/4.17.5#get) under the hood, so it supports any valid `lodash` getter path string.

Alternatively, you can create aliases for nested properties, using ‘as’ syntax:

```js
const store = {
  test: {
    nested: {
      val: "myVal"
    }
  }
};

const sampleComponent = props => (
  <h1>{props.val}</h1>
);

export default connect(["test.nested.val as val"])(sampleComponent);
```

Finally, it also supports functions. Passing functions can be useful when you want to display computed properties.

`connect` injects the `state` into any function passed to it. It expects, therefore, that any function it works with will have one argument, `state`, in its method signature \(e.g., `function myMethod(state) { }`\). The following example demonstrates using a `reselect` selector:

**The Selector**

```js
import { createSelector } from 'reselect'
​
const getVisibilityFilter = state => state.visibilityFilter
const getTodos = state => state.todos
​
export const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

**The Connector**

To pass a function to `connect` add an object with one field to the array. The field name on the object will map to the field name in `props`.

```js
import { getVisibleTodos } from "../selectors";

const sampleComponent = ({todos}) => (
  <ul>
    {todos.map(todo => <li key={todo.id}>{todo.name}</li>}
  </ul>
);

export default connect([{todos: getVisibleTodos}])(sampleComponent);
```

#### Automap _ \[Experimental\]_

NOTE: _This functionality is currently under active development, and is not yet exposed_.

There is an experimental usage of `connect` that automatically maps properties to a component. Any property used in the component will be automatically mapped. Currently, destructured props are not supported. This includes destructured props in method signatures and function bodies. See the examples below for details.

Also, note that the `connect` method is overloaded. It can be called with none, one, or two parameters, as follows:

| Invocation | Notes |
| :--- | :--- |
| connect\(\) | Called when there are only properties, and no actions. Will automatically map properties to the component. |
| connect\(stateMap: Array&lt;string&gt;\) | When an array is passed in as the first argument, the function assumes that the array is a collection of properties to map. |
| connect\(actions: Object\) | When an object is passed in as the first argument, the function assumes that it should auto-map properties, and that the argument is a hash of actions. |
| connect\(stateMap: Array&lt;string&gt;, actions\) | The standard method of calling \`connect\`, the first argument is an array of properties to map. The second argument is a hash of actions. |

A few examples are below:

**Stateless Functional**;

```js
const actions = { actionOne: () => {} };
const sampleComponent = props => <h1 className="test">{props.test}</h1>;
export default connect(actions)(sampleComponent);
```

**Class Based**:

```js
class SampleComponent extends Component {
  render() {
    return <h1 className="test">{this.props.test}</h1>;
  }
}
export default connect()(sampleComponent);
```

_**Not Supported**_:

As noted above, destructured props are** not **officially supported.

```js
// Destructured props in the method signature will not work.
const sampleComponent = ({test}) => <h1 className="test">{test}</h1>;
export default connect()(sampleComponent);
```

```js
class SampleComponent extends Component {
    render() {
      // Destructured props in a method body cannot be guaranteed to reliably work.
      const ({test}) = this.props;

      return (
      <div>
        <h1 className="test">{test}</h1>
      </div>);
  }
}
export default connect()(sampleComponent);
```

#### Ignoring Properties

By default, all props referenced in a component are mapped. There will be times when you don't want automap to map a prop. For example, `reacti18n-next`, uses an HOC to inject its `t` function as a prop. If you refer to `t` in your function body using standard props syntax, automap will overwrite it. To avoid this, use bracket syntax to refer to props you don't want automap to map for you.

For example:

```js
import React, { Component } from "react";
import connect from "reduxigen/connect";
import { translate } from "react-i18next";

class SampleComponent extends Component {
  render() {
    // Assign the prop to a variable using bracket syntax. This will not be automapped.
    const t = props["t"];

    return (
    <div>
      <h1 className="test">{this.props.test}</h1>
      <p>{t("myKey")}</p>
    </div>);
  }
}
export default translate()(connect()(sampleComponent));
```



