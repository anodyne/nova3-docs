# The Theme Class

To provide the most flexibility in how themes are put together, Nova allows for every theme to have a `Theme` class. If the class exists in your theme, Nova will load it and its information automatically. It isn't required, but it gives you a lot of control over different aspects of building up your theme.

## Interfaces

Like other areas of Nova, the theme class utilizes several interfaces to ensure all themes have the same minimum functionality. The base theme class implements two interfaces: `Themeable` and `ThemeableInfo`. In most cases, developers will simply extend from the base theme class, so understanding these interfaces isn't crucial. However, it is also possible to start from scratch with your theme class. In that case, you __must__ implement both interfaces. If you don't, Nova will throw an exception since it doesn't see your theme class implementing the correct interfaces.

### `Themeable`

The `Themeable` interface defines methods for building up your theme when it comes time to render it for the browser. With the `Themeable` interface, Nova defines methods for handling building up the structure, template, menus, footer, pages, Javascript, and others. If there's something you want to be handled different than the default with your theme, this is where you can define those actions for the specific sections you need to modify.

### `ThemeableInfo`

The `ThemeableInfo` interface defines methods for getting the information about an theme, such as the author, credits, full name, location, vendor name, and version information. These methods are defined in the base theme class, but you can override any of them that you choose to.
