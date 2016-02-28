# Events

## Reacting to Events

If you want to skip updating the events config file, you can also register your event listeners in a service provider and react to the event through a callback instead.

_Note: by manually register event listeners in this way, you won't be able to specify the order in which your event listener runs._

There are several ways to register your event. First, you can pull the event instance out of the IoC container either through the `$app` instance or through a facade. (If you use the facade, remember that you'll have to add the using statement.)

<pre>public function boot()
{
	$this->app['events']->listen('nova.start', function () {
		// Your code goes here
	});

	Event::listen('nova.stop', function () {
		// Your code goes here
	});
}</pre>

You can also inject the event dispatcher contract directly in to the `boot` method. (There are various reasons you may want to do this, but in most cases, using the `$app` instance is going to be the easiest way to do this.)

<pre>use Illuminate\Contracts\Events\Dispatcher;

class MyServiceProvider extends ServiceProvider {

	public function boot(Dispatcher $events)
	{
		$events->listen('nova.stop', function () {
			// Your code goes here
		});
	}
}</pre>

_Note: the example above is not complete and meant for illustration purposes only._

## Nova Events

You can get a complete list of events that Nova fires in the events config file located in the `nova/config/events.php` file.
