# PHP Documentation Standards

WordPress uses a customized documentation schema that draws inspiration from PHPDoc, an evolving standard for providing documentation to PHP code, which is maintained by <a href="http://phpdoc.org/">phpDocumentor</a>.

In some special cases – such as WordPress' implementation of hash notations – standards are derived from the <a href="https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md">draft PSR-5 recommendations</a>. This does not mean we are attempting to be "PSR-5 compliant" at this time, it simply means that we've adopted PSR-5 recommendations <em>in part</em>.

<h2>What Should Be Documented</h2>
PHP documentation in WordPress mostly takes the form of either formatted blocks of documentation or inline comments.

The following is a list of what should be documented in WordPress files:
<ul>
 	<li>Functions and class methods</li>
 	<li>Classes</li>
 	<li>Class members (including properties and constants)</li>
 	<li>Requires and includes</li>
 	<li>Hooks (actions and filters)</li>
 	<li>Inline comments</li>
 	<li>File headers</li>
 	<li>Constants</li>
</ul>
<h3>Documenting Tips</h3> 
<h4>Language</h4>

Summaries (formerly Short Descriptions) should be clear, simple, and brief. Avoid describing "why" an element exists, rather, focus on documenting "what" and "when" it does something.

A function, hook, class, or method is a <em>third-person singular</em> element, meaning that <em>third-person singular verbs</em> should be used to describe what each does.

[tip]Need help remembering how to conjugate for third-person singular verbs? Imagine prefixing the function, hook, class, or method summary with "It":
<ul>
 	<li><em>Good:</em> "(It) Does something."</li>
 	<li><em>Bad:</em> "(It) Do something."</li>
</ul>
[/tip]

Summary examples:
<ul>
 	<li><strong>Functions:</strong> <u>What</u> does the function do?
<ul>
 	<li>Good: <em>Displays the last modified date for a post.</em></li>
 	<li>Bad: <em>Display the date on which the post was last modified.</em></li>
</ul>
</li>
 	<li><strong>Filters:</strong> <u>What</u> is being filtered?
<ul>
 	<li>Good: <em>Filters the post content.</em></li>
 	<li>Bad: <em>Lets you edit the post content that is output in the post template.</em></li>
</ul>
</li>
 	<li><strong>Actions:</strong> <u>When</u> does an action fire?
<ul>
 	<li>Good: <em>Fires after most of core is loaded, and the user is authenticated.</em></li>
 	<li>Bad: <em>Allows you to register custom post types, custom taxonomies, and other general housekeeping tasks after a lot of WordPress core has loaded.</em></li>
</ul>
</li>
</ul>

<h4>Grammar</h4>
Descriptive elements should be written as complete sentences. The one exception to this standard is file header summaries, which are intended as file "titles" more than sentences.

The serial (Oxford) comma should be used when listing elements in summaries, descriptions, and parameter or return descriptions.

<h4>Miscellaneous</h4>
<strong>@since:</strong> The recommended tool to use when searching for the version something was added to WordPress is <a href="http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate">svn blame</a>. An additional resource for older hooks is the <a href="http://adambrown.info/p/wp_hooks">WordPress Hooks Database</a>.

If the version number cannot be determined after using these tools, use <code>@since Unknown</code>.

Anything ported over from WPMU should use <code>@since MU (3.0.0)</code>. Existing <code>@since MU (3.0.0)</code> tags should not be changed.

<strong>Code Refactoring:</strong> It is permissible to space out the specific action or filter lines being documented to meet the coding standards, but do not refactor other code in the file.
<h3>Formatting Guidelines</h3>

[info]
WordPress' inline documentation standards for PHP are specifically tailored for optimum output on the <a href="https://developer.wordpress.org/reference/">official Code Reference</a>. As such, following the standards in core and formatting as described below are <em>extremely</em> important to ensure expected output.
[/info]

<h4>General</h4>
DocBlocks should directly precede the hook, action, function, method, or class line. There should not be any opening/closing tags or other things between the DocBlock and the declarations to prevent the parser becoming confused.
<h4>Summary (formerly Short Description)</h4>
No HTML markup or Markdown of any kind should be used in the summary. If the text refers to an HTML element or tag, then it should be written as "image tag" or "img" element, not "&lt;img&gt;". For example:
<ul>
 	<li>Good: <em>Fires when printing the link tag in the header.</em></li>
 	<li>Bad: <em>Fires when printing the &lt;link&gt; tag in the header.</em></li>
