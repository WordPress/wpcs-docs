# PHP Coding Standards

Some parts of the WordPress code structure for PHP markup are inconsistent in their style. WordPress is working to gradually improve this by helping users maintain a consistent style so the code can become clean and easy to read at a glance.

Keep the following points in mind when writing PHP code for WordPress, whether for core programming code, plugins, or themes. The guidelines are similar to <a href="http://pear.php.net/manual/en/standards.php">Pear standards</a> in many ways, but differ in some key respects.

See also: <a href="https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/">PHP Documentation Standards</a>.
<h2>PHP</h2>
<h3>Single and Double Quotes</h3>
Use single and double quotes when appropriate. If you're not evaluating anything in the string, use single quotes. You should almost never have to escape quotes in a string, because you can just alternate your quoting style, like so:

```php
echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
echo "<a href='$link' title='$linktitle'>$linkname</a>";
```

Text that goes into attributes should be run through <code>esc_attr()</code> so that single or double quotes do not end the attribute value and invalidate the HTML and cause a security issue. See <a href="http://codex.wordpress.org/Data_Validation">Data Validation</a> in the Codex for further details.
<h3>Indentation</h3>
Your indentation should always reflect logical structure. Use <strong>real tabs</strong> and <strong>not spaces</strong>, as this allows the most flexibility across clients.

Exception: if you have a block of code that would be more readable if things are aligned, use spaces:

```php
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

For associative arrays, <em>each item</em> should start on a new line when the array contains more than one item:

```php
$query = new WP_Query( array( 'ID' => 123 ) );
```

```php
$args = array(
[tab]'post_type'   => 'page',
[tab]'post_author' => 123,
[tab]'post_status' => 'publish',
);
 
$query = new WP_Query( $args );
```

Note the comma after the last array item: this is recommended because it makes it easier to change the order of the array, and makes for cleaner diffs when new items are added.

```php
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```

For <code>switch</code> structures <code>case</code> should indent one tab from the <code>switch</code> statement and <code>break</code> one tab from the <code>case</code> statement.

```php
switch ( $type ) {
[tab]case 'foo':
[tab][tab]some_function();
[tab][tab]break;
[tab]case 'bar':
[tab][tab]some_function();
[tab][tab]break;
}
```

<strong>Rule of thumb:</strong> Tabs should be used at the beginning of the line for indentation, while spaces can be used mid-line for alignment.
<h3>Brace Style</h3>
Braces shall be used for all blocks in the style shown here:

```php
if ( condition ) {
	action1();
	action2();
} elseif ( condition2 && condition3 ) {
	action3();
	action4();
} else {
	defaultaction();
}
```

If you have a really long block, consider whether it can be broken into two or more shorter blocks, functions, or methods, to reduce complexity, improve ease of testing, and increase readability.

Braces should always be used, even when they are not required:

```php
if ( condition ) {
	action0();
}

if ( condition ) {
	action1();
} elseif ( condition2 ) {
	action2a();
	action2b();
}

foreach ( $items as $item ) {
	process_item( $item );
}
```

Note that requiring the use of braces just means that <em>single-statement inline control structures</em> are prohibited. You are free to use the <a href="http://php.net/manual/en/control-structures.alternative-syntax.php" rel="nofollow">alternative syntax for control structures</a> (e.g. <code>if</code>/<code>endif</code>, <code>while</code>/<code>endwhile</code>)—especially in your templates where PHP code is embedded within HTML, for instance:

```php
<?php if ( have_posts() ) : ?>
	<div class="hfeed">
		<?php while ( have_posts() ) : the_post(); ?>
			<article id="post-<?php the_ID() ?>" class="<?php post_class() ?>">
				<!-- ... -->
			</article>
		<?php endwhile; ?>
	</div>
