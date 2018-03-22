# Uppercase Bot - Simple Slack Bot Example on Node.js

It is a simple Slack bot that converts a message to uppercase letters.

>Before you run this code, you need to replace the token with your own bot access token.

```javascript
const Slack = require('slack'),
      WebSocket = require('ws');

const token = 'Your Bot User OAuth Access Token'
const slackBot = new Slack({token});

// Starts a Real Time Messaging session.
// https://api.slack.com/methods/rtm.start 
slackBot.rtm.start()    
  .then(result => {
    
    // Makes a WebSocket connection to a Slack server
    const ws = new WebSocket(result.url);
      
    ws.on('open', function open() {
      console.log('Slack RTM Connect open.');
    });

    ws.on('message', function incoming(data) {

      const message = JSON.parse(data);
      console.log('Message', message);

      // Catches message data except for what the bot sends
      if (message.type === 'message' && 
          message.subtype !== "bot_message") {
        
        // Replies with a uppercase string
        // https://api.slack.com/methods/chat.postMessage 
        slackBot.chat.postMessage(
          {
            channel: message.channel, 
            text: message.text.toUpperCase()
          }
        )
          .then(result => {
            console.log('PostMessage', result);
          })
          .catch(err => {
            console.log('Error', err);
          })
      }
    });
  })
  .catch(err => {
    console.log('Error', err);
  })  
```

* Slack API Document: https://api.slack.com/
* Slack Web API Client: https://www.npmjs.com/package/slack
* WebSocket Client: https://www.npmjs.com/package/ws
