# Why using Object.prototype.hasOwnProperty() is better than object.hasOwnProperty()?
***Nov 01, 2018 #JavaScript***

During core-reviewing or studying code I oftentimes encounter bug-prone code. In this post I will show you, why you shouldn't call ***hasOwnProperty*** (or other Object.prototype method) on your object directly.

Look at this example:
```js
const myObject = {test: 'test'}
myObject.hasOwnProperty('test')
// it return: true, it's fine but...
```

You never know that if ***myObject*** is constructed correctly. Object could not have object prototype in chain. In other words, it could not have ***hasOwnProperty*** method inherited from ***Object().prototype***. It at times strikes on code, which in some cases gives ***Uncaught TypeError***.
But why? What happens?

The ***hasOwnProperty*** method is calling on null or object without object proptotype in chain.

Examples:
```js
null.hasOwnProperty('test')
Uncaught TypeError: Cannot read property 'hasOwnProperty' of null
```
or 
```js
const myObject = Object.create(null);
myObject.hasOwnProperty('test')
TypeError: myObject.hasOwnProperty is not a function
```
In the second example object myObject doesn't have method inherited from Object().prototype.
That's why it should be called by Object.prototype. hasOwnProperty.call():
```js
const myObject = Object.create(null);
Object.prototype.hasOwnProperty.call(myObject, 'test')
```
or 
```js
Object.prototype.hasOwnProperty.call(null, 'test')
```
always return false. It's bulletproof.

I'll show you another reason why you shouldn't call Object.prototype.hasOwnProperty directly on created object. ***hasOwnProperty*** is a method, not operator.
***myObject.hasOwnProperty*** could be overrided by anything.

I swear, it's the last example:
```js
const myObject = Object.create({test: 'test'});
myObject.hasOwnProperty = null;
myObject.hasOwnProperty('test');
Uncaught TypeError: myObject.hasOwnProperty is not a function
```
I'll hope it will be helpful for you.