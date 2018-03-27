Getting up and running with Reduxigen is easy.

### Configure

1. Create a default state:

```js
export default {  
  test:""
}
```

1. Create a store:

```js
// file name: store.js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk"; // If you're using thunks
import { rootReducer } from "reduxigen/actions";
import DEFAULT from "state";

export default createStore(rootReducer(DEFAULT), applyMiddleware(thunk));
```

### Create Actions

```js
import { update } from 'reduxigen/actions';

// Note that the value "test" corresponds to the "test" field in the state object.
export const setTest = update("test");
```

### Connect actions to your component

Import this action into your component, and connect it using Reduxigen's `connect ` method.

```js
import React from 'react';
import * as actions from './test-actions';
import connect from "reduxigen/connect";

export const Test = ({test, setTest}) => <button onClick={setTest}>{test}</button>;

export default connect(['test'], actions)(Test);
```

Reduxigen's `connect` will map the array of prop names you pass it to `mapStateToProps`. It will automatically map the actions you pass it to `mapDispatchToProps`.

## 



