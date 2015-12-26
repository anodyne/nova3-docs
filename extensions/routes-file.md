# Extension Routes File

While all of Nova's core pages are run through the Page Manager (and we recommend that all extensions do the same), sometimes you don't need to do that. In those instances, you can use a routes file for your extension where you can define the URIs and their resources for anything in your extension. By default, you can create a `routes.php` file at the root of your extension and Nova will pick it up and parse it from there. If you need to change the name of the file or want to split it among multiple files, you'll need to override the `loadFileRoutes` method in your `Extension` class.

## Why You Might Want to Use File-based Routes

## Why You Shouldn't Use File-based Routes

The disadvantage to doing it this way though is that admins who install your extension won't be able to change the routes and may end up manually changing your extension to suit their needs.

__Important: do not use closure-based routes as they can't be cached!__
