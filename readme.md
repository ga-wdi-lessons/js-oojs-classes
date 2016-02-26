# Javascript Prototypes and Constructors

## Learning Objectives

- Explain the importance of OOJS
- Describe the role of constructor functions, and how they work
- Use constructor functions and the `new` keyword to create objects with shared properties
- Describe what a prototype object is, and how they are used in JS
- Diagram the relationship between an object, its constructor, and prototype
- Compare / contrast classical and prototypal inheritance
- Compare `Object.create` vs constructors

## Framing: Object Oriented Programming (10 min)

Hi! This is an object: `{}`. Now you're oriented.

![Bad joke](http://media.giphy.com/media/xn2BRZmrIJxmM/giphy.gif)

We have exposure to objects in JavaScript, using object literal notation. Some of you might have had something like this in your first project:

```js
var game = {
  cards: document.querySelectorAll(".cards"),
  startingTime: 0,
  createBoard: function(){
    var gameBoard = document.querySelector("#game-board")
    gameBoard.style.display = "inline";
  }
};
```
**What is an object in programming?**

>An object encapsulates related data and
behavior in an organized structure.

**Why might we use an OOP approach to programming?**

>Object-oriented programming provides us
with opportunities to clean up our procedural code and model it more-closely to the external world.

>OOP helps us to achieve the following:
  * Abstraction
  * Encapsulation
  * Modularity

OOP becomes **very** important as our front-end code grows in complexity. Even a simple app will have lots of code on the front end to do
things like:

* render data from the back end
* update the state of the page as data changes
* respond to events like clicking buttons / links
* send requests to the back end to fetch / update / destroy data

### Creating Objects (5 min)

So far, we've had to make our objects 'by hand', i.e. using object literals:

```js
var celica = {
  model: 'Toy-Yoda Celica',
  color: 'limegreen',
  fuel: 100,
  drive: function() {
    this.fuel--;
    return 'Vroom!';
  },
  refuel: function() {
    this.fuel = 100;
  }
}

var civic = {
  model: 'Honda Civic',
  color: 'lemonchiffon',
  fuel: 100,
  drive: function() {
    this.fuel--;
    return 'Vroom!';
  },
  refuel: function() {
    this.fuel = 100;
  }
}
```
Although we are taking an object-oriented approach to programming, notice how much duplication there is in that code! Just imagine if we needed a hundred cars in our app!! Our code would certainly not be considered "DRY".

As you may have noticed, some of these properties change between cars (model
and color), and others stay the same, for example, `fuel` starts at 100, and
`drive` and `refuel` are the same functions for every car.

Why don't we build a function that makes these objects for us!

### You-Do Exercise: Create a makeCar function (10 min)

Define a function: `makeCar()` that takes two parameters (model, color) and
makes a new object literal for a car using those params, and returns that object.

```js
// This should return a car object just like the previous example
var celica = makeCar("Toy-Yoda Celica", "limegreen");
```
See solution in `car.js`


## Constructor Functions

### You-Do Exercise: Read JavaScript Constructors, Prototypes, and the 'new' Keyword (5 min)

[Article: JavaScript Constructors, Prototypes, and the 'new' Keyword  Constructors](https://blog.pivotal.io/labs/labs/javascript-constructors-prototypes-and-the-new-keyword)

While reading the article, think about why constructor functions and prototypes might be useful for us as developers.
<!--Please tilt laptops when finished reading -->

#### Turn & Talk- 1/4: Why might we want to use a constructor function in OOP to create objects? (5 min)

### Overview of Constructor Functions (10 min)

It's so common that we need to make objects with similar properties / methods
that programming languages usually have some features to help with this.

In Javascript, we use constructor functions and
prototypes. ***(Note: the newest version, ES6, adds
classes, but they're really just syntactic sugar, behind the scenes it's still using constructors and prototypes)***

Constructor functions are a lot like the `makeCar` function we just created, but
they're supported by JS and we use the `new` keyword to use the constructor
function.
<!--maybe demonstrate that constructor is a property, All objects inherit a constructor property from their prototype:-->

Note that constructor functions start with a capital letter to make it obvious
that they are constructors. This isn't necessary, but is a convention and you
should follow it!

## Break (10 min)

### I-Do: Make a Person Constructor Function (10 min)

```js
function Person(initialName) {
  this.name = initialName;
  this.species = "Homo Sapiens";
  this.speak = function() {
    return "Hello! I'm " + this.name;
  };
}

var adam = new Person("Adam");
var bob =  new Person("Bob");

adam.name // "Adam"
adam.speak() // "Hello! I'm Adam"

bob.name // "Bob"
bob.speak() // "Hello! I'm Bob"
```

Notice the use of `this`, and the fact that we don't return anything. Here's why we write constructor functions this way:

When we run a constructor function with `new`, Javascript will automatically:

1. Create an new, empty object for us.
2. Call the constructor function on that object (`this` -> the new object)
3. Return the object

### You-Do Exercise: Make a Car Constructor Function (10 min)

Write a constructor function to replace our `makeCar` function earlier.

See car.js for a solution.

## Prototypes (15 min)

There's one problem with our constructor function... every time we create a new
car, it's creating new copies of the `refuel` and `drive` functions. This isn't really necessary, since those functions are exactly the same for every car.

Creating all these copies can cause our program to use more memory than it
really needs to.

Thankfully, by using **prototypes**, we can eliminate this duplication. Every
object in JS has a `prototype` property, that points to another **object**.
Almost always, the prototype of a given object will come from it's
**constructor** (which has a prototype).

Any properties / methods defined on an object's prototype are available on the
that object. An example:

```js
function Dog(name, breed) {
  this.name = name;
  this.breed = breed;
}

Dog.prototype.species = "Canis Canis";
Dog.prototype.bark = function() { return "Woof! I'm " + this.name; }

// OR Alternate form:
// The disadvantage here is that we're overwriting any existing properties on
// the prototype
Dog.prototype = {
  species: "Canis Canis",
  speak: function() { return "Woof! I'm " + this.name; }
}

// Our objects work just as they did before!
var spot = new Dog("Spot", "Beagle");
var rufus =  new Dog("Rufus", "Poodle");

spot.name // "Spot"
spot.breed // "Beagle"
spot.bark() // "Hello! I'm Spot"

rufus.name // "Rufus"
rufus.bark() // "Hello! I'm Rufus"
```

![Prototype Chain Diagram - Simple](images/prototype_chain_simple.jpg)

**Note**: `.prototype` is a property on constructor functions, while `.__proto__` is the
property on objects, and is used in the lookup chain.

### You-Do Exercise: Car Constructor with Prototypes (10 min)

Update the constructor function for our car to define the methods on prototypes
rather than on the individual instances themselves.

See `car.js` for a solution.

## Break (10 min)

### You-Do Exercise: Monkey (20 min)

Work on the [OOP Monkey Exercise](https://github.com/ga-dc/oop_monkey/tree/js)

Make sure you check out the `javascript-constructor-functions` branch before beginning!

`$ git checkout javascript-constructor-functions`

## Inheritance (10 min)

In general, we're not going to be using inheritance with javascript, but it's
worth just mentioning how it can be accomplished.

```js
////////////////////////////////
// Animal (Parent) Class
////////////////////////////////
function Animal( name ){
  this.name = name;
}

Animal.prototype.kingdom = "Animalia";
Animal.prototype.breathe = function() {console.log("Inhale... exhale...")};


////////////////////////////////
// HELLO THIS IS DOG
////////////////////////////////
function Dog(name, breed){
  this.name = name;
  this.breed = breed;
}

// Important! Set up the link in the prototype chain connecting Dogs to Animals
Dog.prototype = new Animal();

// Add any methods / properties shared by all dogs.
Dog.prototype.bark = function(){ console.log("Woof")};
Dog.prototype.species = "Canis canis"

////////////////////////////////
// Testing our dawgs
////////////////////////////////
var spot = new Dog("Spot", "Beagle");

// from Animal prototype
spot.kingdom;
spot.breathe();

// from Dog prototype
spot.bark();
spot.species;

// from Dog properties
spot.name;
spot.breed;
```

![Prototypal Inheritance Chain](images/prototype_chain_inheritance.jpg)

## Object.create (5 min)

So we won't use this often (if ever in this class), but you may see examples
of using `Object.create`. This method creates a new object, and sets the
prototype of the new object to be the existing object.

An example might help:

```js
var Person = {
  species: "Homo Sapiens",
  speak: function() { console.log("Hello")}
};

var me = Object.create(Person);

me.speak();
me.species;
```

Just like before, we can use `this` inside our methods:

```js
var Person = {
  species: "Homo Sapiens",
  speak: function() { console.log("Hello! I'm " + this.name)}
};

var me = Object.create(Person);
me.name = "Adam";

me.speak()
me.species
```

## Closing (5 min)

### Sample Quiz Questions

* What are the benefits to using an OOP approach to programming?
* What are constructor functions and the new keyword? How are they related?
* What is a prototype? Why do we care about them?
* Describe how we use prototypes to set up inheritance in JS
* Why would we use a constructor function over an object literal?

### Additional Resources:

* [Eloquent JavaScript Chapter 6: The Secret Life of Objects](http://eloquentjavascript.net/06_object.html)
* [Raganwald: Prototypes Article](http://raganwald.com/2013/02/10/prototypes.html)
* [W3Schools](http://www.w3schools.com/js/js_object_definition.asp)
* [CSS Tricks: Understanding-Javascript-Constructors](https://css-tricks.com/understanding-javascript-constructors/)
