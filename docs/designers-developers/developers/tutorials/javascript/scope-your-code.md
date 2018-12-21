# Scope your code

Historically, JavaScript files loaded in a web page share the same scope. This means that a global variable declared in one file will be seen by the code in other files.

To see how this works, create a web page that loads two JavaScript files. For example, a file called `one.js`:

```js
var yourPluginName = 'MyPlugin';
console.log( 'Plugin name is ', yourPluginName );
```

And another JavaScript file  called `two.js`:

```js
var yourPluginName = 'YourPlugin';
var yourName = 'A great JS developer!';
console.log( 'Plugin ', yourPluginName, ' authored by ', yourName );
```

When loaded on the same page, the value of `yourPluginName` changes because it is overwritten by the second file. By scoping your code, you can avoid the problem of values unexpectedly changing.

## Scoping code within a function

In JavaScript, you can scope your code by writing it within a function. Functions have "local scope", or a scope that is specific only to that function. You can also write an "anonymous function" which will also prevent your function name from being overridden in the global scope.

Scoped `one.js` code:

```js
function() {
	var yourPluginName = 'my plugin';
	console.log( 'Plugin name is ', yourPluginName );
}
```

And `two.js`:

```js
function() {
	var yourName = 'A great JS developer!';
	console.log( 'Plugin ', yourPluginName, ' authored by ', yourName );
}
```

With this trick, the different files won't override each other's variables. Unfortunately, they also won't work, because these functions are being called by no one. We've only _defined_ the functions; we haven't _executed_ them yet.

## Automatically execute anonymous functions

The problem is, how do you execute anonymous functions in JavaScript? It turns out there are a few ways, but the most popular is this:

```js
( function() {
	// your code goes here
} )( )
```

You wrap your function between parentheses, and then call it like any other named function. This pattern is known as [Immediately-Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), or IIFE for short.

This is `one.js` written as an IIFE:

```js
( function() {
	var yourPluginName = 'my plugin';
	console.log( 'Plugin name is ', yourPluginName );
} )( )
```

And this is `two.js`:

```js
( function() {
	var yourName = 'A great JS developer!';
	console.log( 'Plugin ', yourPluginName, ' authored by ', yourName );
} )( )
```

You may also have noticed the parentheses at the end of the code sample as well. This allows you to take a variable from the global scope and pass it into your function. For example, below we pass the `window.wp` variable into our function as the `wordpress` function parameter:

```js
( function( wordpress ) {
	console.log( 'WordPress is ', wordpress );
} )( window.wp )
```

## Future changes

At the beginning we mentioned that:

> Historically, JavaScript files loaded in a web page share the same scope.

Notice the _historically_.

JavaScript has evolved quite a bit since its creation. As of 2015, the language supports modules, also known as _ES6 modules_, that introduce separate scope per file: a global variable in `one.js` wouldn't be exposed to `two.js`. This feature is already [supported by modern browsers](https://caniuse.com/#feat=es6-module), but not all of them do. If your code needs to run in browsers that don't support modules, your last resort is using IIFEs.