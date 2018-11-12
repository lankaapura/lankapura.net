---
title: "New ECMAScript 6 Features"
header:
  og_image: https://i.imgur.com/4qMyMzd.jpg
---

Following are the list of new features available in ECMAScript 6.

* Block-Scoped Variables (Let and Const)

  ```javascript

  // ES6
  for (let i = 0; i < a.length; i++) {
      let x = a[i]
      …
  }

  // ES5
  var i, x, y;
  for (i = 0; i < a.length; i++) {
      x = a[i];
      …
  }
  ```

* Parameter Handling

  * Default
    ```javascript

    // ES6
    function f (x, y = 7, z = 42) {
        return x + y + z
    }
    f(1);

    // ES5
    function f (x, y, z) {
        if (y === undefined)
            y = 7;
        if (z === undefined)
            z = 42;
        return x + y + z;
    };
    f(1);
    ```
    
  * Spread
  
    ```javascript

    // ES6
    var params = [ "hello", true, 7 ]
    var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

    function f (x, y, ...a) {
        return (x + y) * a.length
    }
    f(1, 2, ...params)

    // ES5
    var params = [ "hello", true, 7 ];
    var other = [ 1, 2 ].concat(params); // [ 1, 2, "hello", true, 7 ]

    function f (x, y) {
        var a = Array.prototype.slice.call(arguments, 2);
        return (x + y) * a.length;
    };
    f.apply(undefined, [ 1, 2 ].concat(params));

    ```
    
  - Rest
  
    ```javascript

    // ES6
    function f (x, y, ...a) {
        return (x + y) * a.length
    }
    f(1, 2, "hello", true, 7)

    // ES5
    function f (x, y) {
        var a = Array.prototype.slice.call(arguments, 2);
        return (x + y) * a.length;
    };
    f(1, 2, "hello", true, 7);
    ```
    
* Destructuring Assignment

  * Array Matching
  
    ```javascript

    // ES6
    var list = [ 1, 2, 3 ]
    var [ a, , b ] = list
    [ b, a ] = [ a, b ]

    // ES5
    var list = [ 1, 2, 3 ];
    var a = list[0], b = list[2];
    var tmp = a; a = b; b = tmp;
    ```
  * Object Matching
 
    ```javascript

    // ES6
    var { op, lhs, rhs } = getASTNode()
    var { op: a, lhs: { op: b }, rhs: c } = getASTNode()

    // ES5
    var tmp = getASTNode();
    var op  = tmp.op;
    var lhs = tmp.lhs;
    var rhs = tmp.rhs;

    var tmp = getASTNode();
    var a = tmp.op;
    var b = tmp.lhs.op;
    var c = tmp.rhs;
    ```
  * Object And Array Matching, Default Values

    ```javascript

    // ES6
    var obj = { a: 1 }
    var list = [ 1 ]
    var { a, b = 2 } = obj
    var [ x, y = 2 ] = list

    // ES5
    var obj = { a: 1 };
    var list = [ 1 ];
    var a = obj.a;
    var b = obj.b === undefined ? 2 : obj.b;
    var x = list[0];
    var y = list[1] === undefined ? 2 : list[1];
    ```
    
  * Parameter Context Matching

    ```javascript

    // ES6
    function f ([ name, val ]) {
        console.log(name, val)
    }
    function g ({ name: n, val: v }) {
        console.log(n, v)
    }
    function h ({ name, val }) {
        console.log(name, val)
    }
    f([ "bar", 42 ])
    g({ name: "foo", val:  7 })
    h({ name: "bar", val: 42 })

    // ES5
    function f (arg) {
        var name = arg[0];
        var val  = arg[1];
        console.log(name, val);
    };
    function g (arg) {
        var n = arg.name;
        var v = arg.val;
        console.log(n, v);
    };
    function h (arg) {
        var name = arg.name;
        var val  = arg.val;
        console.log(name, val);
    };
    f([ "bar", 42 ]);
    g({ name: "foo", val:  7 });
    h({ name: "bar", val: 42 });
    ```

  * Fail-Soft Destructuring

    ```javascript

    // ES6
    var list = [ 7, 42 ]
    var [ a = 1, b = 2, c = 3, d ] = list
    a === 7
    b === 42
    c === 3
    d === undefined

    // ES5
    var list = [ 7, 42 ];
    var a = typeof list[0] !== "undefined" ? list[0] : 1;
    var b = typeof list[1] !== "undefined" ? list[1] : 2;
    var c = typeof list[2] !== "undefined" ? list[2] : 3;
    var d = typeof list[3] !== "undefined" ? list[3] : undefined;
    a === 7;
    b === 42;
    c === 3;
    d === undefined;
    ```
 
