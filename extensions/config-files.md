# Extension Config Files

Like many of Laravel and Nova's classes, sometimes there's a need for a config file to store values that shouldn't be changed often (or if changes could have significant impact to the system). In those instances, Nova allows extensions to load a config file with those values. The extension config file can store any values and to any depth of additional arrays that you may need.

By default, all you need to do is to create a `config.php` file at the root of your extension and make sure it returns an array of the values.

```php
&lt;?php

return [
	'key1' => 'value1',
	'key2' => 'value2',
	'key3' => [
		'subkey1' => 'subvalue1',
		'subkey2' => 'subvalue2',
	],
];
```

_If you're using the `nova:make:extension` command, you can add the optional `--include-config` flag to create an empty config file in your extension._

## Retrieving Extension Config Values

Since the config values are stored in the global config array, you can access them the same way you'd access other config items. The only difference with extension configs is that they're stored with a specific signature:

```
extension.{vendor}.{name}.{value}
```

When the vendor and name are stored, they're stored completely lowercased, so getting an item from an extension would look like this:

```php
Config::get('extension.anodyne.myawesomeextension.key1');

// or

config('extension.anodyne.myawesomeextension.key3.subkey2');
```

## Changing How Nova Loads Config Files

In the event you need to load the config file differently (or want to move it or have multiple files), you can override the `loadConfig` method in your `Extension` class. Make sure to see how the base Extension class handles loading first and then make modifications from there.
