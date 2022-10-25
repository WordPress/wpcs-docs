# JavaScript Documentation Standards

WordPress follows the [JSDoc 3 standard](http://jsdoc.app/) for inline JavaScript documentation.

## What Should Be Documented

JavaScript documentation in WordPress takes the form of either formatted blocks of documentation or inline comments.

The following is a list of what should be documented in WordPress JavaScript files:

- Functions and class methods
- Objects
- Closures
- Object properties
- Requires
- Events
- File headers

## Documenting Tips

### Language

Short descriptions should be clear, simple, and brief. Document "what" and "when" - "why" should rarely need to be included. The "why" can go in the long description if needed. For example:

Functions and closures are _third-person singular_ elements, meaning _third-person singular verbs_ should be used to describe what each does.

[tip]
Need help remembering how to conjugate for third-person singular verbs? Imagine prefixing the function, hook, class, or method summary with "It":

- _Good_: "(It) Does something."
- _Bad:_ "(It) Do something."

[/tip]

**Functions**: What does the function do?

- _Good_: Handles a click on X element.
- _Bad_: Included for back-compat for X element.

**`@since`**: The recommended tool to use when searching for the version something was added to WordPress is [`svn blame`](https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate).

If, after using these tools, the version number cannot be determined, use `@since Unknown`.

**Code Refactoring**: Do not refactor code in the file when changes to the documentation.

### Grammar

Descriptive elements should be written as complete sentences. The one exception to this standard is file header summaries, which are intended as file "titles" more than sentences.

The serial (Oxford) comma should be used when listing elements in summaries, descriptions, and parameter or return descriptions.

## Formatting Guidelines

The following guidelines should be followed to ensure that the content of the doc blocks can be parsed properly for inclusion in the code reference.

**Short descriptions**:

Short descriptions should be a single sentence and contain no markup of any kind. If the description refers to an HTML element or tag, then it should be written as "link tag", not "&lt;a&gt;". For example: "Fires when printing the link tag in the header".

**Long descriptions**:

Markdown can be used, if needed, in a long description.

**`@param` and `@return` tags**:

No HTML or markdown is permitted in the descriptions for these tags. HTML elements and tags should be written as "audio element" or "link tag".

### Line wrapping

DocBlock text should wrap to the next line after 80 characters of text. If the DocBlock itself is indented on the left 20 character positions, the wrap could occur at position 100, but should not extend beyond a total of 120 characters wide.

### Aligning comments

Related comments should be spaced so that they align to make them more easily readable.

For example:

```javascript
/**
 * @param {very_long_type} name           Description.
 * @param {type}           very_long_name Description.
 */
```

## Functions

Functions should be formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the function. Use a period at the end.
- **Description**: A supplement to the summary, providing a more detailed description. Use a period at the end.
- **`@deprecated x.x.x`**: Only use for deprecated functions, and provide the version the function was deprecated which should always be 3-digit (e.g. `@deprecated 3.6.0`), and the function to use instead.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g. `@since 3.6.0`). If significant changes are made, additional `@since` tags, versions, and descriptions should be added to serve as a changelog.
- **`@access`**: Only use for functions if private. If the function is private, it is intended for internal use only, and there will be no documentation for it in the code reference.
- **`@class`**: Use for class constructors.
- **`@augments`**: For class constructors, list direct parents.
- **`@mixes`**: List mixins that are mixed into the object.
- **`@alias`**: If this function is first assigned to a temporary variable this allows you to change the name it's documented under.
- **`@memberof`**: Namespace that this function is contained within if JSDoc is unable to resolve this automatically.
- **`@static`**: For classes, used to mark that a function is a static method on the class constructor.
- **`@see`**: A function or class relied on.
- **`@link`**: URL that provides more information.
- **`@fires`**: Event fired by the function. Events tied to a specific class should list the class name.
- **`@listens`**: Events this function listens for. An event must be prefixed with the event namespace. Events tied to a specific class should list the class name.
- **`@global`**: Marks this function as a global function to be included in the global namespace.
- **`@param`**: Give a brief description of the variable; denote particulars (e.g. if the variable is optional, its default) with [JSDoc `@param` syntax](http://jsdoc.app/tags-param.html). Use a period at the end.
- **`@yield`**: For [generator functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*), a description of the values expected to be yielded from this function. As with other descriptions, include a period at the end.
- **`@return`**: Note the period after the description.

```javascript
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
```

## Backbone classes

Backbone's `extend` calls should be formatted as follows:

- **`@lends`** This tag will allow JSDoc to recognize the `extend` function from Backbone as a class definition. This should be placed right before the Object that contains the class definition.

Backbone's `initialize` functions should be formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the function. Use a period at the end.
- **Description**: A supplement to the summary, providing a more detailed description. Use a period at the end.
- **`@deprecated x.x.x`**: Only use for deprecated classes, and provide the version the class was deprecated which should always be 3-digit (e.g. `@deprecated 3.6.0`), and the class to use instead.
- **`@since x.x.x`**:</strong> Should be 3-digit for initial introduction (e.g. `@since 3.6.0`). If significant changes are made, additional `@since` tags, versions, and descriptions should be added to serve as a changelog.
- **`@constructs`**: Marks this function as the constructor of this class.
- **`@augments`**: List direct parents.
- **`@mixes`**: List mixins that are mixed into the class.
- **`@requires`**: Lists modules that this class requires. Multiple `@requires` tags can be used.
- **`@alias`**: If this class is first assigned to a temporary variable this allows you to change the name it's documented under.
- **`@memberof`**: Namespace that this class is contained within if JSDoc is unable to resolve this automatically.
- **`@static`**: For classes, used to mark that a function is a static method on the class constructor.
- **`@see`**: A function or class relied on.
- **`@link`**: URL that provides more information.
- **`@fires`**: Event fired by the constructor. Should list the class name.
- **`@param`**: Document the arguments passed to the constructor even if not explicitly listed in `initialize`. Use a period at the end.
    - Backbone Models are passed `attributes` and `options` parameters.
    - Backbone Views are passed an `options` parameter.

```javascript
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
	 * @param {type}   options.key One of the model's options.
	 */
	initialize: function() {
		//Do stuff.
	}
} );
```

If a Backbone class does not have an initialize function it should be documented by using `@inheritDoc` as follows:

```javascript
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
```

> Note: This currently doesn't provide the expected functionality due to a bug with JSDoc's inheritDoc tag. See [JSDocs3 issue 1012](https://github.com/jsdoc3/jsdoc/issues/1012).

## Local functions

At times functions will be assigned to a local variable before being assigned as a class member.
Such functions should be marked as inner functions of the namespace that uses them using `~`.
The functions should be formatted as follows:

```javascript
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
```

## Local ancestors

At times classes will have Ancestors that are only assigned to a local variable. Such classes should be assigned to the namespace their children are and be made inner classes using `~`.

## Class members

Class members should be formatted as follows:

- **Short description**: Use a period at the end.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g. `@since 3.6.0`). If significant changes are made, additional `@since` tags, versions, and descriptions should be added to serve as a changelog.
- **`@access`**: If the members is private, protected or public. Private members are intended for internal use only.
- **`@type`**: List the type of the class member.
- **`@property`** List all properties this object has in case it's an Object. Use a period at the end.
- **`@member`**: Optionally use this to override JSDoc's member detection in place of `@type` to force a class member.
- **`@memberof`**: Optionally use this to override what class this is a member of.

```javascript
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
```

## Namespaces

Namespaces should be formatted as follows:

- **Short description**: Use a period at the end.
- **`@namespace`**: Marks this symbol as a namespace, optionally provide a name as an override.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g. `@since 3.6.0`). If significant changes are made, additional `@since` tags, versions, and descriptions should be added to serve as a changelog.
- **`@memberof`**: Namespace that this namespace is contained in.
- **`@property`**: Properties that this namespace exposes. Use a period at the end.

