# Markdown Style Guide

## Headings

```md
# Heading h1
## Heading h2
### Heading h3
#### Heading h4
##### Heading h5
###### Heading h6
```
Note: h1 - h4 items will be automatically added to the Table of Contents.

## Emphasis

### Italics

Wrap text with a single `_` for _Italic_ text:

```md
This is _italic text_.
```

### Bold
Wrap text with double `**` for **Bold** text:

```md
This is **bold text**.
```

### Strikethrough
Wrap text with double `~~` for ~~strikethrough~~ text:

```md
This is ~~strikethrough~~ text.
```

## Links

Wrap the title in square brackets `[title]` immediately followed by the URL in `(https://example.com)`:

```md
[WordPress](https://wordpress.org/)
```

## Blockquotes

Use `>` for blockquotes, double `>>` to further indent:

```md
> Blockquote
>> Indented Blockquote
```

## Lists

### Unordered Lists

Use `-` for unordereed lists, and intent two spaces for list subitems:

```md
- List
  - List
- List
- List
```

### Ordered Lists

Use numbered items followed by a `.:

```md
1. One
2. Two
3. Three
```

## Horizontal Rules

Use `---` for a horizontal rules:
```md
---
```

## Tables

```md
| A     | B     |
| ----- | ----- |
| Alpha | Bravo |
```

## Example Code

### Inline Code

Wrap inline code with `\`` backticks:
```md
This is `inline code` wrapped with backticks
```

When documenting an example, use the markdown ``\`\`` code block to demarcate the beginning and end of the code sample:

**Note:** Full support for _fenced code blocks_ is not yet implemented for the handbooks, to workaround this see the shortcodes section below:

### Fenced Code Blocks

#### Javascript
```js
var foo = function (bar) {
  return bar++;
};

console.log(foo(5));
```

#### CSS & SCSS
```css
foo {
  padding: 5px;
  margin-right: 3px;
}

.bar {
  background-color: #f00;
}
```

#### PHP
```php
$array = array(
    "foo" =&gt; "bar",
    "bar" =&gt; "foo",
);
```

```md
This is _italic text_. This is **bold text**.
```

### Shortcodes

#### Javascript
Wrap multiline Javascript code in <code>[</code><code>javascript</code><code>]</code> and <code>[/javascript]</code> each on their own line.

#### CSS & SCSS
Wrap multiline CSS & SCSS code in <code>[</code><code>css</code><code>]</code> and <code>[/css]</code> each on their own line.

#### PHP
Wrap multiline PHP code in <code>[</code><code>php</code><code>]</code> and <code>[/php]</code> each on their own line.

#### Markdown
Wrap multiline Markdown code in <code>[</code><code>md</code><code>]</code> and <code>[/md]</code> each on their own line.

**Note:** The <code>[md]</code> shortcode is also not currently supported.
