# The Laravel Service Container

__Important: This is a highly advanced topic that's intended for developers. It isn't necessary to understand dependecy injection or the service container in order to use Nova or build themes. This is a topic that's targeted toward developers looking to build advanced extensions.__

Any modern PHP application is full of object. One object may facilitate delivering emails while another allows you to persist information into a database. In Nova, we have objects that handle flash notifications while other objects handling locating assets that Nova needs. The point here is that any modern application does a lot of different things and is organized into many objects that handle those tasks.

As an application grows in size and complexity, it can become more of a chore to keep the objects straight and to handle any dependencies those objects may have. This is where a service container comes in. Simply put, a service container is an object that helps you create, organize, and retrieve the multitude of objects in the application.

Before we get into the actual container though, it's _very_ important to understand the underlying principle at work here: dependency injection.

## Dependency injection

From Wikipedia:

> Dependency injection is a software design pattern that allows the removal of hard-coded dependencies and makes it possible to change them, whether at run-time or compile-time.

This quote makes the concept sound much more complicated than it actually is. Dependency Injection is providing a component with its dependencies either through constructor injection, method calls, or the setting of properties.

### Basic Concept

We can demonstrate the concept with a simple, yet naive example.

Here we have a Database class that requires an adapter to speak to the database. We instantiate the adapter in the constructor and create a hard dependency. This makes testing difficult and means the Database class is very tightly coupled to the adapter.

```php
<?php namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
```

This code can be refactored to use Dependency Injection and therefore loosen the dependency.

```php
<?php namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
```

Now we are giving the Database class its dependency rather than it creating it itself. We could even create a method that would accept an argument of the dependency and set it that way, or if the `$adapter` property was public we could set it directly.

## The Service Container



Any modern PHP application is full of objects. One object may facilitate the delivery of email messages while another may allow you to persist information into a database. In your application, you may create an object that manages your product inventory, or another object that processes data from a third-party API. The point is that a modern application does many things and is organized into many objects that handle each task.

This chapter is about a special PHP object in Symfony that helps you instantiate, organize and retrieve the many objects of your application. This object, called a service container, will allow you to standardize and centralize the way objects are constructed in your application. The container makes your life easier, is super fast, and emphasizes an architecture that promotes reusable and decoupled code. Since all core Symfony classes use the container, you'll learn how to extend, configure and use any object in Symfony. In large part, the service container is the biggest contributor to the speed and extensibility of Symfony.

Finally, configuring and using the service container is easy. By the end of this chapter, you'll be comfortable creating your own objects via the container and customizing objects from any third-party bundle. You'll begin writing code that is more reusable, testable and decoupled, simply because the service container makes writing good code so easy.

### What Is a Service?

Put simply, a Service is any PHP object that performs some sort of "global" task. It's a purposefully-generic name used in computer science to describe an object that's created for a specific purpose (e.g. delivering emails). Each service is used throughout your application whenever you need the specific functionality it provides. You don't have to do anything special to make a service: simply write a PHP class with some code that accomplishes a specific task. Congratulations, you've just created a service!

_As a rule, a PHP object is a service if it is used globally in your application. A single Mailer service is used globally to send email messages whereas the many Message objects that it delivers are not services. Similarly, a Product object is not a service, but an object that persists Product objects to a database is a service._

So what's the big deal then? The advantage of thinking about "services" is that you begin to think about separating each piece of functionality in your application into a series of services. Since each service does just one job, you can easily access each service and use its functionality wherever you need it. Each service can also be more easily tested and configured since it's separated from the other functionality in your application. This idea is called service-oriented architecture and is not unique to Symfony or even PHP. Structuring your application around a set of independent service classes is a well-known and trusted object-oriented best-practice. These skills are key to being a good developer in almost any language.

### What Is a Service Container?

A Service Container (or dependency injection container) is simply a PHP object that manages the instantiation of services (i.e. objects).

For example, suppose you have a simple PHP class that delivers email messages. Without a service container, you must manually create the object whenever you need it:

```php
use AppBundle\Mailer;

$mailer = new Mailer('sendmail');
$mailer->send('ryan@example.com', ...);
```

This is easy enough. The imaginary Mailer class allows you to configure the method used to deliver the email messages (e.g. sendmail, smtp, etc). But what if you wanted to use the mailer service somewhere else? You certainly don't want to repeat the mailer configuration every time you need to use the Mailer object. What if you needed to change the transport from sendmail to smtp everywhere in the application? You'd need to hunt down every place you create a Mailer service and change it.

A better answer is to let the service container create the Mailer object for you.

An instance of the AppBundle\Mailer class is now available via the service container. The container is available in any traditional Symfony controller where you can access the services of the container via the get() shortcut method:

When you ask for the app.mailer service from the container, the container constructs the object and returns it. This is another major advantage of using the service container. Namely, a service is never constructed until it's needed. If you define a service and never use it on a request, the service is never created. This saves memory and increases the speed of your application. This also means that there's very little or no performance hit for defining lots of services. Services that are never used are never constructed.

