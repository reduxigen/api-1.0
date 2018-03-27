# appendReducers

If you have an existing Redux application, and would like to work with Reduxigen, but don't want to convert all of your existing code--no problem! Reduxigen exposes an `appendReducers` function that achieves something simliar to Redux's `combineReducers`.  To integrate with Reduxigen, replace your call to `combineReducers` with a call to `appendReducers`. An example is below:

```js
// car-reducer.js
const carReducer = (state, action) => {
  switch (action.type) {
    case "ADD_CAR":
      return { ...state, car: action.payload };
    default:
      return state;
  }
};

// trip-reducer.js
const tripReducer = (state, action) => {
  switch (action.type) {
    case "ADD_DESTINATION":
      return { ...state, destination: action.payload };
    default:
      return state;
  }
};

// store.js
import { addReducers, rootReducer } from "reduxigen/actions";
import { createStore } from "redux";
import { carReducer} from "./car-reducers";
import { tripReducer } from "./trip-reducers";
import DEFAULT from "src/state/state";

addReducers([carReducer, tripReducer]);

export default createStore(rootReducer(DEFAULT));
```



