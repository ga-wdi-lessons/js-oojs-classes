# Object Oriented Javascript and Classes

## Learning Objectives

- Explain the importance of OOJS
- Describe the role of ES2015 Classes and how they work
- Use the `new` keyword to create objects with shared properties
- Create a class that inherits from another using the `extends` and `super` keywords

## Framing (10 minutes / 0:10)

We've already gotten exposure to Javascript objects using object literal notation (i.e., the curly brackets). Some of you might have had something like this in your first project...

```js
const game = {
  cards: document.querySelectorAll(".cards"),
  startingTime: 0,
  createBoard: function(){
    let gameBoard = document.querySelector("#game-board")
    gameBoard.style.display = "inline";
  }
};
```

What's nice about the above code snippet? Try answering that question by comparing it to this...

```js
let cards = document.querySelectorAll(".cards");
let startingTime = 0;
function createBoard(){
  let gameBoard = document.querySelector("#game-board")
  gameBoard.style.display = "inline";
}
```

<details>

  <summary><strong>Some thoughts...</strong></summary>

  > * Related properties and methods are packaged together.
  > * Fewer global variables.
  > * Readability.

</details>

#### How have we been writing code up until this point?

We have been writing **procedural code**, which basically means we are writing and executing code as we need it. We'll define some variables and functions here, maybe some event listeners there. We end up with a lot of separate pieces that contribute to the overall functionality of an application.

#### What is an object in programming?

An object encapsulates related data and behavior in an organized structure.

#### Why might we use an OOP approach to programming?

Object-oriented programming (OOP) provides us with opportunities to clean up our procedural code and model it more-closely to the external world.

OOP helps us to achieve the following...
  * Abstraction
  * Encapsulation
  * Modularity

OOP becomes **very** important as our front-end code grows in complexity. Even a simple app will have lots of code on the front end to do
things like...

* Render data from the back end
* Update the state of the page as data changes
* Respond to events like clicking buttons / links
* Send requests to the back end to fetch / update / destroy data

### Creating Objects (5 minutes / 0:15)

So far, we've had to make our objects 'by hand' (i.e. using object literals)...

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

Even though we're technically using objects to organize our code, we can see a noticeable amount of duplication. Just imagine if we needed a hundred cars in our app! Our code would certainly not be considered "DRY".

As you may have noticed, some of these properties change between cars (`model` and `color`), and others stay the same. For example, `fuel` starts at 100, while the `drive` and `refuel` functions are the same for every car.

Why don't we build a function that makes these objects for us!

### You Do Exercise: Create a `makeCar` Function (10 minutes / 0:25)

> 5 minutes exercise. 5 minutes review.

Define a function `makeCar` that takes two parameters - `model` and `color` - and returns an object literal representing a car using those params.

```js
// This should return a car object just like the previous example
var celica = makeCar("Toy-Yoda Celica", "limegreen");
```

This is the basic idea behind OOP: we define a blueprint for an object and use it to generate multiple instances of it!

## Classes

### Overview (10 minutes / 0:35)

It's so common that we need to make objects with similar properties and methods that programming languages usually have some features to help with this.

In Javascript, we use an ES6 feature called **classes** to accomplish this. Here's a class that serves the same purpose as our car blueprint. We'll dive into the details in just a bit...

```js
class Car = {
  constructor(model, color){
    this.model = model;
    this.color = color;
    this.fuel = 100;
  }
  drive(){
    this.fuel--;
    return "Vroom!";
  }
  refuel(){
    this.fuel = 100;
  }
}

const celica = new Car("Toy-Yoda Celica", "limegreen");
const civic = new Car("Honda Civic", "lemonchiffon");
```

Classes are a lot like the `makeCar` function we just created, but they're supported by JS and we use the `new` keyword to generate instances of an object (just like our earlier `celica` and `civic` examples).

> Note that classes start with a capital letter to make it obvious
that they are constructors. This isn't necessary, but is a convention you should follow.

### I Do: Make a Person Class (10 minutes / 0:45)

