## genericAction

#### `genericAction(name: string, func: function) => (field: string) => redux-action`

Use a `genericAction` when you want to create an action that can use with multiple fields in your state. For example, if you want to write a custom increment, and use it as the update action for more than one part of your state.

#### Arguments

_base function_

1. `name` is used to create the reducer's method name. The method name is created by combining `name`
    with the `field`, e.g., `{field}_{name}`
2. `func` a function that will be applied to the value passed to the reducer. Note that this function can have access to the previous state via injection into the passed in function. Simply pass in state as the second method parameter:  `(value, state) => {}`.

_returned function_

1. `field: string:` The field to update. Reduxigen uses [`lodash/set`](https://lodash.com/docs/4.17.5#set) under the hood, so it supports any valid lodash setter path string.

#### Returns

`redux-action`: A redux action of the form: `{ type, payload }`.

#### Example

```js
// state
export default {
  passengers: "",
  luggage: ""
}


// actions
import { genericAction } from "reduxigen/actions";

const increment = genericAction("increment", value => value + 1);

export const incrementPassengers = increment("passengers");
export const incrementLuggage = increment("luggage");


// example useage
import React from "react";
import * as actions from "./actions";
import connect from "reduxigen/connect";

const Incrementors = ({incrementPassgeners, incrementLuggage}) =>
  <>
    <button onChange={incrementPassengers}>Add Passenger</button>
    <button onChange={incrementLuggage}>Add Luggage</button>
  </>

export default connect(null, actions)(Incrementors);
```