```javascript
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
```

## Inline Comments

Inline comments inside methods and functions should be formatted as follows:

### Single line comments

```javascript
// Extract the array values.
```

### Multi-line comments

```javascript
/*
 * This is a comment that is long enough to warrant being stretched over
 * the span of multiple lines. You'll notice this follows basically
 * the same format as the JSDoc wrapping and comment block style.
 */
```

**Important note:** Multi-line comments must not begin with `/**` (double asterisk). Use `/*` (single asterisk) instead.

## File Headers

The JSDoc file header block is used to give an overview of what is contained in the file.

Whenever possible, all WordPress JavaScript files should contain a header block.

WordPress uses JSHint for general code quality testing. Any inline configuration options should be placed at the end of the header block.

```javascript
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
```

## Supported JSDoc Tags

| Tag            | Description                                                                                  |
|----------------|----------------------------------------------------------------------------------------------|
| `@abstract`    | This method can be implemented (or overridden) by the inheritor.                             |
| `@access`      | Specify the access level of this member (private, public, or protected).                     |
| `@alias`       | Treat a member as if it had a different name.                                                |
| `@augments`    | This class inherits from a parent class.                                                     |
| `@author`      | Identify the author of an item.                                                              |
| `@borrows`     | This object uses something from another object.                                              |
| `@callback`    | Document a callback function.                                                                |
| `@class`       | This function is a class constructor.                                                        |
| `@classdesc`   | Use the following text to describe the entire class.                                         |
| `@constant`    | Document an object as a constant.                                                            |
| `@constructs`  | This function member will be the constructor for the previous class.                         |
| `@copyright`   | Document some copyright information.                                                         |
| `@default`     | Document the default value.                                                                  |
| `@deprecated`  | Document that this is no longer the preferred way.                                           |
| `@description` | Describe a symbol.                                                                           |
| `@enum`        | Document a collection of related properties.                                                 |
| `@event`       | Document an event.                                                                           |
| `@example`     | Provide an example of how to use a documented item.                                          |
| `@exports`     | Identify the member that is exported by a JavaScript module.                                 |
| `@external`    | Document an external class/namespace/module.                                                 |
| `@file`        | Describe a file.                                                                             |
| `@fires`       | Describe the events this method may fire.                                                    |
| `@function`    | Describe a function or method.                                                               |
| `@global`      | Document a global object.                                                                    |
| `@ignore`      | [todo] Remove this from the final output.                                                    |
| `@inner`       | Document an inner object.                                                                    |
| `@instance`    | Document an instance member.                                                                 |
| `@kind`        | What kind of symbol is this?                                                                 |
| `@lends`       | Document properties on an object literal as if they belonged to a symbol with a given name.  |
| `@license`     | [todo] Document the software license that applies to this code.                              |
| `@link`        | Inline tag - create a link.                                                                  |
| `@member`      | Document a member.                                                                           |
| `@memberof`    | This symbol belongs to a parent symbol.                                                      |
| `@mixes`       | This object mixes in all the members from another object.                                    |
| `@mixin`       | Document a mixin object.                                                                     |
| `@module`      | Document a JavaScript module.                                                                |
| `@name`        | Document the name of an object.                                                              |
| `@namespace`   | Document a namespace object.                                                                 |
| `@param`       | Document the parameter to a function.                                                        |
| `@private`     | This symbol is meant to be private.                                                          |
| `@property`    | Document a property of an object.                                                            |
| `@protected`   | This member is meant to be protected.                                                        |
| `@public`      | This symbol is meant to be public.                                                           |
| `@readonly`    | This symbol is meant to be read-only.                                                        |
| `@requires`    | This file requires a JavaScript module.                                                      |
| `@return`      | Document the return value of a function.                                                     |
| `@see`         | Refer to some other documentation for more information.                                      |
| `@since`       | Documents the version at which the function was added, or when significant changes are made. |
| `@static`      | Document a static member.                                                                    |
| `@this`        | What does the 'this' keyword refer to here?                                                  |
| `@throws`      | Describe what errors could be thrown.                                                        |
| `@todo`        | Document tasks to be completed.                                                              |
| `@tutorial`    | Insert a link to an included tutorial file.                                                  |
| `@type`        | Document the type of an object.                                                              |
| `@typedef`     | Document a custom type.                                                                      |
| `@variation`   | Distinguish different objects with the same name.                                            |
| `@version`     | Documents the version number of an item.                                                     |
| `@yield`       | Document the yielded values of a generator function.                                         |

