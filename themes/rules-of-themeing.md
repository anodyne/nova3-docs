# The Rules of Themeing

## Replacing Base Stylesheets

Nova stores its base styles right in the Nova core. This allows Anodyne to update base styles without forcing theme developers to make changes to their themes every time. However, sometimes you may want to do something more extreme and skip the base styles altogether. Nova's theme system is set up in such a way that you can replace any or all of the base stylesheets with your own by understanding the rules below:

- Nova loads a `base.style.css` stylesheet every time __unless__ you put a `style.css` stylesheet in your `design/css` directory, then Nova will skip loading the base stylesheet and use your stylesheet instead.
- Nova will load a `base.icons.css` stylesheet every time __unless__ you put a `icons.css` stylesheet in your `design/css` directory, then Nova will skip loading the base icons stylesheet and use your icon stylesheet instead.
- Nova will load a `base.responsive.css` stylesheet every time __unless__ you put a `responsive.css` stylesheet in your `design/css` directory, then Nova will skip loading the base responsive stylesheet and use your responsive stylesheet instead.

## Supplementing Base Stylesheets

In the event that you want to keep the base styles and supplement them with your own styling (the most likely option for most theme developers), here are some rules to understand:

- Nova will load a `custom.css` stylesheet from your `design/css` directory (if it exists) _after_ the base stylesheet.
- Nova will load a `custom.icons.css` stylesheet from your `design/css` directory (if it exists) _after_ the base icons stylesheet.
- Nova will load a `custom.responsive.css` stylesheet from your `design/css` directory (if it exists) _after_ the base responsive stylesheet.
