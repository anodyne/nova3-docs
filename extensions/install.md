# Installing Your Extension

Extension developers can use Laravel's migration system to modify existing database tables or even create their own database tables for their extensions. This freedom gives developers far more flexibility when modifying Nova. When an admin clicks the Install button from the Extension Manager, the migrations will be run and everything set up. Developers who use the `nova:extension:make` command can use the option to have the migration structure built for them. For more information about the command, review the [Make Extension Command](make-extension-command.md) page.

## What Happens During Installation?

When your extension's migrations are run, Nova will grab the migration files from your extension's `database/migrations` directory and run through them, calling the `up()` method on each of them. After all the migrations have run, Nova will add your extension to the list of active extensions automatically.

## Modifying How Your Extension Is Installed

What if you wanted to change how your extension is installed though? You could have specific requirements, like checking to make sure a certain setting is available or making sure another extension is installed or even checking the version of Nova that's running. You can do those kinds of things by overriding the `install()` method in your `Extension` class.

Let's say we want to make sure Nova 3.1 is installed before we run the installation:

```php
public function install()
{
	if (version_compare(config('nova.app.version.full'), '3.1.0', '>='))
	{
		parent::install();

		return true;
	}

	throw new ExtensionInstallException("This extension requires Nova 3.1!");
}
```

## Laravel's Schema Builder
