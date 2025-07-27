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

  ✅ Callbacks, Promises & async/await (Complete Notes)

1. CALLBACKS
   - A callback is a function passed as an argument to another function.
   Example:
     function greet(name, cb) {
       console.log("Hello " + name);
       cb();
     }
     greet("Ram", () => console.log("Done!"));
   - Problem: "Callback Hell" → nested callbacks, hard to read.
     setTimeout(()=> {
       console.log("Step 1");
       setTimeout(()=> {
         console.log("Step 2");
       }, 1000);
     }, 1000);

2. PROMISES
   - Promise = future value (Pending → Fulfilled/Rejected).
   - Create:
       const promise = new Promise((resolve, reject) => {
         let success = true;
         if(success) resolve("✅ Success!");
         else reject("❌ Error!");
       });
   - Consume:
       promise
         .then(result => console.log(result))
         .catch(err => console.log(err))
         .finally(() => console.log("Always runs"));

3. PROMISE CHAINING
   - Chain .then() for sequential async work.
     Promise.resolve(1)
       .then(x => x + 1)   // 2
       .then(x => x * 2)   // 4
       .then(console.log); // 4

4. PROMISE METHODS
   - Promise.all([p1,p2])
       Waits for all → resolves with array, rejects if any fail.
       Promise.all([Promise.resolve("A"), Promise.resolve("B")])
         .then(console.log); // ["A","B"]
   - Promise.race([p1,p2])
       Resolves/rejects with the FIRST finished.
       Promise.race([
         new Promise(res=>setTimeout(()=>res("Fast"),100)),
         new Promise(res=>setTimeout(()=>res("Slow"),500))
       ]).then(console.log); // "Fast"
   - Promise.allSettled([p1,p2])
       Always resolves with status/value for each.
       Promise.allSettled([
         Promise.resolve("OK"),
         Promise.reject("Fail")
       ]).then(console.log);
       // [{status:"fulfilled",value:"OK"},{status:"rejected",reason:"Fail"}]
   - Promise.any([p1,p2])
       Resolves with first SUCCESS, rejects only if all fail.
       Promise.any([
         Promise.reject("Fail1"),
         Promise.resolve("Win!"),
         Promise.reject("Fail2")
       ]).then(console.log); // "Win!"

5. MICROTASK vs MACROTASK
   - Promise callbacks → microtask (higher priority)
   - setTimeout/setInterval → macrotask (lower priority)
   Example:
     console.log("A");
     setTimeout(()=>console.log("B"),0);     // macrotask
     Promise.resolve().then(()=>console.log("C")); // microtask
     console.log("D");
     // Output: A, D, C, B

6. ASYNC / AWAIT
   - async fn → always returns a Promise.
   - await → pauses inside async fn until Promise resolves.
   Example:
     async function test(){
       console.log("1");
       await Promise.resolve();
       console.log("2");
     }
     test();
     console.log("3");
     // Output: 1,3,2

7. ERROR HANDLING with async/await
   async function getData(){
     try {
       let res = await Promise.reject("Error!");
       console.log(res);
     } catch(err){
       console.log(err); // "Error!"
     }
   }
   getData();

8. async/await with Promise.all
   async function fetchAll(){
     const [a,b] = await Promise.all([
       Promise.resolve("A"),
       Promise.resolve("B")
     ]);
     console.log(a,b); // A B
   }
   fetchAll();

9. CALLBACK → PROMISE → ASYNC/AWAIT
   - Callbacks cause "callback hell".
   - Promises flatten the structure using .then().
   - async/await makes it look synchronous.

10. QUICK SUMMARY
    - Callback → function passed to handle async result.
    - Promise → better way, avoids callback hell.
    - then/catch/finally → handle result/errors.
    - Promise.all/race/any/allSettled → handle multiple promises.
    - async/await → cleaner syntax, still uses Promises.
    - Always wrap async/await in try/catch for errors.
   
      
✅ DOM & Event Handling

1. DOM:
   - DOM = Document Object Model, tree structure of HTML.
   - JS can select & modify elements.
   - Selection:
       document.getElementById("id")
       document.querySelector(".class")
       document.querySelectorAll("div")

2. Modifying DOM:
   const el = document.querySelector("#demo");
   el.textContent = "New Text";
   el.style.color = "red";
   const newDiv = document.createElement("div");
   document.body.appendChild(newDiv);

