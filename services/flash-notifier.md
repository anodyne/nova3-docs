# Flash Notifications

## Flash Helper

The flash notifier comes with a helper you can use anywhere flash notifications need to be set, whether it's inside a controller, in another class, a model, or even in a view if the situation calls for it. There are several ways to use the `flash()` helper.

### With Arguments

Using the flash helper is as easy as passing some arguments to the function.

`flash('info', "Title", "Content");`

Doing so will create an info flash notification that will appear on the next page load. You can use any of the alert levels found in the class itself: info, success, error, and warning.

### Getting an Instance

Sometimes though, instead of doing all that legwork, you may want to simply grab an instance of the flash notifier out of the IOC container. In order to do that, you can call the flash helper without any arguments.

`flash();`

Doing this is the same as either `app('nova.flash')` or `App::make('nova.flash')`. Once you have an instance of the flash notifier class, you can call any of its methods like you would if you can instantiated the class yourself.

`flash()->success("Character Created!");

flash()->error("Post Could Not Be Sent!", "There was a problem sending your post.");`

_The examples above are how Nova handles all of its flash notifications in the Nova core._