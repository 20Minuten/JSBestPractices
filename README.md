# Javascript Best Practices - v.ES6

The Golden Rule for Best Practices in any organisation is  ___Monkey see. Monkey do.___

## Best Practices Rule Number One
__Sweat the small stuff.__

Be precise. Be Pedantic. Demand absolute perfection in everything that you do. From yourself and from others.

You won't get there, but will never become the best programmer you can be if you aren't determined to fail and fail again in your constant quest for perfection

## All the other Rules
Javascript is untyped. Keep it this way.

Don't try any clever optimisation. The compiler is almost certainly better at it than you are.

## Chapter 1
### Use JavaScript
A variable is a placeholder for a value.
A function is a block of code made up of one or more statements or operations which work together to do a predictable something.
An object is a collection of variables which are called "Object attributes" and functions which are called "Object methods"
#### Native Methods
Always use native methods when they are available. See "Preoptimisation" at the bottom of the page.
##### JQuery
JQuery comes with the site, but hey who knows if we will take it out one day.  Yeah ok, it's unlikely, but when you know that JQuery is wrapping a JavaScript native function (or method, naturally), don't use the JQuery. See  Chapter 3 on "JQuery and other LIbraries" near the bottom of this page.
##### Use ES6
We now babelify everything. Use ES6 where it is appropriate. If you don't know where it is appropriate, keep reading.
   * Step 1: Make sure you are using strict everywhere. No exceptions
   * Step 2. Turn on JSLINT / ESLINT Keep it on. Put it on it's nastiest settings, and never turn it off.
   * Step 3. Write ES6.

If it's good enough for NASA, it's good enough for us. (Rule number 10).

http://pixelscommander.com/wp-content/uploads/2014/12/P10.pdf

### Formatting

We LINT on the build. If code doesn't pass lint, it doesn't get built. If it doesn't get built, you can't commit.

Almost all of what follows in this section is built into the linting rules!

#### Object Shapes
Objects should always be declared in exactly the form they are going to be used, at the smallest scope, and must never be mutated. See the NASA rules number 6.

You can modify an object values with blistering speed in JS, but as soon as you change the shape of an object, the JIT compiler has to rebox it in every single reference.

Fix the shape of the object at the top, when you define it, never ever modify it and you're giving the compiler a massive helping hand.

__This__ will always be fast:
```javascript
var myObject = {
	thinga: 12,
	thingb: undefined
}
```
 ```javascript
myObject.thingb = "hello world"
```
__This__ has the potential to slow things down a lot: (in production code, like when it's being used and stuff)
```javascript
var myObject = {
	thinga: 12
}

myObject.thingb = "hello world"

/** myObject has now changed shape and looks like this:
> myObject = {
	thinga: 12,
	thingb = "hello world"
}
**/
```

If you want to know more about this, there are books on the iPads.
_(we have iPads with extensive, constantly updated libraries on them. People are strongly encouraged to take them with them home or on holiday for some light reading :) )_
#### Whitespace
Set up your code editor to expand all tabs to four spaces. Sure, some people prefer two spaces. Some crazies even like eight. There reason for four is that it makes everything line up prettily when initialising variables. The word var has three characters. The space after it counts as another one. That makes four.

We always, always initalise variables using the onevar principle.

Try not to use too many blank lines keeping things compact is easier to read. If you want to group your code by using whitespace between groups, then make sure your code is well organised.

The only JavaScript on the internet that consistently uses spaces inside parenthesis are the source and example code of jQuery.

jQuery source code is, in the opinion of this author, mostly ugly. Look at it!
```javascript
$( "#tags" ).autocomplete({
    source: availableTags
});
```
literally everyone else on the internets omits those spaces. As do we:

```javascript
getCollectionFunction("#tags").autocomplete({
    source: availableTags
});
```
All operators always get a space before and after
```javascript
1 + 1 = 2;

if(something && somethingelse === 2) {

}
```
Opening braces have a single whitespace before them and a new line after them.