<?php endif; ?>
```

<h3>Use <code>elseif</code>, not <code>else if</code></h3>
<code>else if</code> is not compatible with the colon syntax for <code>if|elseif</code> blocks. For this reason, use <code>elseif</code> for conditionals.

<h3>Declaring Arrays</h3>

Using long array syntax ( <code>array( 1, 2, 3 )</code> ) for declaring arrays is generally more readable than short array syntax ( <code>[ 1, 2, 3 ]</code> ), particularly for those with vision difficulties. Additionally, it's much more descriptive for beginners.

Arrays must be declared using long array syntax.

<h3>Closures (Anonymous Functions)</h3>

Where appropriate, closures may be used as an alternative to creating new functions to pass as callbacks. For example:

```php
$caption = preg_replace_callback(
    '/<[a-zA-Z0-9]+(?: [^<>]+>)*/',
    function ( $matches ) {
        return preg_replace( '/[\r\n\t]+/', ' ', $matches[0] );
    },
    $caption
);
```

Closures must not be passed as filter or action callbacks, as they cannot be removed by <code>remove_action()</code> / <code>remove_filter()</code> (see <a href="https://core.trac.wordpress.org/ticket/46635">#46635</a> for a proposal to address this).

<h3>Multiline Function Calls</h3>

When splitting a function call over multiple lines, each parameter must be on a separate line. Single line inline comments can take up their own line.

Each parameter must take up no more than a single line. Multi-line parameter values must be assigned to a variable and then that variable should be passed to the function call.

```php
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: Friend's name */
    esc_html__( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);
 
