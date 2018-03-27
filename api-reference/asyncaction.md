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
2. `func` is a function that will be run on the value when the reducer is called.
3. `asyncOp: async function:` Any kind of async function is supported \(fetch, axios, etc\).
4. `fetchMethod: string:`Optional. If you're using `fetch`, supply the response data method you want to use. For example: 
   `json`, `blob`, etc.

#### Returns

`thunk`: A redux thunk.

#### Example

    import { asnycAction } from "reduxigen/actions";
    import axios from "axios";

    export const updateDropoff = asyncAction(
      "dropoff",
      value => `${value}--hooptie!`,
      event => {
        const resourceId = event.target.value;
        return axios.get(`/dropoffs/${resourceId}`);
      }})
    );



