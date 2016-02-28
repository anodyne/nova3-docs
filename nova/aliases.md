# Aliases

Laravel includes an array of aliases to make referencing classes easier. For Nova, it also has the added benefit of being able to change the classes being used without needing to change anything in the core.

## How Aliases Work

In the Nova core, when we want to reference the User model, we reference the `User` class. The catch though is that there actually isn't a `User` class. Because of how Nova is structured, the actual class is `Nova\Core\Users\Data\User`. It would be a pain to have to type that every time though, so using an alias means that whenever we use `User`, Laravel actually grabs `Nova\Core\Users\Data\User`. That means, that if you want to make changes to the `User` class, you can do so in your own extension and then change the class `User` references the app config file. The next time the Nova core (or anything that uses the `User` class) calls the `User` class, it's using your custom version of that class instead of the one that comes with Nova.

## Updating Aliases

To update an alias, you'll need to update the `config/app.php` file. In a fresh install of Nova 3, there's just an empty array of aliases. All you need to do is find the alias you want to change in `nova/config/app.php`, copy that line, and paste it into the aliases section of the `config/app.php` file.

Let's stick with the `User` class example and assume you've made a change to the `User` class that you want to use everywhere. In order to have Nova start using that new class, you'd head over to `nova/config/app.php` and find where the `User` alias is defined:

```php
'User' => Nova\Core\Users\Data\User::class,
```

Copy that line and come over to your `config/app.php` file to find the empty aliases array:

```php
'aliases' => [],
```

Throw the line you copied into the aliases array and then make your modifications:

```php
'aliases' => [
	'User' => MyExtension\NewUserClass\User::class,
],
```

That's it! Now, every time Nova references the `User` class, it'll actually point to your extension to use it instead.