* Arrow Functions

  * Expression Bodies
  
    ```javascript

    // ES6
    odds  = evens.map(v => v + 1)
    pairs = evens.map(v => ({ even: v, odd: v + 1 }))
    nums  = evens.map((v, i) => v + i)

    // ES5
    odds  = evens.map(function (v) { return v + 1; });
    pairs = evens.map(function (v) { return { even: v, odd: v + 1 }; });
    nums  = evens.map(function (v, i) { return v + i; });
    ```
    
  * Statement Bodies

    ```javascript

    // ES6
    nums.forEach(v => {
       if (v % 5 === 0)
           fives.push(v)
    })

    // ES5
    nums.forEach(function (v) {
       if (v % 5 === 0)
           fives.push(v);
    });
    ```
    
  * Lexical this

    ```javascript

    // ES6
    this.nums.forEach((v) => {
        if (v % 5 === 0)
            this.fives.push(v)
    })

    // ES5
    var self = this;
    this.nums.forEach(function (v) {
        if (v % 5 === 0)
            self.fives.push(v);
    });

    ```

* Template Literals

  * String Interpolation

    ```javascript

    // ES6
    var customer = { name: "Foo" }
    var card = { amount: 7, product: "Bar", unitprice: 42 }
    var message = `Hello ${customer.name},
    want to buy ${card.amount} ${card.product} for
    a total of ${card.amount * card.unitprice} bucks?`

    // ES5
    var customer = { name: "Foo" };
    var card = { amount: 7, product: "Bar", unitprice: 42 };
    var message = "Hello " + customer.name + ",\n" +
    "want to buy " + card.amount + " " + card.product + " for\n" +
    "a total of " + (card.amount * card.unitprice) + " bucks?";
    ```

  * Custom Interpolation
  
    ```javascript

    // ES6
    get`http://example.com/foo?bar=${bar + baz}&quux=${quux}`

    // ES5
    get([ "http://example.com/foo?bar=", "&quux=", "" ],bar + baz, quux);

    ```

* Classes

  * Class Definition

    ```javascript

    // ES6
    class Shape {
        constructor (id, x, y) {
            this.id = id
            this.move(x, y)
        }
        move (x, y) {
            this.x = x
            this.y = y
        }
    }

    // ES5
    var Shape = function (id, x, y) {
        this.id = id;
        this.move(x, y);
    };
    Shape.prototype.move = function (x, y) {
        this.x = x;
        this.y = y;
    };
    ```

  * Class Inheritance

    ```javascript

    // ES6
    class Rectangle extends Shape {
        constructor (id, x, y, width, height) {
            super(id, x, y)
            this.width  = width
            this.height = height
        }
    }
    class Circle extends Shape {
        constructor (id, x, y, radius) {
            super(id, x, y)
            this.radius = radius
        }
    }

    // ES5
    var Rectangle = function (id, x, y, width, height) {
        Shape.call(this, id, x, y);
        this.width  = width;
        this.height = height;
    };
    Rectangle.prototype = Object.create(Shape.prototype);
    Rectangle.prototype.constructor = Rectangle;
    var Circle = function (id, x, y, radius) {
        Shape.call(this, id, x, y);
        this.radius = radius;
    };
    Circle.prototype = Object.create(Shape.prototype);
    Circle.prototype.constructor = Circle;
    ```

* Promises

  ```javascript

  // ES6
  function msgAfterTimeout (msg, who, timeout) {
      return new Promise((resolve, reject) => {
          setTimeout(() => resolve(`${msg} Hello ${who}!`), timeout)
      })
  }
  msgAfterTimeout("", "Foo", 100).then((msg) =>
      msgAfterTimeout(msg, "Bar", 200)
  ).then((msg) => {
      console.log(`done after 300ms:${msg}`)
  })

  // ES5
  function msgAfterTimeout (msg, who, timeout, onDone) {
      setTimeout(function () {
          onDone(msg + " Hello " + who + "!");
      }, timeout);
  }
  msgAfterTimeout("", "Foo", 100, function (msg) {
      msgAfterTimeout(msg, "Bar", 200, function (msg) {
          console.log("done after 300ms:" + msg);
      });
  });
  ```
  
  Code samples from [es6-features.org](http://es6-features.org/).
