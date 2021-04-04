## Welcome To RaeedChat API 
RaeedChat API lets you connect to our servers through a automated bot to interact and entertain users. This allows for a bigger and automated community which can in turn create a better RaeedChat for everyone! We hope you join us in this mission! 

## Getting Started
Get started with your very first bot!


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
    "name": info.response.name,
    "message": message,
    "id": info.response.id,
    "messageid": messageid,
    "pings": pingedusers,
    "badge": badge
  }
}
```

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
