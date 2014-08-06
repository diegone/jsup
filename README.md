# JSUP

JSUP is an extension to JSON (meaning that any valid JSON is also a valid JSUP document)
that tries simplify its syntax with JS-friendly constructs in order to be easier to author.
At the same time, it introduces some XML concepts so that JSUP can be used where XML would be preferable (e.g. markup).

## Logical model
Same as JS object literals with the following exceptions/extensions:
* order of keys in object is preserved
* objects can optionally specify a meta object that may include:
  * type (string)
  * annotations (object)
* objects can optionally specify constructor parameters (array)
* objects can optionally contain an implicit array of children

## Parser/Serializer

Similar to `JSON.parse()` and `JSON.sringify()`

Default reviver loses object order/type/meta/constructor and returns a plain Object just like a JSON parser would. However custom revivers can be plugged in to handle special processing.

##Sample

```
{
    // Single-line comment
    /* Multi-line
        comment */

    // JSON-like features
    "hi1": "hello", // backwards-compatible JSON
    hi2: "hello", // keys don't need quotes like in JS
    hi3: "hello" // colon is optional
    hi4: 'hello' // single quotes ok
    hi5:hello // quotes optional if id-like string

    array: [ 1.1, false, "foo", 'bar'] // like in JS

    nested: { // untyped object like in JS
      inside: true // property like in JS
    }

    // XML-like features
    typedObj1: foo() // like <foo/> in XML
    typedObj2: foo {} // like <foo/> in XML -- parenthesis can be omitted
    typedObj3: bar { param1: true } // like <bar param1="true"/> in XML
    typedObj4: bar(true) // like <bar param1="true"/> in XML, like a JS constructor function
    children1: foo(true) { hello } // like <foo param1="true">hello</foo> in XML
    children2: foo() { 'hello ' b { 'world' } } // like <foo>hello <b>world</b></foo> in XML
    children3: foo() { a:1 'hello ' b { 'world' } z:2 } // like <foo a="1" z="2">hello <b>world</b></foo> in XML
}
```
