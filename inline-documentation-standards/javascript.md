# JavaScript Documentation Standards

WordPress follows the <a href="http://usejsdoc.org/" target="_blank" rel="noopener">JSDoc 3 standard</a> for inline JavaScript documentation.
<h2>What Should Be Documented</h2>
JavaScript documentation in WordPress takes the form of either formatted blocks of documentation or inline comments.

The following is a list of what should be documented in WordPress JavaScript files:
<ul>
	<li>Functions and class methods</li>
	<li>Objects</li>
	<li>Closures</li>
	<li>Object properties</li>
	<li>Requires</li>
	<li>Events</li>
	<li>File headers</li>
</ul>
<h2>Documenting Tips</h2>
<h3>Language</h3>
Short descriptions should be clear, simple, and brief. Document "what" and "when" – "why" should rarely need to be included. The "why" can go in the long description if needed. For example:

Functions and closures are <em>third-person singular</em> elements, meaning <em>third-person singular verbs</em> should be used to describe what each does.

[tip]Need help remembering how to conjugate for third-person singular verbs? Imagine prefixing the function, hook, class, or method summary with "It":
<ul>
	<li><em>Good:</em> "(It) Does something."</li>
	<li><em>Bad:</em> "(It) Do something."</li>
</ul>
[/tip]

<strong>Functions:</strong> What does the function do?
<ul>
	<li>Good: Handles a click on X element.</li>
	<li>Bad: Included for back-compat for X element.</li>
</ul>
<strong>@since</strong>: The recommended tool to use when searching for the version something was added to WordPress is <a href="https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate">svn blame</a>.

If, after using these tools, the version number cannot be determined, use <code>@since Unknown</code>.

<strong>Code Refactoring:</strong> Do not refactor code in the file when changes to the documentation.
<h3>Grammar</h3>
Descriptive elements should be written as complete sentences. The one exception to this standard is file header summaries, which are intended as file "titles" more than sentences.

The serial (Oxford) comma should be used when listing elements in summaries, descriptions, and parameter or return descriptions.
<h2>Formatting Guidelines</h2>
The following guidelines should be followed to ensure that the content of the doc blocks can be parsed properly for inclusion in the code reference.

<strong>Short descriptions:</strong>

Short descriptions should be a single sentence and contain no markup of any kind. If the description refers to an HTML element or tag, then it should be written as "link tag", not "&lt;a&gt;". For example: "Fires when printing the link tag in the header".

<strong>Long descriptions:</strong>

Markdown can be used, if needed, in a long description.

<strong>@param and @return tags:</strong>

No HTML or markdown is permitted in the descriptions for these tags. HTML elements and tags should be written as "audio element" or "link tag".
<h3>Line wrapping</h3>
DocBlock text should wrap to the next line after 80 characters of text. If the DocBlock itself is indented on the left 20 character positions, the wrap could occur at position 100, but should not extend beyond a total of 120 characters wide.
<h3>Aligning comments</h3>
Related comments should be spaced so that they align to make them more easily readable.

For example:

[js]
/**
 * @param {very_long_type} name           Description.
 * @param {type}           very_long_name Description.
 */
[/js]

<h2>Functions</h2>
Functions should be formatted as follows:
<ul>
	<li><strong>Summary:</strong> A brief, one line explanation of the purpose of the function. Use a period at the end.</li>
	<li><strong>Description:</strong> A supplement to the summary, providing a more detailed description. Use a period at the end.</li>
	<li><strong>@deprecated x.x.x:</strong> Only use for deprecated functions, and provide the version the function was deprecated which should always be 3-digit (e.g. @since 3.6.0), and the function to use instead.</li>
	<li><strong>@since x.x.x:</strong> Should be 3-digit for initial introduction (e.g. @since 3.6.0). If significant changes are made, additional @since tags, versions, and descriptions should be added to serve as a changelog.</li>
	<li><strong>@access:</strong> Only use for functions if private. If the function is private, it is intended for internal use only, and there will be no documentation for it in the code reference.</li>
	<li><strong>@class:</strong> Use for class constructors.</li>
	<li><strong>@augments:</strong> For class constuctors, list direct parents.</li>
	<li><strong>@mixes:</strong> List mixins that are mixed into the object.</li>
	<li><strong>@alias:</strong> If this function is first assigned to a temporary variable this allows you to change the name it's documented under.</li>
	<li><strong>@memberof:</strong> Namespace that this function is contained within if JSDoc is unable to resolve this automatically.</li>
	<li><strong>@static:</strong> For classes, used to mark that a function is a static method on the class constructor.</li>
	<li><strong>@see:</strong> A function or class relied on.</li>
	<li><strong>@link:</strong> URL that provides more information.</li>
	<li><strong>@fires:</strong> Event fired by the function. Events tied to a specific class should list the class name.</li>
	<li><strong>@listens:</strong> Events this function listens for. An event must be prefixed with the event namespace. Events tied to a specific class should list the class name.</li>
	<li><strong>@global:</strong> Marks this function as a global function to be included in the global namespace.</li>
	<li><strong>@param:</strong> Give a brief description of the variable; denote particulars (e.g. if the variable is optional, its default) with <a href="http://usejsdoc.org/tags-param.html">JSDoc @param syntax</a>. Use a period at the end.</li>
	<li><strong>@yield:</strong> For <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*">generator functions</a>, a description of the values expected to be yielded from this function. As with other descriptions, include a period at the end.</li>
	<li><strong>@return:</strong> Note the period after the description.</li>
