# Central Reducer

Reduxigen uses an object-based reducer. Using an object-based reducer instead of a `switch` based reducer allows reducers to be dynamically added. It has been noted that this type of reducer doesn't allow for case fallthrough \(same state update for multiple actions\). You can achieve the same end in Reduxigen by creating a `genericAction`, which can be used with multiple state updates---or, because `update`s are independent actions, just reuse an `update`.

This is what Reduxigen's object-based reducer looks like:

```js
const REDUX_INIT = "@@redux/INIT";

export const reducers = {
  [REDUX_INIT]: state => state
};

export rootReducer = defaultState => (state = defaultState, action) => {
  const { type, payload } = action;
  if (reducers.hasOwnProperty(type)) {
    return reducers[type](state, payload);
  } else {
    throw new Error("Reducer not found");
  }
};
```

Each reducer `type` is a property on the object. The `reducer` function simply calls the function associated with that property when it receives an action. If no action is found, it throws an error.

## Custom Reducers

While Reduxigen dynamically creates your reducers for you, if you encounter a situation that Reduxigen can't support, no problem. You can still implement what you need to do the long way, and wire it up to Reduxigen's reducer. Here's an example of manually creating a reducer:

```js
// file name: without-reduxigen.js
import { reducers } from 'reduxigen/actions';

reducers.MY_ACTION = (var1, var2) => { // my code }
```

Once you've added the action to the reducer, you can create your action-creator, and dispatch it just the way you normally would in `react-redux`.

