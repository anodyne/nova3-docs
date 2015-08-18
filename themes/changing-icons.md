# Changing Icons

By default, Nova 3 uses the Material Design icon font. In some cases though, you may want another look for your theme. You can swap out the Material Design icon font for another icon font of your choice by following the directions below. __Note:__ you cannot swap out an icon font for actual icons without making significant changes to the page views in your theme.

1. Override the `include-fonts` partial in your theme and make sure it points toward the CDN hosting your icon font. In the event you aren't using a CDN and will host the icon font on your server, this file should be blank. __Important:__ even if you're storing the font on your own server, you'll still need to override the `include-fonts` partial, otherwise Nova will make an HTTP request to the Google Fonts CDN for the Material Design icon font.
2. Override the `icon` partial in your theme and set the markup to the appropriate markup that the icon font you're using expects. __Important:__ do not override the `icon-setup` partial. Doing so may cause icons in the Setup Center to no longer display.
3. In most cases, you'll likely need different icon styles than what Nova comes with. Make sure you have an `icons.css` stylesheet in your theme (if you in fact need to change those styles) and make the necessary changes. Nova will see the `icons.css` stylesheet and use that instead of the base icons stylesheet.
4. Finally, in order to get the icons working correctly, you'll need to override the `getIconMap` method in your theme's `Theme` class. If you don't have a `Theme` class in your theme, you'll need to create one. Copy the array from the base `Theme` class (in the `nova/src/Setup/Services/Themes/Theme.php` file) and paste it into your own `getIconMap` method. Change the _values_ (what's on the right side of the array) to be the appropriate values for the key from your icon font (i.e. for the `edit` key, you'll want to choose whatever icon from your font works best for that type of action). __Important:__ do _not_ change the array keys (what's on the left side of the array) otherwise none of your icons will work.