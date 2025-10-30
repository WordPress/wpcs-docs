# PHP Coding Standards

These PHP coding standards are intended for the WordPress community as a whole. They are mandatory for WordPress Core and we encourage you to use them for your themes and plugins as well.

While themes and plugins may choose to follow a different _coding style_, these **_coding standards_** are not just about _code style_, but also encompass established best practices regarding interoperability, translatability, and security in the WordPress ecosystem, so, even when using a different _code style_, we recommend you still adhere to the WordPress Coding Standards in regard to these best practices.

While not all code may fully comply with these standards (yet), all newly committed and/or updated code should fully comply with these coding standards.

Also see the [PHP Inline Documentation Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/) for further guidelines.

If you want to automatically check your code against this standard, you can use the official [WordPress Coding Standards](https://github.com/WordPress/WordPress-Coding-Standards) tooling, which is run using [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/).

## General

### Opening and Closing PHP Tags

When embedding multi-line PHP snippets within an HTML block, the PHP open and close tags must be on a line by themselves.

Correct (Multiline):

```php
function foo() {
    ?>
    <div>
        <?php
        echo esc_html(
            bar(
                $baz,
                $bat
            )
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

### No Shorthand PHP Tags

**Important:** Never use shorthand PHP start tags. Always use full PHP tags.

Correct:

```php
<?php ... ?>
<?php echo esc_html( $var ); ?>
```

Incorrect:

```php
<? ... ?>
<?= esc_html( $var ) ?>
```

### Single and Double Quotes

Use single and double quotes when appropriate. If you're not evaluating anything in the string, use single quotes. You should almost never have to escape quotes in a string, because you can just alternate your quoting style, like so:

```php
echo '<a href="/static/link" class="button button-primary">Link name</a>';
echo "<a href='{$escaped_link}'>text with a ' single quote</a>";
```

Text that goes into HTML or XML attributes should be escaped so that single or double quotes do not end the attribute value and invalidate the HTML, causing a security issue. See [Data Validation](https://developer.wordpress.org/plugins/security/data-validation/) in the Plugin Handbook for further details.

### Writing require/include statements

Because `require[_once]` and `include[_once]` are language constructs, they do not need parentheses around the path, so those shouldn't be used. There should only be one space between the path and the require/include keywords.

It is _strongly recommended_ to use `require[_once]` for unconditional includes. When using `include[_once]`, PHP will throw a warning when the file is not found but will continue execution, which will almost certainly lead to other errors/warnings/notices being thrown if your application depends on the file loaded, potentially leading to security leaks. For that reason, `require[_once]` is generally the better choice as it will throw a `Fatal Error` if the file cannot be found.

```php
// Correct.
require_once ABSPATH . 'file-name.php';

// Incorrect.
include_once  ( ABSPATH . 'file-name.php' );
require_once     __DIR__ . '/file-name.php';
```

## Naming

### Naming Conventions

Use lowercase letters in variable, action/filter, and function names (never `camelCase`). Separate words via underscores. Don't abbreviate variable names unnecessarily; let the code be unambiguous and self-documenting.

```php
function some_name( $some_variable ) {}
```

For function parameter names, it is _strongly recommended_ to avoid reserved keywords as names, as it leads to hard to read and confusing code when using the PHP 8.0 "named parameters in function calls" feature.
Also keep in mind that renaming a function parameter should be considered a breaking change since PHP 8.0, so name function parameters with due care!

Class, trait, interface and enum names should use capitalized words separated by underscores. Any acronyms should be all upper case.

```php
class Walker_Category extends Walker {}
class WP_HTTP {}

interface Mailer_Interface {}
trait Forbid_Dynamic_Properties {}
enum Post_Status {}
```

Constants should be in all upper-case with underscores separating words:

```php
define( 'DOING_AJAX', true );
```

Files should be named descriptively using lowercase letters. Hyphens should separate words.

```php
my-plugin-name.php
```

Class file names should be based on the class name with `class-` prepended and the underscores in the class name replaced with hyphens, for example, `WP_Error` becomes:

```php
class-wp-error.php
```

This file-naming standard is for all current and new files with classes, except test classes.
For files containing test classes, the file name should reflect the class name exactly, as per PSR4. This is to ensure cross-version [compatibility with all supported PHPUnit versions](https://github.com/sebastianbergmann/phpunit/pull/4109).

Files containing template tags in the `wp-includes` directory should have `-template` appended to the end of the name so that they are obvious.

```php
general-template.php
```

### Interpolation for Naming Dynamic Hooks

Dynamic hooks should be named using interpolation rather than concatenation for readability and discoverability purposes.

Dynamic hooks are hooks that include dynamic values in their tag name, e.g. `{$new_status}_{$post->post_type}` (publish_post).

Variables used in hook tags should be wrapped in curly braces `{` and `}`, with the complete outer tag name wrapped in double quotes. This is to ensure PHP can correctly parse the given variables' types within the interpolated string.

```php
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

Where possible, dynamic values in tag names should also be as succinct and to the point as possible. `$user_id` is much more self-documenting than, say, `$this->id`.

## Whitespace

### Space Usage

Always put spaces after commas, and on both sides of logical, arithmetic, comparison, string and assignment operators.

```php
SOME_CONST === 23;
foo() && bar();
! $foo;
array( 1, 2, 3 );
$baz . '-5';
$term .= 'X';
if ( $object instanceof Post_Type_Interface ) {}
$result = 2 ** 3; // 8.
```

Put spaces on both sides of the opening and closing parentheses of control structure blocks.

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

[Type casts](https://www.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting) must be lowercase. Always prefer the short, canonical form of type casts, `(int)` instead of `(integer)` and `(bool)` rather than `(boolean)`. For float casts use `(float)`.

```php
$foo = (bool) $bar; // Correct.
$foo = (boolean) $bar; // Incorrect.
```

When referring to array items, only include a space around the index if it is a variable, for example:

```php
$x = $foo['bar']; // Correct.
$x = $foo[ 'bar' ]; // Incorrect.

$x = $foo[0]; // Correct.
$x = $foo[ 0 ]; // Incorrect.

$x = $foo[ $bar ]; // Correct.
$x = $foo[$bar]; // Incorrect.
```

In a `switch` block, there must be no space between the `case` condition and the colon.

```php
switch ( $foo ) {
    case 'bar': // Correct.
    case 'bar' : // Incorrect.
}
```

Unless otherwise specified, parentheses should have spaces inside them.

```php
if ( $foo && ( $bar || $baz ) ) { ...

my_function( ( $x - 1 ) * 5, $y );
```

When using increment (`++`) or decrement (`--`) operators, there should be no spaces between the operator and the variable it applies to.

```php
// Correct.
for ( $i = 0; $i < 10; $i++ ) {}

// Incorrect.
for ( $i = 0; $i < 10; $i ++ ) {}
++   $b; // Multiple spaces.
```

### Indentation

Your indentation should always reflect logical structure. Use **real tabs**, **not spaces**, as this allows the most flexibility across clients.

Exception: if you have a block of code that would be more readable if things are aligned, use spaces:

```php
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

For associative arrays, _each item_ should start on a new line when the array contains more than one item:

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

For `switch` control structures, `case` statements should be indented one tab from the `switch` statement and the contents of the `case` should be indented one tab from the `case` condition statement.

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

**Rule of thumb:** Tabs should be used at the beginning of the line for indentation, while spaces can be used mid-line for alignment.

### Remove Trailing Spaces

Remove trailing whitespace at the end of each line. Omitting the closing PHP tag at the end of a file is preferred. If you use the tag, make sure you remove trailing whitespace.

There should be no trailing blank lines at the end of a function body.

## Formatting

### Brace Style

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

Note that requiring the use of braces means that _single-statement inline control structures_ are prohibited. You are free to use the [alternative syntax for control structures](https://www.php.net/manual/en/control-structures.alternative-syntax.php) (e.g. `if`/`endif`, `while`/`endwhile`)—especially in templates where PHP code is embedded within HTML, for instance:

```php
<?php if ( have_posts() ) : ?>
    <div class="hfeed">
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="<?php echo esc_attr( 'post-' . get_the_ID() ); ?>" class="<?php echo esc_attr( get_post_class() ); ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

### Declaring Arrays

Using long array syntax ( `array( 1, 2, 3 )` ) for declaring arrays is generally more readable than short array syntax ( `[ 1, 2, 3 ]` ), particularly for those with vision difficulties. Additionally, it's much more descriptive for beginners.

Arrays must be declared using long array syntax.

### Multiline Function Calls

When splitting a function call over multiple lines, each parameter must be on a separate line. Single line inline comments can take up their own line.

Each parameter must take up no more than a single line. Multi-line parameter values must be assigned to a variable and then that variable should be passed to the function call.

```php
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: Friend's name */
    __( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);

$a = foo(
    $bar,
    $baz,
    /* translators: %s: cat */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

### Type declarations

Type declarations must have exactly one space before and after the type. The nullability operator (`?`) is regarded as part of the type declaration and there should be no space between this operator and the actual type. Class/interface/enum name based type declarations should use the case of the class/interface/enum name as declared, while the keyword-based type declarations should be lowercased.

Return type declarations should have no space between the closing parenthesis of the function declaration and the colon starting a return type.

These rules apply to all structures allowing for type declarations: functions, closures, enums, catch conditions as well as the PHP 7.4 arrow functions and typed properties.

```php
// Correct.
function foo( Class_Name $parameter, callable $callable, int $number_of_things = 0 ) {
    // Do something.
}

function bar(
    Interface_Name&Concrete_Class $param_a,
    string|int $param_b,
    callable $param_c = 'default_callable'
): User|false {
    // Do something.
}

// Incorrect.
function baz(Class_Name $param_a, String$param_b,      CALLABLE     $param_c )   :   ?   iterable   {
    // Do something.
}
```

[info]
Type declaration usage has the following restrictions based on the minimum required PHP version of an application, whether it is WordPress Core, a plugin or a theme:

- The scalar `bool`, `int`, `float`, and `string` type declarations cannot be used until the minimum PHP version is PHP 7.0 or higher.
- Return type declarations cannot be used until the minimum PHP version is PHP 7.0 or higher.
- Nullable type declarations cannot be used until the minimum PHP version is PHP 7.1 or higher.
- The `iterable` and `void` type declarations cannot be used until the minimum PHP version is PHP 7.1 or higher. The `void` type can only be used as a return type.
- The `object` type declaration cannot be used until the minimum PHP version is PHP 7.2 or higher.
- Property type declarations cannot be used until the minimum PHP version is PHP 7.4 or higher.
- `static` (return type only) cannot be used until the minimum PHP version is PHP 8.0 or higher.
- `mixed` type cannot be used until the minimum PHP version is PHP 8.0 or higher. Take note that the `mixed` type includes `null`, so cannot be made nullable.
- Union types cannot be used until the minimum PHP version is PHP 8.0 or higher.
- Intersection types cannot be used until the minimum PHP version is PHP 8.1 or higher.
- `never` (return type only) cannot be used until the minimum PHP version is PHP 8.1 or higher.
- Disjunctive normal form types (combining union types and intersection types) cannot be used until the minimum PHP version is PHP 8.2 or higher.
[/info]

_Usage in WordPress Core_

[warning]
Adding type declarations to existing WordPress Core functions should be done with extreme care.
[/warning]

The function signature of any function (method) which can be overloaded by plugins or themes should not be touched.
This leaves, for now, only unconditionally declared functions in the global namespace, `private` class methods, and code new to Core, as candidates for adding type declarations.

Note: Using the `array` keyword in type declarations is **strongly discouraged** for now, as most often, it would be better to use `iterable` to allow for more flexibility in the implementation and that keyword is not yet available for use in WordPress Core until the minimum requirements are raised to PHP 7.1.

### Magic constants

The [PHP native `__*__` magic constants](https://www.php.net/manual/en/language.constants.magic.php), like `__CLASS__` and `__DIR__`, should be written in uppercase when used.

When using the `::class` constant for class name resolution, the `class` keyword should be in lowercase and there should be no spaces around the `::` operator.

```php
// Correct.
add_action( 'action_name', array( __CLASS__, 'method_name' ) );
add_action( 'action_name', array( My_Class::class, 'method_name' ) );

// Incorrect.
require_once __dIr__ . '/relative-path/file-name.php';
add_action( 'action_name', array( My_Class :: CLASS, 'method_name' ) );
```

### Spread operator `...`

When using the spread operator, there should be one space or a new line with the appropriate indentation before the spread operator. There should be no spaces between the spread operator and the variable/function call it applies to. When combining the spread operator with the reference operator (`&`), there should be no spaces between them.

```php
// Correct.
function foo( &...$spread ) {
    bar( ...$spread );

    bar(
        array( ...$foo ),
        ...array_values( $keyed_array )
    );
}

// Incorrect.
function fool( &   ... $spread ) {
    bar(...
             $spread );

    bar(
        [...  $foo ],... array_values( $keyed_array )
    );
}
```

[info]
The spread operator (or splat operator as it's known in other languages) can be used for packing arguments in function declarations (variadic functions) and unpacking them in function calls as of PHP 5.6. Since PHP 7.4, the spread operator is also used for unpacking numerically-indexed arrays, with string-keyed array unpacking available since PHP 8.1.
When used in a function declaration, the spread operator can only be used with the last parameter.
[/info]

## Declare Statements, Namespace, and Import Statements

### Namespace declarations

Each part of a namespace name should consist of capitalized words separated by underscores.

Namespace declarations should have exactly one blank line before the declaration and at least one blank line after.

```php
namespace Prefix\Admin\Domain_URL\Sub_Domain\Event; // Correct.
```

There should be only one namespace declaration per file, and it should be at the top of the file. Namespace declarations using curly brace syntax are not allowed. Explicit global namespace declaration (namespace declaration without name) are also not allowed.

```php
// Incorrect: namespace declaration using curly brace syntax.
namespace Foo {
    // Code.
}

// Incorrect: namespace declaration for the global namespace.
namespace {
    // Code.
}
```

_There is currently no timeline for introducing namespaces to WordPress Core._

The use of namespaces in plugins and themes is strongly encouraged. It is a great way to prefix a lot of your code to prevent naming conflicts with other plugins, themes and/or WordPress Core.

Please do make sure you use a unique and long enough namespace prefix to actually prevent conflicts. Generally speaking, using a namespace prefix along the lines of `Vendor\Project_Name` is a good idea.

[warning]
The `wp` and `WordPress` namespace prefixes are reserved for WordPress itself.
[/warning]

[info]
Namespacing has no effect on variables, constants declared with `define()` or non-PHP native constructs, like the hook names as used in WordPress.
Those still need to be prefixed individually.
[/info]

### Using import `use` statements

Using import `use` statements allows you to refer to constants, functions, classes, interfaces, namespaces, enums and traits that live outside of the current namespace.

Import `use` statements should be at the top of the file and follow the (optional) `namespace` declaration. They should follow a specific order based on the **type** of the import:

1. `use` statements for namespaces, classes, interfaces, traits and enums
2. `use` statements for functions
3. `use` statements for constants

Aliases can be used to prevent name collisions (two classes in different namespaces using the same class name).
When using aliases, make sure the aliases follow the WordPress naming convention and are unique.

The following examples showcase the correct and incorrect usage of import `use` statements regarding things like spacing, groupings, leading backslashes, etc.

Correct:

```php
namespace Project_Name\Feature;

use Project_Name\Sub_Feature\Class_A;
use Project_Name\Sub_Feature\Class_C as Aliased_Class_C;
use Project_Name\Sub_Feature\{
    Class_D,
    Class_E as Aliased_Class_E,
}

use function Project_Name\Sub_Feature\function_a;
use function Project_Name\Sub_Feature\function_b as aliased_function;

use const Project_Name\Sub_Feature\CONSTANT_A;
use const Project_Name\Sub_Feature\CONSTANT_D as ALIASED_CONSTANT;

// Rest of the code.
```

Incorrect:

```php
namespace Project_Name\Feature;

use   const   Project_Name\Sub_Feature\CONSTANT_A; // Superfluous whitespace after the "use" and the "const" keywords.
use function Project_Name\Sub_Feature\function_a; // Function import after constant import.
use \Project_Name\Sub_Feature\Class_C as aliased_class_c; // Leading backslash shouldn't be used, alias doesn't comply with naming conventions.
use Project_Name\Sub_Feature\{Class_D, Class_E   as   Aliased_Class_E} // Extra spaces around the "as" keyword, incorrect whitespace use inside the brace opener and closer.
use Vendor\Package\{ function function_a, function function_b,
     Class_C,
        const CONSTANT_NAME}; // Combining different types of imports in one use statement, incorrect whitespace use within group use statement.

class Foo {
    // Code.
}

use const \Project_Name\Sub_Feature\CONSTANT_D as Aliased_constant; // Import after class definition, leading backslash, naming conventions violation.
use function Project_Name\Sub_Feature\function_b as Aliased_Function; // Import after class definition, naming conventions violation.

// Rest of the code.
```

[alert]
Import `use` statements have no effect on dynamic class, function or constant names.
Group `use` statements are available from PHP 7.0, and trailing commas in group `use` statements are available from PHP 7.2.
[/alert]

[info]
Note that, unless you have implemented [autoloading](https://www.php.net/manual/en/language.oop5.autoload.php), the `use` statement won't automatically load whatever is being imported. For OO constructs, you'll either need to set up autoloading or load the file(s) containing the OO declaration(s) using `require[_once]` or `include[_once]` statement(s).
Autoloading is only applicable to OO constructs; for functions and constants, you must always use `require[_once]` or `include[_once]`.
[/info]

**Note about WordPress Core usage**

While import `use` statements can already be used in WordPress Core, it is, for the moment, **strongly discouraged**.

Import `use` statements are most useful when combined with namespaces and a class autoloading implementation.
As neither of these are currently in place for WordPress Core and discussions about this are ongoing, holding off on adding import `use` statements to WordPress Core is the sensible choice for now.

## Object-Oriented Programming

### Only One Object Structure (Class/Interface/Trait/Enum) per File

For instance, if we have a file called `class-example-class.php` it can only contain one class in that file.

```php
// Incorrect: file class-example-class.php.
class Example_Class {}

class Example_Class_Extended {}
```

The second class should be in its own file called `class-example-class-extended.php`.

```php
// Correct: file class-example-class.php.
class Example_Class {}
```

```php
// Correct: file class-example-class-extended.php.
class Example_Class_Extended {}
```

### Trait Use Statements

Trait `use` statements should be at the top of a class and should have exactly one blank line before the first `use` statement, and at least one blank line after the last statement. The only exception is when the class only contains trait `use` statements, in which case the blank line after may be omitted.

The following code examples show the formatting requirements for trait `use` statements regarding things like spacing, grouping and indentation.

```php
// Correct.
class Foo {

    use Bar_Trait;
    use Foo_Trait,
        Bazinga_Trait {
        Bar_Trait::method_name insteadof Bar_Trait;
        Bazinga_Trait::method_name as bazinga_method;
    }
    use Loopy_Trait {
        eat as protected;
    }

    public $baz = true;

    ...
}

// Incorrect.
class Foo {
    // No blank line before trait use statement, multiple spaces after the use keyword.
    use       Bar_Trait;

    /*
     * Multiple spaces when importing traits, no new line after opening brace.
     * Aliasing should be done on the same line as the method it's replacing.
     */
    use Foo_Trait,   Bazinga_Trait{Bar_Trait::method_name    insteadof     Foo_Trait; Bazinga_Trait::method_name
        as     bazinga_method;
            }; // Wrongly indented brace.
    public $baz = true; // Missing blank line after trait import.

    ...
}
```

### Visibility should always be declared

For all constructs that allow it (properties, methods, class constants since PHP 7.1), visibility should be explicitly declared.
Using the `var` keyword for property declarations is not allowed.

```php
// Correct.
class Foo {
    public $foo;

    protected function bar() {}
}

// Incorrect.
class Foo {
    var $foo;

    function bar() {}
}
```

_Usage in WordPress Core_

Visibility for class constants can not be used in WordPress Core until the minimum PHP version has been raised to PHP 7.1.

### Visibility and modifier order

When using multiple modifiers for a _class declaration_, the order should be as follows:

1. First the optional `abstract` or `final` modifier.
2. Next, the optional `readonly` modifier.

When using multiple modifiers for a _constant declaration_ inside object-oriented structures, the order should be as follows:

1. First the optional `final` modifier.
2. Next, the visibility modifier.

When using multiple modifiers for a _property declaration_, the order should be as follows:

1. First a visibility modifier.
2. Next, the optional `static` or `readonly` modifier (these keywords are mutually exclusive).
3. Finally, the optional `type` declaration.

When using multiple modifiers for a _method declaration_, the order should be as follows:

1. First the optional `abstract` or `final` modifier.
2. Then, a visibility modifier.
3. Finally, the optional `static` modifier.

```php
// Correct.
abstract readonly class Foo {
    private const LABEL = 'Book';

    public static $foo;

    private readonly string $bar;

    abstract protected static function bar();
}

// Incorrect.
class Foo {
    protected final const SLUG = 'book';

    static public $foo;

    static protected final function bar() {
        // Code.
    };
}
```

[info]
- Visibility for OO constants can be declared since PHP 7.1.
- Typed properties are available since PHP 7.4.
- Readonly properties are available since PHP 8.1.
- `final` modifier for constants in object-oriented structures is available since PHP 8.1.
- Readonly classes are available since PHP 8.2.
[/info]

### Object Instantiation

When instantiating a new object instance, parenthesis must always be used, even when not strictly necessary.
There should be no space between the name of the class being instantiated and the opening parenthesis.

```php
// Correct.
$foo = new Foo();
$anonymous_class = new class( $parameter ) { ... };
$instance = new static();

// Incorrect.
$foo = new Foo;
$anonymous_class = new class ( $input ) { ... };
```

## Control Structures

### Use `elseif`, not `else if`

`else if` is not compatible with the colon syntax for `if|elseif` blocks. For this reason, use `elseif` for conditionals.

### Yoda Conditions

```php
if ( true === $the_force ) {
    $victorious = you_will( $be );
}
```

When doing logical comparisons involving variables, always put the variable on the right side and put constants, literals, or function calls on the left side. If neither side is a variable, the order is not important. (In [computer science terms](https://en.wikipedia.org/wiki/Value_(computer_science)#Assignment:_l-values_and_r-values), in comparisons always try to put l-values on the right and r-values on the left.)

In the above example, if you omit an equals sign (admit it, it happens even to the most seasoned of us), you'll get a parse error, because you can't assign to a constant like `true`. If the statement were the other way around `( $the_force = true )`, the assignment would be perfectly valid, returning `1`, causing the if statement to evaluate to `true`, and you could be chasing that bug for a while.

A little bizarre, it is, to read. Get used to it, you will.

This applies to `==`, `!=`, `===`, and `!==`. Yoda conditions for `<`, `>`, `<=` or `>=` are significantly more difficult to read and are best avoided.

## Operators

### Ternary Operator

[Ternary operators](https://www.php.net/manual/en/language.operators.comparison.php#language.operators.comparison.ternary) are fine, but always have them test if the statement is true, not false. Otherwise, it just gets confusing. (An exception would be using `! empty()`, as testing for false here is generally more intuitive.)

The short ternary operator must not be used.

For example:

```php
// (if statement is true) ? (do this) : (else, do this);
$musictype = ( 'jazz' === $music ) ? 'cool' : 'blah';
// (if field is not empty ) ? (do this) : (else, do this);
```

### Error Control Operator `@`

As noted in the [PHP docs](https://www.php.net/manual/en/language.operators.errorcontrol.php):

> PHP supports one error control operator: the at sign (@). When prepended to an expression in PHP, any diagnostic error that might be generated by that expression will be suppressed.

While this operator does exist in Core, it is often used lazily instead of doing proper error checking. Its use is highly discouraged, as even the PHP docs also state:

> Warning: Prior to PHP 8.0.0, it was possible for the @ operator to disable critical errors that will terminate script execution. For example, prepending @ to a call of a function that did not exist, by being unavailable or mistyped, would cause the script to terminate with no indication as to why.

### Increment/decrement operators

Pre-increment/decrement should be favored over post-increment/decrement for stand-alone statements.

Pre-increment/decrement will increment/decrement and then return, while post-increment/decrement will return and then increment/decrement.
Using the "pre" version is slightly more performant and can prevent future bugs when code gets moved around.

```php
// Correct: Pre-decrement.
--$a;

// Incorrect: Post-decrement.
$a--;
```

## Database

### Database Queries

Avoid touching the database directly. If there is a defined function that can get the data you need, use it. Database abstraction (using functions instead of queries) helps keep your code forward-compatible and, in cases where results are cached in memory, it can be many times faster.

If you must touch the database, consider creating a [Trac](https://core.trac.wordpress.org/) ticket. There you can discuss the possibility of adding a new function to cover the functionality you wanted, for a future version of WordPress.

### Formatting SQL statements

When formatting SQL statements you may break them into several lines and indent if it is sufficiently complex to warrant it. Most statements work well as one line though. Always capitalize the SQL parts of the statement like `UPDATE` or `WHERE`.

Functions that update the database should expect their parameters to lack SQL slash escaping when passed. Escaping should be done as close to the time of the query as possible, preferably by using `$wpdb->prepare()`

`$wpdb->prepare()` is a method that handles escaping, quoting, and int-casting for SQL queries. It uses a subset of the `sprintf()` style of formatting. Example :

```php
$var = "dangerous'"; // Raw data that may or may not need to be escaped.
$id = some_foo_number(); // Data we expect to be an integer, but we're not certain.

$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

The following placeholders can be used in the query string:

- %d (integer)
- %f (float)
- %s (string)
- %i (identifier, e.g. table/field names)

Note that these placeholders should not be 'quoted'! `$wpdb->prepare()` will take care of escaping and quoting for us. This makes it easy to see at a glance whether something has been escaped or not because it happens right when the query happens.

See [Data Validation](https://developer.wordpress.org/plugins/security/data-validation/) in the Plugin Handbook for further details.

## Recommendations

### Self-Explanatory Flag Values for Function Arguments

Prefer string values to just `true` and `false` when calling functions.

```php
// Incorrect.
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // What does true mean?
eat( 'dogfood', false ); // What does false mean? The opposite of true?
```

PHP only supports named arguments as of PHP 8.0. However, as WordPress currently still supports older PHP versions, we cannot yet use those.
Without named arguments, the values of the flags are meaningless, and each time we come across a function call like the examples above, we have to search for the function definition. The code can be made more readable by using descriptive string values, instead of booleans.

```php
// Correct.
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

When more words are needed to describe the function parameters, an `$args` array may be a better pattern.

```php
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

Be careful when using this pattern, as it can lead to "Undefined array index" notices if input isn't validated before use. Use this pattern only where it makes sense (i.e. multiple possible arguments), not just for the sake of it.

### Clever Code

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

Unless absolutely necessary, loose comparisons should not be used, as their behavior can be misleading.

Correct:

```php
if ( 0 === strpos( $text, 'WordPress' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Incorrect:

```php
if ( 0 == strpos( 'WordPress', 'foo' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Assignments must not be placed in conditionals.

Correct:

```php
$data = $wpdb->get_var( '...' );
if ( $data ) {
    // Use $data.
}
```

Incorrect:

```php
if ( $data = $wpdb->get_var( '...' ) ) {
    // Use $data.
}
```

In a `switch` statement, it's okay to have multiple empty cases fall through to a common block. If a case contains a block, then falls through to the next block, however, this must be explicitly commented.

```php
switch ( $foo ) {
    case 'bar':                // Correct, an empty case can fall through without comment.
    case 'baz':
        echo esc_html( $foo ); // Incorrect, a case with a block must break, return, or have a comment.
    case 'cat':
        echo 'mouse';
        break;                 // Correct, a case with a break does not require a comment.
    case 'dog':
        echo 'horse';
        // no break            // Correct, a case can have a comment to explicitly mention the fall through.
    case 'fish':
        echo 'bird';
        break;
}
```

The `goto` statement must never be used.

The `eval()` construct is _very dangerous_ and is impossible to secure. Additionally, the `create_function()` function, which internally performs an `eval()`, is deprecated since PHP 7.2 and has been removed in PHP 8.0. Neither of these must be used.

### Closures (Anonymous Functions)

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

Closures should not be passed as filter or action callbacks, as removing these via `remove_action()` / `remove_filter()` is complex (at this time)  (see [#46635](https://core.trac.wordpress.org/ticket/46635 "Improve identifying of non–trivial callbacks in hooks") for a proposal to address this).

### Regular Expressions

Perl compatible regular expressions ([PCRE](https://www.php.net/pcre), `preg_` functions) should be used.
Never use the `/e` switch, use `preg_replace_callback` instead.

It's most convenient to use single-quoted strings for regular expressions since, contrary to double-quoted strings, they have only two metasequences which need escaping: `\'` and `\\`.

### Don't `extract()`

Per [#22400](https://core.trac.wordpress.org/ticket/22400 "Remove all, or at least most, uses of extract() within WordPress"):

> `extract()` is a terrible function that makes code harder to debug and harder to understand. We should discourage it's [sic] use and remove all of our uses of it.

Joseph Scott has [a good write-up of why it's bad](https://blog.josephscott.org/2009/02/05/i-dont-like-phps-extract-function/).

### Shell commands

Use of the [backtick operator](https://www.php.net/manual/en/language.operators.execution.php) is not allowed.

Use of the backtick operator is identical to [`shell_exec()`](https://www.php.net/manual/en/function.shell-exec.php), and most hosts disable this function in the `php.ini` file for security reasons.
