# The Make Theme Command

Nova includes an Artisan command for generating the basic skeleton for a theme. This is a great way for theme developers to get something up and running quickly without having to worry about remembering how things are supposed to be set up.

__Note: You'll need to make sure you understand and have the Artisan console command tool set up properly in order to use this command.__

## The Command

The command accepts one mandatory argument: the name of the theme.

When it comes time to create our first theme, we can generate the skeleton simply by opening up the command line (usually on a local machine for development purposes) and running `php artisan nova:make:theme sunny`. That would generate a `sunny` directory in our `themes` folder. Within there, you'd find the necessary directory structure for a theme.

## The Options

In addition to the mandatory argument, the command also comes with several options that can turn different pieces of the creation process on and off if you know ahead of time what you do and don't need for your theme.

- `--override-styles`: Generates a `style.css` stylesheet that when seen by Nova will skip pulling in Nova's base styles.
- `--include-components`: Generates the structure for overriding components.
- `--include-options`: Generates the `options.json` file used for creating theme options.
- `--include-theme-class`: Generate the `Theme` class for overriding how your Theme is built and rendered.

Any combination of these options can be called when running the initial command, like so:

<code>php artisan nova:make:theme sunny --override-styles
php artisan nova:make:theme sunny --include-theme --include-options
php artisan nova:make:theme sunny --include-components --include-theme</code>
