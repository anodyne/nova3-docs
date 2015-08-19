# The Make Command

Nova includes an Artisan command for generating the basic skeleton for an extension. This is a great way for developers to get something up and running quickly without having to worry about remembering how things are supposed to be set up.

## The Command

The command accepts two mandatory arguments. The first argument is the vendor name, or in other words, a way to identify all of your extensions. For Anodyne's first-party extensions, the vendor name is _Anodyne_ (case-sensitive). For your extensions, that could be your name or an alias or a  handle you use. The second argument is the extension name which should be the name of the extension without any spaces.

So let's say we're jumping in to create our first extension and want to generate the skeleton. All we would need to do is open up the command line (usually on a local machine for development purposes) and run `php artisan nova:make:extension MyAwesomeName MyAwesomeExtension`. That would generate a `MyAwesomeName` directory in the `extensions` directory. Within there, you'd find a `MyAwesomeExtension` directory with the necessary structure for your extension, including a service provider, extension class, QuickInstall file, and more.

_It's important to take note of the casing. You can use lowercase vendor and extension names if you want, but the namespaces are generated from those items and will mean all references to your extension will need to have the namespaces lowercased._

## The Options

In addition to the mandatory arguments, the command also comes with several options that can turn off different pieces of the creation process if you know ahead of time what you do and don't need for your extension.

- `--no-controllers`: Generates the skeleton without the HTTP structure and initial controller file.
- `--no-views`: Generates the skeleton without the views directory and initial view file.
- `--no-provider`: Generates the skeleton without a service provider.
- `--include-config`: Generates an empty config file.
- `--include-routes`: Generates an empty routes file for your extension.

Any combination of these options can be called when running the initial command, like so:

`php artisan nova:make:extension MyAwesomeName MyAwesomeExtension --no-controllers --no-views`
`php artisan nova:make:extension MyAwesomeName MyAwesomeExtension --no-provider`
`php artisan nova:make:extension MyAwesomeName MyAwesomeExtension --include-config --include-routes`