</ul>

Inline PHPDoc tags may be used.
<h4>Description (formerly Long Description)</h4>
HTML markup should never be used outside of code examples, though Markdown can be used, as needed, in the description.

1. Lists:

Use a hyphen (-) to create an unordered list, with a blank line before and after.

[php]
 * Description which includes an unordered list:
 *
 * - This is item 1.
 * - This is item 2.
 * - This is item 3.
 *
 * The description continues on ...
[/php]

Use numbers to create an ordered list, with a blank line before and after.

[php]
 * Description which includes an ordered list:
 *
 * 1. This is item 1.
 * 2. This is item 2.
 * 3. This is item 3.
 *
 * The description continues on ...
[/php]

2. Code samples would be created by indenting every line of the code by 4 spaces, with a blank line before and after. Blank lines in code samples also need to be indented by four spaces. Note that examples added in this way will be output in &lt;pre&gt; tags and <em>not</em> syntax-highlighted.

[php]
 * Description including a code sample:
 *
 *    $status = array(
 *        'draft'   => __( 'Draft' ),
 *        'pending' => __( 'Pending Review' ),
 *        'private' => __( 'Private' ),
 *        'publish' => __( 'Published' )
 *    );
 *
 * The description continues on ...
[/php]

3. Links in the form of URLs, such as related Trac tickets or other documentation, should be added in the appropriate place in the DocBlock using the <code>@link</code> tag:

[php]
 * Description text.
 *
 * @link https://core.trac.wordpress.org/ticket/20000
[/php]

<h4><code>@since</code> Section (Changelogs)</h4>
Every function, hook, class, and method should have a corresponding <code>@since</code> version associated with it (more on that below).

No HTML should be used in the descriptions for <code>@since</code> tags, though limited Markdown can be used as necessary, such as for adding backticks around variables, arguments, or parameter names, e.g. <code>`$variable`</code>

Versions should be expressed in the 3-digit <code>x.x.x</code> style:

[php]
 * @since 4.4.0
[/php]

If significant changes have been made to a function, hook, class, or method, additional <code>@since</code> tags, versions, and descriptions should be added to provide a changelog for that function.

"Significant changes" include but are not limited to:
<ul>
 	<li>Adding new arguments or parameters</li>
 	<li>Required arguments becoming optional</li>
 	<li>Changing default/expected behaviors</li>
 	<li>Functions or methods becoming wrappers for new APIs</li>
</ul>
PHPDoc supports multiple <code>@since</code> versions in DocBlocks for this explicit reason. When adding changelog entries to the <code>@since</code> block, a version should be cited, and a description should be added in sentence case and form and end with a period:

[php]
 * @since 3.0.0
 * @since 3.8.0 Added the `post__in` argument.
 * @since 4.1.0 The `$force` parameter is now optional.
[/php]

<h4>Other Descriptions</h4>
<code>@param</code>, <code>@type</code>, <code>@return</code>: No HTML should be used in the descriptions for these tags, though limited Markdown can be used as necessary, such as for adding backticks around variables, e.g. <code>`$variable`</code>.
<ul>
 	<li>Inline <code>@see</code> tags can also be used to auto-link hooks in core:
<ul>
 	<li>Hooks, e.g. <code>{@see 'save_post'}</code></li>
 	<li>Dynamic hooks, e.g. <code>{@see '$old_status_to_$new_status'}</code> (Note that any extra curly braces have been removed inside the quotes)</li>
</ul>
</li>
 	<li>Default or available values should use single quotes, e.g. 'draft'. Translatable strings should be identified as such in the description.</li>
 	<li>HTML elements and tags should be written as "audio element" or "link tag".</li>
