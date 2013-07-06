# Thinky

JavaScript ORM for RethinkDB.  
_Note_: Alpha release

### Quick start 

```javascript
var thinky = require('thinky');
thinky.connect({});

// Create a model
var Cat = thinky.createModel('Cat', {name: String}); 

// Create custom methods
Cat.define('hello', function() { console.log("Hello, I'm "+this.name) });

// Create a new object
kitty = new Cat({name: 'Kitty'});
kitty.hello(); // Log "Hello, I'm Kitty
kitty.save(function(err, result) {
    if (err) throw err;
    console.log("Kitty has been saved in the database");
})
```

### Docs
_Note_: Work in progress. 

#### Thinky

__Thinky.connect(__ options __)__

options (object): object with the fields

- host: RethinkDB host (default "localhost")
- port: RethinkDB port for client (default to 28015)
- db: default database (default to "test")
- poolMax: The maximum number of connections in the pool (default to 10)
- poolMin: The minimum number of connections in the pool (default to 1)
- enforce: Boolean that represent if the schemas should be enforced or not

_Note_: The behavior of enforce may change. Since there are more than two cases

- Be flexible
- Forbid extra fields
- Forbid missing fields
- Forbid extra AND missing fields


__Thinky.getOptions()__  

Returns all the options previously set.



__Thinky.getOption(__ optionName __)__  

Returns the value for _optionName_. Possible values:

    - host: RethinkDB host
    - port: RethinkDB port for client
    - db: default database
    - poolMax: The maximum number of connections in the pool
    - poolMin: The minimum number of connections in the pool
    - enforce: Boolean that represent if the schemas should be enforced or not



__Thinky.setOptions(__ options __)__

Overwrite the options defined in _options_.

The argument _options_ is an object that can have the following fields
    - host: RethinkDB host (default "localhost")
    - port: RethinkDB port for client (default to 28015)
    - db: default database (default to "test")
    - poolMax: The maximum number of connections in the pool (default to 10)
    - poolMin: The minimum number of connections in the pool (default to 1)
Setting a value to null will delete the value.

_Note_: Almost useless for now since we don't recreate/update the pool



__Thinky.disconnect()__

Close all the connections.



__Thinky.createModel(__ name, schema, settings __)__
Create a new model
- name: name of the model
- schema: An object which fields can have the following values:
    - String
    - Number
    - Boolean
    - Array
    - Object
- settings (object): settings for the model
    - enforce: Boolean that represent if the schemas should be enforced or not

Valid schema can be:
```
{ name: String }
{ name: { type: String } } // {name: "Kitty"} or { name: { type: "Kitty" } }?
{ name: { type: String, default: value/function }
{ name: { type: [String, Number, ...] }
{ age: { type: Number, min: ..., max: ...} }
{ comments: { type: Array, min: ..., max: ...} }
{ arrayOfStrings: [ String ] }
{ arrayOfStrings: [ String, maxLength, minLength ] }
```
Coming soon: default with object and arrays.


#### Model
__Model.compile(__ name, schema, settings, thinky __)__

_Internal method_



__Model.createBasedOnSchema(__ result, doc, originalDoc, enforce, prefix, schema __)__

_Internal method_



__Model.checkType(__ result, doc, originalDoc, schema, key, type, typeOf, prefix, enforce __)__

_Internal method_



__Model.define(__ key, method __)__
Define a method on the model that can be called by any instances of the model.



__Model.setSchema(__ schema __)__
Change the schema -- Not tested (I think)



__Model.getSettings(__  __)__
Return the settings of the model.



__Model.getDocument(__  __)__
Return the document.



__Model.getPrimaryKey(__  __)__
Return the primary key 



__Model.save(__ callback, overwrite  __)__
Save the object in the database. Thinky will call insert or update depending
on whether how the object was created.

overwrite: not implemented yet



__Model.get(__ id or [ids], callback __)__

Retrieve one or more documents



__Model.filter(__ filterFunction  __)__

Retrieve document based on the filter.



__Model.count(__  __)__

Return the number of element in the table of your model.



__Model.mapReduce(__ filterFunction  __)__

Not yet implemented

#### Document


__Document.getDocument(__  __)__

_Internal method?_



__Document.getModel(__  __)__

Return the model of the document.



__Document.getSettings(__  __)__



__Document.define(__ name, method  __)__



__Document.replace(__ newDoc  __)__
_Not implemented yet_


All method of EventEmitter are available on Document. They do not pollute the document itself.

### Internals
When you create a new object from a model, the object has the following chain of prototypes
object -> DocumentObject -> Document -> model

Run the tests

```
mocha
```

### Contribute
You are welcome to do a pull request.


### TODO
- Write the docs
- Add more complex queries
- Update pool when poolMax/poolMin changes

### About
Author: Michel Tu -- orphee@gmail.com -- www.justonepixel.com

### License
Copyright (c) 2013 Michel Tu <orphee@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this
software and associated documentation files (the 'Software'), to deal in the Software
without restriction, including without limitation the rights to use, copy, modify, merge,
publish, distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or
substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE
FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
DEALINGS IN THE SOFTWARE.
