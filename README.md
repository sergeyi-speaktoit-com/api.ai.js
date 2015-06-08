api.ai.js - Javascript SDK for api.ai
=====================================
api.ai.js makes it easy to integrate your web application with [api.ai](http://api.ai) natural language processing service. 


Project on Github [https://github.com/sergeyi-speaktoit-com/api.ai.js](https://github.com/sergeyi-speaktoit-com/api.ai.js)  
Demo application sources [https://github.com/sergeyi-speaktoit-com/api.ai.js/tree/master/demo](https://github.com/sergeyi-speaktoit-com/api.ai.js/tree/master/demo)

# Prepare your agent

Use [https://api.api.ai/api-client/](https://api.api.ai/api-client/) to create your [agent](http://api.ai/docs/getting-started/5-min-guide/).
You could import example agent [PizzaDelivery.zip](https://github.com/sergeyi-speaktoit-com/api.ai.js/blob/master/resources/PizzaDelivery.zip)
You'll need [access_token](http://api.ai/docs/getting-started/quick-start-api.html#step-1-obtain-an-access-token) to let client access to the agent.

# Using

Add [api.ai.min.js](https://github.com/sergeyi-speaktoit-com/api.ai.js/blob/master/target/api.ai.min.js) before your main js script in your html file.
```html
<script type="text/javascript" src="js/api.ai.min.js"></script>
<script type="text/javascript" src="js/main.js"></script>
```

Deploy your html file and scripts by using your favorite webserver (it is important go get access to microphone). 


Create instance of ApiAi. 
You can use config to set properties and listeners:

```javascript
var config = {
    server: 'wss://api.api.ai:4435/api/ws/query',
    token: access_token,// Use Client access token there (see agent keys).
    sessionId: sessionId,
    onInit: function () {
        console.log("> ON INIT use config");
    }
};
var apiAi = new ApiAi(config);
```

Or you can assign properties  and listeners directly:

```javascript
apiAi.sessionId = '1234';
apiAi.onInit = function () {
    console.log("> ON INIT use direct assignment property");
    apiAi.open();
};
```

Invoke init() method when you want to ask permissions to use microphone.

```javascript
apiAi.init();
```

After initialisation you could open websocket 
```javascript
apiAi.onInit = function () {
    console.log("> ON INIT use direct assignment property");
    apiAi.open();
};
```

When socket is open you can start listening microphone
 
```javascript
apiAi.onOpen = function () {
    apiAi.startListening();
};
```

To stop listening invoke:
```javascript
apiAi.stopListening();
```

If you use Firefox you don't need interrupt listening manually. End of speech detection do it for you. 

To get access to result use callback onResults(data)

```javascript
apiAi.onResults = function (data) {
    var status = data.status;
    var code;
    if (!(status && (code = status.code) && isFinite(parseFloat(code)) && code < 300 && code > 199)) {
        text.innerHTML = JSON.stringify(status);
        return;
    }
    processResult(data.result);
};
```

# API properties

```javascript
/**
 * 
 * 'wss://api.api.ai:4435/api/ws/query' 
 */
ApiAi.server
/**
 * Client access token of your agent. 
 */
ApiAi.token
/**
 * Unique session identifier to build dialogue. 
 */
ApiAi.sessionId
/**
 * How often reader should send audio-data chunk to the server.
 */
ApiAi.readingInterval
```

# API methods

```javascript
/**
 * Initializes audioContext
 * Set up the recorder (incl. asking permission)
 * Can be called multiple times.
 */
ApiAi.init();
/**
 * Chck if recorder is initialise.
 * @returns {boolean}
 */
ApiAi.isInitialise();
/**
 * Send object as json
 * @param json - javascript map.
 */
ApiAi.sendJson(jsObjectOrMap);
/**
 * Start recording and transcribing
 */
ApiAi.startListening();
/**
 * Stop listening, i.e. recording and sending of new input.
 */
ApiAi.stopListening();
/**
 * Check if websocket is open
 */
ApiAi.isOpen();
/**
 * Open websocket
 */
ApiAi.open();
/**
 * Cancel everything without waiting on the server
 */
ApiAi.close();
```

# API callbacks

```javascript
/**
 * It's triggered after web-socket is open.
 */
ApiAi.onOpen = function () {};
/**
 * It's triggered after web-socket is closed. 
 */
ApiAi.onClose = function () {};
/**
 * It's triggered after initialisation is finished.
 */
ApiAi.onInit = function () {};
/**
 * It's triggered when listening is started. 
 */
ApiAi.onStartListening = function () {};
/**
 * It's triggered when listening is stopped.
 */
ApiAi.onStopListening = function () {};
/**
 *  It's triggered when result is received;
 */
ApiAi.onResults = function (result) {};
/**
 * It's triggered when event is happened; 
 */
ApiAi.onEvent = function () {};
/**
 * It's triggered when error is happened; 
 */
ApiAi.onError = function () {};
```



