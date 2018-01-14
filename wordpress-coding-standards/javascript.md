# JavaScript Coding Standards

JavaScript has become a critical component in developing WordPress-based applications (themes and plugins) as well as WordPress core. Standards are needed for formatting and styling JavaScript code to maintain the same code consistency as the WordPress standards provide for core PHP, HTML, and CSS code.
<blockquote>All code in any code-base should look like a single person typed it, no matter how many people contributed. - <a href="https://github.com/rwaldron/idiomatic.js/">Principles of Writing Consistent, Idiomatic JavaScript</a></blockquote>
The WordPress JavaScript Coding Standards are adapted from the <a href="https://contribute.jquery.org/style-guide/js">jQuery JavaScript Style Guide</a>. Our standard differs from the jQuery guidelines in the following ways:
<ul>
 	<li>WordPress uses single quotation marks for string declarations.</li>
 	<li>Case statements are indented within switch blocks.</li>
 	<li>Function contents are consistently indented, including full-file closure wrappers.</li>
 	<li>Some whitespace rules differ, for consistency with the WordPress PHP coding standards.</li>
 	<li>jQuery’s 100-character hard line limit is encouraged, but not strictly enforced.</li>
</ul>
Many of the examples below have been adapted directly from the jQuery style guide; these differences have all been integrated into the examples on this page. Any of the below standards and examples should be considered best practice for WordPress code, unless explicitly noted as anti-patterns.
<h2>Code Refactoring</h2>
<blockquote>"<a href="https://make.wordpress.org/core/2011/03/23/code-refactoring/">Code refactoring should not be done just because we can.</a>" - Lead Developer Andrew Nacin</blockquote>
Many parts of the WordPress code structure for JavaScript are inconsistent in their style. WordPress is working to gradually improve this, so the code will be clean and easy to read at a glance.

While the coding standards are important, refactoring older .js files simply to conform to the standards is not an urgent issue. "Whitespace-only" patches for older files are strongly discouraged.

All new or updated JavaScript code will be reviewed to ensure it conforms to the standards, and passes JSHint.
<h2>Spacing</h2>
Use spaces liberally throughout your code. "When in doubt, space it out."

These rules encourage liberal spacing for improved developer readability. The minification process creates a file that is optimized for browsers to read and process.
<ul>
 	<li>Indentation with tabs.</li>
 	<li>No whitespace at the end of line or on blank lines.</li>
 	<li>Lines should usually be no longer than 80 characters, and should not exceed 100 (counting tabs as 4 spaces). <em>This is a "soft" rule, but long lines generally indicate unreadable or disorganized code.</em></li>
 	<li><code>if</code>/<code>else</code>/<code>for</code>/<code>while</code>/<code>try</code> blocks should always use braces, and always go on multiple lines.</li>
 	<li>Unary special-character operators (e.g., <code>++</code>, <code>--</code>) must not have space next to their operand.</li>
 	<li>Any <code>,</code> and <code>;</code> must not have preceding space.</li>
 	<li>Any <code>;</code> used as a statement terminator must be at the end of the line.</li>
 	<li>Any <code>:</code> after a property name in an object definition must not have preceding space.</li>
 	<li>The <code>?</code> and <code>:</code> in a ternary conditional must have space on both sides.</li>
 	<li>No filler spaces in empty constructs (e.g., <code>{}</code>, <code>[]</code>, <code>fn()</code>).</li>
 	<li>There should be a new line at the end of each file.</li>
 	<li>Any <code>!</code> negation operator should have a following space.<sup>*</sup></li>
 	<li>All function bodies are indented by one tab, even if the entire file is wrapped in a closure.<sup>*</sup></li>
 	<li>Spaces may align code within documentation blocks or within a line, but only tabs should be used at the start of a line.<sup>*</sup></li>
</ul>
<strong>*</strong>: The WordPress JavaScript standards prefer slightly broader whitespace rules than the jQuery style guide. These deviations are for consistency between the PHP and JavaScript files in the WordPress codebase.

Whitespace can easily accumulate at the end of a line – avoid this, as trailing whitespace is caught as an error in JSHint. One way to catch whitespace buildup is enabling visible whitespace characters within your text editor.
<h3>Objects</h3>
Object declarations can be made on a single line if they are short (remember the line length guidelines). When an object declaration is too long to fit on one line, there must be one property per line. Property names only need to be quoted if they are reserved words or contain special characters:

