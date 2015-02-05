#generator-cc-angular

>Yeoman Generator for Enterprise Angular Projects

This generator is a fork of [https://github.com/cgross/generator-cg-angular](cg-angular), which is an amazing project!
It did not perfectly fit to my needs so I improved it for my purposes:

* clearer separation of config and sources by putting all the application-code in the `src` folder
* integration of `protractor`, executing matching e2e tests just like the unit tests
* updated all the bower-components and npm libs. Now using angular 1.3.x
* using `ui.router` by default
* try to stick to the very good [https://github.com/johnpapa/angularjs-styleguide#services](AngularJS Style Guide)
from [https://github.com/johnpapa](JohnPapa), e.g.:
    * No globals, put everything in closures
    * Use ControllerAs-Syntax
    * Use Named functions instead of anonymous


In the end, there where so many changes, that I decided to rename the generator to "cc-angular" where "cc" stands for
 "clean code".

Features

* Provides a directory structure geared towards large Angular projects.
    * Each controller, service, filter, and directive are placed in their own file.
    * All files related to a conceptual unit are placed together.  For example, the controller, HTML, LESS, and unit test for a partial are placed together in the same directory.
* Provides a ready-made Grunt build that produces an extremely optimized distribution.
   * Build uses [grunt-ng-annotate](https://github.com/olov/ng-annotate) so you don't have to use the Angular injection syntax for safe minification (i.e. you dont need `$inject` or `(['$scope','$http',...`.
   * `grunt serve` task allows you to run a simple development server with watch/livereload enabled.  Additionally, JSHint and the appropriate unit tests are run for the changed files.
* Integrates Bower for package management
* Includes Yeoman subgenerators for directives, services, partials, filters, and modules.
* Integrates LESS and includes Bootstrap via the source LESS files allowing you to reuse Bootstrap vars/mixins/etc.
* Easily Testable - Each sub-generator creates a skeleton unit test.  Unit tests can be run via `grunt test` and they run automatically during the grunt watch that is active during `grunt serve`.

Directory Layout
-------------
All subgenerators prompt the user to specify where to save the new files.  Thus you can create any directory structure you desire, including nesting.  The generator will create a handful of files in the root of your project including `index.html`, `app.js`, and `app.less`.  You determine how the rest of the project will be structured.

In this example, the user has chosen to group the app into an `admin` folder, a `search` folder, and a `service` folder.


    Gruntfile.js ................... Grunt build file
    /src ........................... folder for all app code
        app.less ....................... main app-wide styles
        app.js ......................... angular module initialization and route setup
        index.html ..................... main HTML file
        /admin ......................... example admin module folder
          admin.js ..................... admin module initialization and route setup
          admin.less ................... admin module LESS
          /admin-directive1 ............ angular directives folder
            admin-directive1.js ........ example simple directive
            admin-directive1.spec.js.... example simple directive unit test
          /admin-directive2 ............ example complex directive (contains external partial)
            admin-directive2.js ........ complex directive javascript
            admin-directive2.html ...... complex directive partial
            admin-directive2.less ...... complex directive LESS
            admin-directive2.spec.js ... complex directive unit test
          /admin-partial ............... example partial
            admin-partial.html ......... example partial html
            admin-partial.js ........... example partial controller
            admin-partial.less ......... example partial LESS
            admin-partial.spec.js ...... example partial unit test
        /search ........................ example search component folder
          my-filter.js ................. example filter
          my-filter.spec.js ............ example filter unit test
          /search-partial .............. example partial
            search-partial.html ........ example partial html
            search-partial.js .......... example partial controller
            search-partial.less ........ example partial LESS
            search-partial.spec.js ..... example partial unit test
        /service ....................... angular services folder
            my-service.js .............. example service
            my-service.spec.js ......... example service unit test
            my-service2.js ............. example service
            my-service2.spec.js ........ example service unit test
        /img ........................... images (not created by default but included in /dist if added)
        /dist .......................... distributable version of app built using grunt and Gruntfile.js
        /bower_component................ 3rd party libraries managed by bower
    /node_modules .................. npm managed libraries used by grunt

Getting Started
-------------

Prerequisites: Node, Grunt, Yeoman, and Bower.  Once Node is installed, do:

    sudo npm install -g grunt-cli yo bower

Next, install this generator:

    # for global use, otherwise install in projecc
    sudo npm install -g generator-cc-angular

To create a project:

    mkdir MyNewAwesomeApp
    cd MyNewAwesomeApp

    #if not installed globally, install it in the project
    npm install generator-cc-angular

    #we need webdriver for protractor
    ./node_modules/grunt-protractor-runner/node_modules/protractor/bin/webdriver-manager update

    # let's generate the app
    yo cc-angular

Grunt Tasks
-------------

Now that the project is created, you have 3 simple Grunt commands available:

    grunt serve   #This will run a development server with watch & livereload enabled.
    grunt test    #Run local unit tests.
    grunt build   #Places a fully optimized (minified, concatenated, and more) in /dist

When `grunt serve` is running, any changed javascript files will be linted using JSHint as well as have their appropriate unit tests executed.  Only the unit tests that correspond to the changed file will be run.  This allows for an efficient test driven workflow.

Yeoman Subgenerators
-------------

There are a set of subgenerators to initialize empty Angular components.  Each of these generators will:

* Create one or more skeleton files (javascript, LESS, html, spec etc) for the component type.
* Update index.html and add the necessary `script` tags.
* Update app.less and add the @import as needed.
* For partials, update the app.js, adding the necessary route call if a route was entered in the generator prompts.

There are generators for `directive`,`partial`,`service`, `filter`, `module`, and `modal`.

Running a generator:

    yo cc-angular:directive my-awesome-directive
    yo cc-angular:partial my-partial
    yo cc-angular:service my-service
    yo cc-angular:filter my-filter
    yo cc-angular:module my-module
    yo cc-angular:modal my-modal

The name paramater passed (i.e. 'my-awesome-directive') will be used as the file names.  The generators will derive appropriate class names from this parameter (ex. 'my-awesome-directive' will convert to a class name of 'MyAwesomeDirective').  Each sub-generator will ask for the folder in which to create the new skeleton files.  You may override the default folder for each sub-generator in the `.yo-rc.json` file.

The modal subgenerator is a convenient shortcut to create partials that work as modals for Bootstrap v3.1 and Angular-UI-Bootstrap v0.10 (both come preconfigured with this generator).  If you choose not to use either of these libraries, simply don't use the modal subgenerator.

Subgenerators are also customizable.  Please read [CUSTOMIZING.md](CUSTOMIZING.md) for details.

Submodules
-------------

Submodules allow you to more explicitly separate parts of your application.  Use the `yo cc-angular:module my-module` command and specify a new subdirectory to place the module into.  Once you've created a submodule, running other subgenerators will now prompt you to select the module in which to place the new component.

Preconfigured Libraries
-------------

The new app will have a handful of preconfigured libraries included.  This includes Angular 1.2, Bootstrap 3, AngularUI Bootstrap, AngularUI Utils, FontAwesome 4, JQuery 2, Underscore 1.5, LESS 1.6, and Moment 2.5.  You may of course add to or remove any of these libraries.  But the work to integrate them into the app and into the build process has already been done for you.

Build Process
-------------

The project will include a ready-made Grunt build that will:

* Build all the LESS files into one minified CSS file.
* Uses [grunt-angular-templates](https://github.com/ericclemmons/grunt-angular-templates) to turn all your partials into Javascript.
* Uses [grunt-ng-annotate](https://github.com/olov/ng-annotate) to preprocess all Angular injectable methods and make them minification safe.  Thus you don't have to use the array syntax.
* Concatenates and minifies all Javascript into one file.
* Replaces all appropriate script references in `index.html` with the minified CSS and JS files.
* (Optionally) Minifies any images in `/img`.
* Minifies the `index.html`.
* Copies any extra files necessary for a distributable build (ex.  Font-Awesome font files, etc).

The resulting build loads only a few highly compressed files.

The build process uses [grunt-dom-munger](https://github.com/cgross/grunt-dom-munger) to pull script references from the `index.html`.  This means that **your index.html is the single source of truth about what makes up your app**.  Adding a new library, new controller, new directive, etc does not require that you update the build file.  Also the order of the scripts in your `index.html` will be maintained when they're concatenated.

Importantly, grunt-dom-munger uses CSS attribute selectors to manage the parsing of the script and link tags.  Its very easy to exclude certain scripts or stylesheets from the concatenated files. This is often the case if you're using a CDN. This can also be used to prevent certain development scripts from being included in the final build.

* To prevent a script or stylesheet from being included in concatenation, put a `data-concat="false"` attribute on the link or script tag.  This is currently applied for the `livereload.js` and `less.js` script tags.

* To prevent a script or link tag from being removed from the finalized `index.html`, use a `data-remove="false"` attribute.


Release History
-------------
* 02/04/15 - v0.9.1 - now "reasonably tested" ;-), fixed some "src dir" - bugs
* 02/04/15 - v0.9.0 - fairly tested fork of cg-angular with all the changes mentioned above
