# [Dashbot](http://dashbot.io) - Analytics for your bot

Dashbot gives you easy to access analytics for your bot for free.

Currently we support

* [Facebook Messenger](http://developers.facebook.com)
* [Slack](http://api.slack.com)

## Setup Facebook

Create a free account at [http://dashbot.io](http://dashbot.io) and get an API_KEY.

dashbot is available via NPM.

```bash
npm install --save dashbot
```

Include dashbot.

```javascript
var dashbot = require('./dashbot')(process.env.DASHBOT_API_KEY).facebook;
```

Then log whenver your webhook is called

```javascript
app.post('/facebook/receive/', function(req, res) {
  dashbot.logIncoming(req.body);
  ...
```

Finally, whenever you send a message log the request and response.

```javascript
    const requestData = {
      url: 'https://graph.facebook.com/v2.6/me/messages',
      qs: {access_token: process.env.FACEBOOK_PAGE_TOKEN},
      method: 'POST',
      json: {
        recipient: {id: sender},
        message: {
          text: 'You are right when you say: ' + text
        }
      }
    };
    const requestId = dashbot.logOutgoing(requestData);
    request(requestData, function(error, response, body) {
      dashbot.logOutgoingResponse(requestId, error, response);
    });
```

That is it!

For a complete example see: [facebook-example.js](https://github.com/actionably/dashbot/blob/master/src/facebook-example.js)

## Setup Slack

Dashbot offers a botkit middleware to make plugging into your slack bot easy.

Create a free account at [http://dashbot.io](http://dashbot.io) and get an API_KEY.

dasbot is available via NPM.

```bash
npm install --save dashbot
```

Include dashbot.

```javascript
var dashbot = require('./dashbot')(process.env.DASHBOT_API_KEY).slack;
```

After you create your botkit controller simply add a send and receive middleware

```javascript
const controller = Botkit.slackbot();

controller.configureSlackApp(...);

// Add the dashbot middleware
controller.middleware.receive.use(dashbot.receive);
controller.middleware.send.use(dashbot.send);
```

That is it!

For a complete example see: [slack-example.js](https://github.com/actionably/dashbot/blob/master/src/slack-example.js)
