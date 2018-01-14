# Accessibility Coding Standards

These are standards that WordPress features should meet for accessibility in order to be merged into core. All new or updated code released in WordPress must conform with the WCAG 2.0 guidelines at level AA. These basic guidelines are intended for easy reference during development, but do not cover all possible accessibility issues.
<h2>HTML Semantics</h2>
Take a pragmatic approach to HTML semantics. Don't add semantics purely for the sake of semantics; but if there is an HTML structure that clearly matches the content, use that element. For example, if you have a group of links, it should most likely use a list element.
<h3>Heading structure</h3>
The H1 is the main heading representing the page title on every core page. For subsections, use a reasonable HTML heading structure — including the use of heading elements for page subsections. Heading markup should not be used for presentational purposes.
<ul>
 	<li>Use H2 through H6 to give internal structure to the page.</li>
 	<li>Don’t skip heading levels.</li>
 	<li>Don’t add extra functionality inside a heading, like links or buttons.</li>
</ul>
<h3>Semantics for Controls</h3>
Controls with a native keyboard interaction (buttons or links) are always preferred. If there is a valid target link for the control, either an in-page reference or a link, then the control should use an <code>&lt;a href=""&gt;</code>. If there isn't, it should use a <code>&lt;button&gt;</code>.

If you're updating an existing control:
<table><caption>Button or link decision logic</caption>
<thead>
<tr>
<th scope="col">Scenario</th>
<th scope="col">Choice</th>
</tr>
</thead>
<tbody>
<tr>
<td>Anchors with null or meaningless HREF values: href='#', no href, href='#something' where #something does not exist</td>
<td><code>button</code></td>
</tr>
<tr>
<td>Anchors with meaningful on-page HREF values href='#something' where #something does exist</td>
<td><code>button</code> or <code>a href='#target'</code></td>
</tr>
<tr>
<td>Anchors with meaningful off-page HREF values that are renderable (but actual behavior is AJAX)</td>
<td>Link when JS not available, button the rest of the time.</td>
</tr>
<tr>
<td>Anchors with meaningful off-page HREF values that are *not* renderable</td>
<td>Should be a button, but perhaps the target should be made renderable</td>
</tr>
<tr>
<td>Buttons that direct to new locations on the same page</td>
<td>Could be either a button or a link.</td>
</tr>
<tr>
<td>Buttons that direct to new locations on different pages.</td>
<td>Should be a link.</td>
</tr>
</tbody>
</table>
<h3>Dynamic Content</h3>
When there are dynamic changes within a page without a page reload you must provide audible feedback with ARIA for important changes, like a successful save event, for example.

Use <code>wp.a11y.speak()</code> for all simple AJAX responses. If you are doing a complex interaction, <code>wp.a11y.speak()</code> may not be the best choice. In that case, discuss your usage with the Accessibility team to determine whether extending <code>wp.a11y.speak()</code> or coding your own ARIA live regions is the best choice.
<ul>
 	<li><a href="https://make.wordpress.org/accessibility/2015/04/15/let-wordpress-speak-new-in-wordpress-4-2/">Let WordPress Speak: introduction to <code>wp.a11y.speak()</code>. </a></li>
 	<li><a href="https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions">Mozilla developer documentation on ARIA Live Regions</a></li>
</ul>
<h2>Color Contrast</h2>
In most cases, feature plug-ins are not expected to add or modify colors in core. However, if a feature plug-in needs to add new color combinations, those combinations must meet minimum contrast requirements. Minimum contrast requirements are 4.5:1 for font sizes rendering smaller than 24px or smaller and 3.0:1 for font sizes larger than 24px or 19px and bold.
<ul>
 	<li><a href="https://make.wordpress.org/accessibility/handbook/quick-start-guide/#color-contrast">WordPress Accessibility Quick Start: Color Contrast</a></li>
</ul>
<h2>Keyboard Accessibility</h2>
Users must be able to reach and successfully interact with all elements on the page that are actionable, including all form inputs, buttons and links by using the keyboard. They must be able to see a visual indicator of keyboard focus. You should be aware that keyboard events may operate differently when a screen reader is running.

If you can complete an action with a mouse, you must also be able to complete that action using the keyboard.
<h2>Images and Icons</h2>
Any image resource must include an accessible name. In some cases, the accessible name should be an empty string. An image can be represented by an actual <code>&lt;img&gt;</code> element, an icon font, or an svg element; but any graphical representation is considered an image for these purposes. Different types of elements use different types of accessible names.

For <code>&lt;img&gt;</code> elements, the accessible name should be in the alt attribute. If the img is ornamental, the alt attribute should still be included, but left empty.

For icon fonts, the font icon itself should have the aria-hidden attribute, with screen-reader-text in a neighbor element. If the icon is ornamental, the font icon should still have the <code>aria-hidden</code> attribute, but the screen reader text should be omitted.

[html]<a href="this.html">
<span class="dashicons dashicon-thumbs-up" aria-hidden="true"></span>
<span class="screen-reader-text">Something</span>
</a>[/html]

For SVG, the SVG should be inline, so that accessible information isn't hidden from assistive technology. SVG elements should contain a <code>&lt;title&gt;</code> element with the accessible name of the image. For cross-technology support, the title element should be associated with the svg element via aria-labelledby. For maximum compatibility, all SVG elements used to represent an image should carry the role attribute with a value of 'img'. 

If the SVG element is ornamental, then the title element should be omitted and no aria-labelledby attribute should be present. The SVG element should also carry the <code>aria-hidden</code> attribute. 

<ul>
 	<li><a href="http://www.sitepoint.com/tips-accessible-svg/">More information on SVG Accessibility</a></li>
</ul>
<h2>Labeling</h2>
Existing code uses a mixture of explicitly and implicitly labeled fields, but all new code must use an explicitly associated <code>&lt;label&gt;</code> element (using for/id attributes and <em>not</em> wrapping the form control). Labels are not required to be visible, but must use the .screen-reader-text class when hidden. Placeholders are fine, but are not a substitute for labels. For all labels, clicking on the field label should cause the associated field to receive focus or, for checkboxes and radio selectors, select that choice.

Don't introduce new title attributes to convey information. Use aria-label when you need to provide an alternate label and <code>.screen-reader-text</code> if you're appending additional data.

When creating forms, use <code>&lt;fieldset&gt;</code> and <code>&lt;legend&gt;</code> to group logically related form elements inside complex forms or to group radio buttons and checkboxes under a heading.
