## What is the difference between constant and var in angularJS?

| Value      | Constant | 
|------------|----------|
| ```value()``` can not be injected into a module ```config()``` function   | ```constant()``` can be injected anywhere in the application as well as module ```config()``` function also| 
| it can be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) | it cannot be overridden by an AngularJS [decorator](https://docs.angularjs.org/api/auto/service/$provide#decorator) |
| Value can only be used during run phase | it can be used in run phse as well as configuration function |

Angular value and constant services are an ideal way to provide application wide access to shared data without having to pollute the global namespace

```js
// Storing a single constant value
var app = angular.module('myApp', []);

app.constant('appName', 'My App');

// Now we inject our constant value into a test controller
app.controller('TestCtrl', ['appName', function TestCtrl(appName) {
    console.log(appName);
}]);
```

```js
//Some other module
var otherModule = angular.module('OtherModule',[]);

// Othermodule has a GreetingService
otherModule.service('GreetingService', function(){
    this.sayGreeting = function() {
        return "Hello";
    };
});

// MyApp depends on OtherModule
var myApp = angular.module('MyApp', ['OtherModule']);

// MyApp is being configured with $provide injected
myApp.config(function($provide) {
   
    // Service instance from OtherModule is injected as $delegate
    $provide.decorator('GreetingService', function($delegate){
        var originalGreeting = $delegate.sayGreeting();
        
        $delegate.sayGreeting = function() {
            return originalGreeting + " World!";
        };
       
        // Don't forget to return enhanced $delegate
        return $delegate;
    });
});

// The injected GreetingService in this module will be the decorated service
myApp.controller("MyController", function($scope, GreetingService){
    $scope.service = GreetingService;
});
```
```html
<body ng-app="MyApp" ng-controller="MyController">
    {{service.sayGreeting()}}
    <br/>
    {{service.sayGoodbye()}}
</body>
```
[Demo](http://jsfiddle.net/anandmanisankar/jd86qerb/)

## What is decorator and how to use it?

A good use case of ```$provide.decorator``` is when you need to do minor "tweak" on some third-party/upstream service, on which your module depends, while leaving the service intact (because you are not the owner/maintainer of the service). Here is a demonstration on [plunkr](https://plnkr.co/edit/lj9srM2KXZmwmTxLb1p7?p=preview).

# What is the difference between Service and Factory

The difference between a factory and a service is that a factory injects a plain function so AngularJS will call the function and a service injects a constructor. A constructor creates a new object so new is called on a service and with a factory you can let the function return anything you want.
