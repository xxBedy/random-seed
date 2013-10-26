# random-seed

Gibson Research Corporation's Ultra-High Entropy Pseudo-Random Number Generator
ported to node.

The original library / project page is located here: https://www.grc.com/otg/uheprng.htm

The node project page is here: https://github.com/skratchdot/random-seed

There were a few modifications made to the original library to allow seeding, and to
pass jshint.


## Getting Started

Install the module with: `npm install random-seed`

```javascript
var rand = require('random-seed').create();
var n = rand(100); // generate a random number between 0 - 99
```

## Documentation

### Create a random number generator

```javascript
var gen = require('random-seed'); // create a generator

// these generators produce different numbers
var rand1 = gen.create(); // method 1
var rand2 = new gen();    // method 2
var rand3 = gen();        // method 3

// these generators will produce
// the same sequence of numbers
var seed = 'My Secret String Value';
var rand4 = gen.create(seed);
var rand5 = new gen(seed);
var rand6 = gen(seed);
```

### Random Generator Methods

Once a random generator is created, you have the following methods available.

I typically create a random generator like this:

```javascript
var rand = require('random-seed').create();
```

#### rand(num)


#### rand.seed(seed)

Same as calling rand.initState() followed by rand.hashString(seed). If seed is not
a string, then the seed value will be converted to a string. If you don't pass a
seed argument, then the generator uses Math.random() as the seed.

#### rand.string(count)

Returns a pseudo-random string of 'count' printable characters
ranging from chr(33) to chr(126) inclusive.

#### rand.cleanString(inStr)

Removes leading and trailing spaces and non-printing control characters,
including any embedded carriage-return (CR) and line-feed (LF) characters,
from any string it is handed.  This is also used by the 'hashstring' function (below)
to help users always obtain the same EFFECTIVE uheprng seeding key.

#### rand.hashString(inStr)

Hashes the provided character string after first removing any leading or trailing spaces
and ignoring any embedded carriage returns (CR) or Line Feeds (LF).

#### rand.addEntropy(/* accept zero or more arguments */)

This handy exported function is used to add entropy to our uheprng at any time.

#### rand.initState()

If we want to provide a deterministic startup context for our PRNG,
but without directly setting the internal state variables, this allows
us to initialize the mash hash and PRNG's internal state before providing
some hashing input.

#### rand.done()

We use this (optional) exported function to signal the JavaScript interpreter
that we're finished using the internal "Mash" hash function so that it can free up the
local "instance variables" it will have been maintaining.  It's not strictly
necessary, of course, but it's good JavaScript citizenship.


## Examples

### Default Usage: print 1 random number between 0 - 99
```javascript
var rand = require('random-seed').create();
var n = rand(100); // generate a random number between 0 - 99
```

### Always print same sequence of random numbers
```javascript
var rand = require('random-seed').create();
rand.initState();
var n1 = rand(100); // n1 === 58
var n2 = rand(100); // n2 === 26
rand.initState();   // re-init
var n3 = rand(100); // n3 === 58 && n3 === n1
```

### Create 2 random number generators
```javascript
var rand1 = require('random-seed').create(),
	rand2 = require('random-seed').create();
console.log(rand1(100), rand2(100));
```

### Create 2 random number generators with the same seed
```javascript
var seed = 'Hello World',
	rand1 = require('random-seed').create(seed),
	rand2 = require('random-seed').create(seed);
console.log(rand1(100), rand2(100));
```

## Release History

- v0.1.0 (Released October 26, 2013)


## License

Copyright (c) 2013 skratchdot  

Dual Licensed under the MIT license and the original Public Domain License by GRC.