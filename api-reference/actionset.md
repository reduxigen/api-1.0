## actionSet

#### `actionSet(name: string, func: function) => function`

Use an `actionSet` when you need to update several fields in one action. For example, you may want to authenticate a user, redirect them to their dashboard, and present them with a messages.

#### Arguments

1. `name: string:` The name of the action.
2. `func` a function that will be applied to the value passed to the reducer. In contrast to an `action`, which updates a specific field, an `actionSet` should always return an object. The values in this objec should map to the new values you want the state to have. Note that this function can have access to the previous state via injection into the passed in function. Simply pass in state as the second method parameter:  `(value, state) => {}`.

#### Returns

`redux-action`: A redux action function with a return value of the form: `{ type, payload }`.

#### Example

```js
// state
export default {
  pickup: ""
}


// actions
import { actionSet } from "reduxigen/actions";

export const clearAddress = actionSet("clear", ()) => {
  return {
    address: '',
    city: '',
    state: ''
  }
});


// example use
import React from "react";
import * as actions from "./actions";
import connect from "reduxigen/connect";

const Pickup = ({address, city, state, clearAddress}) =>
  <>
    <input
      name="address"
      onChange={setAddress}
      value={address}
    />
    <input
      name="city"
      onChange={setCity}
      value={city}
    />
     <input
      name="state"
      onChange={setState}
      value={state}
    />
    <button onClick={clearAddress}>Clear</button>
  </>

export default connect(["address", "city", "state"], actions)(Pickup);
```

**Example actionSet with state**:

```js
export const addContact = action("deposit", (value, state) => {
   const balance = state.balance + value;
   const availableBalance = balance - state.pending;
   return {
      balance,
      availableBalance,
      message: availableBalance < balance ? 'Your account will soon be overdrafted.' : ''
   }
});
```



