# This is a simple realtime example

You can swiftly set up a basic chat functionality using React Server. While the provided example primarily focuses on posting messages to a single room, it serves as a solid foundation for building a more advanced chat system.

To maintain an updated list of active users, it's crucial to send an 'unmount' event to the server when a user closes a tab. This ensures their removal from the user list. To achieve this, simply include the `preventUnload` property in your `useComponent` options.
