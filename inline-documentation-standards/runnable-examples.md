# Runnable code examples in DocBlocks

A DocBlock example can be marked as runnable. The Code Reference then shows it with a **Run** button, and readers execute and edit it in the browser through [WordPress Playground](https://developer.wordpress.org/playground/handbook/guides/php-code-snippets/).

This is opt-in. Existing indented examples keep working. Convert an example only when running it teaches something reading it does not.

## Example
````php
/**
 * Generator for a foreach loop to step through each class name for the matched tag.
 *
 * ```php interactive
 * $p = new WP_HTML_Tag_Processor( "<div class='free &lt;egg&gt;\tlang-en'>" );
 * $p->next_tag();
 * foreach ( $p->class_list() as $class_name ) {
 *     echo "{$class_name} ";
 * }
 * // Outputs: free <egg> lang-en
 * ```
 */
````

Three parts: a fenced block, the info string `php interactive`, and a trailing `// Outputs:` comment.

## The fence

- `php interactive`, lowercase, in that order. Anything else (`PHP interactive`, `php-interactive`, `php title="x" interactive`) renders as plain code.
- May be indented, including inside a list item. Code lines need not match the fence indentation.
- Rendered in place, between the surrounding paragraphs.

## The code

Start with the first meaningful line. Do not include `<?php` (the docs site adds it; a snippet starting with it will not run) or `require '/wordpress/wp-load.php';` (WordPress is already loaded).

Print something with `echo`, `print_r()`, or `var_dump()`. Keep it short enough to read comfortably in the source; if it needs a lot of setup, it is a tutorial, not a reference example.

## The output

End with a `// Outputs` comment. The parser strips it from the code that runs, shows it as the expected result before the reader selects **Run**, and keeps it for later output checks.

One line — everything after the colon is literal:
```php
echo esc_html( '<egg>' );
// Outputs: <egg>
```

Several lines — empty `// Outputs:` starts a block, each `//` line is one output line, a final empty `//` keeps a trailing newline:
```php
print_r( array( 'fruit' => 'apple' ) );
// Outputs:
// Array
// (
//     [fruit] => apple
// )
//
```

JSON-encoded — for trailing spaces, tabs, or repeated newlines. Must be one JSON string:
```php
echo "done ";
// Outputs (JSON-encoded): "done "
```

Only a trailing standalone line comment counts. `// Outputs:` inside a string, a block comment, after code on the same line, or followed by more code is treated as code. The older ` ```expected-output ` fence still parses; do not use both.

## Hidden setup

If the example needs a helper function, a post, or an option, put that in a setup Blueprint ([Playground Blueprint](https://developer.wordpress.org/playground/blueprints/) JSON). It runs first and never appears on the page.

Inline, directly before or after the PHP fence:
````arduino
```setup-blueprint
{ "steps": [ { "step": "writeFile", "path": "/wordpress/wp-content/mu-plugins/docs-fixture.php", "data": "<?php\nfunction docs_fixture_greeting() { return 'Hello'; }\n" } ] }
```
```php interactive
echo docs_fixture_greeting();
// Outputs: Hello
```
````

Named and shared — define once, reference with `setup-blueprint=<name>`. A named Blueprint in the file-level DocBlock is available to every example in the file:
````go
```setup-blueprint shared-greeting
{ "steps": [ ... ] }
```
```php interactive setup-blueprint=shared-greeting
echo docs_shared_greeting( 'first' );
// Outputs: Hello, first
```
````

Names are case-sensitive. `blueprint=` and `setupblueprint=` are not recognized.

## Before you commit

- Fence reads exactly `php interactive`.
- No `<?php`, no `require`.
- Prints something, and the `// Outputs` comment matches.
- Still readable as plain text in the source. If not, keep the indented version.

To test, paste the code into a `<php-snippet>` on any HTML page per the [Playground guide](https://developer.wordpress.org/playground/handbook/guides/php-code-snippets/).

## Timing and feedback

The Code Reference is built from the current stable release, so an example in `trunk` appears when that release ships.

Questions: `#live-doc-snippets` on Make WordPress Slack. Parser bugs: [phpdoc-parser](https://github.com/WordPress/phpdoc-parser/issues). Rendering bugs: [wporg-developer](https://github.com/WordPress/wporg-developer/issues).
