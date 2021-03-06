# SimpleBus

Simple Service Bus implementation, for Node.js.

## Installation

Via npm on Node:

```
npm install simplebus
```

## Usage

Reference in your program:

```js
var simplebus = require('simplebus');
```

Create a local message bus:
```js
var bus = simplebus.createBus();
```
Create a local message bus with a fixed size message queue (recommended):
```js
var bus = simplebus.createBus(1000);
```

Send a message to local bus:
```js
bus.post("foo");
bus.post({ operation: 'sale', price: 100, quantity: 10 });
```

Subscribe to a message
```js
// to all message
bus.subscribe(null, function (msg) { ... });
// to messages with property operation === 'sale'
bus.subscribe({ operation: 'sale' }, function(msg) { ... });
// to messages that satisfy a predicate
bus.subscribe(function (msg) { return msg.price < 100; }, function(msg) { ... });
```

Expose a bus as a server:
```js
var server = simplebus.createServer(bus, port, [host]);
server.start();
///
server.stop();
```

Consume as a client:
```js
var client = simplebus.createClient(port, [host]);
client.start(function () { // callback when connection is OK
	client.post("foo");
	client.subscribe(function (msg) { return msg.price > 100 }, function (msg) { .... });
});
///
client.stop();
```

## Development

```
git clone git://github.com/ajlopez/SimpleBus.git
cd SimpleBus
npm install
npm test
```

## Samples

- [Market](https://github.com/ajlopez/SimpleBus/tree/master/samples/Market) A distributed sample. Operator
send buy or sale messages, subscriber listen to selected messages.

## Versions

- 0.0.1: Published
- 0.0.2: Published, bus with maximum message queue size
- 0.0.3: Published, using SimpleRemote 0.0.5, updated engine range
- 0.0.4: Published, with improved error handling, via remster and sidrees

## Contribution

Feel free to [file issues](https://github.com/ajlopez/SimpleBus) and submit
[pull requests](https://github.com/ajlopez/SimpleBus/pulls) � contributions are
welcome.

If you submit a pull request, please be sure to add or update corresponding
test cases, and ensure that `npm test` continues to pass.

(Thanks to [JSON5](https://github.com/aseemk/json5) by [aseemk](https://github.com/aseemk). 
This file is based on that project README.md).