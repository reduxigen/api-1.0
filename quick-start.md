Getting up and running with Reduxigen is easy.

### Configure

1. Create your default state:

```js
// file name: state.js
export default {  
// example state
  test:""
}

```

1. Create your store:

```js
// file name: store.js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk"; // If you're using thunks
import { rootReducer } from "reduxigen/actions";
import DEFAULT from "state";

export default createStore(rootReducer(DEFAULT), applyMiddleware(thunk));
```

### Create Actions

Given the following component:

```js
// file name: test.jsx
import React from 'react';

const Test = ({test}) => <button>{test}</button>;

export default Test;

```

All you need to do to update the `redux` store from your component is create an action:

```js
// file name: test-actions.js
import { update } from 'reduxigen/actions';

// Note that the value "test" corresponds to the "test" field in the state object.
export const setTest = update("test");

```

### Connect actions to your component

Import this action into your component, and connect it to `redux`, using Reduxigen's `connect` method, which simplifies mapping props and dispatch to state.

```js
// file name: test.jsx

import React from 'react';
import * as actions from './test-actions';
import connect from "reduxigen/connect";

export const Test = ({test, setTest}) => <button onClick={setTest}>{test}</button>;

// method signature for bind -- bind(props, actions);
export default connect(['test'], actions)(Test);

```

Reduxigen's `connect` method will map the array of prop names you pass it to `mapStateToProps`. It will automatically map the actions you pass it to `mapDispatchToProps`.

## 