</ul>

[js]
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @since      x.x.x
 * @deprecated x.x.x Use new_function_name() instead.
 * @access     private
 *
 * @class
 * @augments parent
 * @mixes    mixin
 * 
 * @alias    realName
 * @memberof namespace
 *
 * @see  Function/class relied on
 * @link URL
 * @global
 *
 * @fires   eventName
 * @fires   className#eventName
 * @listens event:eventName
 * @listens className~event:eventName
 *
 * @param {type}   var           Description.
 * @param {type}   [var]         Description of optional variable.
 * @param {type}   [var=default] Description of optional variable with default variable.
 * @param {Object} objectVar     Description.
 * @param {type}   objectVar.key Description of a key in the objectVar parameter.
 *
 * @yield {type} Yielded value description.
 *
 * @return {type} Return value description.
 */
[/js]

<h2>Backbone classes</h2>
Backbone's <code>extend</code> calls should be formatted as follows:
<ul>
	<li><strong>@lends</strong> This tag will allow JSDoc to recognize the <code>extend</code> function from Backbone as a class definition. This should be placed right before the Object that contains the class definition.</li>
</ul>
Backbone's <code>initialize</code> functions should be formatted as follows:
<ul>
	<li><strong>Summary:</strong> A brief, one line explanation of the purpose of the function. Use a period at the end.</li>
	<li><strong>Description:</strong> A supplement to the summary, providing a more detailed description. Use a period at the end.</li>
	<li><strong>@deprecated x.x.x:</strong> Only use for deprecated classes, and provide the version the class was deprecated which should always be 3-digit (e.g. @since 3.6.0), and the class to use instead.</li>
	<li><strong>@since x.x.x:</strong> Should be 3-digit for initial introduction (e.g. @since 3.6.0). If significant changes are made, additional @since tags, versions, and descriptions should be added to serve as a changelog.</li>
	<li><strong>@constructs</strong> Marks this function as the constructor of this class.</li>
	<li><strong>@augments:</strong> List direct parents.</li>
	<li><strong>@mixes:</strong> List mixins that are mixed into the class.</li>
	<li><strong>@requires:</strong> Lists modules that this class requires. Multiple <strong>@requires</strong> tags can be used.</li>
	<li><strong>@alias:</strong> If this class is first assigned to a temporary variable this allows you to change the name it's documented under.</li>
	<li><strong>@memberof:</strong> Namespace that this class is contained within if JSDoc is unable to resolve this automatically.</li>
	<li><strong>@static:</strong> For classes, used to mark that a function is a static method on the class constructor.</li>
	<li><strong>@see:</strong> A function or class relied on.</li>
	<li><strong>@link:</strong> URL that provides more information.</li>
	<li><strong>@fires:</strong> Event fired by the constructor. Should list the class name.</li>
	<li><strong>@param:</strong> Document the arguments passed to the constructor even if not explicitly listed in <code>initialize</code>. Use a period at the end.
<ul>
	<li>Backbone Models are passed <code>attributes</code> and <code>options</code> parameters.</li>
	<li>Backbone Views are passed an <code>options</code> parameter.</li>
</ul>
</li>
</ul>

