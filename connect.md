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

#### _Experimental_

NOTE: _This functionality is currently under active development, and is not yet exposed_.

There is an experimental usage of `connect` that automatically maps properties to a component. Any property used in the component will be automatically mapped. While the API for this functionality will not change, its implementation is still under evaluation. This functionality should work with any commonly-used method for creating React components. This said, the current implementation relies on unreliable function internals, and may never actually be included in Reduxigen, unless a more reliable means for accomplishing this end can be found.

Note that, because of the approach used to map properties, certain property names are reserved to prevent their being overridden. These property names map to common reserved property names---such as `t,`which is a commonly used `i18n` property name. Currently, `t` is the only reserved property name.

A few examples are below:

**Stateless Functional**;

```js
const actions = { actionOne: () => {} };
const sampleComponent = ({ test }) => <h1 className="test">{test}</h1>;
const Sut = connect(actions)(sampleComponent);
```

**Class Based**:

```js
class sampleComponent extends Component {
  render() {
    return <h1 className="test">{this.props.test}</h1>;
  }
}
const expected = "test";
const Sut = connect()(sampleComponent);
```



