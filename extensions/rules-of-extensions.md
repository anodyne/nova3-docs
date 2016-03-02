# Rules of Extensions

- If you need to install database tables or make modifications to existing tables, your migrations must be stored in a `database/migrations` folder.
	- The `install()` method must return a boolean.
	- The `uninstall()` method must return a boolean.
- If you install something, you __must__ provide the ability to uninstall everything you install. This includes modifying existing tables. An uninstallation should remove any fields you add or modify any fields you modified during your install.
- Your extension __must__ have an `Extension` class that implements the `Extensible` and `ExtensibleInfo` interfaces.