</ul>
<h4>Line wrapping</h4>
DocBlock text should wrap to the next line after 80 characters of text. If the DocBlock itself is indented on the left 20 character positions, the wrap could occur at position 100, but should not extend beyond a total of 120 characters wide.
<h2>DocBlock Formatting</h2>
The examples provided in each section below show the expected DocBlock content and tags, as well as the exact formatting. Use spaces inside the DocBlock, not tabs, and ensure that items in each tag group are aligned according to the examples.
<h3>1. Functions &amp; Class Methods</h3>
Functions and class methods should be formatted as follows:
<ul>
 	<li><strong>Summary:</strong> A brief, one sentence explanation of the purpose of the function spanning a maximum of two lines. Use a period at the end.</li>
 	<li><strong>Description:</strong> A supplement to the summary, providing a more detailed description. Use a period at the end of sentences.</li>
 	<li><strong>@ignore</strong> Used when an element is meant only for internal use and should be skipped from parsing.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
 	<li><strong>@access:</strong> Only used for core-only functions or classes implementing "private" core APIs. If the element is private it will be output with a message stating its intention for internal use.</li>
 	<li><strong>@see:</strong> Reference to a function, method, or class that is heavily-relied on within. See the note above about inline <code>@see</code> tags for expected formatting.</li>
 	<li><strong>@link:</strong> URL that provides more information. This should never been used to reference another function, hook, class, or method, see <code>@see</code>.</li>
 	<li><strong>@global:</strong> List PHP globals that are used within the function or method, with an optional description of the global. If multiple globals are listed, they should be aligned by type, variable, and description, with each other as a group.</li>
 	<li><strong>@param:</strong> Note if the parameter is <em>Optional</em> before the description, and include a period at the end. The description should mention accepted values as well as the default. For example: <em>Optional. This value does something. Accepts 'post', 'term', or empty. Default empty.</em></li>
 	<li><strong>@return:</strong> Should contain all possible return types, and a description for each. Use a period at the end. Note: <code>@return</code> void should not be used outside of the default bundled themes.</li>
</ul>

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @see Function/method/class relied on
 * @link URL
 * @global type $varname Description.
 * @global type $varname Description.
 *
 * @param type $var Description.
 * @param type $var Optional. Description. Default.
 * @return type Description.
 */
[/php]

<h4>1.1 Parameters That Are Arrays</h4>
Parameters that are an array of arguments should be documented in the "originating" function only, and cross-referenced via an <code>@see</code> tag in corresponding DocBlocks.

Array values should be documented using WordPress' flavor of hash notation style similar to how <a href="http://make.wordpress.org/core/handbook/inline-documentation-standards/php-documentation-standards/#4-hooks-actions-and-filters">Hooks</a> can be documented, each array value beginning with the <code>@type</code> tag, and taking the form of:

[php]
*     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
*                     (aligned with Description, if wraps to a new line)
[/php]

An example of an "originating" function and re-use of an argument array is <a href="https://core.trac.wordpress.org/browser/branches/4.0/src/wp-includes/http.php#L115"><code>wp_remote_request|post|get|head()</code></a>.

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x 
 *
 * @param type  $var Description.
 * @param array $args {
 *     Optional. An array of arguments.
 *
 *     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
 *                     (aligned with Description, if wraps to a new line)
 *     @type type $key Description.
 * }
 * @param type  $var Description.
 * @return type Description.
 */
[/php]

In most cases, there is no need to mark individual arguments in a hash notation as <em>optional</em>, as the entire array is usually optional. Specifying "Optional." in the hash notation description should suffice. In the case where the array is NOT optional, individual key/value pairs may be optional and should be marked as such as necessary.
<h4>1.2 Deprecated Functions</h4>
If the function is deprecated and should not be used any longer, the <code>@deprecated</code> tag, along with the version and description of what to use instead, should be added. Note the additional use of an <code>@see</code> tag – the Code Reference uses this information to attempt to link to the replacement function.

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 * @deprecated x.x.x Use new_function_name()
 * @see new_function_name()
 *
 * @param type $var Optional. Description.
 * @param type $var Description.
 * @return type Description.
 */
[/php]

<h3>2. Classes</h3>
Class DocBlocks should be formatted as follows:
<ul>
 	<li><strong>Summary:</strong> A brief, one sentence explanation of the <strong>purpose</strong> of the class spanning a maximum of two lines. Use a period at the end.</li>
 	<li><strong>Description:</strong> A supplement to the summary, providing a more detailed description. Use a period at the end.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
</ul>

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 */
[/php]

If documenting a sub-class, it's also helpful to include an `@see` tag reference to the super class:

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @see Super_Class
 */
[/php]

<h4>2.1 Class Members</h4>
<h5>2.1.1 Properties</h5>
Class properties should be formatted as follows:
<ul>
 	<li><strong>Summary:</strong> Use a period at the end.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
 	<li><strong>@var:</strong> Formatted the same way as <code>@param</code>, though the description may be omitted.</li>
</ul>

[php]
/**
 * Summary.
 *
 * @since x.x.x
 * @var type $var Description.
 */
[/php]