[javascript]
// Preferred
var map = {
	ready: 9,
	when: 4,
	'you are': 15
};

// Acceptable for small objects
var map = { ready: 9, when: 4, 'you are': 15 };

// Bad
var map = { ready: 9,
	when: 4, 'you are': 15 };
[/javascript]

<h3>Arrays and Function Calls</h3>
Always include extra spaces around elements and arguments:

[javascript]
array = [ a, b ];

foo( arg );

foo( 'string', object );

foo( options, object[ property ] );

foo( node, 'property', 2 );
[/javascript]

Exceptions:

[javascript]
// For consistency with our PHP standards, do not include a space around
// string literals or integers used as key values in array notation:
prop = object['default'];
firstArrayElement = arr[0];
[/javascript]

<h3>Examples of Good Spacing</h3>

[javascript]
var i;

if ( condition ) {
	doSomething( 'with a string' );
} else if ( otherCondition ) {
	otherThing( {
		key: value,
		otherKey: otherValue
	} );
} else {
	somethingElse( true );
}

// Unlike jQuery, WordPress prefers a space after the ! negation operator.
// This is also done to conform to our PHP standards.
while ( ! condition ) {
	iterating++;
}

for ( i = 0; i < 100; i++ ) {
	object[ array[ i ] ] = someFn( i );
	$( '.container' ).val( array[ i ] );
}

try {
	// Expressions
} catch ( e ) {
	// Expressions
}
[/javascript]

<h2>Semicolons</h2>
Use them. Never rely on Automatic Semicolon Insertion (ASI).
<h2>Indentation and Line Breaks</h2>
Indentation and line breaks add readability to complex statements.

Tabs should be used for indentation. Even if the entire file is contained in a closure (i.e., an immediately invoked function), the contents of that function should be indented by one tab:

[javascript]
(function( $ ) {
	// Expressions indented

	function doSomething() {
		// Expressions indented
	}
})( jQuery );
[/javascript]

<h3>Blocks and Curly Braces</h3>
<code>if</code>, <code>else</code>, <code>for</code>, <code>while</code>, and <code>try</code> blocks should always use braces, and always go on multiple lines. The opening brace should be on the same line as the function definition, the conditional, or the loop. The closing brace should be on the line directly following the last statement of the block.

[javascript]
var a, b, c;

if ( myFunction() ) {
	// Expressions
} else if ( ( a && b ) || c ) {
	// Expressions
} else {
	// Expressions
}
[/javascript]

<h3>Multi-line Statements</h3>
When a statement is too long to fit on one line, line breaks must occur after an operator.

[javascript]
// Bad
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c
	+ ' is ' + ( a + b + c ) + '</p>';

// Good
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c +
	' is ' + ( a + b + c ) + '</p>';
[/javascript]

Lines should be broken into logical groups if it improves readability, such as splitting each expression of a ternary operator onto its own line, even if both will fit on a single line.

[javascript]
// Acceptable
var baz = ( true === conditionalStatement() ) ? 'thing 1' : 'thing 2';

// Better
var baz = firstCondition( foo ) && secondCondition( bar ) ?
	qux( foo, bar ) :
	foo;
[/javascript]

When a conditional is too long to fit on one line, successive lines must be indented one extra level to distinguish them from the body.

[javascript]
	if ( firstCondition() && secondCondition() &&
			thirdCondition() ) {
		doStuff();
	}
[/javascript]

<h3>Chained Method Calls</h3>
When a chain of method calls is too long to fit on one line, there must be one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation must be used.

[javascript]
elements
	.addClass( 'foo' )
	.children()
		.html( 'hello' )
	.end()
	.appendTo( 'body' );
[/javascript]

<h2>Assignments and Globals</h2>
<h3>Declaring Variables With var</h3>
Each function should begin with a single comma-delimited <code>var</code> statement that declares any local variables necessary. If a function does not declare a variable using <code>var</code>, that variable can leak into an outer scope (which is frequently the global scope, a worst-case scenario), and can unwittingly refer to and modify that data.

Assignments within the <code>var</code> statement should be listed on individual lines, while declarations can be grouped on a single line. Any additional lines should be indented with an additional tab. Objects and functions that occupy more than a handful of lines should be assigned outside of the <code>var</code> statement, to avoid over-indentation.

