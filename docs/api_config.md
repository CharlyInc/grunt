[Grunt homepage](https://github.com/cowboy/grunt) | [Documentation table of contents](toc.md)

# [The grunt API](api.md) / grunt.config

Access project-specific configuration data defined in the [grunt.js gruntfile](configuring.md).

See the [config lib source](../lib/grunt/config.js) for more information.

## The config API

Note that any method marked with a ☃ (unicode snowman) is available directly on the `grunt` object in addition to being available on the `grunt.config` object. Just so you know. See the [API main page](api.md) for more usage information.

## Initializing Config Data
_Note that the method listed below is also available on the `grunt` object as [grunt.initConfig](api.md)._

### grunt.config.init ☃
Initialize a configuration object for the current project. The specified `configObject` is used by tasks and helpers and can also be accessed using the `grunt.config` method. Nearly every project's [grunt.js gruntfile](configuring.md) will call this method.

```javascript
grunt.config.init(configObject)
```

Note that any specified `<config>` and `<json>` [directives](api_task.md) will be automatically processed when the config object is initialized.

This example contains sample config data for the [lint task](task_lint.md):

```javascript
grunt.config.init({
  lint: {
    all: ['lib/*.js', 'test/*.js', 'grunt.js']
  }
});
```

See the [configuring grunt](configuring.md) page for more configuration examples.

_This method is also available as [grunt.initConfig](api.md)._


## Accessing Config Data
The following methods allow grunt configuration data to be accessed either via dot-delimited string like `'pkg.author.name'` or via array of property name parts like `['pkg', 'author', 'name']`.

Note that if a specified property name contains a `.` dot, it must be escaped with a literal backslash, eg. `'concat.dist/built\\.js'`. If an array of parts is specified, grunt will handle the escaping internally with the `grunt.config.escape` method.

### grunt.config
Get or set a value from the project's grunt configuration. This method serves as an alias to other methods; if two arguments are passed, `grunt.config.set` is called, otherwise `grunt.config.get` is called.

```javascript
grunt.config([prop [, value]])
```

### grunt.config.get
Get a value from the project's grunt configuration. If `prop` is specified, that property's value is returned, or `null` if that property is not defined. If `prop` isn't specified, a copy of the entire config object is returned.

```javascript
grunt.config.get([prop])
```

Any `<% %>` templates in returned values will not be automatically processed, but can be processed afterwards using the [grunt.template.process](api_template.md) method. If you want to do both at once, the `grunt.config.process` method can be used.

### grunt.config.set
Set a value into the project's grunt configuration.

```javascript
grunt.config.set(prop, value)
```

Note that any specified `<config>` and `<json>` [directives](api_task.md) will be automatically processed when the config data is set.

### grunt.config.escape
Escape `.` dots in the given `propString`. This should be used for property names that contain dots.

```javascript
grunt.config.escape(propString)
```

### grunt.config.process
Behaves like `grunt.config.get`, but additionally recursively processes all `<% %>` templates in the returned data.

```javascript
grunt.config.process([prop])
```

## Requiring Config Data

### grunt.config.requires
Fail the current task if one or more required [config](api_config.md) properties is missing. One or more string or array config properties may be specified.

```javascript
grunt.config.requires(prop [, prop [, ...]])
```

See the [grunt.config documentation](api_config.md) for more information about config properties.

_This method is also available as [grunt.task.current.requiresConfig](api.md) or inside tasks as [this.requiresConfig](api.md)._