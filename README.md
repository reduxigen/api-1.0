# ![](/assets/reduxigen-logo.png) Reduxigen

Reduxigen makes working with React and Redux ridiculously simple:

*  No action creators. 
*  No reducers. 
*  No `mapStateToProps`. 
*  No `mapDispatchToProps`. 

What is there? 

* functions that update your state, and 
* a powerful, simple `connect` method to bind React to Redux. 



Reduxigen is a set of utilities: `actions` and `connect`,  designed to make working with Redux simpler.

#### Actions

Reduxigen `actions` simplify the process of updating Redux state. They eliminate the need to write all the boilerplate of reducers and action-creators.

#### Connect

Reduxigen `connect` simplifies connecting state and methods to props when using `react-redux`.

#### Use what you need

Each utility is its own file \(`reduxigen/actions` and `reduxigen/connect`\). You can load only the files you need. `actions` contains Reduxigen's `central-reducer` and all `action` methods. `connect` contains the simplified `react-reduxconnect` method.

