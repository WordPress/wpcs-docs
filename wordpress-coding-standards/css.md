# CSS Coding Standards

Like any coding standard, the purpose of the WordPress CSS Coding Standards is to create a baseline for collaboration and review within various aspects of the WordPress open source project and community, from core code to themes to plugins. Files within a project should appear as though created by a single entity. Above all else, create code that is readable, meaningful, consistent, and beautiful.

Within core stylesheets, inconsistencies will often be found. We are working on addressing these and make every effort to have patches and commits from this point forward follow the CSS coding standards. More information on the above and contributing to UI/front-end development will be forthcoming in a separate set of guidelines.
<h2>Structure</h2>
There are plenty of different methods for structuring a stylesheet. With the CSS in core, it is important to retain a high degree of legibility. This enables subsequent contributors to have a clear understanding of the flow of the document.
<ul>
 	<li>Use tabs, not spaces, to indent each property.</li>
 	<li>Add two blank lines between sections and one blank line between blocks in a section.</li>
 	<li>Each selector should be on its own line, ending in either a comma or an opening curly brace. Property-value pairs should be on their own line, with one tab of indentation and an ending semicolon. The closing brace should be flush left, using the same level of indentation as the opening selector.</li>
</ul>
Correct:

[css]
#selector-1,
#selector-2,
#selector-3 {
	background: #fff;
	color: #000;
}
[/css]

Incorrect:

[css]
#selector-1, #selector-2, #selector-3 {
	background: #fff;
	color: #000;
	}

#selector-1 { background: #fff; color: #000; }
[/css]

<h2>Selectors</h2>
With specificity, comes great responsibility. Broad selectors allow us to be efficient, yet can have adverse consequences if not tested. Location-specific selectors can save us time, but will quickly lead to a cluttered stylesheet. Exercise your best judgement to create selectors that find the right balance between contributing to the overall style and layout of the DOM.
<ul>
 	<li>Similar to the <a href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/#naming-conventions">WordPress PHP Coding Standards</a> for file names, use lowercase and separate words with hyphens when naming selectors. Avoid camelcase and underscores.</li>
 	<li>Use human readable selectors that describe what element(s) they style.</li>
 	<li>Attribute selectors should use double quotes around values</li>
 	<li>Refrain from using over-qualified selectors, <code>div.container</code> can simply be stated as <code>.container</code></li>
</ul>
Correct:

[css]
#comment-form {
	margin: 1em 0;
}

input[type="text"] {
	line-height: 1.1;
}
[/css]

Incorrect:

[css]
#commentForm { /* Avoid camelcase. */
	margin: 0;
}

#comment_form { /* Avoid underscores. */
	margin: 0;
}

div#comment_form { /* Avoid over-qualification. */
	margin: 0;
}

#c1-xr { /* What is a c1-xr?! Use a better name. */
	margin: 0;
}

input[type=text] { /* Should be [type="text"] */
	line-height: 110% /* Also doubly incorrect */
}
[/css]

<h2>Properties</h2>
Similar to selectors, properties that are too specific will hinder the flexibility of the design. Less is more. Make sure you are not repeating styling or introducing fixed dimensions (when a fluid solution is more acceptable).
<ul>
 	<li>Properties should be followed by a colon and a space.</li>
 	<li>All properties and values should be lowercase, except for font names and vendor-specific properties.</li>
 	<li>Use hex code for colors, or rgba() if opacity is needed. Avoid RGB format and uppercase, and shorten values when possible: <code>#fff</code> instead of <code>#FFFFFF</code>.</li>
 	<li>Use shorthand (except when overriding styles) for background, border, font, list-style, margin, and padding values as much as possible. (For a shorthand reference, see <a href="https://codex.wordpress.org/CSS_Shorthand">CSS Shorthand</a>.)</li>
</ul>
Correct:

[css]
#selector-1 {
	background: #fff;
	display: block;
	margin: 0;
	margin-left: 20px;
}
[/css]

Incorrect:

