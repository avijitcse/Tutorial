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

## What is the difference between Service and Factory

The difference between a factory and a service is that a factory injects a plain function so AngularJS will call the function and a service injects a constructor. A constructor creates a new object so new is called on a service and with a factory you can let the function return anything you want.

## Sharing Data between Components in Angular

Fully documented lesson at https://www.youtube.com/watch?v=I317BhehZKM

In this episode, I am going to show you four different ways to share data between Angular components. 

Parent to Child the Input Decorator

When you declare a variable with the Input decorator in the child component, it allows that variable to be received from a parent template. In this case, we define a message variable in the parent, then use square brackets to pass the data to the child. Now the child can display this data in its own template.

Child to Parent via ViewChild

ViewChild allows a one component to be injected into another, giving the parent access to its attributes and functions. One caveat, however, is that child won't be available until after the view has been initialized. This means we need to implement the AfterViewInit lifecycle hook to receive the data from the child.  

In the AfterViewInit function we can access the message variable defined in the child

Child to Parent via Output and EventEmitter

Another way to share data is to emit data from the child, which can be listed to by the parent. This approach is ideal when you want to share data changes that occur on things like button clicks, form entires, and other user events. 

In the child, we declare a messageEvent variable with the Output decorator and set it equal to a new event emitter. Then we create a function named sendMessage that calls emit on this event with the message we want to send. Lastly, we create a button to trigger this function.

In the parent, we create a function to receive the message and set it equal to the message variable. 

The parent can now subscribe to this messageEvent that's outputted by the child component, then run the receive message function whenever this event occurs. 

Share data between any components

When passing data between components that lack a direct connection, such as siblings, grandchildren, etc, you should you a shared service. When you have data that should aways been in sync, I find the RxJS `BehaviorSubject` very useful in this situation. The main benefit that a BehaviorSubject ensures that every component consuming the service receives the most recent data. 
    

In the service, we create a private BehaviorSubject that will hold the current value of the message. We define a currentMessage variable handle this data stream as an observable that will be used by the components. Lastly, we create function that calls next on the BehaviorSubject to change its value. 

The parent, child, and sibling components all receive the same treatment. We inject the DataService in the constructor, then subscribe to the currentMessage observable and set its value equal to the message variable. 

Now if we create a function in any one of these components that changes the value of the message. when this function is executed the new data it's automatically broadcast to all other components. 

That's it for data sharing. See you next time.
