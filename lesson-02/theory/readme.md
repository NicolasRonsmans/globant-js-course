
# Lesson 2: OOP, Design patterns & Intro to jQuery

## Introduction to OOP on JS

## Standard built-in objects
JavaScript has several objects included in its core, for example, there are objects like Math, Object, Array, and String.

## Custom objects
JavaScript uses functions as classes.
Defining a class is as easy as defining a function. In the example below we define a new class called Person.

```javascript
function Person() {
}
```

To create a new instance of an object **Person** we use the statement new **Person**, assigning the result (which is of type obj) to a variable to access it later. 

```javascript
function Person() {
}

var father = new Person();
var son = new Person();
```

## The constructor:
Is not need to explicitly define a constructor method, because is called at the moment of instantiation.
The constructor is used to set the object's properties or to call methods to prepare the object for use. 

```javascript
var Person = function Person(firstName, lastName) {
  console.log('Initializing ' + firstName + ' ' + lastName);
}

var father = new Person('Homero', 'Simpson');
var son = new Person('Bart', 'Simpson');
```

## The properties:
Properties are variables contained in the class; every instance of the object has those properties. Properties are set in the constructor (function) of the class so that they are created on each instance.

Working with properties from within the class is done using the keyword **this**, which refers to the current object.

```javascript
var Person = function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

var father = new Person('Homero', 'Simpson');
var son = new Person('Bart', 'Simpson');

console.log(father.firstName);
console.log(son.firstName);
``` 

## Methods:
Methods follow the same logic as properties; the difference is that they are functions and they are defined as functions. 
Calling a method is similar to accessing a property, but you add **()** at the end of the method name, possibly with arguments.

In JavaScript methods are regular function objects that are bound to an object as a property, which means they can be invoked **"out of the context"**. 

```javascript
var Person = function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function() {
  console.log(this.firstName + ' ' + this.lastName);
}

var father = new Person('Homero', 'Simpson');
var son = new Person('Bart', 'Simpson');

father.getFullName();
son.getFullName();
```

## Inheritance & Encapsulation:
```javascript
var Person = function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function() {
  console.log(this.firstName + ' ' + this.lastName);
}

var Fireman = function Fireman(firstName, lastName, age) {
  // calling to constructor root
  Person.call(this, firstName, lastName);
  
  this.age = age;
}

// Inherits from the Person class
Fireman.prototype = Object.create(Person.prototype);

// We assigned our child class constructor
Fireman.prototype.constructor = Fireman;

// we add a method to the child class
Fireman.prototype.getAge = function getAge() {
  console.log(this.age);
}

var crazyFireman = new Fireman('Flavio', 'Carreño', 32);
crazyFireman.getAge();
crazyFireman.getFullName();
```

## Polymorphism:
```javascript
var Person = function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function() {
  console.log(this.firstName + ' ' + this.lastName);
}

var Fireman = function Fireman(firstName, lastName, age) {
  // calling to the root constructor
  Person.call(this, firstName, lastName);
  
  this.age = age;
}

// Inherits from the Person class
Fireman.prototype = Object.create(Person.prototype);

// We assign the constructor to our child class
Fireman.prototype.constructor = Fireman;

// Add a method to the child class
Fireman.prototype.getAge = function getAge() {
  console.log(this.age);
}

// We redefined the getFullName method
Fireman.prototype.getFullName = function getFullName() {
  console.log(this.lastName + ', ' + this.firstName);
}

var crazyFireman = new Fireman('Flavio', 'Carreño', 32);
crazyFireman.getAge();
crazyFireman.getFullName();
```

