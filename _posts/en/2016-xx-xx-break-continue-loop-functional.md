layout: post

title: Breaking or continuing loop in functional programming
tip-number: XX
tip-username: vamshisuram
tip-username-profile: https://github.com/vamshisuram
tip-tldr:  Uses __.some__ and __.every__ functions


categories:
    - en
---


A common requirement of iteration is cancelation.

Using __for__ loops we can `break` to end iteration early.
```javascript
var a = [0, 1, 2, 3, 4];
for (var i = 0; i < a.length; i++) {
    if (a[i] === 2) {
        break; // stop the loop
    }
    console.log(a[i]);
}
//> 0, 1
```

Another common requirement is to close over our variables.

A quick approach is to use __.forEach__ but
then we lack the ability to `break`. In this situation the closest we get is `continue` functionality through `return`.

```javascript
a.forEach(function(val, i) {
    if (val === 2) {
        // how do we stop?
        return true;
    }
    console.log(val); // your code
});
//> 0, 1, 3, 4
```

The __.some__ is a method on Array prototype. It tests whether some element in the array passes the test implemented by the provided function. If any value is returning true, then it stops executing. Here is a [MDN link](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/some) for more details.

An example quoted from that link
```javascript
function isBiggerThan10(element, index, array) {
  return element > 10;
}
[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
```

Using __.some__ we get iteration functionally similar to __.forEach__ but with the ability to `break` through `return` instead.
```javascript
a.some(function(val, i) {
    if (val === 2) {
        return true;
    }
    console.log(val); // your code
});
//> 0, 1
```


You keep returning `false` to make it `continue` to next item. When you return `true`, the loop will `break` and __a.some(..)__ will `return` `true`.

```javascript
// Array contains 2
var isTwoPresent = a.some(function(val, i) {
    if (val === 2) {
        return true; // break
    }
});
console.log(isTwoPresent);
//> true
```

Also there is __.every__, which can be used. We have to return the opposite boolean compared to __.some__.
