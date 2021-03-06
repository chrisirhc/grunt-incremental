# grunt-incremental
<!-- [![Build Status](https://secure.travis-ci.org/chrisirhc/grunt-incremental.png?branch=master)](http://travis-ci.org/chrisirhc/grunt-incremental) -->

> Run predefined tasks only on changed files whenever watched file patterns are added, changed or deleted.

_This is work in progress. More details about what's coming up or the plan for this project can be found on [this gist](https://gist.github.com/4678098)._

## TODO

* Use a database that supports concurrency to be aware of changes during the build process
* Support queueing of tasks
* Support interrupt
* Better support for other methods of specifying files, instead of using the `files` options and other formats besides those mentioned on this.

## Getting Started
If you haven't used [grunt][] before, be sure to check out the [Getting Started][] guide, as it explains how to create a [gruntfile][Getting Started] as well as install and use grunt plugins. Once you're familiar with that process, install this plugin with this command:

```shell
npm install grunt-incremental --save-dev
```

[grunt]: http://gruntjs.com/
[Getting Started]: http://gruntjs.com/getting-started


## Incwatch task
_Run this task with the `grunt incwatch` command._

### Overview

Inside your `Gruntfile.js` file, add a section named `watch`. This section specifies the files to watch, tasks to run when an event occurs and the options used.
### Settings

There are a number of options available. Please review the [minimatch options here](https://github.com/isaacs/minimatch#options). As well as some additional options as follows:

#### files
Type: `String|Array`

This defines what file patterns this task will watch. Can be a string or an array of files and/or minimatch patterns.

#### tasks
Type: `String|Array`

This defines which tasks to run when a watched file event occurs.

#### options.interrupt
Type: `boolean`
Default: false

As files are modified this watch task will spawn tasks in child processes. The default behavior will only spawn a new child process per target when the previous process has finished. Set the `interrupt` option to true to terminate the previous process and spawn a new one upon later changes.

Example:
```js
watch: {
  scripts: {
    files: '**/*.js',
    tasks: ['jshint'],
    options: {
      interrupt: true
    }
  }
}
```

#### options.debounceDelay
Type: `Integer`
Default: 500

How long to wait before emitting events in succession for the same filepath and status. For example if your `Gruntfile.js` file was `changed`, a `changed` event will only fire again after the given milliseconds.

Example:
```js
watch: {
  scripts: {
    files: '**/*.js',
    tasks: ['jshint'],
    options: {
      debounceDelay: 250
    }
  }
}
```

#### options.interval
Type: `Integer`
Default: 100

The `interval` is passed to `fs.watchFile`. Since `interval` is only used by `fs.watchFile` and this watcher also uses `fs.watch`; it is recommended to ignore this option. *Default is 100ms*.

### Examples

```js
// Simple config to run jshint any time a file is added, changed or deleted
grunt.initConfig({
  watch: {
    files: '**/*',
    tasks: ['jshint']
  }
});
```

```js
// Advanced config. Run specific tasks when specific files are added, changed or deleted.
grunt.initConfig({
  watch: {
    gruntfile: {
      files: 'Gruntfile.js',
      tasks: ['jshint:gruntfile'],
      options: {
        nocase: true
      }
    },
    src: {
      files: ['lib/*.js', 'css/**/*.scss', '!lib/dontwatch.js'],
      tasks: ['default']
    },
    test: {
      files: '<%= jshint.test.src %>',
      tasks: ['jshint:test', 'qunit']
    }
  }
});
```

### FAQs

#### How do I fix the error `EMFILE: Too many opened files.`?
This is because of your system's max opened file limit. For OSX the default is very low (256). Temporarily increase your limit with `ulimit -n 10480`, the number being the new max limit.

#### Can I use this with Grunt v0.3?
Yes. Although `grunt-contrib-watch` is a replacement watch task for Grunt v0.4, version `grunt-contrib-watch@0.1.x` is compatible with Grunt v0.3. `grunt-contrib-watch >= 0.2.x` is **only* compatible and recommended to use with Grunt v0.4.

#### Why is the watch devouring all my memory?
Likely because of an enthusiastic pattern trying to watch thousands of files. Such as `'**/*.js'` but forgetting to exclude the `node_modules` folder with `'!node_modules/**/*.js'`. Try grouping your files within a subfolder or be more explicit with your file matching pattern.


## Release History

 * 2013-01-17   v0.2.0rc7   Updating grunt/gruntplugin dependencies to rc6. Changing in-development grunt/gruntplugin dependency versions from tilde version ranges to specific versions.
 * 2013-01-08   v0.2.0rc5   Updating to work with grunt v0.4.0rc5.
 * 2012-12-14   v0.2.0a   Conversion to grunt v0.4 conventions. Remove node v0.6 and grunt v0.3 support. Allow watch task to be renamed. Use grunt.util.spawn "grunt" option. Updated to gaze@0.3.0, forceWatchMethod option removed.
 * 2012-10-31   v0.1.4   Prevent watch from spawning duplicate watch tasks
 * 2012-10-27   v0.1.3   Better method to spawn the grunt bin Bump gaze to v0.2.0. Better handles some events and new option forceWatchMethod Only support Node.js >= v0.8
 * 2012-10-16   v0.1.2   Only spawn a process per task one at a time Add interrupt option to cancel previous spawned process Grunt v0.3 compatibility changes
 * 2012-10-15   v0.1.1   Fallback to global grunt bin if local doesnt exist. Fatal if bin cannot be found Update to gaze 0.1.6
 * 2012-10-07   v0.1.0   Release watch task Remove spawn from helper Run on Grunt v0.4

---

Task submitted by [Kyle Robinson Young](http://dontkry.com)

*This file was generated on Tue Feb 05 2013 12:44:01.*
