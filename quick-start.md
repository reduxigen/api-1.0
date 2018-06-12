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

Import this action into your component, and connect it using `react-redux`'s `connect` method.

```js
import React from 'react';
import * as actions from './test-actions';
import { connect } from "react-redux";

export const Test = ({test, setTest}) => <button onClick={setTest}>{test}</button>;

const mapStateToProps = state => ({test: state.test});

export default connect(mapStateToProps, actions)(Test);
```

## 



