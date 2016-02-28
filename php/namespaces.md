# Namespaces

The PHP community has a lot of developers creating lots of code. This means that one library's PHP code may use the same class name as another library. When both libraries use the same class name, they collide and cause trouble.

Namespaces solve this problem. As described in the PHP reference manual, namespaces may be compared to operating system directories that namespace files; two files with the same name may co-exist in separate directories. Likewise, two PHP classes with the same name may co-exist in separate PHP namespaces. It's as simple as that.

It is important for you to namespace your code so that it may be used by other developers without fear of colliding with other libraries.

## PSR-4 Autoloading
