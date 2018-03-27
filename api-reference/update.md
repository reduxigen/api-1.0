## update

#### `update(field: string) => function`

1. Creates a reducer for updating a field, if one does not exist, using a naming scheme based on the field name.
2. Creates and returns a redux-action function.

The function returned by `update` accepts a DOM `event` as its default input. It will automatically grab the `target.value` from that `event`. However, you can pass in any value you want to the function. See the example below for more details.

#### Arguments

`field: string`: The field to update. This value will map to a field name on your state object. Reduxigen uses [lodash/set](https://lodash.com/docs/4.17.5#set) under the hood, so it supports any valid lodash setter path string.

#### Returns

`redux-action`: A valid redux action function with a return value of the form: `{ type, payload }`.

#### Example

```js
// state
export default {
  pickup: "",
  nested: {
    value: ""
  },
  number: 22
};

// actions
import { update } from "reduxigen/actions";

export const setPickup = update("pickup");
export const setNested = update("nested.value");
export const setNumber = update("number");


// example use
import React from "react";
import * as actions from "./actions";
import connect from "reduxigen/connect";

const Sample = ({setPickup, pickup, setNested, nested, number, setNumber}) =>
  <>
    <input
      name="pickup"
      onChange={setPickup}
      value={pickup}
    />
    <input
      name="nested-value"
      onChange={setNested}
      value={nested.value}
    />
    <span>{number}</span>
    <button onClick={setNumber(22)}>Reset number to 22</button>
  </>

export default connect(["pickup", "nested", "number"], actions)(Sample)
```