## Unsupported JSDoc Tags

| Tag             | Why it's not supported                                                                        |
|-----------------|-----------------------------------------------------------------------------------------------|
| `@summary`      | Should not be used. See the example of how to separate a summary from the full description.   |
| `@virtual`      | An unsupported synonym. Use `@abstract` instead.                                              |
| `@extends`      | An unsupported synonym. Use `@augments` instead.                                              |
| `@constructor`  | An unsupported synonym. Use `@class` instead.                                                 |
| `@const`        | An unsupported synonym. Use `@constant` instead.                                              |
| `@defaultvalue` | An unsupported synonym. Use `@default` instead.                                               |
| `@desc`         | An unsupported synonym. Use `@description` instead.                                           |
| `@host`         | An unsupported synonym. Use `@external` instead.                                              |
| `@fileoverview` | An unsupported synonym. Use `@file` instead.                                                  |
| `@overview`     | An unsupported synonym. Use `@file` instead.                                                  |
| `@emits`        | An unsupported synonym. Use `@fires` instead.                                                 |
| `@func`         | An unsupported synonym. Use `@function` instead.                                              |
| `@method`       | An unsupported synonym. Use `@function` instead.                                              |
| `@var`          | An unsupported synonym. Use `@member` instead.                                                |
| `@emits`        | An unsupported synonym. Use `@fires` instead.                                                 |
| `@arg`          | An unsupported synonym. Use `@param` instead.                                                 |
| `@argument`     | An unsupported synonym. Use `@param` instead.                                                 |
| `@prop`         | An unsupported synonym. Use `@property` instead.                                              |
| `@returns`      | An unsupported synonym. Use `@return` instead.                                                |
| `@exception`    | An unsupported synonym. Use `@throws` instead.                                                |