[javascript]
// Good
var k, m, length,
	// Indent subsequent lines by one tab
	value = 'WordPress';

// Bad
var foo = true;
var bar = false;
var a;
var b;
var c;
[/javascript]

<h3>Globals</h3>
In the past, WordPress core made heavier use of global variables. Since core JavaScript files are sometimes used within plugins, existing globals should not be removed.

All globals used within a file should be documented at the top of that file. Multiple globals can be comma-separated.

This example would make <code>passwordStrength</code> an allowed global variable within that file:

[javascript]
/* global passwordStrength:true */
[/javascript]

The "true" after <code>passwordStrength</code> means that this global is being defined within this file. If you are accessing a global which is defined elsewhere, omit <code>:true</code> to designate the global as read-only.

<strong>Common Libraries</strong>

Backbone, jQuery, Underscore, and the global <code>wp</code> object are all registered as allowed globals in the root <code>.jshintrc</code> file.

Backbone and Underscore may be accessed directly at any time. jQuery should be accessed through <code>$</code> by passing the <code>jQuery</code> object into an anonymous function:

[javascript]
(function( $ ) {
  // Expressions
})( jQuery );
[/javascript]

This will negate the need to call <code>.noConflict()</code>, or to set <code>$</code> using another variable.
Files which add to, or modify, the <code>wp</code> object must safely access the global to avoid overwriting previously set properties:

[javascript]
// At the top of the file, set "wp" to its existing value (if present)
window.wp = window.wp || {};
[/javascript]

<h2>Naming Conventions</h2>
Variable and function names should be full words, using camel case with a lowercase first letter. This is an area where this standard differs from the <a href="https://make.wordpress.org/core/handbook/coding-standards/php/#naming-conventions">WordPress PHP coding standards</a>.

Constructors intended for use with <code>new</code> should have a capital first letter (UpperCamelCase).

