# saml2js [![npm version](https://badge.fury.io/js/saml2js.svg)](http://badge.fury.io/js/saml2js) [![Build Status](https://travis-ci.org/Aplo/saml2js.svg?branch=master)](https://travis-ci.org/Aplo/saml2js)

> Parses SAML responses into JS objects you can read and manipulate.

## Install

```
$ npm install saml2js --save
```

Saml2js supports Node.js 0.10+ and iojs.

[![NPM](https://nodei.co/npm/saml2js.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/saml2js/)

## Usage

Saml2js was designed for use in any Node.js environment whether that's a web app or a standalone script.

```js
var express = require('express')
    Saml2js = require('saml2js'),
    app     = express();

app.post('/saml/callback/?', function(req, res, next){
  var parser = new Saml2js(res.body.SAMLResponse);
  res.json(parser.toObject());
});

app.listen(3000);
```

### Methods

#### Constructor

To instantiate a new SAML parser:

```js
var Saml2js = require('saml2js');

var parser = new Saml2js(SAMLResponse);
```

After passing your SAML response as a string to the constructor you now have access to the following methods.

#### `toObject()`

Returns the parsed SAML as a JavaScript object.

```
var parsedObject = parser.toObject();
```

Note that if your SAML has attributes have a `Name` attribute that contains a string that is mixed case and contains spaces Saml2js will automatically camel case this name when it is added as a property on the resulting object.

__Example:__

Given the following SAML...

```xml
<saml2:Attribute Name="First Name">
  <saml2:AttributeValue>John</saml2:AttributeValue>
</saml2:Attribute>
```

The resulting JavaScript object returned from `Saml2js().toObject()` will look like this:

```js
{
  firstName: 'John'
}
```

#### `toJSON()`

Returns parsed SAML as a JSON string. Once you've instantiated the module and passed it raw SAML you can get its value as a JSON string with `parser.toJSON()`.

#### `get()`

Returns the value of a SAML attribute by name. The name you pass to this function should be the same as what the attribute value in your SAML is. For example, given this SAML:

```xml
<saml2:Attribute Name="First Name">
  <saml2:AttributeValue>John</saml2:AttributeValue>
</saml2:Attribute>
```

To get the value of `First Name` you would call it like this:

```js
// assuming you've instantiated the library as `parser` with `new Saml2js(SAMLResponse)`...
var firstName = parser.get('first name');
console.log(firstName); //=> 'John'
```

You don't need to worry about case sensitivity. Internally the Lodash `.camelCase()` method is called on the string you pass so when it is compared against the parsed SAML it will automagically match the name of the key as its stored internally if it exists.

#### `parse()`

This is a private method. It is called internally when you pass your SAML to the constructor. You should never need to call this manually. See the source code if you want to know more about it.

## Testing

Testing requires Mocha and Chai.

1. Clone the repository
2. Run `npm install && npm install -g mocha`
3. Finally, run `mocha`

## Contributing

When contributing, be sure to branch off of `develop` to get the latest changes. Contributions are welcome. Please try to write tests for your code so we can merge it in faster.

## Credit

*This is a fork of [saml2json](https://github.com/flesch/saml2json.git) by John Flesch. Thanks for your original work.*

## License

[The MIT License (MIT)](http://flesch.mit-license.org/)

Modified work Copyright (c) 2015 Aplo LLC, https://aploquote.com
Original work Copyright (c) 2013 John Flesch, http://fles.ch

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

