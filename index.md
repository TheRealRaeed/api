# RaeedChat Bot API

**EARLY ACCESS YOU HAVE TO CONTACT ME AT webdevraeed@gmail.com for usage**

## Welcome To RaeedChat API 
RaeedChat API lets you connect to our servers through a automated bot to interact and entertain users. This allows for a bigger and automated community which can in turn create a better RaeedChat for everyone! We hope you join us in this mission! 

## Websocket Stuff
All the web socket events!


### Gateway 
Secure a connection to our gateway which is wss://api.raeedchat.com/websocket/ 

### Initialize
Initialization is highly important! Without proper initialization your requests will be denied! 

Please send a event called INIT to the websocket with the following 
```json 
{
  "d": {
    "token": "YOUR_TOKEN"
  }
}
```  
If this is not provided you will be **disconnected from our websocket** automatically 

### Message Create Event 
The message create event is fired each time a message is sent. 

The official name is MESSAGE_CREATE, so listening for the MESSAGE_CREATE event will allow you to receive messages. This is what the message create event provides: 
```json 
{
  "d": {
    "name": "NAME",
    "message": "MESSAGE",
    "id": "ID",
    "messageid": "MESSAGE_ID",
    "pings": "Pinged Users",
    "badge": "BADGE"
  }
}
``` 
*The badge of highest importance will show if user has multiple badges*

### INIT Failed Event 
This event contains **no response**. When it is fired it means your initialization has been denied. This is most likely due to a wrong token. The actual name is INIT_FAILED. Listen for that if you want to handle this event :)

### INIT Ok Event 
The INIT_OK event is the event that says that the initialization information has been verified and your connection was successful. Listen for INIT_OK to handle this.  

### Message Delete Event 
The MESSAGE_DELETE event is the event that fires when a message is deleted. It would return this 
```json 
{ 
 "d": "MESSAGE_ID" 
}
```

## API Requests 
Send data to our api for sending to the chat. 

### Endpoints 
`https://api.raeedchat.com/api/v1/sendMessage` - Send message with Bot API
`https://api.raeedchat.com/api/v1/sendReply` - Send embed with Bot API

### Sending messages 
To send a message you need to make a request with the `Content-Type: application/json` header and `Authorization: TOKEN` header. Make sure to replace TOKEN with your actual token! Heres the data to send in the body: 
```json 
{
  "message": "YOUR_MESSAGE
}
```
The endpoint you send this request to is `https://api.raeedchat.com/api/v1/sendMessage`. Once completed you should get this: 
```json
{
  "code": 200,
  "success": true,
  "message": {
    "sentcontent": "YOUR_MESSAGE"
  }
}
```
This is a fully successful response. If there is a error then you will get this: 
```json
{
  "code": "ERROR_CODE", 
  "success": false,
  "errormessage": "ERROR_MESSAGE"
}
``` 
*The error code is normally a number please handle it as so!*

![BOT](https://cdn.discordapp.com/attachments/821092261165006861/828393827294642196/unknown.png)
This is what it will look like when sent.


### Sending Embeds
Sending embeds through our bot api is quick and easy! Put your `Content-Type: application/json` header and `Authorization: TOKEN` headers and then write this in the post data: 
```json 
{
  "title": "EMBED TITLE" 
  "description": "DESCRIPTION" 
  "image": "IMAGE URL"
}
```
Any of these are optional but please send at least one. Make this request to `https://api.raeedchat.com/api/v1/sendMessage`. The response is the following: 
```json
{
  "code": 200,
  "success": true,
  "embed": {
    "title": "TITLE",
    "description": "DESCRIPTION",
    "image": "imageurl" 
  }
}
```
If any of the fields aren't defined they will return `undefined` or `null`. 

This is if there is a error:
```json
{
  "code": "ERROR_CODE", 
  "success": false,
  "errormessage": "ERROR_MESSAGE"
}
``` 
*The error code is normally a number please handle it as so!*

