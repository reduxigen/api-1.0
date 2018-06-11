## action

#### `action(field: string, func: function) => function`

Use an `action` when you need to do something more than a simple field update. For example, if you want to append a value to an array in your state, or write a custom increment.

#### Arguments

1. `field: string:` The field to update. Reduxigen supports uses `lodash/set` under the hood, so it supports any valid lodash setter path string.
2. `func` a function that will be applied to the value passed to the reducer. Note that this function can have access to the previous state via injection into the passed in function. Simply pass in state as the second method parameter:  `(value, state) => {}`.

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
import * as actions from "./actions";
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

**Example action with state**:

The following example demonstrates how to add an item to an Array.

```js
export const addContact = action("contacts", (contact, state) => [...state.contacts, contact]);
```



