-JavaScript is a high-level, interpreted programming language primarily used to create interactive and dynamic content on web pages.
-It enables developers to implement complex features such as real-time updates, interactive forms, animations, and more.
-Unlike HTML and CSS, which structure and style web content respectively, JavaScript adds behavior to web pages, allowing for user engagement and dynamic content updates without requiring a page reload.

JavaScript Data Types & Type Conversion

1. Primitive Types (Immutable)
   - string, number, boolean, null, undefined, symbol, bigint
   - Stored by value, not by reference
   - Immutable → cannot change the value directly
   - Example:
     let a = "Hello";
     a[0] = "J";  // ❌ won't change
     a = "Hi";    // ✅ new value created

2. Non-Primitive Types (Mutable)
   - Objects, arrays, functions
   - Stored by reference
   - Mutable → can modify in place
   - Example:
     let obj = {name:"Ram"};
     let ref = obj;
     ref.name = "Shyam";
     console.log(obj.name); // "Shyam"

3. typeof operator quirks
   - typeof null → "object" (JS bug)
   - typeof NaN → "number"
   - typeof [] → "object"
   - typeof function(){} → "function"

4. Type Coercion (Conversion)
   - Implicit (automatic)
     '5' + 2   // "52"  (string concatenation)
     '5' - 2   // 3     (string → number)
     true + 1  // 2
     null + 1  // 1
     undefined + 1 // NaN

   - Explicit (manual)
     Number("42")  // 42
     String(42)    // "42"
     Boolean(0)    // false

5. Falsy values
   - false, 0, "", null, undefined, NaN
   - Everything else is truthy

6. == vs ===
   - == → loose equality (does type conversion)
   - === → strict equality (no conversion)
     0 == false   // true
     0 === false  // false
     '5' == 5     // true
     '5' === 5    // false
     null == undefined  // true
     null === undefined // false

7. Special Numbers
   - NaN === NaN → false
   - Number.isNaN(NaN) → true
   - Infinity > 999999 → true

8. Primitive vs Object Copying
   let x = 10;
   let y = x;
   y = 20;
   console.log(x); // 10 (value copy)

   let obj1 = {a:1};
   let obj2 = obj1;
   obj2.a = 2;
   console.log(obj1.a); // 2 (reference copy)

✅ JavaScript 'this' & call/apply/bind

1. What is 'this'?
   - 'this' refers to the object that is executing the current function.
   - Its value depends on HOW the function is called.

2. 'this' in different contexts:
   - Global scope:
       console.log(this); // window (browser), undefined in strict mode
   - Inside object method:
       const obj = { name: "Ram", sayHi(){ console.log(this.name); } };
       obj.sayHi(); // Ram (this → obj)
   - Standalone function:
       function test(){ console.log(this); }
       test(); // window (non-strict), undefined (strict)
   - Arrow functions:
       const obj = { name: "Ram", sayHi: ()=>console.log(this.name) };
       obj.sayHi(); // undefined (inherits from global)
   - Constructor function:
       function Person(name){ this.name = name; }
       const p = new Person("Ram");
       console.log(p.name); // Ram

3. Changing 'this':
   - call() → calls function immediately with custom this
       greet.call(user, "Hello");
   - apply() → same as call, but args as array
       greet.apply(user, ["Hi"]);
   - bind() → returns a new function with fixed this
       const bound = greet.bind(user, "Hey");
       bound(); // Hey Ram

✅ Event Loop, Microtask & Macrotask

1. Event Loop:
   - JS is single-threaded → one Call Stack.
   - Async tasks go to Web APIs, then their callbacks wait in queues.
   - Event Loop pushes queued tasks to Call Stack when it's free.

2. Two types of task queues:
   a) Microtask Queue (high priority)
      - Runs right after current sync code, before macrotasks.
      - Examples:
          - Promise.then(), Promise.catch()
          - queueMicrotask()
          - MutationObserver

   b) Macrotask Queue (lower priority)
      - Runs after all microtasks finish.
      - Examples:
          - setTimeout(), setInterval()
          - setImmediate() (Node.js)
          - I/O events (network, file)
          - UI events (click, scroll)

3. Example - Microtask vs Macrotask:
   console.log("1");
   setTimeout(()=>console.log("2"),0);      // macrotask
   Promise.resolve().then(()=>console.log("3")); // microtask
   console.log("4");
   Output:
     1 (sync)
     4 (sync)
     3 (microtask)
     2 (macrotask)

4. Execution Priority:
   1. Synchronous code (Call Stack)
   2. Microtasks (Promises, queueMicrotask)
   3. Macrotasks (setTimeout, setInterval)

Quick:
- Microtasks run before macrotasks.
- Promises > setTimeout in priority.