<h5>2.1.2 Constants</h5>
<ul>
 	<li><strong>Summary:</strong> Use a period at the end.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
 	<li><strong>@var:</strong> Formatted the same way as <code>@param</code>, though the description may be omitted.</li>
</ul>

[php]
/**
 * Summary.
 *
 * @since x.x.x
 * @var type $var Description.
 */
const NAME = value;
[/php]

<h3>3. Requires and Includes</h3>
Files required or included should be documented with a summary description DocBlock. Optionally, this may apply to inline <code>get_template_part()</code> calls as needed for clarity.

[php]
/**
 * Summary.
 */
require_once( ABSPATH . WPINC . '/filename.php' );
[/php]

<h3>4. Hooks (Actions and Filters)</h3>
Both action and filter hooks should be documented on the line immediately preceding the call to <code>do_action()</code> or <code>do_action_ref_array()</code>, or <code>apply_filters()</code> or <code>apply_filters_ref_array()</code>, and formatted as follows:
<ul>
 	<li><strong>Summary:</strong> A brief, one line explanation of the purpose of the hook. Use a period at the end.</li>
 	<li><strong>Description:</strong> A supplemental description to the summary, if warranted.</li>
 	<li><strong>@ignore</strong> Used when a hook is meant only for internal use and should be skipped from parsing.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
 	<li><strong>@param:</strong> If the parameter is an array of arguments, document each argument using a hash notation (described above in the <em>Parameters That Are Arrays</em> section), and include a period at the end of each line.</li>
</ul>
Note that <code>@return</code> is <em>not</em> used for hook documentation, because action hooks return nothing, and filter hooks always return their first parameter.

[php]
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @param type  $var Description.
 * @param array $args {
 *     Short description about this hash.
 *
 *     @type type $var Description.
 *     @type type $var Description.
 * }
 * @param type  $var Description.
 */
[/php]

If a hook is in the middle of a block of HTML or a long conditional, the DocBlock should be placed on the line immediately before the start of the HTML block or conditional, even if it means forcing line-breaks/PHP tags in a continuous line of HTML.

Tools to use when searching for the version a hook was added are <a href="http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate">svn blame</a>, or the <a href="http://adambrown.info/p/wp_hooks">WordPress Hooks Database</a> for older hooks. If, after using these tools, the version number cannot be determined, use <code>@since Unknown</code>.
<h4>4.1 Duplicate Hooks</h4>
Occasionally, hooks will be used multiple times in the same or separate core files. In these cases, rather than list the entire DocBlock every time, only the first-added or most logically-placed version of an action or filter will be fully documented. Subsequent versions should have a single-line comment.

For actions:

[php]
/** This action is documented in path/to/filename.php */ 
[/php]

For filters:

[php]
/** This filter is documented in path/to/filename.php */
[/php]

To determine which instance should be documented, search for multiples of the same hook tag, then use <a href="http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate">svn blame</a> to find the first use of the hook in terms of the earliest revision. If multiple instances of the hook were added in the same release, document the one most logically-placed as the "primary".
<h3>5. Inline Comments</h3>
Inline comments inside methods and functions should be formatted as follows:
<h4>5.1 Single line comments</h4>

[php]
// Allow plugins to filter an array.
[/php]

<h4>5.2 Multi-line comments</h4>

[php]
/* 
 * This is a comment that is long enough to warrant being stretched over
 * the span of multiple lines. You'll notice this follows basically
 * the same format as the PHPDoc wrapping and comment block style.
 */
[/php]

<strong>Important note:</strong> Multi-line comments must not begin with <code>/**</code> (double asterisk) as the parser might mistake it for a DocBlock. Use <code>/*</code> (single asterisk) instead.
<h3>6. File Headers</h3>
The file header DocBlock is used to give an overview of what is contained in the file.

Whenever possible, <strong>all</strong> WordPress files should contain a header DocBlock, regardless of the file's contents – this includes files containing classes.

[php]
/**
 * Summary (no period for file headers)
 *
 * Description. (use period)
 *
 * @link URL
 *
 * @package WordPress
 * @subpackage Component
 * @since x.x.x (when the file was introduced)
 */
[/php]

The <em>Summary</em> section is meant to serve as a succinct description of <strong>what</strong> specific purpose the file serves.

Examples:
<ul>
 	<li>Good: "Widgets API: WP_Widget class"</li>
 	<li>Bad: "Core widgets class"</li>
</ul>
The <em>Description </em>section can be used to better explain overview information for the file such as how the particular file fits into the overall makeup of an API or component.

