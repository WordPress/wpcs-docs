# CSS Coding Standards

Like any coding standard, the purpose of the WordPress CSS Coding Standards is to create a baseline for collaboration and review within various aspects of the WordPress open source project and community, from core code to themes to plugins. Files within a project should appear as though created by a single entity. Above all else, create code that is readable, meaningful, consistent, and beautiful.

Within core stylesheets, inconsistencies will often be found. We are working on addressing these and make every effort to have patches and commits from this point forward follow the CSS coding standards. More information on the above and contributing to UI/front-end development will be forthcoming in a separate set of guidelines.

## Structure

There are plenty of different methods for structuring a stylesheet. With the CSS in core, it is important to retain a high degree of legibility. This enables subsequent contributors to have a clear understanding of the flow of the document.

- Use tabs, not spaces, to indent each property.
- Add two blank lines between sections and one blank line between blocks in a section.
- Each selector should be on its own line, ending in either a comma or an opening curly brace. Property-value pairs should be on their own line, with one tab of indentation and an ending semicolon. The closing brace should be flush left, using the same level of indentation as the opening selector.

Correct:

```css
#selector-1,
#selector-2,
#selector-3 {
	background: #fff;
	color: #000;
}
```

Incorrect:

```css
#selector-1, #selector-2, #selector-3 {
	background: #fff;
	color: #000;
	}

#selector-1 { background: #fff; color: #000; }
```

## Selectors

With specificity, comes great responsibility. Broad selectors allow us to be efficient, yet can have adverse consequences if not tested. Location-specific selectors can save us time, but will quickly lead to a cluttered stylesheet. Exercise your best judgement to create selectors that find the right balance between contributing to the overall style and layout of the DOM.

