Phake
====

Phake is a direct port of [Jake](http://github.com/280north/jake) in PHP.

API
---

The API is identical to the Jake API, with PHP syntax.

- `Phake::task(string $name [, array $dependencies = array() [, callback $action = "print" ]])`

Declares a task called name, with an optional array of dependent tasks, and optional function to perform.

- `Phake::file(path, [dependencies], [action])`

Like `task`, but only runs the action if the target file ("name") doesn't exist or was last modified before at least on dependency.

*TODO: better API docs*

Example "Phakefile"
------------------

    function default($t) {
        print($t->name());
    }
    // prints "default":
    Phake::task("default", array(), "default");
    
    // runs tasks "bar" and "baz"
    Phake::task("foo", array("bar", "baz"));
    
    function bar($t) {
        // compile "bar" using "bar.source" here
    }
    // only runs if "bar" is older than "bar.source" or non-existant
    Phake::file("bar", array("bar.source"), "bar");
    
    // does nothing
    Phake::task("baz");

Example Usage
-------------

    # runs "default" task if no task names are given
    phake
    
    # runs "bar", "baz" dependent tasks, then "foo" task
    phake foo
    
    # runs "bar", "baz", "foo", and "default" tasks
    phake foo default
