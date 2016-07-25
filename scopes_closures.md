this and prototype delegation : important concepts of object oriented javascript.

Lexical scope :
- An area where a variable can be accessed by name without access errors.
- a new scope gets created inside everyfunction, vars inside this function cannot be accessed outside.

**Only functions create a new scope**
- for eg, vars created inside `if` statement can be used outside.

Execution Contexts ( inmemory scopes)
 When program runs, it builds storage systems for variables and values which are called execution contexts. does key value mapping. (like an object but not same.)

Lexical vs In memory scopes.
- in memory scopes are at runtime of program
-

### Closure
- a function is accessible after its surrounding scope is returned.
 ```
    var arr = [];
    function a() {
        var a = 10;
        arr.push( function() {
            var b = 20;
            console.log(a * b)
        })
    }
    a();
    arr[0]();
 ```
### this
- this gets bound to which object is on focus.
- in obj.fn(3,4) `this` will be bound to `obj` to left of dot.
-
```
    var fn = function(a,b) {
    console.log(this,a,b)
}```
 in this case, this would be bound to the global scope.
 - if
 ```
    r= {}, r.method = fn, r.method(a,b)
 ```
 in this case, `this` would output contents of r object.

 Case 1: the first method can be over-ridden to use a specific object by using
 `call` method

 eg. fn.call(r,a,b) the `this` will have access to object `r` instead of global scope.

 *what happens if r.method.call(y,a,b) is called ?*
 - y {} will be bound to this as `call` over rides the `.` dot object


*Set timeout*
setTimeout(fn, 1000); since the setTimeout does not have a way to pass parameters, it just calls the function without them, printing undefined logs

if setTimeout(r.method, 1000); this too will have a global `this` as at the time of call, inside setTimeout, it will be called as fn() - no binding.

##### using new
- new r.method(a,b) here `this` will not be bound to r as `new` is used.
- everytime `new` is used, a new object gets created.



#### Prototype Chains
- used for making objects resemble other objects.
- copying properties between objects.
```
var gold = {a:1}
var blue = extend({}, gold)
```
This is a one time copy and if gold or blue changes, the properties are not carried over.

for a continuous lookup, do
```
var gold = {a: 1}
var blue = Object.create(a)
```
if after prototype delegation happens, gold adds more properties,
it can be accessed through lookup of delegation

Every object has a prototype, it can be accessed throguh the objec.toString().
It will return the first .toString function.
Also there is a .constructor function

eg. the array prototype has its own .toString and .slice and other functions in its prototype. thats how they can be accessed.

##### Object Decorator pattern

*Code Reuse*
- use common code in a central library to afford code reuse.

Decorator function

var carlike = function(obj, loc) {
    obj.loc = loc;
    return obj;
}

var amy = carlike({}, 1);

##### Functional classes

Decorator pattern vs classes
- class builds a new object while decorator accepts an object and modifies it.
- functions that produce the new objects are called `constructor`

```
var Car = function(loc) {
    var obj = {loc: loc};
    obj.move = function() {
        this.loc++;
    }
    return obj;
}
```

var amy = Car(1); this is called an instance because it is instantiating the Car class.

if there are more methods, store in variable and extend inside class
```
var Car = function(loc) {
    var obj = {loc: loc};
    extend(obj, Car.methods) // extend is not native js.
    return obj;
}

Car.methods = {
    move: function() {
        this.loc++;
    }
}
```

##### Prototype Class

- Improving performance
```
var Car = function(loc) {
    var obj = Object.create(Car.methods)
    ob.loc = loc;
    return obj;
}

Car.methods = {
    move: function() {
        this.loc++;
    }
}
```

there is an existing prototype method which can be used instead of manual `methods` function
```
var Car = function(loc) {
    var obj = Object.create(Car.prototype)
    ob.loc = loc;
    return obj;
}

Car.prototype.move = function() {
        this.loc++;
}
```

use prototype to move all the functions of the class declaration

##### Pseudoclassical patterns
- common statemenrts like
```
var obj = Object.create(Car.prototype)
return obj;
```
can be removed by using a inbuilt statement `new`

var amy = new Car(7);

##### Super nad Sub classes
if there are 2 types of cars, one bad car and one good car.
to abstract attirbutes of car, you use a super classes where van and cop will be subclass.

```
var Car = function(loc) {
    var obj = {loc:loc};
    obj.move = function() {
        obj.loc++;
    };
    return obj;
}

var Van = function(loc) {
    var obj = Car(loc);
    obj.grab = function() {

    }
}

var Cop = function(loc) {
    var obj = Car(loc);
    obj.call = function() {

    }
}
```

##### Pseudo classical subclasses
 modifying the above pattern using pseudo classical pattern,

 use prototype property

```
var Car = function (loc) {
    this.loc = loc;

}

Car.prototype.move = function() {
    this.loc ++;
}

var Van = function(loc) {
    Car.call(this,loc);
}

var zed = new Car(7);
zed.move();

var amy = new Van(9);
amy.move();
```
since here move is a car prototype, van will not have access to it, so
` Van.prototype = Object.create(Car.prototype)`

since Van's prototype is now set to Car's prototype, amy.cosntructor will point to Car.

Van's constructor is missing. So add,
`Van.prototype.constructor = Van;`
