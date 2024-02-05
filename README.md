<!--
  -- This file is auto-generated from README_js.md. Changes should be made there.
  -->

## UUID Module API Overview

                         | C | O | H | E | R | E | N | T|
                               | --- | --- | --- |

- [`uuid.NIL`](#uuidnil): Represents the nil UUID string (all zeros). Introduced in `uuid@8.3`.
- [`uuid.parse()`](#uuidparsestr): Converts a UUID string to an array of bytes. Introduced in `uuid@8.3`.
- [`uuid.stringify()`](#uuidstringifyarr-offset): Converts an array of bytes to a UUID string. Introduced in `uuid@8.3`.
- [`uuid.v1()`](#uuidv1options-buffer-offset): Generates a version 1 (timestamp) UUID.
- [`uuid.v3()`](#uuidv3name-namespace-buffer-offset): Generates a version 3 (namespace with MD5) UUID.
- [`uuid.v4()`](#uuidv4options-buffer-offset): Generates a version 4 (random) UUID.
- [`uuid.v5()`](#uuidv5name-namespace-buffer-offset): Generates a version 5 (namespace with SHA-1) UUID.
- [`uuid.validate()`](#uuidvalidatestr): Tests a string to check if it is a valid UUID. Introduced in `uuid@8.3`.
- [`uuid.version()`](#uuidversionstr): Detects the RFC version of a UUID. Introduced in `uuid@8.3`.

Please refer to the individual sections for detailed information on each API function. Note that certain features are introduced or enhanced in `uuid@8.3`.

**1. Install**

```shell
npm install uuid
```

**2. Create a UUID** (ES6 module syntax)

```javascript
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
```

... or using CommonJS syntax:

```javascript
const { v4: uuidv4 } = require('uuid');
uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

For timestamp UUIDs, namespace UUIDs, and other options read on ...

## API

### uuid.NIL

The nil UUID string (all zeros).

Example:

```javascript
import { NIL as NIL_UUID } from 'uuid';

NIL_UUID; // ⇨ '00000000-0000-0000-0000-000000000000'
```

### uuid.parse(str)

Convert UUID string to array of bytes

|           |                                          |
| --------- | ---------------------------------------- |
| `str`     | A valid UUID `String`                    |
| _returns_ | `Uint8Array[16]`                         |
| _throws_  | `TypeError` if `str` is not a valid UUID |

Note: Ordering of values in the byte arrays used by `parse()` and `stringify()` follows the left &Rarr; right order of hex-pairs in UUID strings. As shown in the example below.

Example:

```javascript
import { parse as uuidParse } from 'uuid';

// Parse a UUID
const bytes = uuidParse('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b');

// Convert to hex strings to show byte order (for documentation purposes)
[...bytes].map((v) => v.toString(16).padStart(2, '0')); // ⇨ 
  // [
  //   '6e', 'c0', 'bd', '7f',
  //   '11', 'c0', '43', 'da',
  //   '97', '5e', '2a', '8a',
  //   'd9', 'eb', 'ae', '0b'
  // ]
```

### uuid.stringify(arr[, offset])

Convert array of bytes to UUID string

|                |                                                                              |
| -------------- | ---------------------------------------------------------------------------- |
| `arr`          | `Array`-like collection of 16 values (starting from `offset`) between 0-255. |
| [`offset` = 0] | `Number` Starting index in the Array                                         |
| _returns_      | `String`                                                                     |
| _throws_       | `TypeError` if a valid UUID string cannot be generated                       |

Note: Ordering of values in the byte arrays used by `parse()` and `stringify()` follows the left &Rarr; right order of hex-pairs in UUID strings. As shown in the example below.

Example:

```javascript
import { stringify as uuidStringify } from 'uuid';

const uuidBytes = [
  0x6e, 0xc0, 0xbd, 0x7f, 0x11, 0xc0, 0x43, 0xda, 0x97, 0x5e, 0x2a, 0x8a, 0xd9, 0xeb, 0xae, 0x0b,
];

uuidStringify(uuidBytes); // ⇨ '6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'
```

### uuid.v1([options[, buffer[, offset]]])

Create an RFC version 1 (timestamp) UUID

|  |  |
| --- | --- |
| [`options`] | `Object` with one or more of the following properties: |
| [`options.node` ] | RFC "node" field as an `Array[6]` of byte values (per 4.1.6) |
| [`options.clockseq`] | RFC "clock sequence" as a `Number` between 0 - 0x3fff |
| [`options.msecs`] | RFC "timestamp" field (`Number` of milliseconds, unix epoch) |
| [`options.nsecs`] | RFC "timestamp" field (`Number` of nanoseconds to add to `msecs`, should be 0-10,000) |
| [`options.random`] | `Array` of 16 random bytes (0-255) |
| [`options.rng`] | Alternative to `options.random`, a `Function` that returns an `Array` of 16 random bytes (0-255) |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |
| _throws_ | `Error` if more than 10M UUIDs/sec are requested |

Note: The default [node id](https://tools.ietf.org/html/rfc4122#section-4.1.6) (the last 12 digits in the UUID) is generated once, randomly, on process startup, and then remains unchanged for the duration of the process.

Note: `options.random` and `options.rng` are only meaningful on the very first call to `v1()`, where they may be passed to initialize the internal `node` and `clockseq` fields.

Example:

```javascript
import { v1 as uuidv1 } from 'uuid';

uuidv1(); // ⇨ '2c5ea4c0-4067-11e9-8bad-9b1deb4d3b7d'
```

Example using `options`:

```javascript
import { v1 as uuidv1 } from 'uuid';

const v1options = {
  node: [0x01, 0x23, 0x45, 0x67, 0x89, 0xab],
  clockseq: 0x1234,
  msecs: new Date('2011-11-01').getTime(),
  nsecs: 5678,
};
uuidv1(v1options); // ⇨ '710b962e-041c-11e1-9234-0123456789ab'
```

### uuid.v3(name, namespace[, buffer[, offset]])

Create an RFC version 3 (namespace w/ MD5) UUID

API is identical to `v5()`, but uses "v3" instead.

&#x26a0;&#xfe0f; Note: Per the RFC, "_If backward compatibility is not an issue, SHA-1 [Version 5] is preferred_."

### uuid.v4([options[, buffer[, offset]]])

Create an RFC version 4 (random) UUID

|  |  |
| --- | --- |
| [`options`] | `Object` with one or more of the following properties: |
| [`options.random`] | `Array` of 16 random bytes (0-255) |
| [`options.rng`] | Alternative to `options.random`, a `Function` that returns an `Array` of 16 random bytes (0-255) |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |

Example:

```javascript
import { v4 as uuidv4 } from 'uuid';

uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

Example using predefined `random` values:

```javascript
import { v4 as uuidv4 } from 'uuid';

const v4options = {
  random: [
    0x10, 0x91, 0x56, 0xbe, 0xc4, 0xfb, 0xc1, 0xea, 0x71, 0xb4, 0xef, 0xe1, 0x67, 0x1c, 0x58, 0x36,
  ],
};
uuidv4(v4options); // ⇨ '109156be-c4fb-41ea-b1b4-efe1671c5836'
```

### uuid.v5(name, namespace[, buffer[, offset]])

Create an RFC version 5 (namespace w/ SHA-1) UUID

|  |  |
| --- | --- |
| `name` | `String \| Array` |
| `namespace` | `String \| Array[16]` Namespace UUID |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |

Note: The RFC `DNS` and `URL` namespaces are available as `v5.DNS` and `v5.URL`.

Example with custom namespace:

```javascript
import { v5 as uuidv5 } from 'uuid';

// Define a custom namespace.  Readers, create your own using something like
// https://www.uuidgenerator.net/
const MY_NAMESPACE = '1b671a64-40d5-491e-99b0-da01ff1f3341';

uuidv5('Hello, World!', MY_NAMESPACE); // ⇨ '630eb68f-e0fa-5ecc-887a-7c7a62614681'
```

Example with RFC `URL` namespace:

```javascript
import { v5 as uuidv5 } from 'uuid';

uuidv5('https://www.w3.org/', uuidv5.URL); // ⇨ 'c106a26a-21bb-5538-8bf2-57095d1976c1'
```

### uuid.validate(str)

Test a string to see if it is a valid UUID

|           |                                                     |
| --------- | --------------------------------------------------- |
| `str`     | `String` to validate                                |
| _returns_ | `true` if string is a valid UUID, `false` otherwise |

Example:

```javascript
import { validate as uuidValidate } from 'uuid';

uuidValidate('not a UUID'); // ⇨ false
uuidValidate('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'); // ⇨ true
```

Using `validate` and `version` together it is possible to do per-version validation, e.g. validate for only v4 UUIds.

```javascript
import { version as uuidVersion } from 'uuid';
import { validate as uuidValidate } from 'uuid';

function uuidValidateV4(uuid) {
  return uuidValidate(uuid) && uuidVersion(uuid) === 4;
}

const v1Uuid = 'd9428888-122b-11e1-b85c-61cd3cbb3210';
const v4Uuid = '109156be-c4fb-41ea-b1b4-efe1671c5836';

uuidValidateV4(v4Uuid); // ⇨ true
uuidValidateV4(v1Uuid); // ⇨ false
```

### uuid.version(str)

Detect RFC version of a UUID

|           |                                          |
| --------- | ---------------------------------------- |
| `str`     | A valid UUID `String`                    |
| _returns_ | `Number` The RFC version of the UUID     |
| _throws_  | `TypeError` if `str` is not a valid UUID |

Example:

```javascript
import { version as uuidVersion } from 'uuid';

uuidVersion('45637ec4-c85f-11ea-87d0-0242ac130003'); // ⇨ 1
uuidVersion('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'); // ⇨ 4
```

## Command Line

UUIDs can be generated from the command line using `uuid`.

```shell
$ npx uuid
ddeb27fb-d9a0-4624-be4d-4615062daed4
```

The default is to generate version 4 UUIDS, however the other versions are supported. Type `uuid --help` for details:

```shell
$ npx uuid --help

Usage:
  uuid
  uuid v1
  uuid v3 <name> <namespace uuid>
  uuid v4
  uuid v5 <name> <namespace uuid>
  uuid --help

Note: <namespace uuid> may be "URL" or "DNS" to use the corresponding UUIDs
defined by RFC4122
```

## ECMAScript Modules

This library comes with [ECMAScript Modules](https://www.ecma-international.org/ecma-262/6.0/#sec-modules) (ESM) support for Node.js versions that support it ([example](./examples/node-esmodules/)) as well as bundlers like [rollup.js](https://rollupjs.org/guide/en/#tree-shaking) ([example](./examples/browser-rollup/)) and [webpack](https://webpack.js.org/guides/tree-shaking/) ([example](./examples/browser-webpack/)) (targeting both, Node.js and browser environments).

```javascript
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

To run the examples you must first create a dist build of this library in the module root:

```shell
npm run build
```

#### Author: [Sridhar Dharmaraj]