**Referencias**
[Introduction to Object-Oriented JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript) 
[OOP In JavaScript: What You NEED to Know](http://javascriptissexy.com/oop-in-javascript-what-you-need-to-know/) 
[Object-Oriented Javascript](http://nefariousdesigns.co.uk/object-oriented-javascript.html)

----------
## Desing patterns

 - Design patterns are reusable solutions to commonly occurring problems in software design.
 - Design patterns also provide us a common vocabulary to describe solutions.
 - A pattern usually reflects an out of the box solution that can be adapted to suit our own needs.

#### A simple example
Imagine that we have a script where for each DOM element found on a page with class "foo" we wish to increment a counter.
We have 3 different ways to do:

 1. Select all of the elements in the page using regular expressions
 2. Use a modern native browser feature such as `querySelectorAll()` to select all of the elements.
 3. Use a native feature such as `getElementsByClassName()` to similarly get back the desired collection.

**Which is the best solution?**
Developers using jQuery don't have to worry about this problem however, as it's luckily abstracted away for us using the *Facade* pattern. 

Behind the scenes, the library simply opts for the most optimal approach to selecting elements depending on what our current browser supports and we just consume the abstraction layer.

We're probably all also familiar with jQuery's `$("selector")`. This is significantly more easy to use for selecting HTML elements on a page versus having to manually opt for `getElementById()`, `getElementsByClassName()`, `getElementByTagName` and so on.

### Creational Design Patterns ###
This patterns focus on handling object creation mechanisms where objects are created in a manner suitable for the situation we're working in. 

 - Factory
 - Builder
 - Prototype
 - Singleton

### Structural Design Patterns ###
This patterns are concerned with object composition and typically identify simple ways to realize relationships between different objects.

 - Adapter
 - Decorator
 - Facade
 - Proxy

### Behavioral Design Patterns ###
This patterns focus on improving or streamlining the communication between disparate objects in a system.

 - Iterator
 - Mediator
 - Observer
 - Strategy

----------

## JavaScript Design Patterns ##

### The Constructor Pattern ###

**Object creation**
The three common ways to create new objects in JavaScript are as follows:

```javascript
    // preferred, object literal
    var person = {
	    name: "Pepe"
    };
    
    var person = Object.create(Object.prototype);
    
    // antipattern
    var person = new Object();
    person.name = "Pepe";
```

**Basic constructors**
As we saw earlier, JavaScript doesn't support the concept of classes but it does support special constructor functions that work with objects.

```javascript
function Person(name, lastName) {
	this.name = name;
	this.lastName = lastName;
	
	this.getFullName = function () {
		return this.name + " " + this.lastName;
	};
}

// usage
var driver = new Person("Cosme", "Fulanito");
console.log(driver.getFullName());
```

**Constructors with prototypes**
Functions, like almost all objects in JavaScript, contain a "prototype" object. 

```javascript
function Person(name, lastName) {
	this.name = name;
	this.lastName = lastName;
}

Person.prototype.getFullName = function () {
	return this.name + " " + this.lastName;
};

// usage
var driver = new Person("Cosme", "Fulanito");
console.log(driver.getFullName());
```

### The module pattern ###
Modules are an integral piece of any robust application's architecture and typically help in keeping the units of code for a project both cleanly separated and organized.

**Object literals**
Object literals don't require instantiation using the `new` operator.

```javascript
var Person = {
	name: "Cosme",
	lastName: "Fulanito",
	
	getFullName: function() {
		return this.name + " " + this.lastName;
	}
};

// usage
Person.name = "Pepe";
console.log(Person.getFullName());
```

**Module pattern**
The Module pattern was originally defined as a way to provide both private and public encapsulation for classes in conventional software engineering.

In JavaScript, the Module pattern is used to further emulate the concept of classes in such a way that we're able to include both public/private methods and variables inside a single object, thus shielding particular parts from the global scope.

```javascript
var Person = (function () {
	var _name = "Cosme",
		_lastName = "Fulanito";
		
	var	_getFormalName = function () {
			return _lastName + ", " + _name;
		};
	
	return {
		getFullName: function () {
			return _name + " " + _lastName;
		},
		getFormalName: function () {
			return _getFormalName();
		}
	}
})();

// usage
console.log(Person.getFullName());
console.log(Person.getFormalName());
```
**Variations**
*Import mixins*

Demonstrates how globals can be passed in as arguments to our module's anonymous function. This effectively allows us to import them and locally alias them as we wish.
```javascript
var name = "Cosme";
var lastName = "Fulanito";
var Person = (function (n, ln) {
	var _name = n,
		_lastName = ln;
		
	var	_getFormalName = function () {
			return _lastName + ", " + _name;
		};
	
	return {
		getFullName: function () {
			return _name + " " + _lastName;
		},
		getFormalName: function () {
			return _getFormalName();
		}
	}
})(name, lastName);

// usage
console.log(Person.getFullName());
console.log(Person.getFormalName());
```

*Export*

Allows us to declare globals without consuming them and could similarly support the concept of global imports.

```javascript
var Person = (function () {
	var objPerson = {};
	
	// Private variables
	var _name = "Cosme",
		_lastName = "Fulanito";

	// Private method
	function _getFormalName() {
		return _lastName + ', ' + _name;
	};
	
	// Public methods
	objPerson.getFullName = function() {
		return _name + ' ' + _lastName;
	};
	
	objPerson.getFormalName = function() {
		return _getFormalName();
	};
	
	return objPerson;
})();

// usage
console.log(Person.getFullName());
console.log(Person.getFormalName());
```

###The Singleton Pattern###

The Singleton pattern is thus known because it restricts instantiation of a class to a single object. 

```javascript
var Person = (function (){
	var _instance,
		_name = "Cosme",
		_lastName = "Fulanito";

	var objPerson = function() {
		if(_instance){
			return _instance;
		}
		
		this.getFullName = function() {
			return _name + ' ' + _lastName;
		}
		
		this.setName = function(n) {
			_name = n;
		}
		
		_instance = this;
	};
	
	return objPerson;
})();

// usage
var p1 = new Person();
var p2 = new Person();

p2.setName('Pepito');

console.log(p2.getFullName());
console.log(p1.getFullName());
console.log(p1 === p2);
```

###The Observer Pattern###

Is a publish/subscribe pattern which allows a number of observer objects to see an event,  automatically notifying them of any changes to state.

```javascript
var pubsub = {};

(function(myObject) {
 
    // Storage for topics that can be broadcast 
    // or listened to
    var topics = {};
 
    // An topic identifier
    var subUid = -1;
 
    // Publish or broadcast events of interest
    // with a specific topic name and arguments
    // such as the data to pass along
    myObject.publish = function(topic, args) {
        if (!topics[topic]) {
            return false;
        }
 
        var subscribers = topics[topic],
            len = subscribers ? subscribers.length : 0;
 
        while (len--) {
            subscribers[len].func(topic, args);
        }
        return this;
    };
 
    // Subscribe to events of interest
    // with a specific topic name and a
    // callback function, to be executed
    // when the topic/event is observed
    
    myObject.subscribe = function(topic, func) {
        if (!topics[topic]) {
            topics[topic] = [];
        }
 
        var token = (++subUid).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };
 
    // Unsubscribe from a specific
    // topic, based on a tokenized reference
    // to the subscription
    myObject.unsubscribe = function(token) {
        for (var m in topics) {
            if (topics[m]) {
                for (var i = 0, j = topics[m].length; i < j; i++) {
                    if (topics[m][i].token === token) {
                        topics[m].splice( i, 1 );
                        return token;
                    }
                }
            }
        }
        return this;
    };
}(pubsub));

// usage
var messageLogger = function (topics, data) {
    console.log("Logging: " + topics + ": " + data);
};

var subscription = pubsub.subscribe("inbox/newMessage", messageLogger);

pubsub.publish("inbox/newMessage", "hello world!");
pubsub.publish("inbox/newMessage", ["test", "a", "b", "c"]);

pubsub.unsubscribe(subscription);

pubsub.publish("inbox/newMessage", "Hello! are you still there?");
```

###Revelation Pattern###
It is about having private methods, which you also expose as public methods.

```javascript
var Person = (function (){
	var _name = "Cosme",
		_lastName = "Fulanito";

	function _getFullName() {
		return _name + ' ' + _lastName;
	};

	function _getFormalName() {
		return _lastName + ', ' + _name;
	};

	return {
		getFullName: _getFullName,
		getFormalName: _getFormalName
	}

})();

// usage
console.log(Person.getFormalName());
console.log(Person.getFullName());
```

###Facade Pattern###
Provides a simplified interface to a large body of code.

```javascript
var mobileEvent = {
	// ...
	stop:function (e) {
		e.preventDefault();
		e.stopPropagation();
	}
	// ...
};

// usage
var link = document.querySelector('#someAnchorId');
link.addEventListener('click', function(e){
	mobileEvent.stop(e);
});
```

###The Prototype Pattern###


----------

**References:**
[Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/) 
[JS Patterns](http://shichuan.github.io/javascript-patterns/)


----------
#Introduction to jQuery#

jQuery is just a JavaScript library, or set of helpful add-ons, to the JavaScript programming language. 

Makes it easy to manipulate a page of HTML after it's displayed by the browser. It also provides tools that help you listen for a user to interact with your page, tools that help you create animations in your page, etc.

jQuery provides a simple interface for the underlying JavaScript.

----------

## Selectors

A selector is a function which makes use of expressions to find out matching elements from a DOM based on the given criteria. Simply you can say, selectors are used to select one or more HTML elements. Once an element is selected then we can perform various operations on that selected element.

For a full list of all jQuery selectors, see the following page in the official jQuery documentation: 
[jQuery Selectors](http://api.jquery.com/category/selectors/)

A jQuery selector is a string which specifies which HTML elements to select. The selector string is passed to the $() or jQuery() selection function which returns a collection of the selected elements.
The selector syntax is the same as CSS selectors.

jQuery lets you select elements based on the following criteria:

 - Element name
 - Element id
 - Element CSS class
 - Element attributes
 - Element visibility
 - Element order
 - Form Fields
 - Element parents or children
 - Combinations of the above

###Others selection methods:

**Find:** The selection set returned by the `$()` or `jQuery()` functions contains a function called `find()`. The `find()` function can be used to find descendants of elements in the selection set.

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>find demo</title>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    </head>
    <body>
    <p class="findExample first">Morbi luctus <strong>lorem in nulla</strong> varius, sit amet egestas felis consectetur.</p>
    
    <p class="findExample second">Duis fermentum euismod orci, nec accumsan velit iaculis quis. <strong>Morbi</strong> a feugiat arcu, ac sagittis nunc. <strong>Curabitur</strong> nec lacinia <strong>diam</strong>.</p>
    </body>
    </html>

Example:

    $('p.findExample.second').find('strong').css('color','blue');

**Parent:** Get the parent of each element in the current set of matched elements, optionally filtered by a selector.
This method is similar to `.parents()`, except `.parent()` only travels a single level up the DOM tree.

Example: 

    $('strong').parent('.first').css('color', 'red');

**Parents:** Get the ancestors of each element in the current set of matched elements, optionally filtered by a selector.

Example:

    $('strong').parents().css('border', '1px solid blue');

**Children:** Get the children of each element in the set of matched elements, optionally filtered by a selector.

Example:

    $('p').children('strong').css('color', 'pink');

###Shortcut selectors

**Fom selectors:** jQuery contains a set of form field selectors which makes it easier to select the various different types of form fields from the DOM.

The most common field selectors are:

 - :input
 - :text
 - :radio
 - :checkbox
 - :button

Usage:

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>find demo</title>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
      <style>
      .font-xxs {font-size: 12px;}
      .font-xs {font-size: 14px;}
	  .font-md {font-size: 18px;}
	  .color-red {color: red;}
	  .color-blue {color: blue;}
	  .color-green {color: green;}
      </style>
    </head>
    <body>

     <form method="post">
		 <fieldset>
			 <legend>Personal data</legend>
			 <label class="font-xs color-blue">First Name<input type="text" name="firstName" placeholder="Your first name"></label>
			 <label class="font-xs color-blue">Last Name<input type="text" name="lastName" placeholder="Your last name"></label>
		 </fieldset>
		 <fieldset>
			 <legend>Preferences</legend>
			 <label class="font-md color-red"><input type="checkbox" name="preference" value="movies"> Movies</label>
			 <label class="font-md color-red"><input type="checkbox" name="preference" value="music" checked> Music</label>
			 <label class="font-md color-red"><input type="checkbox" name="preference" value="dance"> Dance</label>
		 </fieldset>
		 <input type="submit" value="Submit">
     </form>    

    </body>
    </html>

Example:

    $(':text').css('background-color', 'gray');

*Reference:* [jQuery Selectors](http://api.jquery.com/category/selectors/)

----------

##Accessing to DOM:

JQuery provides methods to manipulate DOM in efficient way. You do not need to write big code to modify the value of any element's attribute or to extract HTML code from a paragraph or division.

We can remove, add or replace a complete DOM element with the specified HTML or DOM elements.

###DOM Manipulation Methods:

**after():**  Inserts HTML after the selected element (outside the element).

Example: 
`$(':text[name="firstName"]').after('*');`

**addClass() / removeClass():** Adds / remove a single class, multiple classes, or all classes from each element in the set of matched elements.

Examples: 

    $('.font-xs').addClass('color-green');
    $('label').removeClass('color-blue');

**attr():** Get the value of an attribute for the first element in the set of matched elements or set one or more attributes for every matched element.

Example: 

    var lastNamePlaceholder = $(':text[name="lastName"]').attr('placeholder');
    console.log(lastNamePlaceholder);
    
    $(':text[name="firstName"]').attr('placeholder', 'Please, your first name');

**append():** Inserts new HTML into the end of the selected HTML element. The new HTML is concatenated with the HTML the element had already.

Example:

    $('fieldset').append('<p>Pellentesque euismod nunc non convallis mattis.</p>');

**before():** Inserts HTML before the selected element (outside the element).

Example: 
`$(':text').before(': ');`

**detach():** Is the same as `.remove()`, except that `.detach()` keeps all jQuery data associated with the removed elements. This method is useful when removed elements are to be reinserted into the DOM at a later time.

Example:

    // selector caching
    var $firstNameField = $(':input[name="firstName"]').parent();
    
    // remove from DOM
    $firstNameField.detach();
    
    // remove color blue class and add new class
    $firstNameField.removeClass('color-blue').addClass('color-red');
    
    // re-insert into DOM
    $(':text[name="lastName"]').parent().after($firstNameField);

**empty():** Removes all child elements of the selected HTML element.

Example: 
`$('label.font-xs').empty();`

**prop():** Get the value of a property for the first element in the set of matched elements or set one or more properties for every matched element. It should be used when properties contain only two possible values true or false, such as "checked", "selected", "disabled", etc.

Example:

    // selectors caching
    var $musicField = $(':checkbox[value="music"]');
    var $lastNameField = $(':text[name="lastName"]');
    
    // return checked property
    console.log($musicField.prop('checked'));
    
    // disable last name field
    $lastNameField.prop('disabled', true);

**remove():** Remove the set of matched elements from the DOM. Use `.remove()` when you want to remove the element itself, as well as everything inside it.

Example: 
`$('label.font-xs').remove();`

**removeAttr():** Remove an attribute from each element in the set of matched elements.

Example: 
`$(':text[name="firstName"]').removeAttr('placeholder');`

**wrap():** The `wrap()` method can wrap the selected HTML element in another HTML element.

Example: 
`$('label.font-xs').wrap('<div>');` 

*Reference:* [jQuery Manipulation](http://api.jquery.com/category/manipulation/)

---

##Events
jQuery makes it straightforward to set up event-driven responses on page elements. These events are often triggered by the end user's interaction with the page, such as when text is entered into a form element or the mouse pointer is moved. 

In some cases, such as the page load and unload events, the browser itself will trigger the event.

We could define six major groups related to:

 1. Browser
 2. Document
 3. Handler Attachment
 4. Form
 5. Keyboard
 6. Mouse

###Browser events

**error():** The `error()` event is sent to elements, such as images, that are referenced by a document and loaded by the browser. It is called if the element was not loaded correctly.

Example HTML:

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>find demo</title>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
      <style>
    	  img {
    		width: 85px;
    		height: 85px;
    		border: 1px solid #E1E1E1;
    		padding: 5px;
    	}
      </style>
    </head>
    <body>
    
    <img alt="Some image" src="fhfkh.gif">
    <img alt="Some image" src="https://www.google.com.ar/images/srpr/logo11w.png">
    <img alt="Some image" src="fhfkh.gif">
    
    </body>
    </html>

Usage:

    $('img').error(function () {
      $(this).attr('src', 'https://browshot.com/static/images/not-found.png');
    });

**resize():** The `resize` event is sent to the window element when the size of the browser window changes.

Example: 

    $(window).resize(function () {
    	console.log('window size changed!');
    });

**scroll():** The scroll event is sent to an element when the user scrolls to a different place in the element.

###Document events

**load():** The `load` event is sent to an element when it and all sub-elements have been completely loaded. This event can be sent to any element associated with a URL: images, scripts, frames, iframes, and the window object. **Deprecated: use .on('load', handler)**

**ready():** The document ready event signals that the DOM of the page is now ready, so you can manipulate it without worrying that parts of the DOM has not yet been created. The document ready event fires before all images etc. are loaded, but after the whole DOM itself is ready.

Usage:

    $(document).ready(function () {
    	console.log('DOM is fully loaded');
    });

**unload():** The `unload` event is sent to the window element when the user navigates away from the page. **Deprecated: use .on('unload', handler)**

###Handler Attachment

 - on()
 - off()
 - one()
 - trigger()
 - jQuery.proxy()

###Form events

 - blur()
 - change()
 - focus()
 - select()
 - submit()

###Keyboad events

 - keydown()
 - keypress()
 - keyup()

###Mouse events

 - click()
 - hover()
 - dbclick()
 - mouserover()

events / on / off / one / proxy / trigger

---

modifiers css / attr / prop / data / toggle

---

ajax / post / get / shortcuts

---

utils parse / extend / each / 

----------
**References:**
[jQuery Documentation](http://api.jquery.com/)
