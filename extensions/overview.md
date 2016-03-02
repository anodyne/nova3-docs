# Extensions

Nova 3 comes with an extension system that allows developers to modify existing functionality and even create new functionality for Nova without the need to modify core files that could be changed during an update. That encapsulated environment provides developers with the ultimate flexibility as well as the tools to create just about anything the could want to.

## What Are Extensions?

## How Do Extensions Work?

## Limitations of Extensions

There are some inherent limitations to extensions in Nova 3.

- Extensions cannot pick and choose which parts of something they override/replace. For example, you can't write an extension that just replaces the `textarea` in the story writing pages. You would have to replace the entire story writing page.

## Building Extensions

Extensions live in the `extensions` directory at the root of the Nova installation. From there, extensions are __required__ to be inside a vendor folder that can be anything of your choosing. (In most cases you'd want a nickname or organization name or handle to be used for the vendor name so that all of your extensions are grouped together under a single vendor.) Within your vendor directory, you'd have a directory for each extension.

Nova 3 comes with an Artisan command that will create the directory structure for your extension to get you started. Using the Artisan command isn't required, but it's definitely a time saver!

With your directory structure in place, you can organize your extension however you see fit. There's lots of information in the Extensions section here about the rules of writing extensions, service providers, database interaction, and much more.
