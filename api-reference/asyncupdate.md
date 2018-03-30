## asyncUpdate

#### `asyncUpdate(field: string, asyncOp: function [, fetchMethod: string]) => thunk`

Like an `update`, but asynchronous. See [`update`](/api-reference/update.md) for details.

The async operation can be any valid async function---a call to fetch, or axios, or flavor-of-the-month. If you're using fetch--because getting data out of fetch is a two-step process, with options---you need to provide what type of data you are fetching.

`asyncUpdate` does the following:

1. Dispatches an `isLoading` action, which uses update to dynamically add a loading property to your state associated with the field to update. This property has the form: {field\_name}\_LOADING. It is a Boolean property set to the appropriate loading state.
2. Dispatches a `hasError` action, which uses update to dynamicaly add an error property to your state associated with the field to update. This property has the form: {field\_name}\_ERROR. It is a Boolean property set to the appropriate error state.
3. If the data loads from the resource, then the loading state is updated to false, and anupdate is dispatched with the returned value.
4. If there is an error, then the error state is updated with the error returned.

#### Arguments

1. `field: string`: The field to update. Reduxigen supports uses lodash/set under the hood, so it supports any valid lodash setter path string.
2. `asyncOp: async function`: Any kind of async function is supported \(fetch, axios, etc\).
3. `fetchMethod: string:[Optional]`. If you're using fetch, supply the response data method you want to use. For example: json, blob, etc.

#### Returns

`redux-thunk`: A Redux thunk.

#### Examples

```js
// state
export default {
  cars: []
}


// actions
import { asnycUpdate } from "reduxigen/actions";
import axios from "axios";

export const displayCars = asyncUpdate("cars",  query => axios.get("http://cars.com/cars", query));


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

#### Example using `fetch`

```js
import { asnycUpdate } from "reduxigen/actions";

export const displayCars = asyncUpdate("cars",  query => fetch('http://cars.com/' + query), 'json');
```



