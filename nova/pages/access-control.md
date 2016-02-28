# Controlling Access

Access control is how Nova determines whether a user has the correct permissions to access a given page. Roles in Nova are made up of many different permissions and you can use either of those concepts for building access to a page that you create. (If you need a refresher on Nova's access control system, you should review the [authentication](../authentication.md) and [authorization](../authorization.md) documents.)

## Basic Pages

There are two options for controlling who has access to a basic page: restricting by access role or permission key. You can only choose one or the other, not both. If you have a scenario where you need to do both access roles and permission keys, you'll have to convert your page to an advanced page and write the access logic yourself.

With both access options, you can select as many roles or permissions as you want and tell Nova if you want _all_ the items to be required in order to access the page or just _one_ of the items to be required.

Regardless of which option you use, indicating an access role or permission key means that Nova will also require the visitor to be logged in to the site in order to access the page. If the user is not logged in or does not have the appropriate access, they'll be shown a page that they are not authorized to view the page.

## Advanced Pages

When it comes to controlling access with an advanced page, it's up to the developer to write the logic in their controller methods or in Request classes. There are three options for controlling access to an advanced page: checking the user's access role(s), checking the user's access role(s) permissions, or writing a policy class to define the roles/permissions. For Nova's core pages, we use policy classes.
