# Javascript-cheat-sheet

## ES6

### SCOPE
* Block scope
```javascript
// limit in curly {}
// const, let has block scope
{
  const name = 'PHUOC';
}
console.log(name); // error: name is not defined
```

* Function scope
```javascript
function sayHello() {
  const name = 'PHUOC';
  console.log(name); // 'PHUOC'
}
sayHello();
console.log(name); // fName is not defined
```

* Lexical scope
```javascript
const name = 'PHUOC';

function sayHello() {
  const a = 1;
  console.log(name); // the lexical scope of name is global scope

  function a() {
    console.log(a); // the lexical scope of a is a function scope (sayHello)
  }
}
```
* Global scope
```javascript
// Global object in different environment
// In Browser: window
// In NodeJS: global
// In Worker: self
// globalThis depends on environment
```

* Scope chain
```javascript
// Khi giải quyết một biến, JS bắt đầu với scope bên trong, sau đó tìm kiếm dần mở rộng ra bên ngoài các biến/object/function cho đến khi chúng được tìm thấy.
```

### HOISTING
```javascript
// var, function declaration: hoisting
// const/let: hoisting but prevent by TEMPORAL DEAD ZONE
// function expression: no hoisting
```

### IIFE - IMMEDIATELY INVOKED FUNCTION EXPRESSION
```javascript
(function sayHello() {
  console.log('IIFE');
})();

(() => {
  console.log('IIFE');
})();
```

### CLOSURES
#### A closures is a function **having access to the parent scope**, **even after the parent function has closed**.

```javascript
function init() {
  let name = 'PHUOC';

  function showName() { // the inner function, a closure
    alert(name);
  }
  
  showName();
}
init();
```

```javascript
function createCounter(initValue) {
  let value = initValue || 0; // private variable

  function increase() {
    value++;
  }
  function decrease() {
    value--;
  }
  function getValue() {
    console.log(value);
    return value;
  }

  return {
    getValue,
    increase,
    decrease,
  };

const counter = createCounter();
counter.getValue(); // 0
counter.increase();
counter.increase();
counter.getValue(); // 2
counter.decrease();
counter.getValue(); // 1
console.log(counter.value); // undefined
}
```

### FUNCTION
```javascript
// Function types
function sayHello() {}; // function declaration
const sayHello = function() {}; // function expression
const sayHello = () => {}; // arrow function
```

```javascript
// Default parameter
function sum(a = 5, b = 1) {
  return a + b;
}

sum(); // = 6
sum(4,3) // = 7
sum(undefined, undefined) // = 6 
sum(null, 2) // = 2 Number(null) = 0
```

```javascript
// Rest parameter
function sum(...numberList) {
  return numberList.reduce((total, number) => total + number, 0);
}

sum(1); // = 1
sum(1, 2, 3); // = 6

// Spread operator
const numberList = [1, 2, 3];
sum(...numberList); // 6
```

```javascript
// Curry function
// generate increase id 
function createIdGenerator(startId = 1) {
  let id = startId;

  return function () { // A closure 
    return id++;
  };
}
const getNextId = createIdGenerator(10);
getNextId(); // 10
getNextId(); // 11
getNextId(); // 12
```

### ENHANCED OBJECT PROPS 
```javascript
// Computed property name
const key = 'Power';
const student = {
  id: 1,
  name: 'Thanh Phuoc',
  'hero type': 'iron man', // key with spaces

  [key]: 50,
  [`get${key}`]: function () {
    return 100;
  },
};
student.id; // 1
student.Power; // 50
student.hero type; // syntax error
student['hero type']; // 'iron man'
student.Power; // 50
student[key]; // 50
student.getPower(); // 100
```

```javascript
// Method property
const student = {
  sayHello: function () {
    console.log('Thanh Phuoc');
  }, // ES5
  sayHello() {
    console.log('Thanh Phuoc');
  }, // ES6
};
```

```javascript
// Destructuring
// object destructuring
const student = {
 id: 1,
 name: 'Thanh Phuoc',
}
const { id, name } = student;

// array destructuring
const numberList = [5, 10, 15];
const [a, b] = numberList; // a = 5 , b = 10

// swap
let x = 10;
let y = 20;
[y, x] = [x, y]; // swap
console.log(x); // 20
console.log(y); // 10

// rename prop
// destructuring value
const student = {
 id: 1,
 name: 'Thanh Phuoc',
}
const { id: studentId, name, age = 18 } = student;
console.log(studentId); // 1
console.log(age); // 18
console.log(id); // ReferenceError: id is not defined
```

### THIS
![alt](https://scontent.xx.fbcdn.net/v/t1.15752-9/276025514_4839922316120730_3965096238014186140_n.png?stp=dst-png_s640x640&_nc_cat=111&ccb=1-5&_nc_sid=aee45a&_nc_ohc=hiwevZ87L8YAX85cPgP&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AVLAM2AX7rAWxAMpsx-iC55xIZrWpc5SYxJFSDi9OEpeEQ&oe=626D287E)

## DATA TYPES

* STRING
```javascript
'THANH PHUOC'.length(); // 11
'PHUOC'.charAt(0); === string[0]; // P

'PHUOC'.padStart(7, '*'); // '**PHUOC'
'PHUOC'.padEnd(7, '*'); // 'PHUOC**'

'PHUOC'.repeat(3); // 'PHUOCPHUOCPHUOC'

'    PHUOC    '.trim(); // 'PHUOC'

'phuoc'.toUpperCase(); // 'PHUOC'
'PHUOC'.toLowerCase(); // 'phuoc'

'phuoco'.indexOf('o'); // 3
'phuoco'.lastIndexOf('o'); // 5

const fullName = 'Le Thanh Phuoc';
fullName.startsWith('Le'); // true
fullName.endsWith('Le'); // false

fullName.includes('Thanh'); // true
fullName.includes('Le'); // true
```

* Object
```javascript
// Clone object
// v1
const clonedObjectV1 = Object.assign({}, object1, object2);
// v2 (recommend)
const clonedObjectV2 = {
  ...object1,
  ...object2,
};

// Deep clone object
const clonedObject = {
  ...object1,
  key: {
    ...object1.key,
  },
};
// Loop keys in object
const keyList = Object.keys(object);
// v1
for (let i = 0; i < keyList.length; i++) {
  const key = keyList[i];
  const value = object[key];
}
// v2
Object.keys(object).forEach((key) => {
  const value = object[key];
});
// v3 (recommend)
for (let key in object) {
  console.log('key:', key);
  console.log('value:', object[key]);
}
```

* Array
```javascript

```