3. Event Handling:
   - Add events using addEventListener:
       btn.addEventListener("click", ()=>console.log("Clicked!"));

4. Event Propagation:
   - Phases: Capturing → Target → Bubbling.
   - addEventListener("click", fn, true) → capturing
   - addEventListener("click", fn, false) → bubbling (default)

5. Event Delegation:
   - Add listener to parent to handle child events.
   parent.addEventListener("click", e => {
     if(e.target.matches(".child")) console.log("Child clicked");
   });

6. preventDefault vs stopPropagation:
   - preventDefault() → stops browser’s default action.
     Example: stop link navigation or form reload.
   - stopPropagation() → stops event from bubbling to parent.

7. Examples:
   - Link:
       link.addEventListener("click", e=>{
         e.preventDefault(); // stops opening link
       });
   - Form:
       form.addEventListener("submit", e=>{
         e.preventDefault(); // stops page reload
       });

✅ Copy Concepts in JavaScript

1. Reference Assignment (No Copy)
   let a = [1,2,3];
   let b = a; // same reference
   b.push(4);
   console.log(a); // [1,2,3,4] ❌ affected

2. Shallow Copy
   - Copies only 1st level, nested objects still shared.
   - Arrays:
       let arr = [1,2,3];
       let copy1 = [...arr];
       let copy2 = arr.slice();
   - Objects:
       let obj = {a:1,b:2};
       let copy = {...obj};
   - Problem:
       let obj = {a:1,nested:{x:10}};
       let shallow = {...obj};
       shallow.nested.x = 99;
       console.log(obj.nested.x); // 99 ❌

3. Deep Copy
   - Fully independent copy including nested.
   - Methods:
       ✅ JSON:
         let deep = JSON.parse(JSON.stringify(obj));
       ✅ structuredClone (modern):
         let deep = structuredClone(obj);
       ✅ lodash:
         let deep = _.cloneDeep(obj);
   - Example:
       let obj = {a:1,nested:{x:10}};
       let deep = structuredClone(obj);
       deep.nested.x = 99;
       console.log(obj.nested.x); // 10 ✅

4. Quick Comparison
   - b=a → same reference
   - Shallow copy → copies top level only, nested shared
   - Deep copy → completely new independent copy

5. Full Example
   let original = { name:"Ram", skills:["JS"], details:{age:25} };

   let ref = original;
   ref.name="Sham";
   console.log(original.name); // Sham ❌

   let shallow = {...original};
   shallow.details.age = 30;
   console.log(original.details.age); // 30 ❌

   let deep = structuredClone(original);
   deep.details.age = 40;
   console.log(original.details.age); // 30 ✅

   ✅ Looping Through Arrays in JavaScript

1. Classic for loop
   let arr = [10,20,30];
   for(let i=0; i<arr.length; i++){
     console.log(arr[i]);
   }
   ✅ Use when you need index & control.

2. for...of
   for(let val of arr){
     console.log(val);
   }
   ✅ Simple, just values.

3. forEach()
   arr.forEach((val, idx) => console.log(idx, val));
   ✅ Easy, but cannot break/continue.

4. map()
   let doubled = arr.map(x => x*2);
   ✅ Returns new transformed array.

5. filter()
   let evens = arr.filter(x => x%2===0);
   ✅ Returns subset of array.

6. reduce()
   let sum = arr.reduce((acc,cur)=>acc+cur,0);
   ✅ Reduces array to a single value.

7. find() & findIndex()
   arr.find(x=>x>10);      // first match
   arr.findIndex(x=>x>10); // index of first match

8. some() & every()
   arr.some(x=>x>10);  // true if any matches
   arr.every(x=>x>0);  // true if all match

9. for...in (not for arrays)
   for(let key in arr){ console.log(key, arr[key]); }
   ⚠️ Best for objects, not arrays.

10. while loop
    let i=0;
    while(i<arr.length){ console.log(arr[i]); i++; }

11. Array.from()
    - Converts iterable or array-like to array.
    let str = "abc";
    console.log(Array.from(str)); // ['a','b','c']

    - Can also map:
    console.log(Array.from([1,2,3], x=>x*2)); // [2,4,6]