Parenthesis do not have a space in front of them.

Why treat braces and parens differently?

Because braces set aside a portion of code (and in the latest versions of ES6, also produce block-level scope), parenthesis belong explicitly to whatever comes before them - parenthesis declaration to a function, boolean test to an if or a switch, function call, passing params to a function call. This space / no space helps understand the semantic differences of braces and parens.
This are some of the rules for whitespace. Not all of them.

All of the code in this document follows our whitespace rules. So does all of the core code in Twenty. Read it and do the same.

#### Being Lazy
We're not writing Coffee Script (thank goodness).

Don't omit your braces - not even in really really simple if-else statements.

Also not in an else-if statement

Don't rely on semicolon insertion. Put them in yourself. Seriously. There's lazy, and there's not-able-to-find-the-semicolon-key lazy. Don't be _natftsk_ lazy.
#### Indenting
Opening braces never go on their own line. They are (almost always) followed immediately by a new line.

This Bad and is not ok:
```javascript
if(params.exist) {return false}
```
This is Baddish and is just about ok. If you must:
```javascript
var myReturnFromAFunction = myFunction({params: true});
```

If you don't understand the difference between Bad and Badish, then the second one is not ok either and you should do this instead:
```javascript
 var myReturnFromAFunction = myFunction({
    params: true
});
```
The line following an opening brace is always indented a further 4 spaces.

A closing brace always occupies a line all on its own, and will be indented 4 spaces fewer than the line above it.

## Chapter 2
### Variables

