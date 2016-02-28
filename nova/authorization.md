# Authorization

Authorization is the process of determining if a user is allowed to take an action in Nova. Despite the use of a role-based system, previous versions of Nova have used a fairly rigid authorization system that didn't allow for granular control or modification. Moving forward though, administrators will have full control over what sorts of things roles can (and can't) do.

## Anatomy of Authorization

Every user is granted one or more __roles__ in Nova. The big change with this is that previously, a user could only have a single role. Having multiple roles means you can create more roles with smaller scopes of permissions and then pick and choose which of those roles a user has. Doing this also eliminates the duplication found in previous versions of Nova across different roles that shared permissions.

A good example of this is posting. Most users have access to post mission entries, but since a user could only have one role, those permissions had to be granted on every role. In the event a game switched away from Nova's posting system, all the roles would have to be updated. In Nova 3, the only role that has the posting permissions is the Active User role. Since every user has that role, every user has permission to post. In the event you wanted to remove that ability for everyone, you only have to edit the Active User role and remove that permission in order to make the change for everyone.

So what exactly is a role? Simply put, a role is a collection of __permissions__ that define the component and the action that can be taken. A role can have as many (or as few) permissions as you want. By default, Nova's roles are sets of permissions that make the most sense being together. You are free to adjust the permissions attached to a role to suit your game's needs, but we think the default roles will do a pretty good job for most games.

Now that Nova has permissions though, administrators and third-party developers will be able to create their own permissions that can be used in new or existing roles. This means that a third-party developer writing an extension with new reports can create new permissions when the extension is installed so that administrators know only the right people are seeing those new reports.

Like was mentioned earlier, a permission is simply a matter of defining a __component__ and an __action__. For example, in the `menu.create` permission, _menu_ is the component and _create_ is the action. That means that any user with a role that contains the `menu.create` permission will be able to create new menus, but not edit or remove existing ones. Nearly every permission through Nova digs down to this level to make it easier to allow site administrators to be able to control _exactly_ what users can see and do.

## Policies

A new concept in Nova's authorization system are _policies_. A policy is used to precisely define the actions a user can take on a given resource.

When dealing with the Page Manager, the Page Policy class defines `create()`, `edit()`, `remove()`, and `manage()` methods: the exact actions that can be taken on a page record. The logic inside the policy methods is very straightforward in this case. If you want to create a page, you have to have a role that includes the `page.create` permission. The policy method simply checks to see if the current user has that ability and returns a boolean of whether that's the case.

So why move this type of work out of the controller and into its own class?

Imagine that you want to make sure that the only way someone can create a new page is if they have the `page.create` permission _as well as_ a new `site.admin` permission that you created. Previously, there would be no good or easy way to make that change. In Nova 3, it's a matter of extending that policy class and making a change. It might look a little something like this:

<pre>public function create(User $user)
{
	return $user->can('page.create') and $user->can('site.admin');
}</pre>

Because you've extended the policy class (and updated the alias to it), Nova will just use the class you specified and start basing its criteria for creating a new page on what you told it to in the page policy's `create()` method.

## Putting It All Together

There are several ways to use Nova's authorization system depending on what you're doing and where you're doing it.

### Controller Authorization

Inside any controller that extends Nova's base controller, developers have access to the `authorize()` method. Using controller authorization will do a bunch of things for you that you would have to otherwise do yourself like displaying an error page, writing to the log files, and create a Nova system event.

<pre>public function index()
{
	$page = $this->data->page = new Page;

	$this->authorize('manage', $page, "You do not have permission to manage pages.");

	...
}</pre>

### Blade Directives

In Blade templates, there's a distinct directives that can be used in places of if/else logic blocks for checking permissions and/or policies: `@can`.

<pre>@can('manage', $page)
	Show something if the user can manage the page.
@endcan

@can('page.remove')
	Show something if the user can remove the page.
@else
	Show something if the user cannot remove the page.
@endcan</pre>

### The Gate Facade

### The Policy Helper
