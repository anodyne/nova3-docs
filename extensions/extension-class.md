# Extension Class

Every extension requires the existence of an `Extension` class that extends from a base class in the Nova core. This class handles some of the basic things extensions need to do, like installation, uninstallation, and initialization.

## Interfaces

Like other areas of Nova, the extension class utilizes several interfaces to ensure all extensions have the same minimum functionality. The base extension class implements two interfaces: `Extensible` and `ExtensibleInfo`. In most cases, developers will simply extend from the base extension class, so understanding these interfaces isn't crucial. However, it is also possible to start from scratch with your extension class. In that case, you __must__ implement both interfaces. If you don't, Nova will throw an exception since it doesn't see your extension class implementing the correct interfaces.

### `Extensible`

The `Extensible` interface defines methods for installing and uninstalling your extension. By default, the base extension will call the migrations in your extension's `database/migrations` folder and nothing else. If you need something else done during installation, you'll need to override the `install` method and make the necessary adjustments. The same goes for the `uninstall` method as well.

### `ExtensibleInfo`

The `ExtensibleInfo` interface defines methods for getting the information about an extension, such as the author, credits, full name, location, vendor name, and version information. These methods are defined in the base extension class, but you can override any of them that you choose to.

## Initializing

If you need to do some work during the start up of the extension, you can override the `initialize` method and make your changes from there.

By default, the `initialize` method loads the config file (if it exists) and the routes file (if it exists). In the event you don't need either of those things changed but still want to do some initialization work, you'll need to make sure the parent is called first.

`protected function initialize()
{
	parent::initialize();

	// Your code here
}`

If you want to change the way config files or routes are loaded, you don't have to call the parent method and can instead put your code directly in the method.

`protected function initialize()
{
	// Config loading

	// Route loading
}`

In the event that you just need to change one of the loading operations, you can override either the `loadConfig` method or the `loadFileRoutes` method from the base class file.
