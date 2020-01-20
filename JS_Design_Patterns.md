# Design Patterns
Design patterns are solutions to the commonly occuring problems in software engineering. This avoid reinventing the wheels regarding the code design.

**Creational Patterns**
> For handling  object creation mechanisms
* [Constructor pattern](#constructor-pattern)
* [Prototype pattern](#prototype-pattern)
* [Module/RevealingModule pattern](#module-pattern)
* [Factory pattern](#factory-pattern)
* [Abstract factory pattern](#abstract-factory-pattern)
* [Builder pattern](#builder-pattern)
* [Singleton pattern](#singleton-pattern)

**Structural Patterns**
> For identifying ways to realize relationships between objects
* [Adapter pattern](#adapter-pattern)
* [Decorator pattern](#decorator-pattern)
* [Composite pattern](#composite-pattern)
* [Facade pattern](#facade-pattern)
* [Flyweight pattern](#flyweight-pattern)
* [Proxy pattern](#proxy-pattern)

**Behavioral Patterns**
> For handling communication between different objects
* [Command pattern](#command-pattern)
* [Iterator pattern](#iterator-pattern)
* [Chain of Responsibility pattern](#chain-of-responsibility-pattern)
* [Observer pattern](#observer-pattern)
* [Mediator pattern](#mediator-pattern)
* [State pattern](#state-pattern)
* [Strategy pattern](#strategy-pattern)
* [Template pattern](#template-pattern)


---

### **Constructor Pattern:**
Example of classical OOPS. The class constructor gets called everytime an instance is created.

```
class car {
  carName: string;

  constructor(name: string){
    this.carName = name;
  }

  addCar(){
    // call method to add
    return `${this.carName} added successfully`;
  }
}

let carA = new car('Ford');
let carB = new car('Honda');
carA.addCar();
carB.addCar();
```
**Disadvantage:** The property of the created objects are repeated.

[Back to Top](#design-patterns)


### **Prototype pattern:** 
Cloning an object from an existing prototype/structure and add more properties to it as needed.

```
const vehicle = { //existing prototype
 type: "",
 transport(item) {
   console.log("transport", item);
 }
};
 
const car = Object.assign({}, vehicle);//cloning
car.type = "civic";
car.engine = "v6";// Adding more properties
 
const car2 = Object.assign({}, vehicle);
car2.type = "ford";
car2.engine = "v8";
 
car.transport("some apples"); //Passing data as required
car2.transport("bananas");
 
console.log("1 --->>>>", car);
console.log("2 --->>>>", car2);
```
**Disadvantage:** The property which are not required is also copied. No control over the access type of the property.

[Back to Top](#design-patterns)


### **Module Pattern:**
Keeping a piece of code independent of other components. This help loose coupling to maintain well structured code.

**Note:** Directly mapping the methods to return object properties is called Revealing Module Pattern.
```
//Modular pattern reveals the public and private functions
var myModule = ((() => {
	var myPrivateVariable = 'private';
	var myPublicVariable = 'public';
	var myPrivateMethod = () => {
		console.log(myPrivateVariable);
	}
  var myPublicFunction = () => {
    myPrivateMethod();
  }
	return {
		myPublicVariable,
		myPublicMethodRevealing: myPublicFunction, // revealing module pattern (just map the private function)
    myPubiicMethod: () => { // module pattern (create a separate function to access a private function)
      myPrivateMethod();
    }
	};
})());

console.log(myModule.myPublicVariable); // prints: public
myModule.myPublicMethod(); // prints: private
console.log(myModule.myPrivateVariable); // prints: undefined
myModule.myPrivateMethod(); // throws TypeError: myModule.myPrivateMethod is not a function
```

```
// Practical Example
function EmployeeContainter () {
    const container = [];

    function add (name) {
        container.push(name);
    }

    function getAll() {
        return container;
    }

    function remove(name) {
        const index = container.indexOf(name);
        if(index < 1) {
            throw new Error('Employee not found');
        }
        container.splice(index, 1)
    }

    return {
        addEmp: add,
        getEmp: getAll,
        removeEmp: remove
    }
}

const container = EmployeeContainter();
container.addEmp('Sam');
container.addEmp('Mark');
container.addEmp('Mike');

console.log(container.getEmp()); //Array(3) ["Sam", "Mark", "Mike"]
container.removeEmp('Mike');
console.log(container.getEmp()); //Array(2) ["Sam", "Mark"]
```

[Back to Top](#design-patterns)


### **Factory Pattern:**
A factory is an object which creates other objects. Creates instances of several derived classes.

**Steps Below:** 
1. Factory Method
   
2. Methods (Base on distance)
   
* Bike Delivery Class
  
* Car Delivery Class

* Truck Delivery Class

```
// Delivery Classes
class DeliveryByBike {
  constructor(address, item) {
    this.address = address;
    this.item = item;
    this.rate = 5;
  }
  // rest of the class
}

class DeliveryByTruck {
  constructor(address, item) {
    this.address = address;
    this.item = item;
    this.rate = 15;
  }
  // rest of the class
}

class DeliveryByCar {
  constructor(address, item) {
    this.address = address;
    this.item = item;
    this.rate = 10;
  }
  // rest of the class
}
```

```
// Factory Method
class deliveryFactory {
  function deliveryFac(address, item) {
    const distance = 20;
    if (distance > 10 && distance < 50) {
      return new DeliveryByCar(address, item);
    }

    if (distance > 50) {
      return new DeliveryByTruck(address, item);
    }

    return new DeliveryByBike(address, item);
  }
}
```

```
const deliveryObj = new deliveryFactory();
const delivery = deliveryObj.deliveryFac('12 West lan, NY, 12343', 'Shirt');
console.log(delivery.rate); // 10
```
Caution: Don't use it if you don't have to create multiple instances of the same object.

[Back to Top](#design-patterns)


### **Abstract Factory Pattern:**
An abstrat factory abstracted out a theme which is used by other objects. Creates an instance of serveral families of classes.

**Steps Below:**
1. Abstract Factory Method
  
2. Factory Method 1st level (Based on the type of delivery - SameDay/Express)
	
* SameDay Delivery Class
  
* Express Delivery Class

3. Factory Method 2nd level (Based on distance)

* SameDay/Express Bike Delivery Class

* SameDay/Express Car Delivery Class

* SameDay/Express Truck Delivery Class

```
// Abstract Factory Method
function abstractFactory(address, item, options) {
 if (options.isSameday) {
   return sameDayDeliveryFactory(address, item);
 }
 if (options.isExpress) {
   return expressDeliveryFactory(address, item);
 } 
 return deliveryFactory(address, item);
}
```

[Back to Top](#design-patterns)


### **Builder Pattern:** 
 It separates object construction from its representation. Example: Instead of passing too many arguments to update an object (say, profile), we may use **set** and **build** functions to build the object before updating it.

```
// Normal way of doing (When the attributes increases it becomes ugly)
class Profile {
 constructor(
   menuLocation = "top",
   borders = "normal"
   theme = "dark",
   profileImage = "default.jpg"
 ) {
   this.menuLocation = menuLocation;
   this.borders = borders;
   this.theme = theme;
   this.profileImage = profileImage;
 }
}
```

Builder pattern comes as follows:
```
// Builder class
class ProfileBuilder { 
 setMenu(position) {
     this.menuLocation = position;
     return this;
 }
 
 setBorders(style) {
     this.borders = style;
     return this;
 }
 
 setProfileFont(fontFamily) {
     this.profileFont = fontFamily;
     return this;
 }
 
 build() {
     return new Profile(this);
 }
}
```
```
class Profile {
 constructor(data) {
   this.profileData = data;
 }
 saveData(){
	 // Call api function or db function
 }
}
```
```
// Build and then call the saveProfile
const userA = new ProfileBuilder()
 .setBorders("dotted")
 .setMenu("left")
 .setProfileFont("San Serif")
 .build();

userA.saveData();
```

[Back to Top](#design-patterns)


### **Singleton Pattern:**
This allows only for single instantiation, but many instances of the same object. If it finds an existing object, it returns it rather than creating a new one. Used for service/db connection creations.
```
const singleton = (function() {
  let instance;
  
  function init() {
    return {
      name: 'Peter',
      age: 24,
    };
  }

  return {
    getInstance: function() {
      if(!instance) {
        instance = init();
      }      
      return instance;
    }
  }
})();
const instanceA = singleton.getInstance();
const instanceB = singleton.getInstance();
console.log(instanceA === instanceB); // prints true
```

[Back to Top](#design-patterns)



### **Adapter Pattern**
When the functionlity is updated with new feature, new adapter is inserted so that the old functionality would work without issues along with the newly implemented features. Chaning the interface/flow compitable for both old and new functional flow so that the client is not forced to reinstall the app.

```
// old interface
class OldCalculator {
  constructor() {
    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          return term1 + term2;
        case 'sub':
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

// new interface
class NewCalculator {
  constructor() {
    this.add = function(term1, term2) {
      return term1 + term2;
    };
    this.sub = function(term1, term2) {
      return term1 - term2;
    };
    this.multiply = function(term1, term2) {
      return term1 * term2;
    };
    this.divide = function(term1, term2) {
      return term1/term2;
    };
  }
}

// Adapter Class
class CalcAdapter {
  constructor() {
    const newCalc = new NewCalculator();

    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          // using the new implementation under the hood
          return newCalc.add(term1, term2);
        case 'sub':
          return newCalc.sub(term1, term2);
        case 'mul':
          return newCalc.multiply(term1, term2);
        case 'div':
          return newCalc.divide(term1, term2);
        default:
          return NaN;
      }
    };
  }
}

// Old calc
const oldCalc = new OldCalculator();
console.log(oldCalc.operations(10, 5, 'add')); // 15

// New calc
const newCalc = new NewCalculator();
console.log(newCalc.add(10, 5)); // 15

// Adapter
const adaptedCalc = new CalcAdapter();
console.log(adaptedCalc.operations(10, 5, 'add')); // 15;
```

[Back to Top](#design-patterns)


### **Decorator Pattern**
We can use an object structure with some customizations on its properties so as to not disturbing the base code.

```
// Usual style for ordering a default chicken sandwitch
let ingredients = {
  bread: "plain",
  meat: "chicken",
  mayo: true,
  mastard: true,
  lettuce: true,
  type: "6 inch"
};
class Sandwich {
  constructor(ingredients) {
    this.ingredients = ingredients;
  }

  getPrice() {
    return 5.5;
  }

  setBread(bread) {
    this.ingredients.bread = bread;
  }
}

let chickenSandwitch = new Sandwich(ingredients);
console.log(chickenSandwitch.getPrice());
```

```
// Adding Customizations to the sandwitch (Type changed to 'Foot long')
function footLong(ingredients) {
 let sandwich = new Sandwich(ingredients);
 
 sandwich.ingredients.type = "foot long";
 
 let price = sandwich.getPrice();
 sandwich.getPrice = function() {
   return price + 3;
 };
 return sandwich;
}
 
let footlong = footLong(ingredients);
console.log("---->>", footlong.getPrice());
console.log("type", footlong.ingredients);
```

```
// Adding Customizations to the sandwitch (Type = 'Foot long' & Meat = 'Beaf')
function beefSandwichFootLong(ingredients) {
 let sandwich = footLong(ingredients);
 sandwich.ingredients.meat = "beef";
 let price = sandwich.getPrice();
 sandwich.getPrice = function() {
   return price + 1;
 };
 return sandwich;
}
 
let beefFootLong = beefSandwichFootLong(ingredients);
console.log("Beef foot", beefFootLong.ingredients.meat);
console.log("Beef foot price", beefFootLong.getPrice());
```

[Back to Top](#design-patterns)


### **Composite Pattern**
This helps us to structure our code like a tree like hierarchy. First comes Base class, then it can have children classes. The child class can have children further. In each stage the parent class had to be extended. The parent class properties could be altered in the child class.

```
// Vehicle class with car data as default
class Vehicle {
 constructor() {
   this.type = "Car";
   this.model = "Honda Civic";
   this.engine = "v6";
   this.speed = "180 km/h";
   this.weight = "4000 kg";
   this.engineType = "gasoline";
   this.fuel = "Gas";
 }
 
 drive() {
   console.log(`Driving ${this.type} ${this.model} with ${this.speed}`);
 }
 
 refuel() {
   console.log(`Fueling with ${this.fuel}`);
 }
 
 getEngine() {
   console.log(`Getting Engine ${this.engine}`);
 }
}
 
const generic = new Vehicle();
generic.getEngine();
// Getting Engine v6
```

```
// Truck is the child of Vehicle
class Truck extends Vehicle {
 constructor() {
   super();
   this.type = "Truck";
   this.model = "Generic Truck Type";
   this.speed = "220 km/h";
   this.weight = "8000 kg";
 }
 
 drive() {
   super.drive();
   console.log(`A ${this.type} with max speed of ${this.speed}`);
 }
}
 
const truck = new Truck();
console.log("truck", truck.drive()); 
// Driving Truck Generic Truck Type with 220 km/h
// A Truck with max speed of 220 km/h
```

[Back to Top](#design-patterns)


### **Facade Pattern**
A single class/function that represents a sub system.

```
// jQuery
$(document).ready(..);
$(<element>).css();
```

```
// Call just one function to set, get and run (Facade integrated with modular pattern)
var module = (function() { 
    var _private = {
        i: 5,
        get: function() {
            console.log( "current value:" + this.i);
        },
        set: function( val ) {
            this.i = val;
        },
        run: function() {
            console.log( "running" );
        },
        jump: function(){
            console.log( "jumping" );
        }
    };
 
    return { 
        facade: function( args ) {
            _private.set(args.val);
            _private.get();
            if ( args.run ) {
                _private.run();
            }
        }
    };
}());
 
// Outputs: "current value: 10" and "running"
module.facade( {run: true, val: 10} );
```

[Back to Top](#design-patterns)


### **Flyweight Pattern**
This helps effective data sharing by efficiency and memory conservation purposes. (Example, avoid loading the same image/data twice)

```
// Flyweight class
class Icecream {
  constructor(flavour, price) {
    this.flavour = flavour;
    this.price = price;
  }
}

// Factory for flyweight objects
class IcecreamFactory {
  constructor() {
    this._icecreams = [];
  }

  createIcecream(flavour, price) {
    let icecream = this.getIcecream(flavour);
    if (icecream) {
      return icecream;
    } else {
      const newIcecream = new Icecream(flavour, price);
      this._icecreams.push(newIcecream);
      return newIcecream;
    }
  }

  getIcecream(flavour) {
    return this._icecreams.find(icecream => icecream.flavour === flavour);
  }
}

// Usage
const factory = new IcecreamFactory();

const chocoVanilla = factory.createIcecream('chocolate and vanilla', 15);
const vanillaChoco = factory.createIcecream('chocolate and vanilla', 15);

// reference to the same object
console.log(chocoVanilla === vanillaChoco); // true
```

[Back to Top](#design-patterns)


### **Proxy Pattern**
An object representing another object (Example: To decide the source of data)

```
function GeoCoder() {
 
    this.getLatLng = function(address) {
        
        if (address === "Amsterdam") {
            return "52.3700° N, 4.8900° E";
        } else if (address === "London") {
            return "51.5171° N, 0.1062° W";
        } else if (address === "Paris") {
            return "48.8742° N, 2.3470° E";
        } else if (address === "Berlin") {
            return "52.5233° N, 13.4127° E";
        } else {
            return "";
        }
    };
}
 
function GeoProxy() {
    var geocoder = new GeoCoder();
    var geocache = {};
 
    return {
        getLatLng: function(address) {
            if (!geocache[address]) {
                geocache[address] = geocoder.getLatLng(address);
            }
            log.add(address + ": " + geocache[address]);
            return geocache[address];
        },
        getCount: function() {
            var count = 0;
            for (var code in geocache) { count++; }
            return count;
        }
    };
};
 
// log helper
 
var log = (function() {
    var log = "";
 
    return {
        add: function(msg) { log += msg + "\n"; },
        show: function() { alert(log); log = ""; }
    }
})();
 
function run() {
    var geo = new GeoProxy();
 
    // geolocation requests
 
    geo.getLatLng("Paris");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("London");
    geo.getLatLng("London");
 
    log.add("\nCache size: " + geo.getCount());
    log.show();
}
```

[Back to Top](#design-patterns)


### **Command Pattern:** 
The command class that encapsulates actions/operations as objects so that it could be executed through the methods in the command class.

```
// Operations
class SpecialMath {
  constructor(num) {
    this._num = num;
  }

  square() {
    return this._num ** 2;
  }

  cube() {
    return this._num ** 3;
  }

  squareRoot() {
    return Math.sqrt(this._num);
  }
}

// Command class
class Command {
  constructor(subject) {
    this._subject = subject;
    this.commandsExecuted = [];
  }
  execute(command) {
    this.commandsExecuted.push(command);
    return this._subject[command]();
  }
}

// Execute the command
const x = new Command(new SpecialMath(5));
x.execute('square');
x.execute('cube');
console.log(x.commandsExecuted); // ['square', 'cube']
```

[Back to Top](#design-patterns)


### **Iterator Pattern:** 
It provides a way to access the elements of an aggregate object sequentially without exposing the logic behind it.

```
// Iterating an arry in jQuery
$.each( ["john","dave","rick","julian"], function( index, value ) {
  console.log( index + ": " + value);
});

// Iterating the li present in the dom
$( "li" ).each( function ( index ) {
  console.log( index + ": " + $( this ).text());
});
```

```
// Iterator function
function* iteratorUsingGenerator(collection) {
  var nextIndex = 0;

  while (nextIndex < collection.length) {
    yield collection[nextIndex++];
  }
}

// Usage
const gen = iteratorUsingGenerator(['Hi', 'Hello', 'Bye']);
console.log(gen.next().value); // 'Hi'
console.log(gen.next().value); // 'Hello'
console.log(gen.next().value); // 'Bye'
```

[Back to Top](#design-patterns)


### **Chain of Responsibility Pattern:** 
The payload/data can pass through a chain of functions/process and it gets executed in the appropriate function(s) only. (Example: NodeJs middleware)

```
// Different functions to process put in an array
function AppleProcess(req) {
 if (req.payload === "apple") {
   console.log("Processing Apple");
 }
}

function OrangeProcess(req) {
 if (req.payload === "orange") {
   console.log("Processing Orange");
 }
}

function MangoProcess(req) {
 if (req.payload === "mango") {
   console.log("Processing Mango");
 }
}
const chain = [AppleProcess, OrangeProcess, MangoProcess];
```

```
// Execute the array
function processRequest(chain, request) {
 let lastResult = null;
 let i = 0;
 chain.forEach(func => {
   func(request);
 });
}
 
let sampleMangoRequest = {
 payload: "mango"
};
processRequest(chain, sampleMangoRequest);
```

[Back to Top](#design-patterns)


### **Observer Pattern:** 
An Object (Subject) maintains a list of objects(Observers) and notifies them of any changes. (Example, live chat, live listening) (jQuery's On, RxJs, etc.)

```
// Observable call which has Subscribe/Nofity/Unsubscribe functions
class Observable {
 constructor() {
   this.observers = [];
 }
 subscribe(f) {
   this.observers.push(f);
 }
 unsubscribe(f) {
   this.observers = this.observers.filter(subscriber => subscriber !== f);
 }
 notify(data) {
   this.observers.forEach(observer => observer(data));
 }
}
```

```
// Some DOM references
const input = document.querySelector(".input");
const a = document.querySelector(".a");
const b = document.querySelector(".b");
const c = document.querySelector(".c");

// Observers
const updateA = text => a.textContent = text;
const updateB = text => b.textContent = text;
const updateC = text => c.textContent = text;

// Subscribe to some observers/listeners
const headingsObserver = new Observable();
headingsObserver.subscribe(updateA);
headingsObserver.subscribe(updateB);
headingsObserver.subscribe(updateC);
 
// Notify all observers about new data on event
input.addEventListener("keyup", e => {
 headingsObserver.notify(e.target.value);
});
```

```
// Observe the state
class Subject {
  constructor() {
    this._observers = [];
  }

  subscribe(observer) {
    this._observers.push(observer);
  }

  unsubscribe(observer) {
    this._observers = this._observers.filter(obs => observer !== obs);
  }

  fire(change) {
    this._observers.forEach(observer => {
      observer.update(change);
    });
  }
}

class Observer {
  constructor(state) {
    this.state = state;
    this.initialState = state;
  }

  update(change) {
    let state = this.state;
    switch (change) {
      case 'INC':
        this.state = ++state;
        break;
      case 'DEC':
        this.state = --state;
        break;
      default:
        this.state = this.initialState;
    }
  }
}

// usage
const sub = new Subject();

const obs1 = new Observer(1);
const obs2 = new Observer(19);

sub.subscribe(obs1);
sub.subscribe(obs2);

sub.fire('INC');

console.log(obs1.state); // 2
console.log(obs2.state); // 20
```

[Back to Top](#design-patterns)


### **Mediator Pattern:** 
Objects communicates through mediator objects which avoid complexities when communicating. (Say, Chat-Room class, Airlan-Tower class)

**Disadvantage:** 
* Since everything goes through a single object, a single point of failure will create a big issue.

**Advantage:**
* Avoids many-many relations and changes it to many to one.
* Adding subscribers is easy with the level of decoupling present.

```
// Participant class
class Participant {
  constructor(name) {
    this.name = name;
    this.chatroom = null;
  }

  send(message, to) {
    this.chatroom.send(message, this, to);
  }

  receive(message, from) {
    log.add(from.name + " to " + this.name + ": " + message);
  }
}
 
// Chatroom function
let Chatroom = function() {
  let participants = {};

  return { 
    register: function(participant) {
      participants[participant.name] = participant;
      participant.chatroom = this;
    },
  
    send: function(message, from, to) {
      if (to) {                      // single message
        to.receive(message, from);    
      } else {                       // broadcast message
        for (let key in participants) {   
          if (participants[key] !== from) {
            participants[key].receive(message, from);
          }
        }
      }
    }
  };
};

// log helper
log = (function() {
    let log = '';
    return {
        add: msg => { log += msg + '\n'; },
        show: () => { alert(log); log = ''; }
    }
})();
 
function run() {
  let yoko = new Participant('Yoko'),
      john = new Participant('John'),
      paul = new Participant('Paul'),
      ringo = new Participant('Ringo'),
      chatroom = new Chatroom(),

  chatroom.register(yoko);
  chatroom.register(john);
  chatroom.register(paul);
  chatroom.register(ringo);

  yoko.send('All you need is love.');
  yoko.send('I love you John.');
  john.send('Hey, no need to broadcast', yoko);
  paul.send('Ha, I heard that!');
  ringo.send('Paul, what do you think?', paul);

  log.show();
}

run();
```

```
// Airplain - Tower
class TrafficTower {
  constructor() {
    this._airplanes = [];
  }

  register(airplane) {
    this._airplanes.push(airplane);
    airplane.register(this);
  }

  requestCoordinates(airplane) {
    return this._airplanes.filter(plane => airplane !== plane).map(plane => plane.coordinates);
  }
}

class Airplane {
  constructor(coordinates) {
    this.coordinates = coordinates;
    this.trafficTower = null;
  }

  register(trafficTower) {
    this.trafficTower = trafficTower;
  }

  requestCoordinates() {
    if (this.trafficTower) return this.trafficTower.requestCoordinates(this);
    return null;
  }
}

// usage
const tower = new TrafficTower();

const airplanes = [new Airplane(10), new Airplane(20), new Airplane(30)];
airplanes.forEach(airplane => {
  tower.register(airplane);
});

console.log(airplanes.map(airplane => airplane.requestCoordinates())) 
```

[Back to Top](#design-patterns)


### **State Pattern**
It allows to change the behaviour of the object when its state changes. (Example, Traffic light system)
```
// Traffic light system
class TrafficLight {
  constructor() {
    this.states = [new GreenLight(), new RedLight(), new YellowLight()];
    this.current = this.states[0];
  }

  change() {
    const totalStates = this.states.length;
    let currentIndex = this.states.findIndex(light => light === this.current);
    if (currentIndex + 1 < totalStates) this.current = this.states[currentIndex + 1];
    else this.current = this.states[0];
  }

  sign() {
    return this.current.sign();
  }
}

class Light {
  constructor(light) {
    this.light = light;
  }
}

class RedLight extends Light {
  constructor() {
    super('red');
  }

  sign() {
    return 'STOP';
  }
}

class YellowLight extends Light {
  constructor() {
    super('yellow');
  }

  sign() {
    return 'STEADY';
  }
}

class GreenLight extends Light {
	constructor() {
		super('green');
	}

	sign() {
		return 'GO';
	}
}

// Usage
let trafficLight = new TrafficLight();

function changeLight(i){
  trafficLight.change();
  console.log(trafficLight.sign());
  if (i < 10){
    setTimeout(()=>{ changeLight(++i); }, 5000);
  }
}

changeLight(0);
```

[Back to Top](#design-patterns)


### **Strategy Pattern**
It encapsulates the algorithm inside a class. (Example, calculating cost for various courier delivery, calculating time for different modes of travel)
```
// Encapsulation
class Commute {
  travel(transport) {
    return transport.travelTime();
  }
}

class Vehicle {
  travelTime() {
    return this._timeTaken;
  }
}

// strategy 1
class Bus extends Vehicle {
  constructor() {
    super();
    this._timeTaken = 10;
  }
}

// strategy 2
class Taxi extends Vehicle {
  constructor() {
    super();
    this._timeTaken = 5;
  }
}

// usage
const commute = new Commute();
console.log(commute.travel(new Taxi())); // 5
console.log(commute.travel(new Bus())); // 10
```

[Back to Top](#design-patterns)


### **Template Pattern**
It provides a class with a template of methods and properties. It leaves some steps for the sub class. (Example, handing the different types of employees)

```
// Employee class
class Employee {
  constructor(name, salary) {
    this._name = name;
    this._salary = salary;
  }

  work() {
    return `${this._name} handles ${this.responsibilities() /* gap to be filled by subclass */}`;
  }

  getPaid() {
    return `${this._name} got paid ${this._salary}`;
  }
}

class Developer extends Employee {
  constructor(name, salary) {
    super(name, salary);
  }

  // details handled by subclass
  responsibilities() {
    return 'application development';
  }
}

class Tester extends Employee {
  constructor(name, salary) {
    super(name, salary);
  }

  // details handled by subclass
  responsibilities() {
    return 'testing';
  }
}

// Usage
const dev = new Developer('Nathan', 100000);
console.log(dev.getPaid()); // 'Nathan got paid 100000'
console.log(dev.work()); // 'Nathan handles application development'

const tester = new Tester('Brian', 90000);
console.log(tester.getPaid()); // 'Brian got paid 90000'
console.log(tester.work()); // 'Brian handles testing'
```

[Back to Top](#design-patterns)


**Reference Links:**

* DoFactory: https://www.dofactory.com/javascript/design-patterns
* Addyosmani Book: https://addyosmani.com/resources/essentialjsdesignpatterns/book/#introduction
* Medium1: https://medium.com/better-programming/javascript-design-patterns-25f0faaaa15
* Medium2: https://blog.bitsrc.io/understanding-design-patterns-in-javascript-13345223f2dd
* LogRocket: https://blog.logrocket.com/javascript-design-pattern-214d888096a3/
* Blog: https://www.telerik.com/blogs/design-patterns-in-javascript