[css]
#selector-1 {
	background:#FFFFFF;
	display: BLOCK;
	margin-left: 20PX;
	margin: 0;
}
[/css]

<h3>Property Ordering</h3>
<blockquote>"Group like properties together, especially if you have a lot of them."
-- Nacin</blockquote>
Above all else, choose something that is meaningful to you and semantic in some way. Random ordering is chaos, not poetry. In WordPress Core, our choice is logical or grouped ordering, wherein properties are grouped by meaning and ordered specifically within those groups. The properties within groups are also strategically ordered to create transitions between sections, such as background directly before color. The baseline for ordering is:
<ul>
 	<li>Display</li>
 	<li>Positioning</li>
 	<li>Box model</li>
 	<li>Colors and Typography</li>
 	<li>Other</li>
</ul>
Things that are not yet used in core itself, such as CSS3 animations, may not have a prescribed place above but likely would fit into one of the above in a logical manner. Just as CSS is evolving, so our standards will evolve with it.

Top/Right/Bottom/Left (TRBL/trouble) should be the order for any relevant properties (e.g. margin), much as the order goes in values. Corner specifiers (e.g. border-radius-*-*) should be top-left, top-right, bottom-right, bottom-left. This is derived from how shorthand values would be ordered.

Example:

[css]
#overlay {
	position: absolute;
	z-index: 1;
	padding: 10px;
	background: #fff;
	color: #777;
}
[/css]

Another method that is often used, including by the Automattic/WordPress.com Themes Team, is to order properties alphabetically, with or without certain exceptions.

Example:

[css]
#overlay {
	background: #fff;
	color: #777;
	padding: 10px;
	position: absolute;
	z-index: 1;
}
[/css]

<h3>Vendor Prefixes</h3>
Updated on 2/13/2014, after <a href="https://core.trac.wordpress.org/changeset/27174">[27174]</a>:

We use <a href="https://github.com/postcss/autoprefixer">Autoprefixer</a> as a pre-commit tool to easily manage necessary browser prefixes, thus making the majority of this section moot. For those interested in following that output without using Grunt, vendor prefixes should go longest (-webkit-) to shortest (unprefixed). All other spacing remains as per the rest of standards.

[css]
.sample-output {
	-webkit-box-shadow: inset 0 0 1px 1px #eee;
	-moz-box-shadow: inset 0 0 1px 1px #eee;
	box-shadow: inset 0 0 1px 1px #eee;
}
[/css]

<h2>Values</h2>
There are numerous ways to input values for properties. Follow the guidelines below to help us retain a high degree of consistency.
<ul>
 	<li>Space before the value, after the colon.</li>
 	<li>Do not pad parentheses with spaces.</li>
 	<li>Always end in a semicolon.</li>
 	<li>Use double quotes rather than single quotes, and only when needed, such as when a font name has a space or for the values of the <code>content</code> property.</li>
 	<li>Font weights should be defined using numeric values (e.g. <code>400</code> instead of <code>normal</code>, <code>700</code> rather than <code>bold</code>).</li>
 	<li>0 values should not have units unless necessary, such as with transition-duration.</li>
 	<li>Line height should also be unit-less, unless necessary to be defined as a specific pixel value. This is more than just a style convention, but is worth mentioning here. More information: <a href="http://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/">http://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/</a></li>
 	<li>Use a leading zero for decimal values, including in rgba().</li>
 	<li>Multiple comma-separated values for one property should be separated by either a space or a newline. For better readability newlines should be used for lengthier multi-part values such as those for shorthand properties like box-shadow and text-shadow, including before the first value. Values should then be indented one level in from the property.</li>
 	<li>Lists of values within a value, like within rgba(), should be separated by a space.</li>
</ul>
Correct:

[css]
.class { /* Correct usage of quotes */
	background-image: url(images/bg.png);
	font-family: "Helvetica Neue", sans-serif;
	font-weight: 700;
}

.class { /* Correct usage of zero values */
	font-family: Georgia, serif;
	line-height: 1.4;
	text-shadow:
		0 -1px 0 rgba(0, 0, 0, 0.5),
		0 1px 0 #fff;
}

