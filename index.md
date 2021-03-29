## Welcome To RaeedChat API 
RaeedChat API lets you connect to our servers through a automated bot to interact and entertain users. This allows for a bigger and automated community which can in turn create a better RaeedChat for everyone! We hope you join us in this mission! 

## Getting Started
Getting started is completely easy! With our gaia.db databases and token generations, your project will be easy to make. 
Project Requirements: 
 - Node.js

### Gateway connection
You will have been given a token by me for using the api. 

First you need to start a gateway connection, you can use the ws library to acheive this. Type `npm i ws, node-fetch` into your console. (node-fetch will be used later)

```js
const WebSocket = require("ws");
const ws = new WebSocket("wss://api.raeedchat.com/websocket/");
```
This will ensure you can receive messages from the chat 

### Initial authentication
To verify your bot and login you have to include your token. THIS IS REQUIRED.
```js
ws.on("open", () => {
  ws.send(
    JSON.stringify({
      e: "INIT",
      d: {
        token: "YOUR_TOKEN"
      }
    })
  );
});
```
This ensures proper connection to the server

### Reading messages
When reading a message you can do this: 
```js
ws.on("message", (json) => {
  const { e, d } = JSON.parse(json);
  if (e === "MESSAGE_CREATE") {
    if (d.message == "!help") {
      console.log("received")
    }
  }
});
````
Here the bot's prefix is "!" and when the user types "!help" it will log "received" to the console

### Sending messages
Now we need to send a response to the user, to achieve this we can use node-fetch. We have installed it in the gateway connection step. 
```js
ws.on("message", (json) => {
  const { e, d } = JSON.parse(json);
  if (e === "MESSAGE_CREATE") {
    if (d.message == "!help") {
      fetch(`https://api.raeedchat.com/api/v1/sendMessage`, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
          authorization: "Bearer YOUR_TOKEN"
        },
        body: JSON.stringify({
          message: "YOUR_MESSAGE"
        })
      })
        .then((res) => res.json())
        .then((data) => {
          if (data.error) {
            console.log(data.error.message);
          }
        });
    }
   });
``` 
If all goes well, when you type "!help" in RaeedChat you should get your message, make sure there isn't a bot already using "!" if there is use another prefix or change your command name. 
Here is the response the server sends when your message is sent successfully 
```json
{
 response: {
   code: 200,
   message: "Sent your message - Early Access Bot API"
 }
}
```
You can read this by changing
```js
  .then((data) => {
          if (data.error) {
            console.log(data.error.message);
          }
  });
```
to 
```js
  .then((data) => {
      if (data.error) {
          console.log(data.error.message);
      } 
        if(data.response) {
          console.log(data.response.message)
        }
   });
````
This is reading the object data.response.message from the json. 

### Sending replies
Now, you may want to send a reply instead, this is simple 
```js
ws.on("message", (json) => {
  const { e, d } = JSON.parse(json);
  if (e === "MESSAGE_CREATE") { //checks for MESSAGE_CREATE event
    if (d.message == "!help") {
      fetch(`https://api.raeedchat.com/api/v1/sendReply`/* Gateway */, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
          authorization: "Bearer YOUR_TOKEN"//authorization
        },
        body: JSON.stringify({
          title: "YOUR_EMBED_TITLE",//optional but recommended
          description: "YOUR_DESCRIPTION",//optional but recommended
          image: "YOUR_IMAGE"// optional not needed if you don't have a image
        })
      })
        .then((res) => res.json())
        .then((data) => {
          if (data.error) {
            console.log(data.error.message);
          }
        });
    }
   });
```

### WEBSOCKET EVENTS AND RESPONSES

MESSAGE_DELETE = Deleted message 

Returns:
```json 
{e:"MESSAGE_DELETE", d: "id"}
```

INIT_FAIL = initialization failed
Returns: 
```json
{e: "INIT_FAIL"}
```
BOT_EMBED = Bot embed
```json 
{e: "BOT_EMBED", d: {
  name: "NAME OF BOT",
  title: "EMBED TITLE",
  message: "EMBED DESCRIPTION",
  image: "IMAGE URL"
}}
```

Example
```js
ws.on("message", (json) => {
  const { e, d } = JSON.parse(json); // parses the json
  if (e === "BOT_EMBED") { //checks for the event
      console.log(d.name) // take the name from the d object 
  });
```