$a = foo(
    $bar,
    $baz,
    /* translators: %s: cat */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

<h3>Regular Expressions</h3>
Perl compatible regular expressions (<a href="http://php.net/pcre">PCRE</a>, <code>preg_</code> functions) should be used in preference to their POSIX counterparts. Never use the <code>/e</code> switch, use <code>preg_replace_callback</code> instead.

It's most convenient to use single-quoted strings for regular expressions since, contrary to double-quoted strings, they have only two metasequences: <code>\'</code> and <code>\\</code>.


<h3>Opening and Closing PHP Tags</h3>
When embedding multi-line PHP snippets within an HTML block, the PHP open and close tags must be on a line by themselves.

Correct (Multiline):

```php
function foo() {
    ?>
    <div>
        <?php
        echo bar(
            $baz,
            $bat
        );
        ?>
    </div>
    <?php
}
```

Correct (Single Line):

```php
<input name="<?php echo esc_attr( $name ); ?>" />
```

Incorrect:

```php
if ( $a === $b ) { ?>
<some html>
<?php }
```

<h3>No Shorthand PHP Tags</h3>
<strong>Important:</strong> Never use shorthand PHP start tags. Always use full PHP tags.

Correct:

```php
<?php ... ?>
<?php echo $var; ?>
```

Incorrect:

```php
<? ... ?>
<?= $var ?>
```

<h3>Remove Trailing Spaces</h3>
Remove trailing whitespace at the end of each line of code. Omitting the closing PHP tag at the end of a file is preferred. If you use the tag, make sure you remove trailing whitespace.
<h3>Space Usage</h3>
Always put spaces after commas, and on both sides of logical, comparison, string and assignment operators.

```php
x === 23
foo && bar
! foo
array( 1, 2, 3 )
$baz . '-5'
$term .= 'X'
```

Put spaces on both sides of the opening and closing parentheses of <code>if</code>, <code>elseif</code>, <code>foreach</code>, <code>for</code>, and <code>switch</code> blocks.

```php
foreach ( $foo as $bar ) { ...
```

When defining a function, do it like so:

```php
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...

function my_other_function() { ...
```

When calling a function, do it like so:

```php
my_function( $param1, func_param( $param2 ) );
my_other_function();
```

When performing logical comparisons, do it like so:

```php
if ( ! $foo ) { ...
```

<a title="type casting" href="http://www.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting" target="_blank">Type casts</a> must be lowercase. Always prefer the short form of type casts, <code>(int)</code> instead of <code>(integer)</code> and <code>(bool)</code> rather than <code>(boolean)</code>. For float casts use <code>(float)</code>.:

```php
foreach ( (array) $foo as $bar ) { ...

$foo = (bool) $bar;
```

When referring to array items, only include a space around the index if it is a variable, for example:

```php
$x = $foo['bar']; // correct
$x = $foo[ 'bar' ]; // incorrect

$x = $foo[0]; // correct
$x = $foo[ 0 ]; // incorrect

$x = $foo[ $bar ]; // correct
$x = $foo[$bar]; // incorrect
```

In a <code>switch</code> block, there must be no space before the colon for a case statement.

```php
switch ( $foo ) {
	case 'bar': // correct
	case 'ba' : // incorrect
}
```

Similarly, there should be no space before the colon on return type declarations.

```php
function sum( $a, $b ): float {
	return $a + $b;
}
```

Unless otherwise specified, parentheses should have spaces inside of them.

```php
if ( $foo && ( $bar || $baz ) ) { ...

my_function( ( $x - 1 ) * 5, $y );
```

<h3>Formatting SQL statements</h3>
When formatting SQL statements you may break it into several lines and indent if it is sufficiently complex to warrant it. Most statements work well as one line though. Always capitalize the SQL parts of the statement like <code>UPDATE</code> or <code>WHERE</code>.

Functions that update the database should expect their parameters to lack SQL slash escaping when passed. Escaping should be done as close to the time of the query as possible, preferably by using <code>$wpdb-&gt;prepare()</code>

<code>$wpdb-&gt;prepare()</code> is a method that handles escaping, quoting, and int-casting for SQL queries. It uses a subset of the <code>sprintf()</code> style of formatting. Example :

```php
$var = "dangerous'"; // raw data that may or may not need to be escaped
$id = some_foo_number(); // data we expect to be an integer, but we're not certain

$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

<code>%s</code> is used for string placeholders and <code>%d</code> is used for integer placeholders. Note that they are not 'quoted'! <code>$wpdb-&gt;prepare()</code> will take care of escaping and quoting for us. The benefit of this is that we don't have to remember to manually use <code><a href="https://developer.wordpress.org/reference/functions/esc_sql/">esc_sql</a>()</code>, and also that it is easy to see at a glance whether something has been escaped or not, because it happens right when the query happens.

See <a title="Data Validation" href="http://codex.wordpress.org/Data_Validation" target="_blank">Data Validation</a> in the Codex for more information.
<h3>Database Queries</h3>
Avoid touching the database directly. If there is a defined function that can get the data you need, use it. Database abstraction (using functions instead of queries) helps keep your code forward-compatible and, in cases where results are cached in memory, it can be many times faster.

If you must touch the database, get in touch with some developers by posting a message to the <a title="wp-hackers mailing list" href="http://codex.wordpress.org/Mailing_Lists#Hackers" target="_blank">wp-hackers mailing list</a>. They may want to consider creating a function for the next WordPress version to cover the functionality you wanted.
<h3>Naming Conventions</h3>
Use lowercase letters in variable, action/filter, and function names (never <code>camelCase</code>). Separate words via underscores. Don't abbreviate variable names unnecessarily; let the code be unambiguous and self-documenting.

```php
function some_name( $some_variable ) { [...] }
```

Class names should use capitalized words separated by underscores. Any acronyms should be all upper case.

```php
class Walker_Category extends Walker { [...] }
class WP_HTTP { [...] }
```

Constants should be in all upper-case with underscores separating words:

```php
define( 'DOING_AJAX', true );
```

Files should be named descriptively using lowercase letters. Hyphens should separate words.

```php
my-plugin-name.php
```

Class file names should be based on the class name with <code>class-</code> prepended and the underscores in the class name replaced with hyphens, for example <code>WP_Error</code> becomes:

```php
class-wp-error.php
```

This file-naming standard is for all current and new files with classes. There is one exception for three files that contain code that got ported into BackPress: class.wp-dependencies.php, class.wp-scripts.php, class.wp-styles.php. Those files are prepended with <code>class.</code>, a dot after the word class instead of a hyphen.

Files containing template tags in <code>wp-includes</code> should have <code>-template</code> appended to the end of the name so that they are obvious.

```php
general-template.php
```

<h3>Only one object structure (class/interface/trait) should be declared per file</h3>

For instance, if we have a file called `class-example-class.php` it can only contain one class in that file.

```php
// Incorrect: file class-example-class.php.
class Example_Class { [...] }

class Example_Class_Extended { [...] }
```

The second class should be in its own file called `class-example-class-extended.php`.

```php
// Correct: file class-example-class.php.
class Example_Class { [...] }
```

```php
// Correct: file class-example-class-extended.php.
class Example_Class_Extended { [...] }
```

<h3>Self-Explanatory Flag Values for Function Arguments</h3>
Prefer string values to just <code>true</code> and <code>false</code> when calling functions.

```php
// Incorrect
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // what does true mean?
eat( 'dogfood', false ); // what does false mean? The opposite of true?
```

Since PHP doesn't support named arguments, the values of the flags are meaningless, and each time we come across a function call like the examples above, we have to search for the function definition. The code can be made more readable by using descriptive string values, instead of booleans.

```php
// Correct
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

When more words are needed to describe the function parameters, an <code>$args</code> array may be a better pattern.

```php
// Even Better
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

<h3>Interpolation for Naming Dynamic Hooks</h3>
Dynamic hooks should be named using interpolation rather than concatenation for readability and discoverability purposes.

Dynamic hooks are hooks that include dynamic values in their tag name, e.g. <code>{$new_status}_{$post-&gt;post_type}</code> (publish_post).

Variables used in hook tags should be wrapped in curly braces <code>{</code> and <code>}</code>, with the complete outer tag name wrapped in double quotes. This is to ensure PHP can correctly parse the given variables' types within the interpolated string.

```php
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

Where possible, dynamic values in tag names should also be as succinct and to the point as possible. <code>$user_id</code> is much more self-documenting than, say, <code>$this-&gt;id</code>.
<h3>Ternary Operator</h3>
<a title="Ternary" href="http://en.wikipedia.org/wiki/Ternary_operation" target="_blank">Ternary</a> operators are fine, but always have them test if the statement is true, not false. Otherwise, it just gets confusing. (An exception would be using <code>! empty()</code>, as testing for false here is generally more intuitive.)

The short ternary operator must not be used.

For example:

```php
// (if statement is true) ? (do this) : (else, do this);
$musictype = ( 'jazz' === $music ) ? 'cool' : 'blah';
// (if field is not empty ) ? (do this) : (else, do this);
```

<h3>Yoda Conditions</h3>

```php
if ( true === $the_force ) {
	$victorious = you_will( $be );
}
```

When doing logical comparisons involving variables, always put the variable on the right side and put constants, literals, or function calls on the left side. If neither side is a variable, the order is not important. (In <a href="https://en.wikipedia.org/wiki/Value_(computer_science)#Assignment:_l-values_and_r-values">computer science terms</a>, in comparisons always try to put l-values on the right and r-values on the left.)

In the above example, if you omit an equals sign (admit it, it happens even to the most seasoned of us), you'll get a parse error, because you can't assign to a constant like <code>true</code>. If the statement were the other way around <code>( $the_force = true )</code>, the assignment would be perfectly valid, returning <code>1</code>, causing the if statement to evaluate to <code>true</code>, and you could be chasing that bug for a while.

A little bizarre, it is, to read. Get used to it, you will.

This applies to ==, !=, ===, and !==. Yoda conditions for &lt;, &gt;, &lt;= or &gt;= are significantly more difficult to read and are best avoided.
<h3>Clever Code</h3>
In general, readability is more important than cleverness or brevity.

```php
isset( $var ) || $var = some_function();
```

Although the above line is clever, it takes a while to grok if you're not familiar with it. So, just write it like this:

```php
if ( ! isset( $var ) ) {
	$var = some_function();
}
```

Unless absolutely necessary, loose comparisons should not be used, as their behaviour can be misleading.

Correct:

```php
if ( 0 === strpos( 'WordPress', 'foo' ) ) {
	echo __( 'Yay WordPress!' );
}
```

Incorrect:

```php
if ( 0 == strpos( 'WordPress', 'foo' ) ) {
	echo __( 'Yay WordPress!' );
}
```

Assignments must not be placed in conditionals.

Correct:

```php
$data = $wpdb->get_var( '...' );
if ( $data ) {
    // Use $data
}
```

Incorrect:

```php
if ( $data = $wpdb->get_var( '...' ) ) {
    // Use $data
}
```

In a <code>switch</code> statement, it's okay to have multiple empty cases fall through to a common block. If a case contains a block, then falls through to the next block, however, this must be explicitly commented.

```php
switch ( $foo ) {
	case 'bar':	      // Correct, an empty case can fall through without comment.
	case 'baz':
		echo $foo;    // Incorrect, a case with a block must break, return, or have a comment.
	case 'cat':
		echo 'mouse';
		break;        // Correct, a case with a break does not require a comment.
	case 'dog':
		echo 'horse';
		// no break   // Correct, a case can have a comment to explicitly mention the fall through.
	case 'fish':
		echo 'bird';
		break;
}
```

The <code>goto</code> statement must never be used.

The <code>eval()</code> construct is <em>very dangerous</em>, and is impossible to secure. Additionally, the <code>create_function()</code> function, which internally performs an <code>eval()</code>, is deprecated in PHP 7.2. Both of these must not be used.

<h3>Error Control Operator <code>@</code></h3>
As noted in the <a href="http://www.php.net//manual/en/language.operators.errorcontrol.php">PHP docs</a>:
<blockquote>PHP supports one error control operator: the at sign (@). When prepended to an expression in PHP, any error messages that might be generated by that expression will be ignored.</blockquote>
While this operator does exist in Core, it is often used lazily instead of doing proper error checking. Its use is highly discouraged, as even the PHP docs also state:
<blockquote>Warning: Currently the "@" error-control operator prefix will even disable error reporting for critical errors that will terminate script execution. Among other things, this means that if you use "@" to suppress errors from a certain function and either it isn't available or has been mistyped, the script will die right there with no indication as to why.</blockquote>
<h3>Don't <code>extract()</code></h3>
Per <a title="Remove all, or at least most, uses of extract() within WordPress" href="https://core.trac.wordpress.org/ticket/22400">#22400</a>:
<blockquote><code>extract()</code> is a terrible function that makes code harder to debug and harder to understand. We should discourage it's [sic] use and remove all of our uses of it.

Joseph Scott has <a class="ext-link" href="https://blog.josephscott.org/2009/02/05/i-dont-like-phps-extract-function/">a good write-up of why it's bad</a>.</blockquote>
<h2>Credits</h2>
<ul>
 	<li>PHP standards: <a href="http://pear.php.net/manual/en/standards.php" target="_blank">Pear standards</a></li>
</ul>
<h3>Major Changes</h3>
<ul>
 	<li>November 13, 2013: <a href="http://make.wordpress.org/core/2013/11/13/proposed-coding-standards-change-always-require-braces/">Braces should always be used, even when they are optional</a></li>
 	<li>June 20, 2014: Add <a href="#error-control-operator">section</a> to discourage use of the <a href="http://www.php.net//manual/en/language.operators.errorcontrol.php">error control operator</a> (<code>@</code>). See <a href="https://irclogs.wordpress.org/chanlog.php?channel=wordpress-dev&amp;day=2014-06-20&amp;sort=asc#m873356">#wordpress-dev</a>.</li>
 	<li>October 20, 2014: Update brace usage to indicate that the alternate syntax for control structures is allowed, even encouraged. It is single-line inline control structures that are forbidden.</li>
 	<li>January 21, 2014: Add section to forbid extract().</li>
</ul>
