# What is the difference between constant and var in angularJS?

| Value      | Constant | 
|------------|----------|
| ```value()``` can not be injected into a module ```config()``` function   | ```constant()``` can be injected anywhere in the application as well as module ```config()``` function also| 
| it can be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) | it cannot be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) |


```js
some javascript
```
