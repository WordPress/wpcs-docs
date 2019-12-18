# Coding Standards Markdown Style Guide

# Heading h1 (One `#`)
## Heading h2 (Two `#`'s)
### Heading h3 (Three `#`'s)
#### Heading h4 (Four `#`'s)
##### Heading h5 (Five `#`'s)
###### Heading h6 (Six `#`'s)

### Emphasis

Use single `*` for *Italic* or single underscore for _Italic_ text.

Use double `*` for *Bold* or double underscore for __Bold__ text.

### Links

Wrap the title in square brackets `[title]`, e.g. [WordPress](https://wordpress.org),

Links can also be autoconverted e.g. https://wordpress.org

### Blockquotes

> Use `>` to indent for blockquote
>> Further indent using 2 `>>` Nested Blockquote
>>> Or even 3 `>>>` for Deeper Nested Blockquote

### Lists

Lists can use `*`, and intent two spaces for subitems `  *`:
* List
  * List
* List

Lists can use `-`, and intent two spaces for subitems `  -`:

- List
  - List
- List

Numbered Lists
1. One
2. Two
3. Three

1) One
2) Two
3) Three

Offset numbered lists
42. 42
1. 43
1. 44 

### Horizontal Rules

---

***

### Code

`Inline code` with backticks

```
# code block using 3 backticks
```

### Syntax highlighting

``` js
var foo = function (bar) {
  return bar++;
};

console.log(foo(5));
```

``` css
foo {
  padding: 5px;
  margin-right: 3px;
}

.bar {
  background-color: #f00;
}
```

``` php
<?php
$array = array(
    "foo" => "bar",
    "bar" => "foo",
);

?>
```
