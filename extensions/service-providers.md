# Service Providers

Service providers are the central place of all Laravel application bootstrapping. But what exactly do we mean when we talk about "bootstrapping" the application? In general, this means registering things, including service container bindings, event listeners, middleware, and even routes. Service providers are a central location to configure the application for use.

_Note: while service providers are available for use in your extension(s), they are not required to be in your extension._

If you need to do some setup for your extension or create classes or set up event listeners or anything of the sort, Nova lets you create your own service provider for registering and booting the extension. If you're familiar with Laravel, you'll know that all the providers are declared in the application config file. To simplify extension development and installation in Nova though, registering of extension service providers is handled automatically for you based on __activated__ extensions. If your extension is marked as being inactive, the service provider will not be registered when Nova starts up.

## The `register` Method

 Within the `register` method, you should only bind things into the service container. You should __never__ attempt to register any event listeners, routes, or any other piece of functionality within the `register` method. Otherwise, you may accidentally use a service that is provided by a service provider which has not been loaded yet.

 <pre>&lt;?php namespace Extension\VendorName\ExtensionName;

 use Illuminate\Support\ServiceProvider as LaravelServiceProvider;

 class ServiceProvider extends LaravelServiceProvider {

	 public function register()
	{
	    $this->app->singleton('Riak\Contracts\Connection', function ($app) {
	        return new Connection(config('riak'));
	    });
	}

}</pre>

## The `boot` Method

So, what if you need to register a view composer within the service provider? This should be done within the `boot` method. This method is called after all other service providers have been registered, meaning you have access to all other services that have been registered by Laravel and Nova.

<pre>&lt;?php namespace Extension\VendorName\ExtensionName;

use Illuminate\Support\ServiceProvider as LaravelServiceProvider;

class ServiceProvider extends LaravelServiceProvider {

	public function boot()
	{
	    view()->composer('view', function () {
	        //
	    });
	}

	public function register()
	{
		//
	}

}</pre>

__Important: The register method is required in all service providers whether you use it or not.__

## Deferred Providers

If your provider is only registering bindings in the service container, you may choose to defer its registration until one of the registered bindings is actually needed. Deferring the loading of such a provider will improve the performance of your application, since it is not loaded from the filesystem on every request.

To defer the loading of a provider, set the `defer` property to true and define a `provides` method. The `provides` method returns the service container bindings that the provider registers.

<pre>&lt;?php namespace Extension\VendorName\ExtensionName;

use Illuminate\Support\ServiceProvider as LaravelServiceProvider;

class ServiceProvider extends LaravelServiceProvider {

	protected $defer = true;

	public function provides()
	{
	    return ['Riak\Contracts\Connection'];
	}

}</pre>
