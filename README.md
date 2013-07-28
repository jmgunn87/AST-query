easy-AST
================

Tentative to a simple JavaScript AST modification library


Getting Started
================

First, you always need to create a `Tree` which you'll edit

``` javascript
var Tree = require("easy-AST");
var tree = new Tree("var a = 'foo'");
```

To get the source code back after modifying the AST, call the `toString` method on the
tree.

``` javascript
// ...
tree.var("a").value("'bar'");

console.log( tree.toString() );
// LOG: var a = 'bar';
```


API
================

Tree
----------------

### `new Tree( sourceCode )`
- **sourceCode**
- type: String
- The source code to edit.

### `tree.var( name )`
- **name**
- type: String
- The variable name
- Returns: A `variable` token. [See object methods](#variable-token)

### `tree.object()`
- Returns: An `object` literal token. [See object methods](#object-literal-token)


Variable token
-----------------

### `.value( value )`
- **value**
- type: String|function
- A string containing the new variable value or a function taking the current value as
a parameter and returning the new variable value

### `.rename( name )`
- **name**
- type: String
- Change the variable name

### `.replaceWith( value )`
- **value**
- type: String
- Replace the current variable declaration string with a new one. Note this method only
replace the string between the var declarations tokens, e.g. `var <value goes here>;`

### `.insertAfter( value )`
- **value**
- type: String
- Insert a string after the variable declaration code blocks. If there's multiple
declaration inside the code block, the string will be inserted after everyone.

### `.insertBefore( value )`
- **value**
- type: String
- Insert a string before the `var` keyword

### `.delete()`
- Delete the variable declaration. If the declaration block contain a single variable,
it'll delete it all, otherwise, it'll only delete the relevant section


Object literal token
--------------------

### `.assignedTo( name )`
- **name**
- type: String
- Only select object assigned to a `var`.

### `.passedTo( name )`
- **name**
- type: String
- Only select object passed as argument to a `function` or a `method`
- Known issue: Currently only work with objects nested two levels down. (PR welcomed)

Example:
```javascript
var tree = new Tree("grunt.init({ key: 'value' })");
tree.object().passedTo("grunt.init").key("key").value("'foo'");
console.log( tree.toString() );
```

### `.key( name )`
- **name**
- type: String
- A key name to select
- Returns: A `Property` token. [See object methods](#property-token)


Property token
------------------

### `.value( value )`
- **value**
- type: String|function
- A string containing the new property value or a function taking the current value as
a parameter and returning the new property value

### `.rename( name )`
- **name**
- type: String
- Change the variable property key

### `.delete()`
- Delete the property from the object.