.class { /* Correct usage of short and lengthier multi-part values */
	font-family: Consolas, Monaco, monospace;
	transition-property: opacity, background, color;
	box-shadow:
		0 0 0 1px #5b9dd9,
		0 0 2px 1px rgba(30, 140, 190, 0.8);
}
[/css]

Incorrect:

[css]
.class { /* Avoid missing space and semicolon */
	background:#fff
}

.class { /* Avoid adding a unit on a zero value */
	margin: 0px 0px 20px 0px;
}

.class {
	font-family: Times New Roman, serif; /* Quote font names when required */
	font-weight: bold; /* Avoid named font weights */
	line-height: 1.4em;
}

.class { /* Incorrect usage of multi-part values */
	text-shadow: 0 1px 0 rgba(0, 0, 0, 0.5),
                 0 1px 0 #fff;
	box-shadow: 0 1px 0 rgba(0, 0,
		0, 0.5),
		0 1px 0 rgba(0,0,0,0.5);
}
[/css]

<h2>Media Queries</h2>
Media queries allow us to gracefully degrade the DOM for different screen sizes. If you are adding any, be sure to test above and below the break-point you are targeting.
<ul>
 	<li>It is generally advisable to keep media queries grouped by media at the bottom of the stylesheet.
<ul>
 	<li>An exception is made for the wp-admin.css file in core, as it is very large and each section essentially represents a stylesheet of its own. Media queries are therefore added at the bottom of sections as applicable.</li>
</ul>
</li>
 	<li>Rule sets for media queries should be indented one level in.</li>
</ul>
Example:

[css]
@media all and (max-width: 699px) and (min-width: 520px) {

		/* Your selectors */
}
[/css]

<h2>Commenting</h2>
<ul>
 	<li>Comment, and comment liberally. If there are concerns about file size, utilize minified files and the SCRIPT_DEBUG constant. Long comments should manually break the line length at 80 characters.</li>
 	<li>A table of contents should be utilized for longer stylesheets, especially those that are highly sectioned. Using an index number (1.0, 1.1, 2.0, etc.) aids in searching and jumping to a location.</li>
 	<li>Comments should be formatted much as PHPDoc is. The <a href="http://web.archive.org/web/20070601200419/http://cssdoc.net/">CSSDoc</a> standard is not necessarily widely accepted or used but some aspects of it may be adopted over time. Section/subsection headers should have newlines before and after. Inline comments should not have empty newlines separating the comment from the item to which it relates.</li>
</ul>
For sections and subsections:

[css]
/**
* #.# Section title
*
* Description of section, whether or not it has media queries, etc.
*/

.selector {
	float: left;
}
[/css]

For inline:

[css]
/* This is a comment about this selector */
.another-selector {
	position: absolute;
	top: 0 !important; /* I should explain why this is so !important */
}
[/css]

<h2>Best Practices</h2>
Stylesheets tend to get long in length. Focus slowly gets lost whilst intended goals start repeating and overlapping. Writing smart code from the outset helps us retain the overview whilst remaining flexible throughout change.
<ul>
 	<li>If you are attempting to fix an issue, attempt to remove code before adding more.</li>
 	<li>Magic Numbers are unlucky. These are numbers that are used as quick fixes on a one-off basis. Example: <code>.box { margin-top: 37px }</code>.</li>
 	<li>DOM will change over time, target the element you want to use as opposed to "finding it" through its parents. Example: Use <code>.highlight</code> on the element as opposed to <code>.highlight a</code> (where the selector is on the parent)</li>
 	<li>Know when to use the height property. It should be used when you are including outside elements (such as images). Otherwise use line-height for more flexibility.</li>
 	<li>Do not restate default property &amp; value combinations (for instance <code>display: block;</code> on block-level elements).</li>
</ul>
<h2>Related Links</h2>
<ul>
 	<li>Principles of writing consistent, idiomatic CSS: <a href="https://github.com/necolas/idiomatic-css">https://github.com/necolas/idiomatic-css</a></li>
</ul>
