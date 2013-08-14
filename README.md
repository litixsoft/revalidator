# revalidator [![Build Status](https://secure.travis-ci.org/flatiron/revalidator.png)](http://travis-ci.org/flatiron/revalidator)

A cross-browser / node.js validator used by resourceful and flatiron.

## Example
The core of `revalidator` is simple and succinct: `revalidator.validate(obj, schema)`: 
 
``` js
  var revalidator = require('revalidator');
  
  console.dir(revalidator.validate(someObject, {
    properties: {
      url: {
        description: 'the url the object should be stored at',
        type: 'string',
        pattern: '^/[^#%&*{}\\:<>?\/+]+$',
        required: true
      },
      challenge: {
        description: 'a means of protecting data (insufficient for production, used as example)',
        type: 'string',
        minLength: 5
      },
      body: {
        description: 'what to store at the url',
        type: 'any',
        default: null
      }
    }
  }));
```

This will return with a value indicating if the `obj` conforms to the `schema`. If it does not, a descriptive object will be returned containing the errors encountered with validation.

``` js
  {
    valid: true // or false
    errors: [/* Array of errors if valid is false */]
  }
```

In the browser, the validation function is exposed on `window.validate` by simply including `revalidator.js`.

## Installation

### Installing npm (node package manager)
``` bash
  $ curl http://npmjs.org/install.sh | sh
```

### Installing revalidator
``` bash 
  $ [sudo] npm install revalidator
```

## Usage

`revalidator` takes json-schema as input to validate objects.

### revalidator.validate (obj, schema, options)

This will return with a value indicating if the `obj` conforms to the `schema`. If it does not, a descriptive object will be returned containing the errors encountered with validation.

``` js
{
  valid: true // or false
  errors: [/* Array of errors if valid is false */]
}
```

#### Available Options
* __validateFormats__: Enforce format constraints ( __default: `true`__ )
* __validateFormatsStrict__: When `validateFormats` is `true` treat unrecognized formats as validation errors ( __default `false`__ )
* __validateFormatExtensions__: When `validateFormats` is `true` also validate formats defined in `validate.formatExtensions`.
This option is used for lx-valid format extensions and additional custom formats. Those are stored in here. ( __default: `true`__ )
* __cast__: Enforce casting of some types (for integers/numbers are only supported) when it's possible,
e.g. `"42" => 42`, but `"forty2" => "forty2"` for the integer type. ( __default: `undefined`__ )
* __deleteUnknownProperties__: Deletes all properties from object which are not declared in the schema. ( __default: `false`__ )
* __convert__: Converts a property by the format defined in the schema. Modifies the original object. ( __default: `undefined`__ )
* __trim__: Trims all properties of type `string`. Modifies the original object. ( __default: `false`__ )
* __strictRequired__: Sets validity of empty `string` to `false`. ( __default: `false`__ )

### Schema
For a property an `value` is that which is given as input for validation where as an `expected value` is the value of the below fields

#### required
If true, the value should not be empty

```js
{ required: true }
```

#### type
The `type of value` should be equal to the expected value

```js
{ type: 'string' }
{ type: 'number' }
{ type: 'integer' }
{ type: 'array' }
{ type: 'boolean' }
{ type: 'object' }
{ type: 'null' }
{ type: 'any' }
{ type: ['boolean', 'string'] }
```

#### pattern
The expected value regex needs to be satisfied by the value

```js
{ pattern: /^[a-z]+$/ }
```

#### maxLength
The length of value must be greater than or equal to expected value

```js
{ maxLength: 8 }
```

#### minLength
The length of value must be lesser than or equal to expected value

```js
{ minLength: 8 }
```

#### minimum
Value must be greater than or equal to the expected value

```js
{ minimum: 10 }
```

#### maximum
Value must be lesser than or equal to the expected value

```js
{ maximum: 10 }
```

#### exclusiveMinimum
Value must be greater than expected value

```js
{ exclusiveMinimum: 9 }
```

### exclusiveMaximum
Value must be lesser than expected value

```js
{ exclusiveMaximum: 11 }
```

#### divisibleBy
Value must be divisible by expected value

```js
{ divisibleBy: 5 }
{ divisibleBy: 0.5 }
```

#### minItems
Value must contain more then expected value number of items

```js
{ minItems: 2 }
```

#### maxItems
Value must contains less then expected value number of items

```js
{ maxItems: 5 }
```

#### uniqueItems
Value must hold a unique set of values

```js
{ uniqueItems: true }
```

#### enum
Value must be present in the array of expected value

```js
{ enum: ['month', 'year'] }
```

#### format
Value must be a valid format

```js
{ format: 'url' }
{ format: 'email' }
{ format: 'ip-address' }
{ format: 'ipv6' }
{ format: 'date-time' }
{ format: 'date' }
{ format: 'time' }
{ format: 'color' }
{ format: 'host-name' }
{ format: 'utc-millisec' }
{ format: 'regex' }
{ format: ['ip-address', 'ipv6'] }
```

#### conform
Value must conform to constraint denoted by expected value

```js
{ conform: function (v) {
    if (v%3==1) return true;
    return false;
  }
}
```

#### dependencies
Value is valid only if the dependent value is valid

```js
{
  town: { required: true, dependencies: 'country' },
  country: { maxLength: 3, required: true }
}
```

### Nested Schema
We also allow nested schema

```js
{
  properties: {
    title: {
      type: 'string',
      maxLength: 140,
      required: true
    },
    author: {
      type: 'object',
      required: true,
      properties: {
        name: {
          type: 'string',
          required: true
        },
        email: {
          type: 'string',
          format: 'email'
        }
      }
    }
  }
}
```

### Custom Messages
We also allow custom message for different constraints

```js
{
  type: 'string',
  format: 'url'
  messages: {
    type: 'Not a string type',
    format: 'Expected format is a url'
  }
```

```js
{
  conform: function () { ... },
  message: 'This can be used as a global message'
}
```

### Convert option
Converts a property by the format defined in the schema and returns the converted object in the result.

```js
var data = {
  birthdate: '1979-03-01T15:55:00.000Z'
};

var schema = {
  properties: {
    birthdate: {
      type: 'string',
      format: 'date-time'
    }
  }
};

var convertFn = function(format, value) {
  if (format === 'date-time') {
    return new Date(value);
  }

  return value;
};

var result = validate(data, schema, {convert: convertFn});

// birthdate was converted
typeof result.convertedObject === 'object';
typeof result.convertedObject.birthdate === 'object';

```


## Tests
All tests are written with [vows][0] and should be run with [npm][1]:

``` bash
  $ npm test
```

#### Author: [Charlie Robbins](http://nodejitsu.com), [Alexis Sellier](http://cloudhead.io)
#### Contributors: [Fedor Indutny](http://github.com/indutny), [Bradley Meck](http://github.com/bmeck), [Laurie Harper](http://laurie.holoweb.net/)
#### License: Apache 2.0

[0]: http://vowsjs.org
[1]: http://npmjs.org