```js
class Person {
  // We use the constructor method to initialize properties for a class instance.
  // It takes whatever arguments we want to pass into an instance.
  constructor(initialName){
    this.name = initialName;
    this.species = "Homo Sapiens";
  }
  // We define any methods accessible to an instance outside of the constructor
  speak(){
    return `Hello! I'm ${this.name}`;
  }
}

const adrian = new Person("Adrian");
adrian.speak(); // "Hello, I'm Adrian"
```

#### `this`

Notice the use of `this`, and the fact that we don't return from the class. Here's why we write classes this way...

When we generate a class instance using `new`, Javascript will automatically...
  1. Create an new, empty object for us  
  2. Generate a context for that object (`this` -> the new object)  
  3. Return the object  

#### Where are the Commas?

Unlike object notation, you do not need to use commas when separating class methods.

### You Do: Make an ATM Class (20 minutes / 1:05)

> 15 minutes exercise. 5 minutes review.

```bash
$ touch index.html script.js  # create html and js files
$ open index.html # open index.html in the browser so you can test your code in the console
$ atom . # open the files in Atom and start coding
```

For this exercise you will be creating an ATM class.

It will have the following properties...
* `type` (e.g., "checking"), which should be determined by some input
* `money`, which should start out as `0`

It should have the following methods...
* `withdraw`, which should increase the amount of money by some input
* `deposit`, which should decrease the amount of money by some input
* `showBalance`, which should print the amount of money in the bank to the console.

The `Atm` class has a `transactionHistory` property which keeps track of the withdrawals and deposits made to the account.
* Make sure to indicate whether the transaction increased or decreased the amount of money in the bank.

#### Bonus

Give the `Atm` class a `backupAccount` property that can, optionally, contain a reference to another instance of the class, determined by some input
* Whenever an ATM's balance goes below zero, it will remove money from the instance stored in `backupAccount` so that its balance goes back to zero.
* This should trigger a withdrawal in the back up account equal to the amount of money that was withdrawn from the original account.

<details>
  <summary><strong>Solution</strong></summary>

  ```js
  class Atm {
    constructor(type){
      this.type = type;
      this.money = 0;
      this.transactionHistory = [];
    }
    withdraw(amount){
      this.money -= amount;
      this.transactionHistory.push(-amount)
    }
    deposit(amount){
      this.money += amount;
      this.transactionHistory.push(amount)
    }
    showBalance(){
      console.log("The current balance is:", this.money);
    }
  }

  let savings = new Atm("savings");
  ```

</details>

<details>
  <summary><strong>Solution (with Bonus)</strong></summary>

  ```js
  class Atm {
    constructor(type, backup=null){
      this.type = type;
      this.money = 0;
      this.transactionHistory = [];
      this.backupAccount = backup;
    }
    withdraw(amount){
      this.money -= amount;
      this.transactionHistory.push(-amount);
      if(this.money < 0){
        console.log("Dipping into savings!");
        let difference = -this.money;
        this.money = 0;
        this.backupAccount.withdraw(difference);
      }
    }
    deposit(amount){
      this.money += amount;
      this.transactionHistory.push(amount);
    }
    showBalance(){
      console.log("The current balance is:", this.money);
    }
  }

  let savings = new Atm("savings");
  let checking = new Atm("checking", savings);
  ```

</details>

## Break (10 minutes / 1:15)

## Inheritance (15 minutes / 1:30)

Although OOP can help us keep our Javascript nice and clean, it's still easy to duplicate code when defining multiple classes. Consider the following example...

<!-- AM: Insert blurb about what relationship Animal, Dog, Cat have, pre-code -->

```js
class Dog {
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.waggingTail = true;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}

class Cat {
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.numLives = 9;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
  meow(){
    return `Meow! I am not a dog! My name is ${this.name}`
  }
}
```

Here we have two classes: `Dog` and `Cat`. They have some things in common: `name`, `breed`, `diet` and `eat`. They do differ, however, in that one `bark`s and the other `meow`s.

Imagine that we had to create a number of other classes - `Horse`, `Goat`, `Pig`, etc. - all of which share the same aforementioned properties but also have methods that are particular to the class.

How could we refactor this so that we don't have to keep writing out the shared class properties and methods. Enter **inheritance**...

```js
class Animal{
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
}

