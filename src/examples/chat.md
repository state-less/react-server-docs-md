# This is a simple realtime example

You can quickly spin up a basic chat with react server. The example doesn't do much more than posting messages to a single room, but you can use this as a starting point in case you want a sophisticated chat experience.

In order to keep track of the active users we need to send an unmount event to the server when the user closes a tab. 

This makes sure he will be removed from the user list. In order to send unmount events to the server you need to pass the `preventUnload` property in your `useComponent` options.
