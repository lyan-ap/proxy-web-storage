# proxyStorage

A more convenient way to use storage through proxy.

## Install

```shell
npm i proxyStorage
```

## Features

### Base

Get what you set and change array and object directly.

```js
import { local, session } from 'proxyStorage';

local.test = 'Hello proxyStorage'; // works
delete local.test; // works

// number
local.test = 0;
local.test === 0; // true

// boolean
local.test = false;
local.test === false; // true

// undefined
local.test = undefined;
local.test === undefined; // true

// null
local.test = null;
local.test === null; // true

// object
local.test = { hello: 'world' };
local.test.hello = 'proxyStorage'; // works

// array
local.test = ['hello'];
local.test.push('proxyStorage'); // works
local.test.length // 2

// Date
local.test = new Date('2000-01-01T00:00:00.000Z');
local.test.getTime() === 946684800000; // true

// RegExp
local.test = /d(b+)d/g;
local.test.test("cdbbdbsbz"); // true

// function
local.test = function() {
  return 'Hello proxyStorage!';
};
local.test() === 'Hello proxyStorage!'; // true
```

### Subscribe

listen for changes.

```js
import { local } from 'proxyStorage';

local.on('test', function(newVal, oldVal) {
  console.log('test', newVal, oldVal);
});
local.on('test.a', function(newVal, oldVal) {
  console.log('test.a', newVal, oldVal);
});

local.test = {};
// test {} undefined

local.test.a = 1;
// test.a 1 undefined
```

#### on

Subscribe to an item.

- `key`: the name of the item to subscribe to. Support `obj.a` for `Object` and `list[0]` for `Array`.
- `callback`: the function to call when the item is changed. Includes `newValue` and `oldValue`.

#### once

Subscribe to an item only once.

- `key`: the name of the item to subscribe to. Support `obj.a` for `Object` and `list[0]` for `Array`.
- `callback`: the function to call when the item is changed. Includes `newValue` and `oldValue`.

#### off

Unsubscribe from an item or all items.

- `key(optional)`: the name of the item to unsubscribe from. If no key is provided, it unsubscribes you from all items.
- `callback(optional)`: the function used when binding to the item. If no callback is provided, it unsubscribes you from all functions binding to the item.

### Expired

set expires for items.

```js
import { local } from 'proxyStorage';

local.test = 'hello proxyStorage';
local.setExpires('test', Date.now() + 10000);

// after 10's
local.test // undefined
```

#### setExpires

set expires for an item.

- `key`: the name of the item to set expires.
- `expires`: accept `string`、`number` and `Date`.

#### getExpires

return the expires(`Date`) of the item.

- `key`: the name of the item that has set expires.

#### removeExpires

cancel the expires of the item.

- `key`: the name of the item that has set expires.