[js]
Class = Parent.extend( /** @lends namespace.Class.prototype */{
	/**
	 * Summary. (use period)
	 *
	 * Description. (use period)
	 *
	 * @since      x.x.x
	 * @deprecated x.x.x Use new_function_name() instead.
	 * @access     private
	 *
	 * @constructs namespace.Class
	 * @augments   Parent
	 * @mixes      mixin
	 *
	 * @alias    realName
	 * @memberof namespace
	 *
	 * @see   Function/class relied on
	 * @link  URL
	 * @fires Class#eventName
	 *
	 * @param {Object} attributes     The model's attributes.
	 * @param {type}   attributes.key One of the model's attributes.
	 * @param {Object} [options]      The model's options.
	 * @param {type}   attributes.key One of the model's options.
	 */
	initialize: function() {
		//Do stuff.
	}
} );
[/js]

If a Backbone class does not have an initialize function it should be documented by using @inheritDoc as follows:

[js]
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @since      x.x.x
 * @deprecated x.x.x Use new_function_name() instead.
 * @access     private
 *
 * @constructs namespace.Class
 * @memberOf   namespace
 * @augments   Parent
 * @mixes      mixin
 * @inheritDoc
 *
 * @alias    realName
 * @memberof namespace
 *
 * @see   Function/class relied on
 * @link  URL
 */
Class = Parent.extend( /** @lends namespace.Class.prototype */{
// Functions and properties.
} );
[/js]

<blockquote>Note: This currently doesn't provide the expected functionality due to a bug with JSDoc's inheritDoc tag. See the issue <a href="https://github.com/jsdoc3/jsdoc/issues/1012">here</a></blockquote>
<h2>Local functions</h2>
At times functions will be assigned to a local variable before being assigned as a class member.
Such functions should be marked as inner functions of the namespace that uses them using <code>~</code>.
The functions should be formatted as follows:

[js]
/**
 * Function description, you can use any JSDoc here as long as the function remains private.
 *
 * @alias namespace~doStuff
 */
var doStuff = function () {
// Do stuff.
};

Class = Parent.extend( /** @lends namespace.Class.prototype */{
	/**
	 * Class description
	 *
	 * @constructs namespace.Class
	 *
	 * @borrows namespace~doStuff as prototype.doStuff
	 */
	initialize: function() {
	//Do stuff.
	},

	/*
	 * This function will automatically have it's documentation copied from above.
	 * You should make a comment ( not a DocBlock using /**, instead use /* or // )
	 * noting that you're describing this function using @borrows.
	 */
	doStuff: doStuff,
} );
[/js]

<h2>Local ancestors</h2>
At times classes will have Ancestors that are only assigned to a local variable. Such classes should be assigned to the namespace their children are and be made inner classes using <code>~</code>.
These should be documented as follows:
<h2>Class members</h2>
Class members should be formatted as follows:
<ul>
	<li><strong>Short description:</strong> Use a period at the end.</li>
	<li><strong>@since x.x.x:</strong> Should be 3-digit for initial introduction (e.g. @since 3.6.0). If significant changes are made, additional @since tags, versions, and descriptions should be added to serve as a changelog.</li>
	<li><strong>@access:</strong> If the members is private, protected or public. Private members are intended for internal use only.</li>
	<li><strong>@type:</strong> List the type of the class member.</li>
	<li><strong>@property</strong> List all properties this object has in case it's an Object. Use a period at the end.</li>
	<li><strong>@member:</strong> Optionally use this to override JSDoc's member detection in place of <strong>@type</strong> to force a class member.</li>
	<li><strong>@memberof:</strong> Optionally use this to override what class this is a member of.</li>
</ul>

[js]
/**
 * Short description. (use period)
 *
 * @since  x.x.x
 * @access (private, protected, or public)
 *
 * @type     {type}
 * @property {type} key Description.
 *
 * @member   {type} realName
 * @memberof className
 */
[/js]

<h2>Namespaces</h2>
Namespaces should be formatted as follows:
<ul>
	<li><strong>Short description:</strong> Use a period at the end.</li>
	<li><strong>@namespace:</strong> Marks this symbol as a namespace, optionally provide a name as an override.</li>
	<li><strong>@since x.x.x:</strong> Should be 3-digit for initial introduction (e.g. @since 3.6.0). If significant changes are made, additional @since tags, versions, and descriptions should be added to serve as a changelog.</li>
	<li><strong>@memberof:</strong> Namespace that this namespace is contained in.</li>
	<li><strong>@property:</strong> Properties that this namespace exposes. Use a period at the end.</li>
</ul>

[js]
/**
 * Short description. (use period)
 *
 * @namespace realName
 * @memberof  parentNamespace
 *
 * @since x.x.x
 *
 * @property {type} key Description.
 */
[/js]

<h2>Inline Comments</h2>
Inline comments inside methods and functions should be formatted as follows:
<h3>Single line comments</h3>

