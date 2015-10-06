# JavaScript: an unloved language

## History

Created in 1996 by Brendan Eich working at Netscape, at the time leader of innovation in the browser domain.

Made for small scripts to run in the browser, like form validation or on click action.

Should have been named LiveScript but popularity of the rising Java led to JavaScript (two capitals!) for marketing purpose.

That, and superficial similarity with Java syntax lead to confusion where people think that one is an abbreviation of the other!

## Presentation

Normalised as ECMAScript, ECMA-262.

Syntax is similar to Java (C-like), but semantic (functions) is close of Lisp / Scheme, prototypal inheritance comes from Self and regular expressions from Perl.

Dynamically and weakly typed. Type of variables isn't declared and we can assign them values of varying type.

Object-oriented but not based on classes, based on prototypes.

Functions are first-class citizens.

Not limited to the browser: Java has a built-in interpreter, Node.js is a server-side Web framework, etc.

## Why no love?

Mostly from users coming from Java or C#, not using idiosyncrasies of JS.

Dynamic typing imply runtime errors instead of compilation errors. And poor tooling.

No classes, prototype inheritance not known nor understood.

Language has weaknesses, pitfalls and dark sides.

### Things of the past

Browser implementations were unequal, and inconsistent.

JavaScript speed was awful. It is much improved now.

## Types

Variables are not typed, values are (but can have implicit conversions).

Numbers are 64-bit floating point values. No true integers.

Strings are made of 16-bit Unicode characters. No char type. Delimited by `"` or `'`. `\` escape, `\uHHHH` notation. Immutables. Concatenation with `+`, comparison with `==`.

Boolean type but lot of values are implicitly `false` (`0`, `""`, `null`, `undefined`, `NaN`), all others are `true`.

`null` and `undefined` are types with only one possible value.

Objects are everything else...

## Statements

Variables can be used without being declared and are then global!

The `var` statement defines a private variable whose scope is the current function.

The soft braces `{ }` don't define a scope!

Classical conditional, looping and disruptive statements.

## Expressions

An expression statement can assign a value to one or more variables, can call a function or can delete a property from an object.

## Operators

JavaScript has the classical Java operators, including bitwise ones (coercing numbers to integers).

The `typeof` operator produces a string: one of 'number', 'string', 'boolean', 'undefined', 'function' and 'object'.

Invocation `()` executes a function, optionally with parameters.

Refinement `[]` specifies a property or element of an object or array.

## Objects

Object is the only mutable type. Objects are keyed collections, containers of properties, ie. name and value pairs.

Arrays, functions and regular expressions are objects.

A property name can be any string.
A property value can be any value, except `undefined`.

So called global variables are actually properties of the global object, named `window` in a browser.

### Object literals

An object literal is made of curly braces around a comma-separated list of name-value pairs (pair separated by a colon).

    var empty = {}
    var somebody = { "given name": "Arthur", name: "Rambo", age: 27,
      'skin color': { r: 102, g: 55, b: 27 } };

Quotes around the name can be omitted if the name is a valid identifier (not a keyword).

A value can be another object literal, literal declarations can be nested.

### Retrieval

To get the value of a property, use the notation `o.propertyName` if the name is a legal identifier, or `o['property name']` in all cases.

Return value is `undefined` if the name is unknown. Retrieving a value from `undefined` throws a TypeError exception.

    // Get value with default
    var speed = ennemy.speed || 0;
    // Guard against TypeError
    var posX = enemy.coordinates && enemy.coordinates.x;

### Update

Just assign the value. If the property doesn't exist, it is created.

Objects are passed around by reference, not by copy.

### Prototype

Every object is linked to a prototype object from which it inherits properties. By default they inherit from `Object.prototype`, built in JavaScript.
When we try to retrieve a property value from an object and it is not found, JavaScript looks in the prototype object. It goes up in the chain until it finds the property, up to `Object. prototype`. If not found, the result is undefined. That's delegation.

A new property in a prototype is immediately visible in all objects based on that prototype.

### Reflection

`typeof`  operator allows to find out the type of a property.

`hasOwnProperty` method tells if a property is defined in the object itself or is inherited.

### Enumeration and deletion

`for in` loop enumerates properties from the whole inheritance chain.

`delete` allows to remove a property from an object: `delete foo.obsolete`.

## Functions





## Stuff

### return

If not explicit, a function returns `undefined`. If a function is invoked as a constructor, the implicit return value is `this`, the constructed object.

### Exceptions

Can be thrown and caught. Can be any object.

    if (bad) throw { errorCode: 405, message: "You goofed!" }