Names should be descriptive, but not excessively so. Exceptions are allowed for iterators, such as the use of <code>i</code> to represent the index in a loop.
<h2>Comments</h2>
Comments come before the code to which they refer, and should always be preceded by a blank line. Capitalize the first letter of the comment, and include a period at the end when writing full sentences. There must be a single space between the comment token (<code>//</code>) and the comment text.

Single line comments:

[javascript]
someStatement();

// Explanation of something complex on the next line
$( 'p' ).doSomething();
[/javascript]

Multi-line comments should be used for long comments, see also the <a href="https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/javascript/#multi-line-comments">JavaScript Documentation Standards</a>:

[javascript]
/*
 * This is a comment that is long enough to warrant being stretched
 * over the span of multiple lines.
 */
[/javascript]

Inline comments are allowed as an exception when used to annotate special arguments in formal parameter lists:

[javascript]
function foo( types, selector, data, fn, /* INTERNAL */ one ) {
	// Do stuff
}
[/javascript]

<h2>Equality</h2>
Strict equality checks (<code>===</code>) must be used in favor of abstract equality checks (<code>==</code>). The <em>only</em> exception is when checking for both <code>undefined</code> and <code>null</code> by way of <code>null</code>.

[javascript]
// Check for both undefined and null values, for some important reason.
if ( undefOrNull == null ) {
	// Expressions
}
[/javascript]

<h2>Type Checks</h2>
These are the preferred ways of checking the type of an object:
<ul>
 	<li>String: <code>typeof object === 'string'</code></li>
 	<li>Number: <code>typeof object === 'number'</code></li>
 	<li>Boolean: <code>typeof object === 'boolean'</code></li>
 	<li>Object: <code>typeof object === 'object'</code> or <code>_.isObject( object )</code></li>
 	<li>Plain Object: <code>jQuery.isPlainObject( object )</code></li>
 	<li>Function: <code>_.isFunction( object)</code> or <code>jQuery.isFunction( object )</code></li>
 	<li>Array: <code>_.isArray( object )</code> or <code>jQuery.isArray( object )</code></li>
 	<li>Element: <code>object.nodeType</code> or <code>_.isElement( object )</code></li>
 	<li>null: <code>object === null</code></li>
 	<li>null or undefined: <code>object == null</code></li>
 	<li>undefined:
<ul>
 	<li>Global Variables: <code>typeof variable === 'undefined'</code></li>
 	<li>Local Variables: <code>variable === undefined</code></li>
 	<li>Properties: <code>object.prop === undefined</code></li>
 	<li>Any of the above: <code>_.isUndefined( object )</code></li>
</ul>
</li>
</ul>
Anywhere Backbone or Underscore are already used, you are encouraged to use <a href="http://underscorejs.org/#isElement">Underscore.js</a>'s type checking methods over jQuery's.
<h2>Strings</h2>
Use single-quotes for string literals:

[javascript]
var myStr = 'strings should be contained in single quotes';
[/javascript]

When a string contains single quotes, they need to be escaped with a backslash (<code>\</code>):

[javascript]
// Escape single quotes within strings:
'Note the backslash before the \'single quotes\'';
[/javascript]

<h2>Switch Statements</h2>
The usage of <code>switch</code> statements is generally discouraged, but can be useful when there are a large number of cases - especially when multiple cases can be handled by the same block, or fall-through logic (the <code>default</code> case) can be leveraged.

When using <code>switch</code> statements:

- Use a <code>break</code> for each case other than <code>default</code>. When allowing statements to "fall through," note that explicitly.
- Indent <code>case</code> statements one tab within the <code>switch</code>.

[javascript]
switch ( event.keyCode ) {

	// ENTER and SPACE both trigger x()
	case $.ui.keyCode.ENTER:
	case $.ui.keyCode.SPACE:
		x();
		break;
	case $.ui.keyCode.ESCAPE:
		y();
		break;
	default:
		z();
}
[/javascript]

It is not recommended to return a value from within a switch statement: use the <code>case</code> blocks to set values, then <code>return</code> those values at the end.

[javascript]
function getKeyCode( keyCode ) {
	var result;

	switch ( event.keyCode ) {
		case $.ui.keyCode.ENTER:
		case $.ui.keyCode.SPACE:
			result = 'commit';
			break;
		case $.ui.keyCode.ESCAPE:
			result = 'exit';
			break;
		default:
			result = 'default';
	}

	return result;
}
[/javascript]

<h2>Best Practices</h2>
<h3>Arrays</h3>
Creating arrays in JavaScript should be done using the shorthand <code>[]</code> constructor rather than the <code>new Array()</code> notation.

[javascript]
var myArray = [];
[/javascript]

You can initialize an array during construction:

[javascript]
var myArray = [ 1, 'WordPress', 2, 'Blog' ];
[/javascript]

In JavaScript, associative arrays are defined as objects.
<h3>Objects</h3>
There are many ways to create objects in JavaScript. Object literal notation, <code>{}</code>, is both the most performant, and also the easiest to read.

[javascript]
var myObj = {};
[/javascript]

Object literal notation should be used unless the object requires a specific prototype, in which case the object should be created by calling a constructor function with <code>new</code>.

[javascript]
var myObj = new ConstructorMethod();
[/javascript]

Object properties should be accessed via dot notation, unless the key is a variable, a <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Reserved_Words">reserved word</a>, or a string that would not be a valid identifier:

[javascript]
prop = object.propertyName;
prop = object[ variableKey ];
prop = object['default'];
prop = object['key-with-hyphens'];
[/javascript]

<h3>"Yoda" Conditions</h3>
For consistency with the <a href="https://make.wordpress.org/core/handbook/coding-standards/php/#yoda-conditions">PHP code standards</a>, whenever you are comparing an object to a string, boolean, integer, or other constant or literal, the variable should always be put on the right hand side, and the constant or literal put on the left.

[javascript]
if ( true === myCondition ) {
	// Do stuff
}
[/javascript]

"A little bizarre, it is, to read. Get used to it, you will."
<h3>Iteration</h3>
When iterating over a large collection using a <code>for</code> loop, it is recommended to store the loop's max value as a variable rather than re-computing the maximum every time:

[javascript]
// Good & Efficient
var i, max;

// getItemCount() gets called once
for ( i = 0, max = getItemCount(); i < max; i++ ) {
	// Do stuff
}

// Bad & Potentially Inefficient:
// getItemCount() gets called every time
for ( i = 0; i < getItemCount(); i++ ) {
	// Do stuff
}
[/javascript]

<h3>Underscore.js Collection Functions</h3>
Learn and understand Underscore's <a href="http://underscorejs.org/#collections">collection and array methods</a>. These functions, including <code>_.each</code>, <code>_.map</code>, and <code>_.reduce</code>, allow for efficient, readable transformations of large data sets.

Underscore also permits jQuery-style chaining with regular JavaScript objects:

[javascript]
var obj = {
	first: 'thing 1',
	second: 'thing 2',
	third: 'lox'
};

var arr = _.chain( obj )
	.keys()
	.map(function( key ) {
		return key + ' comes ' + obj[ key ];
	})
	// Exit the chain
	.value();

// arr === [ 'first comes thing 1', 'second comes thing 2', 'third comes lox' ]
[/javascript]

<h3>Iterating Over jQuery Collections</h3>
The only time jQuery should be used for iteration is when iterating over a collection of jQuery objects:

[javascript]
$tabs.each(function( index, element ) {
	var $element = $( element );

	// Do stuff to $element
});
[/javascript]

Never use jQuery to iterate over raw data or vanilla JavaScript objects.
<h2>JSHint</h2>
<a href="http://jshint.com">JSHint</a> is an automated code quality tool, designed to catch errors in your JavaScript code. JSHint is used in WordPress development to quickly verify that a patch has not introduced any logic or syntax errors to the front-end.
<h3>Installing and Running JSHint</h3>
JSHint is run using a tool called <a href="https://gruntjs.com/">Grunt</a>. Both JSHint and Grunt are programs written in <a href="https://nodejs.org/">Node.js</a>. A configuration file that comes with the WordPress development code makes it easy to install and configure these tools.

To install Node.js, click the Install link on the <a href="https://nodejs.org/">Node</a> website. The correct install file for your operating system will be downloaded. Follow the installation steps for your operating system to install the program.

Once Node.js is installed, open a command line window and navigate to the directory where you <a href="https://make.wordpress.org/core/handbook/tutorials/installing-wordpress-locally/from-svn/">checked out a copy of the WordPress SVN repository</a> (use <code>cd ~/directoryname</code>). You should be in the root directory which contains the <code>package.json</code> file.

Next, type <code>npm install</code> into the command line window. This will download and install all the Node packages used in WordPress development.

Finally, type <code>npm install -g grunt-cli</code> to install the Grunt Command Line Interface (CLI) package. Grunt CLI is what is used to actually run the Grunt tasks in WordPress.

You should now be able to type <code>grunt jshint</code> to have Grunt check all the WordPress JavaScript files for syntax and logic errors. To only check core code, type <code>grunt jshint:core</code>; to only check unit test .js files, type <code>grunt jshint:tests</code>.

<strong>JSHint Settings</strong>

The configuration options used for JSHint are stored within a <a title="WordPress JSHint file in svn trunk" href="https://develop.svn.wordpress.org/trunk/.jshintrc"><code>.jshintrc</code> file</a> in the WordPress SVN repository. This file defines which errors JSHint should flag if it finds them within the WordPress source code.

<strong>Target A Single File</strong>

To specify a single file for JSHint to check, add <code>--file=filename.js</code> to the end of the command. For example, this will only check the file named "admin-bar.js" within WordPress's core JavaScript files:

<code>grunt jshint:core --file=admin-bar.js</code>

And this would only check the "password-strength-meter.js" file within the unit tests directory:

<code>grunt jshint:tests --file=password-strength-meter.js</code>

Limiting JSHint to a single file can be useful if you are only working on one or two specific files and don't want to wait for JSHint to process every single file each time it runs.
<h3>JSHint Overrides: Ignore Blocks</h3>
In some situations, parts of a file should be excluded from JSHint. As an example, the script file for the admin bar contains the minified code for the jQuery HoverIntent plugin - this is third-party code that should not pass through JSHint, even though it is part of a WordPress core JavaScript file.

To exclude a specific file region from being processed by JSHint, enclose it in JSHint directive comments:

[javascript]
/* jshint ignore:start */
if ( typeof jQuery.fn.hoverIntent === 'undefined' ) {
	// hoverIntent r6 - Copy of wp-includes/js/hoverIntent.min.js
	(function(a){a.fn.hoverIntent=...............
}
/* jshint ignore:end */
[/javascript]

<h2>Credits</h2>
<ul>
 	<li>The jQuery examples are adapted from the <a href="https://contribute.jquery.org/style-guide/js">jQuery JavaScript Style Guide</a>, which is made available under the MIT license.</li>
</ul>