The `new` keyword should be avoided wherever possible. It is _always_ possible. (Except when it's not. See later... )

Primitives should be initialised as an instance of what you want them to be. This avoids a lot of internal overhead of having to haul ass all the way up the prototype chain.
```javascript
 var itemName = '',
     setOfThings = [],
     params = {};
```
The short example above also illustrates three other important Best Practices – onevar, tabbing and naming conventions.
#### Naming Javascript Variables

Use camelCase. This is how Javascript works internally.
There is no good argument against continuing this established pattern.
Use the shortest name that is long enough to be proper English words, to be descriptive enough to be useful, and to be unique
```javascript
var spHoldEl = ... // bad
var spaceHolderElement = ... // good
var spaceHolderElementBeforeInsertion = ... // bad unless there is a need to distinguish it from other spaceHolderElements
```

That was words, by the way, not numbers
```javascript
    var myCoolElement1 = ... // not cool
    var myCoolElement2 = ... // very bad
    var myCoolElement3 = ... // someone's getting a written warning
```

##### Onevar and Scopes

Javascript used to have three scopes. Now it has four.

|scopes then| scopes now|
|----|----|
|global | global |
|function |function |
|eval| eval|
||block|


Variables should be declared once at the top of the scope to which they belong. Seeing as we avoid global variables wherever possible (which is, basically, always. We have one global variable - Twenty. That should be enough for anyone.), this means at the top of the function in which they are used.
Using onevar is the best defence you have against leaking variables into the global scope.
Leaking variables into the global scope is Very Bad.

###### Scope
If you are using the global scope you are either the head chief top dog system architect, or you have cleared it with him / her, or you have done something wrong. My money is on the last of these.
Avoid global variables wherever possible (which is, basically, always. We have one global variable - Twenty. That should be enough for anyone.)

Also, variable hoisting.

Variable hoisting is massively powerful in Javascript, and massively unintuitive for people who are more used to other, more didactic, programming languages. This document isn't going to go into what variable hoisting it, other than to say that onevar is your pest protection against weird bugs caused by it.

You can read about it here. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var# var_hoisting

Do this:
 ```javascript
var veryGoodFunction = function() {
    var privateVariable,
        someMethod,
        i;
   privateVariable = 25;
}
```

This is how to leak into the global scope. Don't do this:
```javascript
 var veryBadFunction = function() {
     var privateVariable;
     var anotherPrivateVariable;
     // do some stuff
     leakyVariable = "hello world";
 };
```

There is a difference (especially with functions!) between reserving a little bit of memory for a variable value and declaring that variable with a value. Sometimes you have to be a bit careful with this distinction.
This example technically obeys the onevar principle, but it  does not give us the clarity that we are looking for. Declare your variables first, assign them later
But don't do this:
```javascript
 var veryBadFunction = function() {
     var privateVariable = 1,
         anotherPrivateVariable = {hello: "world"},
         methodThing = function(stuff) {return stuff};
 };
```

Onevar is not about optimisation, hoisting or minification, it's about explicitly declaring our variables in one place (the place where the interpreter puts them anyway) and guarding ourselves against pollutiing the global space. Assigning variables values in a onevar context does not help with any of these thiings.
This will be enforced at check in by a JSLint pre-commit hook. Not JSHint. We do this because we love you. Love hurts sometimes.
#### Default values
Use them.
The old way of setting default values was with the super-pretty || "hack", or the purer, but less refined typeof check
```javascript
function myFunction(a, b) {
	a = a || "hello world";
	b = typeof b !== 'undefined' ?  b : 42;
}
```

The new funky way will be familiar to php developers.
```javascript
function myFunction(a = "hello world", b = 42) {
}
```
In perl there are always a million way to do the same, simple thing. If you fancy a laugh, read [this from perlmonks](http://www.perlmonks.org/?node_id=161091)


#### When to use var, let and const.
##### Const
const means a constant. It doesn't mean "a variable which is unlikely to change". It means "a placeholder for a value which will not change". It has a fixed value.
We make it really very clear that this is a `const` by using `ALLCAPS` for the conts name

Use Const for example:
   * to avoid magic numbers
   * as a store for values not involved in the flow of the programme (but required by it)
   * as a shortcut to stop you having to type stuff too much
Good consts might be

```javascript
const APPKEY = "276925d8d98cd956d43cd659051232f7",
const STORYCOUNT = 16;
const STANDARDBORDER = "12px";
```

##### Var
Use vars in function scope and when it's scope spreads over multiple blocks (and it's not a const).
Use vars for any values which are going to be returned out of the function
```javascript
var mySuperFunction = function(a) {
	var goodvar = 2,
		anotherthing;
	if(someCondition) {
		let c = 12;
		anotherthing = goodvar++ * c;
	} else {
		anotherthing = goodvar * a
	}
	return anotherthing;
}
```
##### Let
Let is a variable which is scoped to a block. The concept of "blocks" as self-contained is totally new to JavaScript in ES6, so it's worth making sure we get it right.
A block is any portion of code between two curly braces. So functions are blocks, but we already know that these are function blocks and have a different scope to block blocks and should always use vars.
aka: you should never have a let at the top of a function.
In this example, if we had used vars instead of lets, they would get hoisted to the top of the scope, and memory would be allocated to them, even though it might never be reached.
This is a good place to use a let.

	$.getJSON(this.state.nextpage, function(result) {
         if(result) {
              let oldResultsList = this.state.items,
                  newResultsList = oldResultsList.concat(result.content.items.item);
             /* do things */
		}
    });

## Chapter 3
### Primitives
#### Booleans
Possibly the most important thing to know, remember, and use to your advantage are the six falsey values in Javascript.

  * "" the empty string
  * null
  * undefined
  * +0 / -0
  * false
  * NaN

The strength of this is that you can test for "truthiness" without having to know if you are testing for null, false, undefined or 0. You test the truthiness of a variable by looking at it:

 if(myVariable) {
     // do something
 }

When the JavaScript interpreter looks inside the condition part of an if block (the 'test' inside the parenthesis), it is looking at the literal truthiness of the condition inside it. That means, put something in there which could be either truthy or falsey. Never do the test yourself.
The most common way to get this wrong is if you need to know if an array has any  items in it. You do not need to look to see if the length is greater than zero. This is the same as "not equal to zero" (if we assume arrays can't have negative lengths, which is a good assumption, because they can't)  and "not equal to zero" is the same as "compare this to a falsey value".

__Don't do__ these
 ```javascript
if(myArray && myArray.length > 0) {
     // do something
 }
if(someString === false) {
     // do something
 }
 ```
/** do this **/
 ```javascript
if(myArray && myArray.length) {
     // do something
}

if(someString) {
     // do something
 }
```
Don't forget that sometimes falsey values may actually be a valid value, in which case, triple equals them to the expected type:
 ```javascript
 if(someNumber || someNumber === 0) {
     // do something
 }
```
Which effectively documents your code, and returns the expected result

#### Strings
How To String
There are now three exciting ways to write strings.

We write normal strings with double quotes. Not single quotes. Some people think that this is one of those "the OK button goes on the right" type problems - one where there are two equally valid answers and neither of them is more correct than the other.

In cases like this there are two options. One, is to find a reason for one over the other. Two is do what the styleguide tells you to do and don't waste energy arguing it.

However, there is a really good reason why we should use double quotes. English.

English uses apostrophes a lot. Apostrophes are indistinguishable from single quotes. If you want to use apostrophes in your strings and single quotes to encode your strings, you need to escape the apostrophes. And that is ugly.
```javascript
var coverText = 'Don't Panic';     // unexpected identifier error
var coverText = 'Don\'t Panic';    // got hit with the ugly stick
var coverText = "Don't Panic";     // ahhh, that's better.
```

##### Template Strings
New in ES6 2015. Template strings have an almost magical ability to make your code more complicated and ugly.

So, I don't like template strings. But, they are in the spec, so we shall use them.

(this isn't entirely whimsey. If it's in the spec, and if it's new, then there will probably be some splendid optimisations  of the JIT that make this sort of thing faster and better and more reliable. And, the ability to do functional replacing might be useful. I'm not sure yet)

So, let's use it. But how?
For things like this:

__Do use__ template strings to concatenate actual strings and string variables to make a longer string.
```javascript
let idref = "somesting";
newPage = $("<div id=" + idref + "></div>");     // es-old
newPage = $("<div id='${idref}'></div>");        // es-new
```

__Do not use__ template strings as a direct parameter to a sizzle lookup.
```javascript
let node = {id: "myNode"};
$("#" + node.id);        // es-old
$(`#${node.id}`);        // es-new
```
Anyone trying to tell me that the new version of this is easier to read, better sugar or less prone to errors had better have some pretty big arguments to back them up. Don't do this. It's horrible.

__Do not use__ template strings because you have a multiline string and you are too lazy to concatenate at the end of every line
```javascript
let longString = `Shall I compare thee to a summer’s day?
		Thou art more lovely and more temperate:
		Rough winds do shake the darling buds of May,
		And summer’s lease hath all too short a date`;
```
#### Functions
Like this:
```javascript
var myFunction = function(params) {
     var aPrivateVariable,
         anotherPrivateVariable,
         i;
     return {
         publicVariable: aPrivateVariable
     }
 };
```

There is no space between function and (). There is a space between the parenthesis and the opening brace. The opening brace is on the same line as the parenthesis. The closing brace is on a line all of its own.

Javascript functions are first-order. This means that they can accept functions as parameters. A common reason to accept a function as a parameter is to create recursive functions. These should probably be avoided, as they can create quite a lot of noise on the stack and, if used poorly, are probably the best way to crash your application.

The can also accept primitives or objects. (And arrays, but arrays are a specific sort of object).

##### Function Parameters
If you want your function to accept multiple parameters, you can separate them with commas. This has two advantages: one, it will be faster; two, it can help a lot with minification.
However, it has one serious disadvantage, and that is of maintainability. If at some point in the future you need to add a new parameter, you can end up with really difficult function calls, for example; say you have a utility function for opening a new window with a bunch of optional parameters:
```javascript
 var createWindow = function(height, width, message, content) {
     // function body
 };
```
and at some point in the future, you want to add in a new switch to, say, re-use an old window instead of creating a new one. You add a boolean switch to the parameters
```javascript
 var createWindow = function(height, width, message, content, reuseExisting) {
     // function body
 };
```
If you want to call this will default settings, but then invoke the reuseExisting switch, you have to call it like this:
```javascript
 var createWindow(null, null, null, null, true);
```
which is pretty awful.
Instead, make the params an object, to which you can then add new properties as required;
```javascript
 var createWindow = function(params) {
     var reuseExisting = params.reuseExisting;
 };
 createWindow({
     reuseExisting: true
 });
```

if you then call this with a parameter object with no reuseExisting, the private variable will be undefined. Which is, as we know, falsy. Meaning you can do the following, with no extra coercion.
```javascript
 var createWindow = function(params) {
     var reuseExisting = params && params.reuseExisting;
     if(reuseExisting) {
         // reuse the existing window.
         // Otherwise, default behaviour
     }
 };
```
There is a time when individual parameters is to be recommended over a parameter object - when you are doing serious optimisation to very intensive of often used core functionality. We don't really have much of this in the app. We do a lot of object munging, but not a lot of hardcore mathematics. So, generally, stick to the parameter object

##### Naming Function Parameters
Well known types and frequently used objects must be consistently named. Examples:
   * domNodes should be called `el`, collections of domNodes are called `els` or `elements`
   * objects of type `TwentyPageParams` should always be called `pageParams`
   * native and Twenty events are always called `e`
   * unique identifiers are always called something descriptive with a suffix of `Id` - `storyId`,`categoryId`, etc
   * If your parameters are simple primitives (ie of type string, number, boolean), then give them sensible, descriptive names. -`categoryName`, `position`, etc
   * self-rolled objects described in the method's JSDoc used to configure the appearance or configuration of an object should always be called "params"
   * function callbacks are always called "callback"

Look at what is happening in existing code and do the same.


##### Function invocation
There are 2 ways of closing a Javascript self invoked function. We close it like this:
```javascript
// our style, use this
}());
```
```javascript
// if you find this style please replace it with the one above
})();
```

##### Arrow Functions
New in ES6 2015. Arrow functions are an almost entirely pointless sugar for people who can't use
`bind`, designed to make writing functions more annoying.

At least, this is my current thinking on it. People more into new things than I am tell me I am wrong and that I will learn to love them. I don't see it myself, but in order to accept and embrace the future, I am making arrow functions a part of our Best Practices. Right up until I either "get" them, or I find a convincing argument against using them.

So, these are our rules:

|use an arrow function|do not use an arrow function|
|----|----|
|as a parameter to a built in array method | as an object method |
| in promises | when it is named |


I like to keep things simple.
```javascript
// built in array method:
myArray.filter(el => el.hasThisParameter);
 ```
```javascript
// promises
promise.then(result => result.prop).catch(error => Twenty.error.log(error));
```


#### Dates

Fans of the `new` keyword rejoice, creation of `new Date` is currently one of the few places where `new` is actually a good idea.
If you want a milliseconds "now" date, create a new Date and grab the now(). See below for why we no longer coerce it to a number with the + operator.

The large 13-digit number starting with "14..." is the number of milliseconds since 12 midnight on the 1st January 1970.
No matter how much I learn about computers, I will always be amazed that they count this...
```javascript
new Date().now()
> 1498740286267
```
##### Formatting Dates
Let's face it, this used to be a ball-ache, and the only thing that Java developers could ever use to score points against us. "Ha, but you can't even format dates". Well, let them try it now. The JavaScript date formatting functions are without a doubt, unsurpassed in the whole universe of computer cleverness.

[Read this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString)

But mostly, look at one example, and be amazed. Oh sod it, let's have two examples. Look at two examples, be amazed, and then go and look at the insane and bewildering set of options that you can feed into the options parameter.
```javascript
var options =  {weekday:"short"}
var timeString = new Date("2017-06-30T00:00:00.000+02:00").toLocaleString("de-CH", options);
>"Do"
```
```javascript
var options = {
	weekday: 'long',
	year: 'numeric',
	month: 'long',
	day: 'numeric'
}
new Date("2017-06-30T00:00:00.000+02:00").toLocaleString("de-CH", options)
> "Freitag, 30. Juni 2017"
```
#### Or and And
The double-pipe or operator and the double-ampersand and operator stop evaluation as soon as possible, and return either the value of the truthy test, or false.

This means that they can be used for accessing properties without knowing if an object is set, or for setting default values. Some people don't like this, because it looks a lot different than in other languages. However, we are not writing in other languages, we are writing in Javascript and therefore it is elegant and powerful.

##### Setting a default value:
```javascript
var myDefaultValue = parameter || 12;
```
But don't forget the fancy new cool stuff in ES6 if this variable is passed into a method as a parameter. See above in "variables"

The JS engine looks to see if parameter is set. If it is, it stops evaluating here and returns the value of parameter. If not, it returns the value of 12.

Setting a value to a property (or method) of an object, but only if the object is available
```javascript
var myDeepLinkValue = paramObj && paramObj.someValue;
```
If paramObj does not exist, the JS engine stops here and returns false.
It if does exist, it looks to see if paramObj has a property (or method) called someValue. If it does, it returns the value of someValue, if it does not, it returns false.

#### instanceof vs typeof

Use `instanceof` for custom types, use `typeof` for built in prototypes.

Seeing as we don't currently use custom types, that means always use `typeof`.
Is there an argument for using a factory for our custom objects, and using an instanceof check when we pass them around? Yes, there probably is, but we don't do it just yet.
(Twenty custom types here: http://api.graham.tamedia.ch/global.html# Twenty )
```javascript
var ClassFirst = function () {};
var ClassSecond = function () {};

