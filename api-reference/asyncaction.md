## asyncAction

#### `asyncAction(name: string, func: function, asyncOp: async function[, fetchMethod: string]) => thunk`

The `asyncOp` can be any async function, e.g., a call to `fetch`, or `axios`, or `flavor-of-the-month`. If you're using `fetch`, because getting data out of`fetch`is a two-step process, with options, you need to provide what type of data you are fetching.

`asyncUpdate` does the following:

1. Dispatches an `isLoading` action, which uses `update` to dynamically add a loading property to your state associated with the field to update. This property has the form: `{field_name}_LOADING`. It is a Boolean property set to the appropriate loading state.
2. Dispatches a `hasError` action, which uses `update` to dynamicaly add an error property to your state associated with the field to update. This property has the form: `{field_name}_ERROR`. It is a Boolean property set to the appropriate error state.
3. If the data loads from the resource, then the loading state is updated to false, and an`action` is dispatched with the returned value.
4. If there is an error, then the error state is updated with the error returned.

#### Arguments

1. `field: string:` The field to update. Reduxigen supports uses `lodash/set` under the hood, so it supports dot-delimited field identifier strings.
2. `func` a function that will be applied to the value passed to the reducer. Note that this function can have access to the previous state via injection into the passed in function. Simply pass in state as the second method parameter:  `(value, state) => {}`.
3. `asyncOp: async function:` Any kind of async function is supported \(fetch, axios, etc\).
4. `fetchMethod: string:`Optional. If you're using `fetch`, supply the response data method you want to use. For example: 
   `json`, `blob`, etc.

#### Returns

`thunk`: A redux thunk.

#### Example

```js
import { asnycAction } from "reduxigen/actions";
import axios from "axios";

export const fetchCars = asyncAction(
  "cars",
  value => `${value}--hooptie!`,
  event => {
    const resourceId = event.target.value;
    return axios.get(`/cars/${resourceId}`);
  }})
);


// example use
import React, { Component } from "react";
import * as actions from "./actions";
import connect from "reduxigen/connect";

class CarList extends Component {
  componentDidMount() {
    this.props.fetchCars(5);
  }

  render() {
    return (
      <div>
       { displayCars(this.props) }
      </div>
    )
  }
}

function displayCars({ cars, cars_loading, cars_error }) {
  if(cars_error) {
    return (<h1>An error has occurred</h1>);
  } else if(cars_loading) {
    return (<h1>Loading...</h1>);
  } else {
    return (
      <ul>
        { cars.map(car => <li key={car.id}>{car.name}</li> }
      </ul>
     );
  }
}

export default connect(["cars"], actions)(CarList);
```