Examples:
<ul>
 	<li>Good: "The Widgets API is comprised of the WP_Widget and WP_Widget_Factory classes in addition to a variety of top-level functionality that implements the Widgets and related sidebar APIs. WordPress registers a number of common widgets by default."</li>
</ul>
<h3>7. Constants</h3>
The constant DocBlock is used to give a description of the constant for better use and understanding.

Constants should be formatted as follows:
<ul>
 	<li><strong>Summary:</strong> Use a period at the end.</li>
 	<li><strong>@since x.x.x:</strong> Should always be 3-digit (e.g. <code>@since 3.9.0</code>). Exception is <code>@since MU (3.0.0)</code>.</li>
 	<li><strong>@var:</strong> Formatted the same way as <code>@param</code>. The description is optional.</li>
</ul>

[php]
/**
 * Summary.
 *
 * @since x.x.x (if available)
 * @var type $var Description.
 */
[/php]

<h2>PHPDoc Tags</h2>
Common PHPDoc tags used in WordPress include <code>@since</code>, <code>@see</code>, <code>@global</code> <code>@param</code>, and <code>@return</code> (see table below for full list).

For the most part, tags are used correctly, but not all the time. For instance, sometimes you'll see an <code>@link</code> tag inline, linking to a separate function or method. "Linking" to known classes, methods, or functions is not necessary, as the Code Reference automatically links these elements. For "linking" hooks inline, the proper tag to use is <code>@see</code> – see the <em>Other Descriptions</em> section.
<table>
<tbody>
<tr>
<th style="width: 10%">Tag</th>
<th style="width: 20%">Usage</th>
<th style="width: 70%">Description</th>
</tr>
<tr>
<td><strong>@access</strong></td>
<td>private</td>
<td>Only used in limited circumstances, and only when private, such as for core-only functions or core classes implementing "private" APIs. Used directly below the <strong>@since</strong> line in block.</td>
</tr>
<tr>
<td><strong>@deprecated</strong></td>
<td>version x.x.x
replacement function name</td>
<td>What version of WordPress the function/method was deprecated. Use 3-digit version number. Should be accompanied by a matching <code>@see</code> tag.</td>
</tr>
<tr>
<td><strong>@global</strong></td>
<td>datatype $variable
description</td>
<td>Document global(s) used in the function/method. For boolean and integer types, use <code>bool</code> and <code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong>@internal</strong></td>
<td>information string</td>
<td>Typically used for adding notes for internal use only.</td>
</tr>
<tr>
<td><strong>@ignore</strong></td>
<td>(standalone)</td>
<td>Used to skip parsing of the entire element.</td>
</tr>
<tr>
<td><strong>@link</strong></td>
<td>URL</td>
<td>Link to additional information for the function/method.
For an external script/library, links to source.
Not to be used for related functions/methods; use <strong>@see</strong> instead.</td>
</tr>
<tr>
<td><strong>@method</strong></td>
<td>returntype
description</td>
<td>Shows a "magic" method found inside the class.</td>
</tr>
<tr>
<td><strong>@package</strong></td>
<td>packagename</td>
<td>Specifies package that all functions, includes, and defines in the file belong to. Found in DocBlock at top of the file. For core (and bundled themes), this is always <strong>WordPress</strong>.</td>
</tr>
<tr>
<td><strong>@param</strong></td>
<td>datatype $variable
description</td>
<td>Function/method parameter of the format: parameter type, variable name, description, default behavior. For boolean and integer types, use <code>bool</code> and <code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong>@return</strong></td>
<td>datatype description</td>
<td>Document the return value of functions or methods. <code>@return void</code> should not be used outside of the default bundled themes. For boolean and integer types, use <code>bool</code> and <code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong>@see</strong></td>
<td>elementname</td>
<td>References another function/method/class the function/method relies on. Should only be used inline for "linking" hooks.</td>
</tr>
<tr>
<td><strong>@since</strong></td>
<td>version x.x.x</td>
<td>Documents release version function/method was added. Use 3-digit version number – this is to aid with version searches, and for use when comparing versions in code. Exception is <code>@since MU (3.0.0)</code>.</td>
</tr>
<tr>
<td><strong>@static</strong></td>
<td>(standalone)</td>
<td>Note: This tag has been used in the past, but should no longer be used.
Just using the static keyword in your code is enough for PhpDocumentor on PHP5 to recognize static variables and methods, and PhpDocumentor will mark them as static.</td>
</tr>
<tr>
<td><strong>@staticvar</strong></td>
<td>datatype $variable
description</td>
<td>Note: This tag has been used in the past, but should no longer be used.
Document a static variable's use in a function/method. For boolean and integer types, use <code>bool</code> and <code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong>@subpackage</strong></td>
<td>subpackagename</td>
<td>For page-level DocBlock, specifies the Component that all functions and defines in file belong to. For class-level DocBlock, specifies the subpackage/component the class belongs to.</td>
</tr>
<tr>
<td><strong>@todo</strong></td>
<td>information string</td>
<td>Documents planned changes to an element that have not been implemented.</td>
</tr>
<tr>
<td><strong>@type</strong></td>
<td>datatype description for an argument array value</td>
<td>Used to denote argument array value types. See the <strong>Hooks</strong> or <strong>Parameters That Are Arrays</strong> sections for example syntax.</td>
</tr>
<tr>
<td><strong>@uses</strong></td>
<td>class::methodname()
class::$variablename
functionname()</td>
<td><strong>Note:</strong> This tag has been used in the past, but should no longer be used.
References a key function/method used. May include a short description.</td>
</tr>
<tr>
<td><strong>@var</strong></td>
<td>datatype description</td>
<td>Data type for a class variable and short description. Callbacks are marked <strong>callback</strong>.</td>
</tr>
</tbody>
</table>
[info] PHPDoc tags work with some text editors/IDEs to display more information about a piece of code. It is useful to developers using those editors to understand what the purpose is, and where they would use it in their code. PhpStorm and Netbeans already support PHPDoc.

