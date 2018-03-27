# Reduxigen

Reduxigen is a thin wrapper around `redux`, `react-redux`, `redux-thunk`, and an application's `reducer`. When using Reduxigen, you will rarely need to write a `reducer`, or `thunk`.

Working with state using Reduxigen involves three concepts:

* **Store**: A standard Redux store.
* **Actions**: Reduxigen exposes several `action` types that update the store.
* **View**: Reduxigen exposes a `connect` method that simplifies connecting state and methods to props.

The workhorse of Reduxigen is the `action`. When you create an `action`, Reduxigen manages the reducer and action-creator for that `action`.

If you're not using React, but you are using Redux, you can still use Reduxigen. You can load only the files you need. There are two distribution files: `actions`, and `connect`. `actions` contains Reduxigen's `central-reducer` and all `action` methods. `connect` contains the simplified `react-reduxconnect` method.

