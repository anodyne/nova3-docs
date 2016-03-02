# Working with Laravel Config Files

In Laravel, config files are simply PHP arrays that are returned from the config file. This allows the config file to be loaded and read into memory so the values can be accessed from anywhere in the application. It's important to distinguish between config values and settings in Nova. Config values are primarily for the Laravel framework (though Nova does store a few important items in config files so they can't be changed without knowing what you're doing) whereas settings are for the Nova application.

Here is a simple example of what a config file looks like:

```php
<?php

return [

	'app' => [
		'name'	=> "Nova 3",

		'version' => [
			'full'	=> '3.0.0',
			'major'	=> 3,
			'minor'	=> 0,
			'patch'	=> 0,
		],
	],

];
```

## Accessing Config Values

So now that you know how items are stored in config files, how do you actually get those values?

The easiest way is to use the global `config` helper function. Configuration values can be accessed using "dot" syntax, which includes the name of the file and option you want to access.

```php
config('nova.app.name'); // Will return "Nova 3"
```

In this instance, we're grabbing the `nova.php` config file and pulling back the `name` key that's inside of the `app` array.

__Note:__ Nova does not provide for printing out config values with the page compiler for security reasons. If you want to print out config values on a page, you'll need to create an advanced page and write the view file yourself.

## Setting Config Values

You can also set config values at runtime by passing an array to the `config` helper:

```php
config(['app.timezone', 'America/Los_Angeles']);
```

__Note:__ Setting a config value is a one-time thing. You will need to set the value every time the application runs if you want to persist the change.
