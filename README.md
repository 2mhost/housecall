Push functions returning a function to the queue. Set the concurrency, and
optionally a cooldown.

## Install

```
npm install housecall
```

## Usage

```javascript
var housecall = require('housecall');
var axios = require('axios');

var queue = housecall({ concurrency: 2, cooldown: 1000 });

queue.push(() => axios.get('https://twitter.com/').then((response, body) => {
	// The eventual response of Twitter
}));

queue.push(() => axios.get('https://some.com/').then((response, body) => {
	// ...
}));

queue.push(() => axios.get('https://test.com/').then((response, body) => {
	// This will not run until time of termination of one of the above + 1000 ms.
}));

```

Make sure you wrap the function. Otherwise it will run immediately and defeat the purpose of the queue.

```javascript
// WILL NOT BE CORRECTLY ENQUEUED
queue.push(axios.get('https://twitter.com/').then((response, body) => {
	// The queue will not control the start of execution here.
}));
```

## Errors

The queue continues on any error, but the error will be outputted to `housecall:queue:error`. Set `DEBUG=housecall:queue:error`
to see it.