[js]
// Extract the array values.
[/js]

<h3>Multi-line comments</h3>

[js]
/*
 * This is a comment that is long enough to warrant being stretched over
 * the span of multiple lines. You'll notice this follows basically
 * the same format as the JSDoc wrapping and comment block style.
 */
[/js]

<strong>Important note:</strong> Multi-line comments must not begin with <code>/**</code> (double asterisk). Use <code>/*</code> (single asterisk) instead.
<h2>File Headers</h2>
The JSDoc file header block is used to give an overview of what is contained in the file.

Whenever possible, all WordPress JavaScript files should contain a header block.

WordPress uses JSHint for general code quality testing. Any inline configuration options should be placed at the end of the header block.

[js]
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @link   URL
 * @file   This files defines the MyClass class.
 * @author AuthorName.
 * @since  x.x.x
 */
 
/** jshint {inline configuration here} */
[/js]

<h2>Supported JSDoc Tags</h2>
<table>
<thead>
<tr>
<th style="width: 15%">Tag</th>
<th style="width: 85%">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>@abstract</td>
<td>This method can be implemented (or overridden) by the inheritor.</td>
</tr>
<tr>
<td>@access</td>
<td>Specify the access level of this member (private, public, or protected).</td>
</tr>
<tr>
<td>@alias</td>
<td>Treat a member as if it had a different name.</td>
</tr>
<tr>
<td>@augments</td>
<td>This class inherits from a parent class.</td>
</tr>
<tr>
<td>@author</td>
<td>Identify the author of an item.</td>
</tr>
<tr>
<td>@borrows</td>
<td>This object uses something from another object.</td>
</tr>
<tr>
<td>@callback</td>
<td>Document a callback function.</td>
</tr>
<tr>
<td>@class</td>
<td>This function is a class constructor.</td>
</tr>
<tr>
<td>@classdesc</td>
<td>Use the following text to describe the entire class.</td>
</tr>
<tr>
<td>@constant</td>
<td>Document an object as a constant.</td>
</tr>
<tr>
<td>@constructs</td>
<td>This function member will be the constructor for the previous class.</td>
</tr>
<tr>
<td>@copyright</td>
<td>Document some copyright information.</td>
</tr>
<tr>
<td>@default</td>
<td>Document the default value.</td>
</tr>
<tr>
<td>@deprecated</td>
<td>Document that this is no longer the preferred way.</td>
</tr>
<tr>
<td>@description</td>
<td>Describe a symbol.</td>
</tr>
<tr>
<td>@enum</td>
<td>Document a collection of related properties.</td>
</tr>
<tr>
<td>@event</td>
<td>Document an event.</td>
</tr>
<tr>
<td>@example</td>
<td>Provide an example of how to use a documented item.</td>
</tr>
<tr>
<td>@exports</td>
<td>Identify the member that is exported by a JavaScript module.</td>
</tr>
<tr>
<td>@external</td>
<td>Document an external class/namespace/module.</td>
</tr>
<tr>
<td>@file</td>
<td>Describe a file.</td>
</tr>
<tr>
<td>@fires</td>
<td>Describe the events this method may fire.</td>
</tr>
<tr>
<td>@function</td>
<td>Describe a function or method.</td>
</tr>
<tr>
<td>@global</td>
<td>Document a global object.</td>
</tr>
<tr>
<td>@ignore</td>
<td>[todo] Remove this from the final output.</td>
</tr>
<tr>
<td>@inner</td>
<td>Document an inner object.</td>
</tr>
<tr>
<td>@instance</td>
<td>Document an instance member.</td>
</tr>
<tr>
<td>@kind</td>
<td>What kind of symbol is this?</td>
</tr>
<tr>
<td>@lends</td>
<td>Document properties on an object literal as if they belonged to a symbol with a given name.</td>
</tr>
<tr>
<td>@license</td>
<td>[todo] Document the software license that applies to this code.</td>
</tr>
<tr>
<td>@link</td>
<td>Inline tag - create a link.</td>
</tr>
<tr>
<td>@member</td>
<td>Document a member.</td>
</tr>
<tr>
<td>@memberof</td>
<td>This symbol belongs to a parent symbol.</td>
</tr>
<tr>
<td>@mixes</td>
<td>This object mixes in all the members from another object.</td>
</tr>
<tr>
<td>@mixin</td>
<td>Document a mixin object.</td>
</tr>
<tr>
<td>@module</td>
<td>Document a JavaScript module.</td>
</tr>
<tr>
<td>@name</td>
<td>Document the name of an object.</td>
</tr>
<tr>
<td>@namespace</td>
<td>Document a namespace object.</td>
</tr>
<tr>
<td>@param</td>
<td>Document the parameter to a function.</td>
</tr>
<tr>
<td>@private</td>
<td>This symbol is meant to be private.</td>
</tr>
<tr>
<td>@property</td>
<td>Document a property of an object.</td>
</tr>
<tr>
<td>@protected</td>
<td>This member is meant to be protected.</td>
</tr>
<tr>
<td>@public</td>
<td>This symbol is meant to be public.</td>
</tr>
<tr>
<td>@readonly</td>
<td>This symbol is meant to be read-only.</td>
</tr>
<tr>
<td>@requires</td>
<td>This file requires a JavaScript module.</td>
</tr>
<tr>
<td>@return</td>
<td>Document the return value of a function.</td>
</tr>
<tr>
<td>@see</td>
<td>Refer to some other documentation for more information.</td>
</tr>
<tr>
<td>@since</td>
<td>Documents the version at which the function was added, or when significant changes are made.</td>
</tr>
<tr>
<td>@static</td>
<td>Document a static member.</td>
</tr>
<tr>
<td>@this</td>
<td>What does the 'this' keyword refer to here?</td>
</tr>
<tr>
<td>@throws</td>
<td>Describe what errors could be thrown.</td>
</tr>
<tr>
<td>@todo</td>
<td>Document tasks to be completed.</td>
</tr>
<tr>
<td>@tutorial</td>
<td>Insert a link to an included tutorial file.</td>
</tr>
<tr>
<td>@type</td>
<td>Document the type of an object.</td>
</tr>
<tr>
<td>@typedef</td>
<td>Document a custom type.</td>
</tr>
<tr>
<td>@variation</td>
<td>Distinguish different objects with the same name.</td>
</tr>
<tr>
<td>@version</td>
<td>Documents the version number of an item.</td>
</tr>
<tr>
<td>@yield</td>
<td>Document the yielded values of a generator function.</td>
</tr>
</tbody>
</table>
<h2>Unsupported JSDoc Tags</h2>
<table>
<thead>
<tr>
<th style="width: 15%">Tag</th>
<th style="width: 85%">Why it's not supported</th>
</tr>
</thead>
<tbody>
<tr>
<td>@summary</td>
<td>Should not be used. See the example of how to separate a summary from the full description.</td>
</tr>
<tr>
<td>@virtual</td>
<td>An unsupported synonym. Use @abstract instead.</td>
</tr>
<tr>
<td>@extends</td>
<td>An unsupported synonym. Use @augments instead.</td>
</tr>
<tr>
<td>@constructor</td>
<td>An unsupported synonym. Use @class instead.</td>
</tr>
<tr>
<td>@const</td>
<td>An unsupported synonym. Use @constant instead.</td>
</tr>
<tr>
<td>@defaultvalue</td>
<td>An unsupported synonym. Use @default instead.</td>
</tr>
<tr>
<td>@desc</td>
<td>An unsupported synonym. Use @description instead.</td>
</tr>
<tr>
<td>@host</td>
<td>An unsupported synonym. Use @external instead.</td>
</tr>
<tr>
<td>@fileoverview</td>
<td>An unsupported synonym. Use @file instead.</td>
</tr>
<tr>
<td>@overview</td>
<td>An unsupported synonym. Use @file instead.</td>
</tr>
<tr>
<td>@emits</td>
<td>An unsupported synonym. Use @fires instead.</td>
</tr>
<tr>
<td>@func</td>
<td>An unsupported synonym. Use @function instead.</td>
</tr>
<tr>
<td>@method</td>
<td>An unsupported synonym. Use @function instead.</td>
</tr>
<tr>
<td>@var</td>
<td>An unsupported synonym. Use @member instead.</td>
</tr>
<tr>
<td>@emits</td>
<td>An unsupported synonym. Use @fires instead.</td>
</tr>
<tr>
<td>@arg</td>
<td>An unsupported synonym. Use @param instead.</td>
</tr>
<tr>
<td>@argument</td>
<td>An unsupported synonym. Use @param instead.</td>
</tr>
<tr>
<td>@prop</td>
<td>An unsupported synonym. Use @property instead.</td>
</tr>
<tr>
<td>@returns</td>
<td>An unsupported synonym. Use @return instead.</td>
</tr>
<tr>
<td>@exception</td>
<td>An unsupported synonym. Use @throws instead.</td>
</tr>
</tbody>
</table>
