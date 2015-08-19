# Extension Config Files

Like many of Laravel and Nova's classes, sometimes there's a need for a config file to store values that shouldn't be changed often (of if changes could have significant impact to the system). In those instances, Nova allows extensions to load a `config.php` file with those values.

You can store any values and any depth of additional arrays in your config file for whatever you may need.

_If you're using the `nova:make:extension` command, you can add the optional `--include-config` flag to create an empty config file in your extension._

## Retrieving Extension Config Values

Since the config values are stored in the global config array, you can access them the same way you'd access other config items. The only difference with extension configs is that they're stored with a specific signature:

`config('extension.{vendor}.{name}.{value}')`

When the vendor and name are stored, they're stored completely lowercased, so getting an item from an extension would look like this:

`config('extension.anodyne.myawesomeextension.foo')`

## Changing How Nova Loads Config Files

In the event you need to load the config file differently (or want to move it or have multiple files), you can override the `loadConfig` method in your extension's Extension class. Make sure to see how the base Extension class handles loading first and then make modifications from there.