The following text editors/IDEs have extensions/bundles you can install that will help you auto-create DocBlocks:
<ul>
 	<li>Notepad++: <a href="http://sourceforge.net/projects/nppdocit/">DocIt for Notepad++</a> (Windows)</li>
 	<li>TextMate: <a href="https://github.com/textmate/php.tmbundle">php.tmbundle</a> (Mac)</li>
 	<li>SublimeText: <a href="https://packagecontrol.io/search/phpdoc">sublime packages</a> (Windows, Mac, Linux)</li>
</ul>

Note: Even with help generating DocBlocks, most code editors don't do a very thorough job – it's likely you'll need to manually fill in certain areas of any generated DocBlocks.
[/info]
<h3>Deprecated Tags</h3>
<blockquote><strong>Preface:</strong> For the time being, and for the sake of consistency, WordPress Core will continue to use <code>@subpackage</code> tags – both when writing new DocBlocks, and editing old ones.

Only when the new – external – PSR-5 recommendations are finalized, will across-the-board changes be considered, such as deprecating certain tags.</blockquote>
As proposed in the <a href="https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md">new PSR-5</a> recommendations, the following PHPDoc tag should be deprecated:
<ul>
 	<li><code>@subpackage</code> (in favor of a unified package tag: <code>@package Package\Subpackage</code>)</li>
</ul>
<h3>Other Tags</h3>
<strong>@package Tag in Plugins and Themes (bundled themes excluded)</strong>

Third-party plugin and theme authors <strong>must not</strong> use <code>@package WordPress</code> in their plugins or themes. The <code>@package</code> name for plugins should be the plugin name; for themes, it should be the theme name, spaced with underscores: <code>Twenty_Fifteen</code>.

<strong>@author Tag</strong>

It is WordPress' policy not to use the <code>@author</code> tag, except in the case of maintaining it in external libraries. We do not want to imply any sort of "ownership" over code that might discourage contribution.

<strong>@copyright and @license Tags</strong>

The <code>@copyright</code> and <code>@license</code> tags are used in external libraries and scripts, and should not be used in WordPress core files.
<ul>
 	<li><code>@copyright</code> is used to specify external script copyrights.</li>
 	<li><code>@license</code> is used to specify external script licenses.</li>
</ul>
<h2>Resources</h2>
<ul>
 	<li><a href="http://en.wikipedia.org/wiki/PHPDoc">Wikipedia</a></li>
 	<li><a href="http://pear.php.net/manual/en/standards.sample.php">PEAR Standards</a></li>
 	<li><a href="http://www.phpdoc.org/">phpDocumentor</a></li>
 	<li><a href="http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.pkg.html">phpDocumentor Tutorial Tags</a></li>
 	<li><a href="https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md">Draft PSR-5 recommendations</a></li>
</ul>
