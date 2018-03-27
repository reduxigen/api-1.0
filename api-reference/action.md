## action

#### `action(name: string, func: function) => function`

Use an `action` when you need to do something more than a simple field update. For example, if you want to write a custom increment.

#### Arguments

1. `field: string:` The field to update. Reduxigen supports uses `lodash/set` under the hood, so it supports any valid lodash setter path string.
2. `func` a function that will be applied to the value passed to the reducer.

#### Returns

`redux-action`: A redux action function with a return value of the form: `{ type, payload }`.

#### Example

```js
// state
export default {
  pickup: ""
}


// actions
import { action } from "reduxigen/actions";

export const setPickup = action("pickup", value => `${value}_woop!`);


// example use
import React from "react";
import actions from "./actions";
import connect from "reduxigen/connect";

const Pickup = ({pickup, setPickup}) =>
  <>
    <input
      name="pickup"
      onChange={setPickup}
      value={pickup}
    />
  </>

export default connect(["pickup"], actions)(Pickup);

```



