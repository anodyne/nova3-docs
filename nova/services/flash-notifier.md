# Flash Notifications

Flash notifications allow messages to be passed from one request into the next for confirmation of an action or notification of an error. Nova utilizes a home-grown flash notification class that interfaces with the SweetAlert Javascript library to give Nova's notifications a bit of life and vibrance.

## The Flash Helper

The Flash Notifier comes with a helper you can use anywhere flash notifications need to be set, whether it's inside a controller, in another class, or even in a view if the situation calls for it. Getting started with the Flash helper is as easy as passing some arguments to the function:

```php
flash('info', "Some Info", "The more you know...");
```

Using the above code will create an info flash notification that will appear on the next page load.

You can also access the underlying instance of the class and chain any of the class methods to give you cleaner, more readable code:

```php
flash()->success('Congrats!');

flash()->error('Uh Oh!');

flash()->warning('Better Watch Out!');
```

_The above is how flash notifications are set in all of Nova's core controllers._

## Class Methods

### create($title, $message, $level, $key)

In reality, all of the other methods in the class simply call the `create()` method and just pass the appropriate parameters in. This gives the class a unified way of building these messages.

- title (default: null) - The title of the flash message
- message (default: null) - Any explanation you may feel the title needs
- level (default: info) - The level of the message (info, success, error, warning)
- key (default: flash_message) - The key used to store the information in the session for flashing to the next page load

### error($title, $message)

Error messages should be used to show when something unexpected has happened or when something the user was trying to do failed.

```php
flash()->error("Post Creation Failed!", "We couldn't create your mission post because of an error.");
```

### info($title, $message)

Info messages are catch-all messages that should be used when the other available levels don't make sense.

```php
flash()->info("Nova Updated!", "Nova was recently updated to version 3.1. See the new features...");
```

### success($title, $message)

Success messages should be used to show a user that an action they're trying to take succeeded.

```php
flash()->success("New Story Created!");
```

### warning($title, $message)

Warning messages don't necessarily indicate a problem, but is something the user should be made aware of.

```php
flash()->warning("Nova Update Available!", "Nova 3.1.5 is available and addresses issues with...");
```

### overlay($title, $message, $level)

## Getting an Instance of the Class

The class itself is stored within the IOC container with a key of `nova.flash`. Because an instance of the class is in the container, it means you don't have to instantiate your own instance of the class (unless you have a really good reason to). There are several ways to get the instance of the class to store in a variable:

- Use the App facade and "make" the instance stored in the container: `$flash = App::make('nova.flash');`
- Use the App helper as a shortcut to using the App facade: `$flash = app('nova.flash');`
- Use the Flash class's own facade: `$flash = Flash::getFacadeRoot();`
- Use the Flash helper to get at the existing instance of the class: `$flash = flash();`
