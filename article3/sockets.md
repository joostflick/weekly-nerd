# WebSockets <!-- omit in toc -->

A brief introduction to WebSockets and how to use them.

## Table of contents <!-- omit in toc -->

- [What are WebSockets](#What-are-WebSockets)
  - [What does a WebSocket do](#What-does-a-WebSocket-do)
  - [What problem do WebSockets solve](#What-problem-do-WebSockets-solve)
  - [How do WebSockets work](#How-do-WebSockets-work)
- [Why would you use WebSockets](#Why-would-you-use-WebSockets)
- [Socket .io](#Socket-io)
  - [What is socket .io?](#What-is-socket-io)
  - [Example](#Example)
- [Sources](#Sources)

## What are WebSockets

### What does a WebSocket do

Simply put, WebSockets allow for realtime communication between the client (the browser in this case) and the server. You could look at it as an open telephone line over which both parties can communicate messages to eachother.

This technology allows for very reactive interfaces, as you no longer have to check whether something has been updated, but get notified by each update instead.

### What problem do WebSockets solve

An example for an application that can make great use of WebSockets would be a chat application. Before sockets, an app would have to check for new messages on a set interval, for example every 2 seconds. With sockets however, the app can just wait until it gets notified that a new message has arrived.

Or from a more technical perspective; The predecessor of sockets, HTTP long-polling, had to make an HTTP request, which the server kept open until new data was ready to be sent. This costs a lot more resources, both in terms of processing power as well as bandwith consumption.

### How do WebSockets work

Firstly, the clientside and the serverside have to 'meet' and 'agree' to a WebSocket connection. This is done via a normal HTTP request in which the client asks the server to initiate a WebSocket. This is called the _WebSocket handshake_.

Once the server accepts this handshake, the HTTP connection is then replaced with an continuously open WebSocket connection. From here on the client and the server can communicate in realtime with eachother without making any new requests.

## Why would you use WebSockets

So why would I want to use these WebSockets? Most of the reasons have already been stated above, but here is a quick overview of the benefits of utilizing sockets:

- Uninterrupted connection between the client's browser and the server, which means the two can exchange data at any given time and the client does not have to make multiple requests.
- Full 'duplex' connection. Information can be sent both ways at any given moment while the connection is open, as opposed to the one-way nature of HTTP requests.
- It is quicker. Since there aren't any new requests being made for each action, the standard HTTP protocol (including headers and cookies for example) isn't used as much. This means you are sending less data per interaction, which saves both time and bandwith.

## Socket .io

So far I have been using `socket.io` to manage WebSockets in my application. I found `socket.io` to be very userfriendly and easy to pick up, which is why I'd like to give it a small introduction in this article.

### What is socket .io?

`Socket.io` is a JavaScript framework, which enables you to use all the great functionalities of WebSockets, using JavaScript logic.

It utilizes a client- and a serverside API, so you'd use clientside javascript and communicate with your serverside javascript, and vice versa.

I think the best way to introduce `socket.io` would be to show a (very) brief example of the way it works.

### Example

One would start by installing all dependencies needed, and creating a basic application which has a front- and a backend. For this purpose [express](https://www.npmjs.com/package/express) could be used, since it is a lightweight web framework for node.

Once you have some basic server logic in your app and a homescreen HTML template that includes the `socket.io` script, you can start using `socket.io`.

First we will have to do the _socket handshake_ as discussed earlier. For `socket.io` this means the following:

_Server side_

```javascript
io.on('connection', function(socket) {
  console.log('if you see this a user is connected')
})
```

_Client side_

```javascript
var socket = io()
```

Once a user nagivates to our HTML page, our server will log the following message to the console:

`if you see this a user is connected`

Next I will show you how to send/receive messages from the client to the server, and from the server to the client.

_Client side_

```javascript
socket.emit('test', 'hello')
```

This will send the message `hello` as a `test` event.

Next we will tell the server what to do when receiving a `test` event:

_Server side_

```javascript
// this is the code we already had for the handshake
io.on('connection', function(socket) {
  // the following code listens for events on 'test'
  socket.on('test', function(message) {
    console.log('received on test: ' + message)
  })
})
```

The above code will print the message received on the `test` event to the console. This is also the place where you could instantly reply to the event by sending a message back.
_Server side_

```javascript
// this is the code we already had for the handshake
io.on('connection', function(socket) {
  // the following code listens for events on 'test'
  socket.on('test', function(message) {
    if (message === 'hello') {
      io.emit('test', 'right back at you')
    } else {
      console.log('received on test: ' + message)
    }
  })
})
```

What this will do is check whether the message is `hello`, and if it is send a reply back. If the message reads something else it still gets logged to the console.

Lastly, we can control what happens when the clientside receives the reply by the server:

_Client side_

```javascript
socket.on('test', function(msg) {
  console.log(msg)
})
```

As you can see this is almost identical to the way you receive messages on the serverside. With the above code, the client would log the reply from the server to the browsers console.

## Sources

- [What are WebSockets](https://medium.com/front-end-weekly/what-are-websockets-7bf0e2e1af2)
- [Socket.io get started tutorial](https://socket.io/get-started/chat/)