- Similar to the [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/#naming-conventions) for file names, use lowercase and separate words with hyphens when naming selectors. Avoid camelcase and underscores.
- Use human readable selectors that describe what element(s) they style.
- Attribute selectors should use double quotes around values
- Refrain from using over-qualified selectors, `div.container` can simply be stated as `.container`

Correct:

```css
#comment-form {
	margin: 1em 0;
}

input[type="text"] {
	line-height: 1.1;
}
```

Incorrect:

```css
#commentForm { /&042; Avoid camelcase. &042;/
	margin: 0;
}

#comment_form { /&042; Avoid underscores. &042;/
	margin: 0;
}

div#comment_form { /&042; Avoid over-qualification. &042;/
	margin: 0;
}

#c1-xr { /&042; What is a c1-xr?! Use a better name. &042;/
	margin: 0;
}

input[type=text] { /&042; Should be [type="text"] &042;/
	line-height: 110% /&042; Also doubly incorrect &042;/
}
```

## Properties

Similar to selectors, properties that are too specific will hinder the flexibility of the design. Less is more. Make sure you are not repeating styling or introducing fixed dimensions (when a fluid solution is more acceptable).

- Properties should be followed by a colon and a space.
- All properties and values should be lowercase, except for font names and vendor-specific properties.
- Use hex code for colors, or rgba() if opacity is needed. Avoid RGB format and uppercase, and shorten values when possible: `#fff` instead of `#FFFFFF`.
- Use shorthand (except when overriding styles) for background, border, font, list-style, margin, and padding values as much as possible. (For a shorthand reference, see [CSS Shorthand](https://codex.wordpress.org/CSS_Shorthand).)

Correct:

```css
#selector-1 {
	background: #fff;
	display: block;
	margin: 0;
	margin-left: 20px;
}
```

Incorrect:

```css
#selector-1 {
	background:#FFFFFF;
	display: BLOCK;
	margin-left: 20PX;
	margin: 0;
}
```

### Property Ordering

> "Group like properties together, especially if you have a lot of them." 
> -- Nacin

Above all else, choose something that is meaningful to you and semantic in some way. Random ordering is chaos, not poetry. In WordPress Core, our choice is logical or grouped ordering, wherein properties are grouped by meaning and ordered specifically within those groups. The properties within groups are also strategically ordered to create transitions between sections, such as background directly before color. The baseline for ordering is:

- Display
- Positioning
- Box model
- Colors and Typography
- Other

Things that are not yet used in core itself, such as CSS3 animations, may not have a prescribed place above but likely would fit into one of the above in a logical manner. Just as CSS is evolving, so our standards will evolve with it.

Top/Right/Bottom/Left (TRBL/trouble) should be the order for any relevant properties (e.g. margin), much as the order goes in values. Corner specifiers (e.g. border-radius-*-*) should be top-left, top-right, bottom-right, bottom-left. This is derived from how shorthand values would be ordered.

Example:

```css
#overlay {
	position: absolute;
	z-index: 1;
	padding: 10px;
	background: #fff;
	color: #777;
}
```

Another method that is often used, including by the Automattic/WordPress.com Themes Team, is to order properties alphabetically, with or without certain exceptions.

Example:

```css
#overlay {
	background: #fff;
	color: #777;
	padding: 10px;
	position: absolute;
	z-index: 1;
}
```

### Vendor Prefixes

Updated on 2014-02-13, after [[27174](https://core.trac.wordpress.org/changeset/27174)]:

We use [Autoprefixer](https://github.com/postcss/autoprefixer) as a pre-commit tool to easily manage necessary browser prefixes, thus making the majority of this section moot. For those interested in following that output without using Grunt, vendor prefixes should go longest (-webkit-) to shortest (unprefixed). All other spacing remains as per the rest of standards.

```css
.sample-output {
	-webkit-box-shadow: inset 0 0 1px 1px #eee;
	-moz-box-shadow: inset 0 0 1px 1px #eee;
	box-shadow: inset 0 0 1px 1px #eee;
}
```

## Values

There are numerous ways to input values for properties. Follow the guidelines below to help us retain a high degree of consistency.

- Space before the value, after the colon.
- Do not pad parentheses with spaces.
- Always end in a semicolon.
- Use double quotes rather than single quotes, and only when needed, such as when a font name has a space or for the values of the `content` property.
- Font weights should be defined using numeric values (e.g. `400` instead of `normal`, `700` rather than `bold`).
- 0 values should not have units unless necessary, such as with transition-duration.
- Line height should also be unit-less, unless necessary to be defined as a specific pixel value. This is more than just a style convention, but is worth mentioning here. More information: [https://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/](https://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/).
- Use a leading zero for decimal values, including in rgba().
- Multiple comma-separated values for one property should be separated by either a space or a newline. For better readability newlines should be used for lengthier multi-part values such as those for shorthand properties like box-shadow and text-shadow, including before the first value. Values should then be indented one level in from the property.
- Lists of values within a value, like within rgba(), should be separated by a space.

Correct:

```css
.class { /&042; Correct usage of quotes &042;/
	background-image: url(images/bg.png);
	font-family: "Helvetica Neue", sans-serif;
	font-weight: 700;
}

.class { /&042; Correct usage of zero values &042;/
	font-family: Georgia, serif;
	line-height: 1.4;
	text-shadow:
		0 -1px 0 rgba(0, 0, 0, 0.5),
		0 1px 0 #fff;
}

.class { /&042; Correct usage of short and lengthier multi-part values &042;/
	font-family: Consolas, Monaco, monospace;
	transition-property: opacity, background, color;
	box-shadow:
		0 0 0 1px #5b9dd9,
		0 0 2px 1px rgba(30, 140, 190, 0.8);
}
```

Incorrect:

```css
.class { /&042; Avoid missing space and semicolon &042;/
	background:#fff
}

.class { /&042; Avoid adding a unit on a zero value &042;/
	margin: 0px 0px 20px 0px;
}

.class {
	font-family: Times New Roman, serif; /&042; Quote font names when required &042;/
	font-weight: bold; /&042; Avoid named font weights &042;/
	line-height: 1.4em; /&042; Avoid adding a unit for line height &042;/
}

.class { /&042; Incorrect usage of multi-part values &042;/
	text-shadow: 0 1px 0 rgba(0, 0, 0, 0.5),
                 0 1px 0 #fff;
	box-shadow: 0 1px 0 rgba(0, 0,
		0, 0.5),
		0 1px 0 rgba(0,0,0,0.5);
}
```

## Media Queries

Media queries allow us to gracefully degrade the DOM for different screen sizes. If you are adding any, be sure to test above and below the break-point you are targeting.

- It is generally advisable to keep media queries grouped by media at the bottom of the stylesheet.
    - An exception is made for the wp-admin.css file in core, as it is very large and each section essentially represents a stylesheet of its own. Media queries are therefore added at the bottom of sections as applicable.
- Rule sets for media queries should be indented one level in.

Example:

```css
@media all and (max-width: 699px) and (min-width: 520px) {
        /&042; Your selectors &042;/
}
```

## Commenting

- Comment, and comment liberally. If there are concerns about file size, utilize minified files and the SCRIPT_DEBUG constant. Long comments should manually break the line length at 80 characters.
- A table of contents should be utilized for longer stylesheets, especially those that are highly sectioned. Using an index number (1.0, 1.1, 2.0, etc.) aids in searching and jumping to a location.
- Comments should be formatted much as PHPDoc is. The [CSSDoc](https://web.archive.org/web/20070601200419/http://cssdoc.net/) standard is not necessarily widely accepted or used but some aspects of it may be adopted over time. Section/subsection headers should have newlines before and after. Inline comments should not have empty newlines separating the comment from the item to which it relates.

For sections and subsections:

```css
/**
 * #.# Section title
 *
 * Description of section, whether or not it has media queries, etc.
 */

.selector {
	float: left;
}
```

For inline:

```css
/* This is a comment about this selector */
.another-selector {
	position: absolute;
	top: 0 !important; /* I should explain why this is so !important */
}
```

## Best Practices

Stylesheets tend to grow in length and complexity, and as they grow the chance of redundancy increases. By following some best practices we can help our CSS maintain focus and flexibility as it evolves:

- If you are attempting to fix an issue, attempt to remove code before adding more.
- Magic Numbers are unlucky. These are numbers that are used as quick fixes on a one-off basis. Example: `.box { margin-top: 37px }`.
- DOM will change over time, target the element you want to use as opposed to "finding it" through its parents. Example: Use `.highlight` on the element as opposed to `.highlight a` (where the selector is on the parent)
- Know when to use the height property. It should be used when you are including outside elements (such as images). Otherwise use line-height for more flexibility.
- Do not restate default property &amp; value combinations (for instance `display: block;` on block-level elements).


### WP Admin CSS

Check out the [WP Admin CSS Audit](https://wordpress.github.io/css-audit/public/wp-admin), a report generated to document the health of the WP Admin CSS code. Read more in [the repository's README](https://github.com/WordPress/css-audit/blob/trunk/README.md).

## Related Links

- Principles of writing consistent, idiomatic CSS: [https://github.com/necolas/idiomatic-css](https://github.com/necolas/idiomatic-css).
