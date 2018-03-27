# Connect

Wiring up props and actions to a react component is a pain. Reduxigen's `connect` method aims to simplify this by doing the work for you. It does this by creating the `mapStateToProps` and `mapDispatchToProps` functions for you, then calling `react-redux`'s `connect` method.

```js
import * as actions from "./booking-actions";
import connect from "reduxigen/connect";

// Your component here.

const stateMap = ["pickup", "dropoff", "pickupDate", "time", "cars", "cars_loading"];
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



