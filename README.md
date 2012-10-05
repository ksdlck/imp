imp
===

Implicit type conversion for Javascript
---------------------------------------

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Imp should be both browser and node ready out of the box.
`lib/imp.js` uses [RequireJS](http://requirejs.org/) for browsers and the standard `require` with node, courtesy of [amdefine](https://github.com/jrburke/amdefine).

SYNOPSIS
--------

All functions are executed on an Imp context, which is a new instance of the single function the module exports.
Contexts expose two mechanisms for type-based conversion: `conv` based on an object and a destination type, and `wrap` that allows you to define a type signature and wrap one of your own functions, doing all the conversion for you.
The same mechanism for defining conversions is used for both: `def`.
Here's an example:

    /* create a new Imp context */
    var imp = new (require("imp"));

    var A = function(name) {
      this.name = name;
    };

    var B = function(name, birthday) {
      this.name = name;
      this.birthday = birthday;
    };

    /* define conversion from A to String */
    imp.def(A, String, function(a) {
      return a.name;
    });

    /* define conversion from B to String */
    imp.def(B, String, function(b) {
      return b.name + ":" + b.birthday;
    });

    var a = new A("Alice");
    var b = new B("Bob", "1970-01-01");

    /* prints "Alice" */
    console.log(imp.conv(a, String));

    /* prints "Bob:1970-01-01" */
    console.log(imp.conv(b, String));

    /* define a function that takes two Strings and prints them */
    var f = imp.wrap(String, String)(function(a, b){
      console.log(a);
      console.log(b);
    });

    /* prints "Alice\nBob:1970-01-01" */
    f(a, b);

There are two other functions you might need: `undef` lets you remove type conversions and `isDef` lets you check if they exist.
See the `examples/` for futher information.


WARNING
-------

Imp assumes you know what you're doing.
If you do not weild the dark power to control it, it may engage in mischief or worse.
`def` will overwrite existing definitions if the types match, and `undef` will continue silently even if there is no definition for the provided spec.
`isDef` is your ally in these cases, as it lets you manually detect either case if the former is not the desired behavior.
`wrap` may exhibit a variety of behavior should the provided signature not correspond well to its arguments, from strange null errors when conversions are not defined to dropped trailing arguments to other oddities.
Write your code well.


BUILDING
--------

Versions of this module released on NPM do not need to be built.
If you have cloned the repository and are not working in [Coco](https://github.com/satyr/coco/), you will need to get it (`npm install -g coco`) and build the module via Coke (`coke build`).


LICENSE
-------

Copyright (c) 2012 Karel Sedláček

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