const dog = new Animal("Fido", "Beagle");
```

Here we've defined an `Animal` class. It contains the properties and methods that are common among specific animal classes. Wouldn't it be nice if `Dog` and `Cat` could just reference this "parent" `Animal` class so that the only things we need to put in their "child" class definitions are the properties and methods that are particular to them (e.g., `bark`, `meow`).

Lucky for us, we can do that...

```js
class Animal {
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(diet);
  }
}

class Dog extends Animal {
  constructor(name, breed, tail){
    this.waggingTail = true;
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}

class Cat extends Animal {
  constructor(name, breed, lives){
    this.numLives = lives;
  }
  meow(){
    return `Meow! I am not a dog! My name is ${this.name}`
  }
}
```

The clincher is `extends`. Whatever class is to the left of the `extends` keyword should inherit the properties and methods that belongs to the class to the right of the keyword. Let's see if this works...

```js
// Let's test out our parent. It just needs a name and breed.
const goat = new Animal("Gregory", "Mountain Goat");

// And now the children.
const fido = new Dog("Fido", "Beagle", true);
console.log(fido); // "dog is not defined"
```

That didn't work out the way we expected. That's because we're forgetting one thing. When creating an instance of a child class, we need to make sure it invokes the constructor of the parent (`Animal`) class.

We can do that using the keyword `super()`...

```js
class Dog extends Animal {
  constructor(name, breed, tail){
    super(name, breed);
    this.waggingTail = true;
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}
```

`super()` calls the constructor of the parent class. In the above example, once `super` does what it needs to do, it then runs through the rest of `Dog`s constructor.

> In order to give an instance of a child class context (i.e., be able to use `this`), you must call `super`.

### You Do: Inheritance (20 minutes / 1:50)

> 15 minutes exercise. 5 minutes review.

#### Create a `User` class.

It should have the following properties...
* `username`, determined by some input
* `password`, determined by some input

It should have the following methods...
* `changePassword`, which allows a user to change his password to some other string

#### Create an `Admin` class.

It should inherit from `User`. An admin has the same properties and can run the same methods a user can.

It should also have the following properties...
* `accessLevel`, which is an arbitrary integer determined by some input

It should also have the following methods...
* `overridePassword`, which should take another user and a new password as an argument. When executed, this method changes the password for the passed-in user.

#### Bonus

`Admin` should also have a `deleteUser` method, which takes a user instance as an input and then **DELETES IT FROM EXISTENCE**.

<details>
  <summary><strong>Solution</strong></summary>

  ```js
  class User {
    constructor(username, password){
      this.username = username;
      this.password = password;
    }
    changePassword(newPassword){
      this.password = newPassword;
    }
  }

  class Admin extends User {
    constructor(username, password, accessLevel){
      super(username, password);
      this.accessLevel = accessLevel;
    }
    overridePassword(user, newPassword){
      user.changePassword(newPassword);
    }
  }

  let normalUser = new User("john", "123");
  let superUser = new Admin("webmaster", "w00t", 100);
  ```

</details>

<details>
  <summary><strong>Solution (with Bonus)</strong></summary>

  

</details>

### Closing / Questions (10 minutes / 2:00)

* What are the benefits to using an OOP approach to programming?
* What is a class? What is `new`? How are they related?
* What does it mean to use "inheritance" when working with classes?
* How do we indicate that one class inherits from another?
* What does `super` mean?

### Additional Resources

* [Eloquent JavaScript Chapter 6: The Secret Life of Objects](http://eloquentjavascript.net/06_object.html)
* [Raganwald: Prototypes Article](http://raganwald.com/2013/02/10/prototypes.html)
* [W3Schools](http://www.w3schools.com/js/js_object_definition.asp)
* [CSS Tricks: Understanding-Javascript-Constructors](https://css-tricks.com/understanding-javascript-constructors/)
* [JavaScript Prototype in Plain Language](http://javascriptissexy.com/javascript-prototype-in-plain-detailed-language/)

<!-- AM: Need to update with ES6 class resources -->
