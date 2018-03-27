## genericAction

### `genericAction(name: string, func: function) => (field: string) => redux-actiopn`

Use a `genericAction` when you want to create an action that can use with multiple fields in your state. For example, if you want to write a custom increment, and use it as the update action for more than one part of your state.

#### Arguments

_base function_

1. `name`
    is used to create the reducer's method name. The method name is created by combining 
   `name`
    with the 
   `field`
   , e.g., 
   `{field}_{name}`
2. `func`
    a function that will be applied to the value passed to the reducer.

_returned function_

1. `field: string:`
    The field to update. Reduxigen supports uses 
   `lodash/set`
    under the hood, so it supports any valid lodash setter path string.

#### Returns

`redux-action`: A redux action of the form: `{ type, payload }`.

#### Example

```
//
 state
```