var instance = new ClassFirst();
typeof instance; // object
typeof instance == 'ClassFirst'; //false

instance instanceof Object; //true
instance instanceof ClassFirst; //true
instance instanceof ClassSecond; //false

'example string' instanceof String; // false
typeof 'example string' == 'string'; //true
'example string' instanceof Object; //false
typeof 'example string' == 'object'; //false

true instanceof Boolean; // false
typeof true == 'boolean'; //true

99.99 instanceof Number; // false
typeof 99.99 == 'number'; //true

function() {} instanceof Function; //true
typeof function() {} == 'function'; //true
```

But use `instanceof`
 for complex built in types
```javascript
/regularexpression/ instanceof RegExp; // true
typeof /regularexpression/; //object

[] instanceof Array; // true
typeof []; //object

{} instanceof Object; // true
typeof {}; //object
```

And, this being JS, therefore wtf, don't forget:
```javascript
typeof null; //object
```
## Chapter 4
### Getting in and out of the DOM
#### Reading

You don't have to use jQuery for everything.
jQuery is good at doing a set of manipulations on a set of nodes. If you're trying to just return one element, or if you are only going to do simple javascript-native stuff, then don't feel like you have to use jQuery. Use the javascript native stuff.
Don't neglect DOM native lookups. jQuery just wraps them anyway.
document.getElementById
document.getElementsByClassName
document.querySelector
document.querySelectorAll
You can take a look at this website to find out the javascript alternative of a jQuery function: http://youmightnotneedjquery.com/
Once you have found your elements, if you are going to use their properties more than once, save them somewhere.
#### Writing
This is the most expensive operation we have available to us. Use it as infrequently as possible.
If you have to perform a series of operations creating dom elements, store them all up in a DocumentFragment and once everything is finished, make the last thing you do appending the DocumentFragment to the DOM.

If this isn't possible, do your maths and your rendering in two different cycles.
For example with non-contiguous elements, (modifying elements is another example), store all your changes up in Javascript memory (create objects and hold on to them) whilst you are working out what all the new values are. Once they are all done, loop through the newly created array of objects, and write them all into the DOM without performing any further calculation or manipulation of values.

Then destroy the objects in memory.

## Chapter 5
### Twenty
#### Read the Documentation.
An extensive API is provided. With docs and tests. Mostly. Please use them. Using the functionality provided ensures the highest coding standards in the most important parts of the application. It stops memory leaks, avoids repetition and is tested and documented.
Please add things that you think are useful here.

#### Nesting Blocks
We write our code using the principle of "I already knew that". I don't know if this has a proper name. I expect it does, most things in programming do. And, again, in common with most things in programming, this is easiest explained by looking at some code.
Some Code:

This is a cut down skeleton look at a bit of Twenty.app. It's a namespace declaration, the contents of which are produced by the revealing module pattern (technically described as: do stuff, hide it's workings in private variables and return an object with privileged access from a self-invoking anonymous function. Whew.)
What can we deduce about the structure of this function then? Lists are good for things like this.
   * We used the onevar principle
   * The variables are defined in the same order as they were declared
   * The return object is also in the same order as the variables
   * Things that should not be seen to the outside world are not shown to the outside world

Accessor functions can change private variables. You can't change the private variable yourself.
If a function needs to know about or use another function or variable, it comes after it
Side effect... if a method / object / thing as an init function this is always going to be the last item before the return

There are good reasons for all of this, to do with variable hoisting, and especially function hoisting. But it's also pretty and neat, so we like it just for that.
Because of the way the Twenty framework is written, function hoisting isn't particularly something that we need to think about when nesting components (components are basically just functions, don't forget), but we consider it Best Practice to still use the "I already knew that" principle.
Or in other words, if a component uses another component, the component being used should have already been declared and initialised and be ready working and available.

Consider the following component:


If we were going to write all of these parts in one file (which we mostly won't do), it would be structured like this
Checkbox - declare a checkbox
Checkbox Set - needs checkboxes - asks itself "What is a checkbox" - aha - I already knew that!
Form - needs a checkbox set.  Already knows what one of those is!
Button - declare a button
Component - needs a button and a form - already knows what they are!

#### Controller
To change pages, always use the named page changing functionality in Twenty.app.controller. If the thing you are looking for isn't there, feel free to write it. Use a sensible name (read the other names, and decide for yourself what is sensible based on what has gone before)

#### Errors
If you are writing a part of the app which is going to be used by another part of the app, it is valid to assume that the developer using your library / component / core cleverness is going to RTFM and not send crap inputs.

Or, in other words, if you say that "to use this function, you should send a string parameter", then you don't need to test that the function was indeed called with a string, and you don't need to notify the other developer when they send a callback function as the parameter and blow the application up.

You can let them swear for a bit, declare your code to be "the worst EVAH" and generally get themselves worked up for a bit until they figure out that they probably should have read the fine documentation in the first place.
Don't throw any errors.

Use Twenty.error.log This does not do much other than log errors at the moment. Should we want to know more about errors at some point in the future, we can do a lot with them if they are all already being collated in one place. So do it. Note good style:
```javascript
Twenty.error.log("Twenty.datamodel.getUniqueKey: You need to provide a prefix");
```
#### Core and Dom
Put generic useful functions-which-manipulate stuff here.
Functions which modify javsacript itself, or provide string or number or date like functions (core javascript things) go in Twenty.core

Things which assume a browser - dom manipulations, form validation, window scrolling, etc go in Twenty.dom

#### DataModel
Things which manipulate feed data go in Twenty.datamodel

## Chapter 6
### JQuery and other Libraries
##### JQuery
As we said above, don't think that you have to use jQuery for everything. In the webapp, jQuery and React are available to you. React handles all the page creation, transition, routing stuff and does it really quite well.
Again as said above, but worth repeating, don't use jQuery just to lookup a node in order to read a property or change and attribute.
It is ok to use jQuery to:
return a collection of nodes that you want to do a single thing to.
return a single node you want to chain a series of jQuery methods to.
return a single node you want to do something to that jQuery makes significantly easier than normal JS (or CSS) manipulations.
It is not ok to use jQuery to
perform simple className operations on a single element
change a simple attribute or property
(that means no $('lookupnode").text() )
do stuff that would be better with CSS.
JQuery also makes it easy to perform className operations on a set of children of a parent. Would it be better to change the class of the parent and alter your CSS?
Dollar Vars
If you have bravely decided to use jQuery to retrieve some nodes from the DOM, you should save this collection into a variable.
That variable should explicitly declare itself as a jQuery Object by starting with a dollar sign. Reuse this variable when you can. Don't go back to jQuery to look up the same things again.
```javascript
$doc = $(document);
$topElement = $doc.find(".topelement");
$pageNumber = $topElement.find(".pagenumber");
```

A note on find()
Find is faster than one-hit lookups. This is all to do with how selectors work anyway and is a big a complicated task, but basically selectors work backwards. If you want to find a certain bunch of list items in a document, then you might think of this

```javascript
var myListItems = document.querySelectorAll("# aCertainSection" .aTypeofUL li);
```

Now, this looks as if it is going to work left to right, first of all finding your section, then the particular type of unordered list you are looking for, then the list items in it. This is not what happens.

What actually happens is that the parser first finds all the `li` list items in the document. Then it chucks out all the ones not in the `certainTypeofUL`. Then it looks that remaining ones, and keeps just those which are in the `aCertainSection` that you are interested in.

You can force it to return left to right by remembering that your selector methods work on all NodeLists, not just on the document so:

```javascript
var myListItems = document.getElementById("aCertainSection").querySelectorAll(".aTypeofUL").getElementsByTagName("li");
```
would almost certainly be a LOT faster than the single shot above.

This is what jQuery's find does, but my example will always be faster, because I am being more specific in my selectors.

I win, jQuery, I WIN.
(of course... round here we don't make wild claims and take a bow. We make JSPerfs and take a bow: http://jsperf.com/jquery-child-selector-vs-find/18 )

#### And other Libraries
If you want to cause bloat in a front end application (we don't. Just saying) the very best way to do this is to include third party libraries instead of coding stuff yourself.

Other people's code is always terrible. Don't assume that just because you don't know who wrote it, or that because it was written by a whole bunch of people in their spare time that it is any good. It might be, but hey, you're awesome. You can do better!

This does not mean "Do not use other people's libraries". It means that when you do, make sure that you document the fact, put them in the proper place (currently /app_dev/src/js/libs), and tell your esteemed colleagues about it next time you present at Demo Day.

A library that is only used once isn't a library, it's a hack.

### Preoptimisation
The JS engine is better at optimising than you are.
Here's a great example.
```javascript
1 var timestamp = +new Date();
2 var timestamp = Date.now();
```
The first of these was used for a long while by developers who thought themselves proper clever because it has that funky coercive plus thing in front. To really understand why this works, it requires you to read the specification in way more detail than anyone who ever used this actually did. "It forces date to be a number" isn't actually a very good answer to the question "why does this work?", but because it sounds like a hidden, but self-evident truth, it was used a lot. Thing is, it's really slow. And if you are going to use sneaky optimisations based on a thorough reading of the spec, then do it because it is fast (or as some advantage). Being clever is not enough.
If you want the full "stop it with your damn preoptimisation tricks" JSPerf, look no further than this `Date` example.

_Everything you have read about how to optimise your javascript code for speed is probably either wrong, outdated, or soon to be wrong and outdated._

On top of that, everything you know about Javascript-specific optimising on the desktop is irrelevant on the mobile.

That doesn't mean doing stupid things is OK. It's not. But esoteric nonsense because you read it somewhere MUST be backed up by your own tests.

If you can't write the tests (see http://jsperf.com/ for how to make it easy), you don't understand the "optimisation" well enough to use it. Here is the perf

## Chapter 7
### The Specification

[Read the ECMA spec once a year.](http://www.ecma-international.org/publications/standards/Ecma-262.htm)

The best notes on the spec are [here](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/). You should read them as well.
