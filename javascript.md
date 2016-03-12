# Javascript style guide

This won't be a complete guide to every case but should illustrate some common patterns.

## General style tips

<!-- language: lang-js -->
```
/**
 * This is my function that does something. #1
 *
 * @param {Function} arg1 Description
 * @param {number} arg2 Description
 * @param {object} arg3 Description
 */
function namedFunction(arg1, arg2, arg3) {
    var myVar; // #2 #4
    var name = "abc"; // # 10
    var i = 0;
    
    // A single line comment about this: #3
    return arg1(i)
        .then(function (x) { // #5
            // Another single line comment #6
            return x + arg2;
        })
        .finally(function () {
            jumpUpAndDown();
        })
    ; // #7
}

largeSignature( // #8
    iHaveGot,
    lotsOf,
    arguments,
    Math.min(confusion),
    function (withNew) {
        return withNew + 1;
    },
    lines
);

if (sunny === true) { // #9 #11
    takeOffCoat();
}

var abc = janek === null // #12
    ? "pozniack"
    : "sham shirt"
;

```

From this you can see:

1. [Use jsDoc where possible](http://usejsdoc.org/) to document functions.

2. Indent using 4 spaces.

3. Leave a space above single line comments, unless at the beginning of a block #6.

4. Declare variables at the top of a function where possible, each with a separate declaration.

5. Use a new line and indent for chained object properties to give the program flow more clarity.

6. (see 3)

7. Always use semicolons to avoid ambiguity, and balance the indentation of the semicolon with the start of the
expression. This makes it easier to continue a chained expression without editing the end of the line each time.

8. If a line or function call is going over 120 chars you should rewrite it with one argument or parameter per line
so that it is still readable and doesn't disappear to the right.

9. Always use curly brackets `{}` for if statements and for loops. It's less error prone and clearer to read.

10. Prefer "double quotes" for strings over 'single quotes'.

11. Always use a `===` / `!==` equality check rather than `==` / `!=` to avoid potential bugs.

12. Ternary expressions are good, but separate and indent over two lines if this improves clarity.


## Other best practice

1. Avoid mutable state. In other words, if you assign a 'variable' avoid changing it, because this will add complexity
to your code.

    <!-- language: lang-js -->
    ```
    // avoid
    var a = 5;
    a = 20;

    // prefer
    var a = 5;
    var a2 = 20;

    // even better (stupid example)
    var a = 25;

    ```

2. Prefer iterating over an array as `[...].forEach(fn)` rather than as a `for () {}` loop. (Or `.map()`, or
`.filter()`)

    <!-- language: lang-js -->
    ```
    // avoid
    for (var i = 0; i < xs.length; i++) {
        conbobulate(xs[i]);
    }

    // prefer (much cleaner)
    xs.forEach(function (x) {
        conbobulate(x);
    });
    ```

3. Prefer a functional style of code over side-effects. In other words, try to write functions that return a value
instead of a `void` function that has a side-effect. If you need a side-effect, consider injecting that as a separate
function:

    <!-- language: lang-js -->
    ```
    // this has limited functionality:
    function printReport1(data) {
        console.log("Year report");
        console.log("Data 1", data[0]);
        console.log("Data 2", data[1]);
    }

    // more flexible:
    function printReport2(data, output) {
        output("Year report");
        output("Data 1", data[0]);
        output("Data 2", data[1]);
    }

    // a stupid example, but returning a value instead:
    function prepareReport3(data, output) {
        return [
            ["Year report"],
            ["Data 1", data[0]],
            ["Data 2", data[1]],
        ];
    }
    ```

4. Avoid negations adding unnecessary complexity.

    <!-- language: lang-js -->
    ```
    // avoid
    var x = y !== null
        ? "food"
        : "bard"
    ;

    // prefer (easier not to think about the negative if expression can be reversed)
    var x = y === null
        ? "bard"
        : "food"
    ;
    ```

## Naming things

Use `camelCase` for all variable and function names.

If you have an abbreviation as part of the name avoid using all caps:

<!-- language: lang-js -->
```
// avoid
getSQLQuery();
carID = 12345;

// prefer
getSqlQuery();
carId = 12345;
```

This choice is mainly because it is a pain in the ass to type rows of capitals. But also we want to separate the words
clearly. In `getSQLQuery`, "SQL" seems to merge into "Query".

If you have a function constructor (you will use the function with `new`) it should start with a capital letter:

<!-- language: lang-js -->
```
function Car(make, engine) {
    this.make = make;
    this.engine = engine;
}
Car.prototype.drive = function () {
    return engine.start();
};

var car1 = new Car("Ford Mustang", petrolV8);
```

If you have short functions where you operate on one value, use `x` as a default name:

<!-- language: lang-js -->
```
[1, 2, 3]
    .map(function (x) {
        return x + 1;
    })
;
```

Other good defaults are `i` for an index or integer count, `row` if you are iterating over rows of items, or `result` 
for the result of an operation.

Otherwise, use longer descriptive names:

<!-- language: lang-js -->
```
var contractMileageTotal = leaseContract.replacementCycle * leaseContract.contractMileageAnnual;
var paymentProfile = leaseContract.paymentProfile.join(":");
```


## EcmaScript 5 (ES5)

For browsers, we don't support IE8 unless this is a special requirement for a project so we can use ES5 features, such as:

<!-- language: lang-js -->
```
[0, 1, 2, 3, 4, 5]
    .filter(function (x) {
        return x % 2 === 0;
    })
;

Object.defineProperty(...);
```


## EcmaScript 6 (ES6)

For node, we aim to target node version 4.x (Argon), which allows many useful ES6 features. In particular:

<!-- language: lang-js -->
```
// arrow function syntax (with lexical binding of this)
// this really helps make more concise, readable code
[0, 1, 2].map(x => x + 1);

// const for "variables" that shouldn't change once assigned:
const fs = require("fs");
```

