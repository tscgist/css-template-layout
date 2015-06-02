# JavaScript (jQuery) implementation of the CSS Template Layout Module #

The project aims at providing web designers with a way to use the W3's [CSS Template Layout Module](http://www.w3.org/TR/2009/WD-css3-layout-20090402/) today. As a jQuery plug-in, the script parses a given set of CSS rules and displays the content as indicated in the specification.

Options include the ability to select the CSS parsed, as well as an optional prefix to use for the CSS rules. Specifying a prefix allows style rules that are interoperable with a possible future browser implementations.

## Script Usage ##

Any usage requires [jQuery](http://jquery.com) and this script to be referenced in the document's head:
```
<script src="jQuery.js" type="text/javascript"></script>
<script src="tpl_layout.js" type="text/javascript"></script>
```
Then, `$.setTemplateLayout` needs to be run once the DOM is ready. Thus by running it under:
```
$(document).ready(function() { /* run $.setTemplateLayout here */ });
```

Or simply:
```
$(function() { /* run $.setTemplateLayout here */ });
```

There are several ways provide the source CSS data and additional options for your template:

### No parameters ###
```
$.setTemplateLayout();
```
  * Use the CSS from any STYLE elements on the page, no prefix.

### File and prefix parameters ###
```
$.setTemplateLayout(filename, [prefix]);
```
  * filename is the CSS file name as a string.
  * prefix is the optional prefix to use for the template properties

### Object parameter ###
```
$.setTemplateLayout(options);
```
  * options is an object with the following possible keys:
    * file (string) Same as filename mentioned above
    * prefix (string) Same as prefix mentioned above
    * text (string), CSS data with template information
    * delay (integer) Milliseconds for the script to wait until it applies the template information
    * fallback (string) Name of class to give to fallback link/style elements. This way you can set a stylesheet for non-JS users, without the styles being used for those using the template.

### Examples ###
```
$(function() {
	$.setTemplateLayout('style.css', 'js');
});
```
Will use the template data found in `style.css` where the display and property rules start with `-js-`
```
$(function() {
	$.setTemplateLayout({
		text:	$('textarea').val(),
		delay:	1000
	});
});
```
Will use the CSS data given in a textarea, but only after a one second delay. No prefix is given, so it will just use regular `display` and `property` values.

### Additional features ###
  * `$.templateLayoutShowOnReady()` - Run this **before** the DOM is loaded to prevent the brief appearance of the un-templated page.
  * `$.redoTemplateLayout()` - Use if you need to run the template process again, already done by default on a window's resize.
  * To change `display` and `position` template properties on elements, you can use jQuery's `css( name, value )` function.

## CSS Usage ##
The script is designed to be fully compatible with the [W3 CSS Template Layout Module](http://www.w3.org/TR/2009/WD-css3-layout-20090402/), so the specification is probably the best source for examples on how to use it.

Since the spec hasn't been finalized yet, it is recommended that you specify and use a prefix for the "display" and "position" template properties. The prefix can be any specified string, though you should avoid those used by browsers like moz, ms, o, webkit, and khtml.

For example, when using `$.setTemplateLayout('style.css', 'foo')` you will want to want your `style.css` file to include rules like:
```
body {
	margin: 0;
	padding: 0;
	-foo-display: "abc";
}

div#navigation { -foo-position: a; }
div#content { -foo-position: b; }
div#more { -foo-position: c; }
```

## Supported browsers ##
The script has been tested and confirmed to work in the following browsers:
  * Internet Explorer 6+ (IE6 may have some minor bugs)
  * Firefox 2+
  * Opera 9+
  * Safari 3.1+
  * Chrome 1+

## Demos ##
For a collection of demonstrations see:
http://a.deveria.com/csstpl/


## Notes ##
  * In order to get the correct style information from the `style` tag, the script uses a GET request of the current page in Internet Explorer. Thus, it will likely fail if the page is the result of a POST request (a submitted form).
  * Any non-template styles submitted under the "text" parameter will also be applied to the page.

## Known Bugs ##
  * Template widths are not used if total of row widths is smaller
  * CSS parser won't read data nested under `@media`, `@import`, etc.
  * min/max-width for elements is not supported (though equivalents do work in slots)
  * template content should flow in first slot if not @ is found
## Changelog ##

V1.01

  * Replaced .css() notation for width and height with shorter notations
  * Improved the way multiple elements per slot appear

V1.1
**Note** This version includes some significant changes to the way elements appear in template slots. It is more in line with the specification's text rather than its examples.

  * Fixed margin/border/padding behavior
  * Changed slot behavior to match spec text rather than examples
  * Limited ::slot() values to allowed values
  * Support for ::slot() vertical-align "middle" and "bottom" added
  * Support for ::slot() overflow hidden/visible added
  * Proper support for ::slot() background properties
  * Support for template background in combination with slot backgrounds

V1.1.1

  * Added support for comma-separated selectors
  * Fixed background positioning for body templates.
  * Added support for setting template display/position properties using jQuery().css()

V1.1.2

  * Added/fixed support for width/height set in CSS for elements in a slot
  * Allow fallback class to be set, to hide fallback CSS rules
  * Improved display property parsing, corrected handling of incorrect width values
  * Added support for spaces between minmax() values
  * Added $.templateLayoutShowOnReady to hide the non-template rendering.
  * Added support for sub-templates with same letters

V1.1.3

  * Fixed IE bugs

V1.1.4

  * Made body appear visible on native support
  * Fixed vertical positioning for multiple elements with margins