As a bonus, the Mailer service is only created once and the same instance is returned each time you ask for the service. This is almost always the behavior you'll need (it's more flexible and powerful), but you'll learn later how you can configure a service that has multiple instances in the "How to Define Non Shared Services" cookbook article.




The Laravel service container is a powerful tool for managing class dependencies and performing dependency injection. Dependency injection is a fancy phrase that essentially means this: class dependencies are "injected" into the class via the constructor or, in some cases, "setter" methods.

## Binding

Almost all of your service container bindings will be registered within service providers, so all of these examples will demonstrate using the container in that context. However, there is no need to bind classes into the container if they do not depend on any interfaces. The container does not need to be instructed on how to build these objects, since it can automatically resolve such "concrete" objects using PHP's reflection services.

Within a service provider, you always have access to the container via the $this->app instance variable. We can register a binding using the bind method, passing the class or interface name that we wish to register along with a Closure that returns an instance of the class:

```php
$this->app->bind('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app['HttpClient']);
});
```

Notice that we receive the container itself as an argument to the resolver. We can then use the container to resolve sub-dependencies of the object we are building.

#### Binding a Singleton

You may also bind an existing object instance into the container using the instance method. The given instance will always be returned on subsequent calls into the container:

```php
$fooBar = new FooBar(new SomethingElse);

$this->app->instance('FooBar', $fooBar);
```

### Binding Interfaces to Implementations

A very powerful feature of the service container is its ability to bind an interface to a given implementation. For example, let's assume we have an EventPusher interface and a RedisEventPusher implementation. Once we have coded our RedisEventPusher implementation of this interface, we can register it with the service container like so:

```php
$this->app->bind('App\Contracts\EventPusher', 'App\Services\RedisEventPusher');
```

This tells the container that it should inject the RedisEventPusher when a class needs an implementation of EventPusher. Now we can type-hint the EventPusher interface in a constructor, or any other location where dependencies are injected by the service container:

### Contextual Binding

Sometimes you may have two classes that utilize the same interface, but you wish to inject different implementations into each class. For example, when our system receives a new Order, we may want to send an event via PubNub rather than Pusher. Laravel provides a simple, fluent interface for defining this behavior:

```php
$this->app->when('App\Handlers\Commands\CreateOrderHandler')
          ->needs('App\Contracts\EventPusher')
          ->give('App\Services\PubNubEventPusher');
```

You may even pass a Closure to the give method:

```php
$this->app->when('App\Handlers\Commands\CreateOrderHandler')
          ->needs('App\Contracts\EventPusher')
          ->give(function () {
                  // Resolve dependency...
              });
```

#### Binding Primitives

Sometimes you may have a class that receives some injected classes, but also needs an injected primitive value such as an integer. You may easily use contextual binding to inject any value your class may need:

```php
$this->app->when('App\Handlers\Commands\CreateOrderHandler')
          ->needs('$maxOrderCount')
          ->give(10);
```

### Tagging

Occasionally, you may need to resolve all of a certain "category" of binding. For example, perhaps you are building a report aggregator that receives an array of many different Report interface implementations. After registering the Report implementations, you can assign them a tag using the tag method:

```php
$this->app->bind('SpeedReport', function () {
    //
});

$this->app->bind('MemoryReport', function () {
    //
});

$this->app->tag(['SpeedReport', 'MemoryReport'], 'reports');
```

Once the services have been tagged, you may easily resolve them all via the tagged method:

```php
$this->app->bind('ReportAggregator', function ($app) {
    return new ReportAggregator($app->tagged('reports'));
});
```

## Resolving

There are several ways to resolve something out of the container. First, you may use the make method, which accepts the name of the class or interface you wish to resolve:

```php
$fooBar = $this->app->make('FooBar');
```

Secondly, you may access the container like an array, since it implements PHP's ArrayAccess interface:

```php
$fooBar = $this->app['FooBar'];
```

Lastly, but most importantly, you may simply "type-hint" the dependency in the constructor of a class that is resolved by the container, including controllers, event listeners, queue jobs, middleware, and more. In practice, this is how most of your objects are resolved by the container.

The container will automatically inject dependencies for the classes it resolves. For example, you may type-hint a repository defined by your application in a controller's constructor. The repository will automatically be resolved and injected into the class:

## Container Events

The service container fires an event each time it resolves an object. You may listen to this event using the resolving method:

```php
$this->app->resolving(function ($object, $app) {
    // Called when container resolves object of any type...
});

$this->app->resolving(FooBar::class, function (FooBar $fooBar, $app) {
    // Called when container resolves objects of type "FooBar"...
});
```

As you can see, the object being resolved will be passed to the callback, allowing you to set any additional properties on the object before it is given to its consumer.
