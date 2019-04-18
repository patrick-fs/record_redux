# Record Redux Demo
Using FullStory [custom events](https://help.fullstory.com/develop-js/363565-fs-event-api-sending-custom-event-data-into-fullstory) to record Redux state with middleware.

This example is borrowed from https://github.com/reduxjs/redux/tree/master/examples/counter-vanilla.

More information about Redux middleware: https://redux.js.org/advanced/middleware#the-final-approach.

## What this is demonstrating
This is what the site looks like (`index.html`):
![image](https://user-images.githubusercontent.com/45576380/56386678-771dd580-61f0-11e9-8695-0829c726a58d.png)
Clicking the "BOOM!" button will invoke the `boom()` function via the Redux reducer:
```JavaScript
// Redux reducer
function counter(state, action) {
  if (typeof state === 'undefined') {
    return 0;
  }

  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    case 'BOOM':
      boom();
    default:
      return state;
  }
}
```

```JavaScript
// example error
function boom() {
  var thisIsaReferenceError = e;
}
```
This causes a reference error to be thrown (since `e` is not defined anywhere).

A middleware function called `crashReporter()` is defined to catch errors thrown from reducers:
```JavaScript
// middleware for capturing errors from https://redux.js.org/advanced/middleware#the-final-approach
const crashReporter = store => next => action => {
  try {
    return next(action);
  } catch (err) {
    // FullStory custom event
    // https://help.fullstory.com/develop-js/363565-fs-event-api-sending-custom-event-data-into-fullstory
    FS.event('Redux error', {
      error: {
        name: err.name,
        message: err.message,
        fileName: err.fileName,
        lineNumber: err.lineNumber,
        stack: err.stack,
      },
      counter: store.getState(), //NOTE: this could be a state object, strip out any sensitive fields first
    });
    throw err;
  }
};
```

```JavaScript
// apply middleware when creating the Redux store
const store = Redux.createStore(counter, Redux.applyMiddleware(crashReporter));
```

### Searching in FullStory

Once Redux error events are flowing into FullStory, they can be used to search for sessions:
![image](https://user-images.githubusercontent.com/45576380/56391161-d7664480-61fb-11e9-8250-c535a9c8bfb3.png)
And viewed in replay:
![image](https://user-images.githubusercontent.com/45576380/56391201-f8c73080-61fb-11e9-9950-142845827d30.png)



## How to run this demo on your local environment
```
npm install
npm run serve
```
Please make sure that you replace `window['_fs_org'] = 'YOUR ORG ID HERE';` in the FullStory snippet with your actual Org Id. You can find your Org Id in the sample snippet provided on the FullStory settings page: https://help.fullstory.com/using/recording-snippet.

