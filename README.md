# Record Redux
Using FullStory [custom events](https://help.fullstory.com/develop-js/363565-fs-event-api-sending-custom-event-data-into-fullstory) to record Redux state with middleware.

This example is borrowed from https://github.com/reduxjs/redux/tree/master/examples/counter-vanilla.

More information about Redux middleware: https://redux.js.org/advanced/middleware#the-final-approach.

## How to run
```
npm install
npm run serve
```
Please make sure that you replace `window['_fs_org'] = 'YOUR ORG ID HERE';` in the FullStory snippet with your actual Org Id. You can find your Org Id in the sample snippet provided on the FullStory settings page: https://help.fullstory.com/using/recording-snippet.
