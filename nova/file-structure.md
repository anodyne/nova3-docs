# File Structure

In previous versions of Nova, the goal was to keep the file structure at the root as compact and simple as possible. What came out of that was a 2 folder structure: `application` and `nova`. It was immediately clear that `application` was for things for your own application and that `nova` was for the Nova core. Unfortunately, that simplicity led to a lot more complication and questions. With Nova 3, the goal is to provide simplicity, but also context at the same time. To that end, the root file structure is more expanded than it's been before.

- assets
- config
- extensions
- nova
- ranks
- storage
- themes

There's more going on here, but it's immediately more clear what each folder does than it was previously. Let's dig in and look at what's in each of the folders.

## `assets`

## `config`

The `config` directory, as the name implies, contains Nova's configuration files. For the most part, settings that you'll change regularly will be found in Nova itself, but highly sensitive settings or settings that could create issues if they're modified, are kept in these PHP files.

Most Laravel applications will store all of their config files here, but given Nova's unique requirements, the config files are stored in the Nova core and you can override those settings by creating a file with the appropriate array keys in this folder.

## `extensions`

The `extensions` directory stores any extensions (referred to as MODs in previous versions of Nova) that are intended to extend Nova's core functionality. In Nova 3, extensions stand on their own (similar to a module) and can contain all kinds of code and are the recommended way to significantly modify Nova. More information about extensions can be found [here](../extensions/overview.md).

## `nova`

The Nova core is contained within the `nova` directory. When it comes time to update to a newer version of Nova, you'll simply delete this directory and upload the newer version that you downloaded.

#### `bootstrap`

Not to be confused with the Bootstrap framework that Nova 3 uses, the `bootstrap` folder contains files that are used to "bootstrap" the framework. ("Bootstrapping" is the process by which the program is actually started.) Including during bootstrapping is the autoload configuration (to make sure everything is available when called) as well as a `cache` folder that contains a few framework generated files for bootstrap performance optimization.

#### `config`

Much like the root `config` directory, the `nova/config` directory stores all of the framework config files. Whereas the root `config` folder only contains stuff that needs to be changed from the default, the `nova/config` folder contains _all_ of the config files. If you want to change a config value from its default, you'll need to come into the file in this folder and copy the array key(s) you want to modify and then copy them into a similarly named file in the root `config` directory. More information about working with config files can be found [here](../laravel/config-files.md).

#### `resources`

The `resources` directory contains all of the Javascript, CSS, hosted fonts, views, and other presentation files used by Nova. If you need to override a view file, this is where you'll come to find the base view file.

#### `src`

The `src` directory contains all of the Nova source code.

#### `tests`

The `tests` directory contains all of the automated tests that run to ensure Nova is working the way it should. If you've downloaded Nova from the Anodyne website, it's likely you won't see this directory as these tests are removed during the process that prepares the zip files for release.

#### `vendor`

The `vendor` directory contains all of the third-party dependencies that Laravel and Nova rely on.

## `ranks`

The `ranks` directory is where rank images are kept to be used throughout Nova.

## `storage`

This really is the only "gotcha" out of the entire root file structure. This directory is actually something that's used primarily by Laravel (though Nova utilizes it for several things as well) for storing different types of files and logs. The `storage` folder is broken up into `app`, `framework`, and `logs` directories.

- The `app` directory is used to store files utilized by the Nova core.
- The `framework` directory is used to store framework generated files and caches (compiled Blade templates, file-based sessions, file caches, and other files).
- The `logs` directory stores any error logs that Nova and/or Laravel generate.

As a general rule of thumb, you should never have to edit files in the `storage` directory (unless explicitly told to by the Anodyne support staff). You may need to reference log files if you're running into errors, but those will be just for reference.

## `themes`

As you may have guessed, the `themes` directory is where all of Nova's theme (referred to as skins in previous versions of Nova) are stored. If you have a theme you want to install or are looking to build your own, this is where all that work will be done. More information about themes can be found [here](../themes/overview.md).
