# Nova Base Repository

Every repository that interacts with the database in Nova extends the `BaseRepository` class. Doing this allows us to provide a consistent API for interacting with the Nova database. While the individual repositories have their own methods and signatures, they all have access to the base methods in the base repository.

___Important: Some repositories override the default implementation of the base repository method for various reasons. Please see the repository classes for notes about such instances.___

## Methods

#### A Note About the `$with` Argument

Many of the methods below accept a `$with` parameter. In all cases this is an _optional_ __array__ that specifies which of the model's relationships should be eager loaded. Why eager load relationships? When you don't eager load relationships, every time you call that relationship, the database is queried. If you were to reference a non-eager loaded relationship inside of a loop, you could end up making a significant number of additional queries against the database, in some cases, even hundreds. This is the dreaded "n+1" query situation and can lead to slow pages.

When developing an extension, be sure to take the time to eager loading relationships you may need at a later point in a page or controller.

#### A Note About the `$resource` Argument

Several of the methods below accept a `$resource` parameter. In any cases you see this argument it is __required__. To provide maximum flexibility, the parameter can be either a numerical primary ID (in which case Nova will query the database for the record with the matching ID) or an instance of a model previously returned from another method. (Obviously if you've already queried the database for a record, there's no sense hitting the database again for the same record!)

#### A Note About the `$data` Argument

Several of the methods below accept a `$data` argument. In any cases you see this argument, it is __required__ and must be an __array__. As you build the array, keep in mind that any keys of the array should correspond to the database columns and any values of the array should be the data you want in the respective columns.

### `all($with)`

Gets all records in a database table.

`$items = $this->storyRepo->all();

$itemsWithRelations = $this->storyRepo->all(['posts', 'posts.authors'])`

_Returns:_ a collection of models

### `countBy($column, $value)`

Counts everything in a database table using the key-value pair as criteria for the query.

`$count = $this->tabRepo->countBy('parent_id', 11);`

_Returns:_ an integer

### `create($data)`

Creates a new record in the database with the data passed to it.

`$post = $this->postRepo->create([
	'title' => "My Post",
	'location' => "Main Office Building",
	'timeline' => "Monday morning",
	'content' => "We open on an office building on a Monday morning...",
]);`

### `delete($resource)`

Deletes the resource.

`$page = $this->pageRepo->getById(14);

$deletedPage = $this->pageRepo->delete($page);

$deletedPage2 = $this->pageRepo->delete(27);`

_Returns:_ an instance of the model that was deleted

### `forceDelete($resource)`

In some cases, models use soft deleting (keeping an item in the database but setting a deleted at timestamp). In those instances, if you want to totally remove the resource from the database, you'll need to do so with a force delete.

`$character = $this->characterRepo->forceDelete(18);`

_Returns:_ an instance of the model that was deleted

### `getById($id)`

Get a record from the database by its numerical primary ID.

`$user = $this->userRepo->getById(4);`

_Returns:_ an instance of the model

### `getByPage($page, $limit, $with)`

### `getFirstBy($column, $value, $with, $operator)`

Get the first record that matches the criteria of the key-value pair. By default, Nova will use an _equals_ operator for the query, but you can pass any operator to the fourth parameter (=, !=, <, >, <=, >=, <>, LIKE, NOT LIKE, IS NOT NULL, IS NULL, IS NOT, IS, REGEXP, NOT REGEXP).

`$menuItem = $this->menuItemRepo->getFirstBy('title', 'Home');

$user = $this->userRepo->getFirstBy('name', 'Bob', [], 'LIKE');`

_Returns:_ an instance of the model

`getManyBy($column, $value, $with, $operator)`

Get all the records that match the criteria of the key-value pair. By default, Nova will use an _equals_ operator for the query, but you can pass any operator to the fourth parameter (=, !=, <, >, <=, >=, <>, LIKE, NOT LIKE, IS NOT NULL, IS NULL, IS NOT, IS, REGEXP, NOT REGEXP).

`$menuItems = $this->menuItemRepo->getManyBy('title', 'Admin%', [], 'LIKE');`

_Returns:_ a collection of models

`listAll($key, $value)`

_Returns:_ an array

`listAllBy($key, $value, $displayKey, $displayValue)`

_Returns:_ an array

`listAllFiltered($key, $value, $filters)`

_Returns:_ an array

`listCollection($collection, $displayKey, $displayValue)`

_Returns:_ an array

`make($with)`

The `make` method creates a new instance of that particular model. With the return value, you can chain additional Builder methods like `where`. The `$with` variable is an array of relationships to eager load and is optional.

_Returns:_ a Builder instance

`update($resource, $data)`

Updates a record in the database. Like other instances, the resource can be a model or a numerical primary ID. Additionally, the record will be updated with the data array passed.

_Returns:_ an instance of the model updated or `false` if the resource didn't exist

`updateOrder($resource, $newOrder)`

Many of the database tables in Nova allow for ordering entries. This method provides a convenient way to set a new order on a record. Like other instances, the resource can be a model or a numerical primary ID. Additionally, the `$newOrder` is an integer that represents what the new order of the record should be.

_Returns:_ an instance of the model updated or `false` if the resource didn't exist
