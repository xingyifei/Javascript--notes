#### 遇到的一些神奇的例子

```javascript
// js ----------------
function foo() {
  let a = b = 0;
  a++;
  return a;
}

foo();
typeof a; // => ???
typeof b; // => ???

-->>>>>
  
function foo() {
  let a;
  window.b = 0;
  a = window.b;
  a++;
  return a;
}

foo();
typeof a;        // => 'undefined'
typeof window.b; // => 'number'

```

```javascript
const length = 4;
const numbers = [];
for (var i = 0; i < length; i++);{
  numbers.push(i + 1);
}

numbers; // => ???

-->>>>>>

const length = 4;
const numbers = [];
var i;
for (i = 0; i < length; i++) {
  // does nothing
}
{ 
  // a simple block
  numbers.push(i + 1);
}

numbers; // => [5]
```

```javascript
function arrayFromValue(item) {
  return
    [item];
}

arrayFromValue(10); // => ???

-->>>>>
  
function arrayFromValue(item) {
  return;
  [item];
}

arrayFromValue(10); // => undefined
```

