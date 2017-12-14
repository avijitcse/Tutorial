# What is the difference between constant and var in angularJS?

| Value      | Constant | 
|------------|----------|
| ```value()``` can not be injected into a module ```config()``` function   | ```constant()``` can be injected anywhere in the application as well as module ```config()``` function also| 
| it can be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) | it cannot be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) |
| Value can only be used during run phase | it can be used in run phse as well as configuration function |

Angular value and constant services are an ideal way to provide application wide access to shared data without having to pollute the global namespace
```js
// Storing a single constant value
var app = angular.module(‘myApp’, []);

app.constant(‘appName’, ‘My App’);

// Now we inject our constant value into a test controller
app.controller(‘TestCtrl’, [‘appName’, function TestCtrl(appName) {
    console.log(appName);
}]);
```
```
// Storing a single value
var app = angular.module(‘myApp’, []);

app.value(‘usersOnline’, 0);

// Now we inject our constant value into a test controller
app.controller(‘TestCtrl’, [‘usersOnline’, function TestCtrl(usersOnline) {
    console.log(usersOnline);
    usersOnline = 15;
    console.log(usersOnline);
}]);
```
