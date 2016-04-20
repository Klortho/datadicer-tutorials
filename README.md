# DataDicer Tutorials

DataDicer is a framework for the creation of web apps that can query and display data.  To make development easier, DataDicer borrows its architectural paradigm from AngularJS and uses declarative programming to define the user interface and imperative programming to define business logic.  The interface is written or declared in HTML and JavaScript is used to connect  the pieces together to the data store.  DataDicer does much of the wiring for the developer in a manner that is supposed to be simple yet flexible to deal with multiple types of data and data summaries such as facets.

The overall architecture is a hierarchical model view controller.  The model view controller is a standard design template where the model controls the business logic and data, the view displays the data, and the controller accepts input and communicates it to the model and view.  A hierarchical model view controller is used to logically group together components that talk only to each other and do not necessarily have to talk to other components on the page, other than through a parent, and where the parent might gather together multiple requests to the backend for maximal performance.

## Hello, World

We'll begin by creating and explaining a simple application that shows a grid view displaying a table of information from a solr back end.   The code for this section can be found at https://stash.ncbi.nlm.nih.gov/projects/NGRAM/repos/datadicerui.boilerplate.app/browse?at=refs%2Fheads%2FminimalConf.

The application consists of two parts, the html, which declares the application UI, and the JavaScript, which wires the grid to the back end and deals with events occurring on the page.

***The HTML***

```html
<!doctype html>
<html class="no-js">
  <head>
    <!-- third party js library css -->
    <link rel=stylesheet href=bower_components/datadicer/dist/styles/vendor.css>
    <!-- the css for the components in this app -->
    <link rel=stylesheet href=bower_components/datadicer/dist/styles/main.css>
  </head>
  <body>
    <!-- define a div that contains the app TestApp with the controller gridController. -->
    <div ng-app=TestApp 
         ng-controller=gridController 
         class="container">
      <!-- insert the grid-table component using the query object "query" and the 
        config "gridConfig" both of which are defined under the $scope application 
        object -->
      <grid-table query=query config=gridConfig></grid-table>
    </div>
    <!-- angular and 3rd party js libraries -->
    <script src=bower_components/datadicer/dist/scripts/vendor.js></script>
    <!-- datadicer libraries -->
    <script src=bower_components/datadicer/dist/scripts/scripts.js></script>
    <!-- compiled component html templates for efficiency -->
    <script src=bower_components/datadicer/dist/scripts/templates.js></script>
    <!-- our application script -->
    <script src="src/script.js"></script>
  </body>
</html>
```

Starting at the top of the code, the links within the head load in the CSS style sheets for both third party libraries and for the components used in the application.

In the body, we create the application itself by creating a div that has the attribute `ng-app`, giving the attribute the name of the application.  This name will be used in the JavaScript to refer to the app.  We also declare the name of the controller as `gridController` using the attribute `ng-controller`.  The name `gridController` will also be used in the JavaScript.

`<grid-table>` is an Angular JS component, a type of AngularJS directive.  When loaded in a browser, an HTML page is converted into a tree like structure called the Document Object Model (DOM).  AngularJS looks in the DOM for objects it recognizes, and then replaces them with its own objects.  A component is a particular kind of directive used to create the components of an application.  To give an example of this, `<grid-table>` in the sample HTML is recognized by AngularJS in the DOM and replaced by a UI component that is part of the DataDicer library that shows a grid (aka table).

The rest of the sample application loads in JavaScript libraries, including those from AngularJS and DataDicer.  For efficiency, templates.js contains compiled HTML used in the UI components.

***The JavaScript***

```javascript
// angular.module defines an app.  The app is called TestApp. The array lists dependencies 
// of the app.  These are angular modules to be injected into the app.  Angular modules are
// code factories.  In this case, it is the ngramApp library
angular.module('TestApp', ["ngramApp"]) 

  // define a controller in the app.  An app can have multiple controllers.  The name of the
  // controller is gridController and the array lists angular modules that are dependencies of
  // the controller.  In this case the only dependency is the $scope object, which is the
  // application object.
  .controller('gridController', ['$scope', 
    function ($scope) {
      // define within the application object the solr query.  In the future, this will be
      // given two way binding to communicate between components.  A component is the smallest
      // functional UI subunit.  A module can consist of multiple components.
      $scope.query = { 
        // matches all fields to all values, use empty string for ngram query and *:* for 
        // solr query
        matching: '*:*'
      };
      $scope.gridConfig = { // configures the grid to be displayed
        serviceEngine: 'solr', // what backend engine to use
        // the solr query url format to use:
        serviceQueryUrl: 'http://iwebdev2.ncbi.nlm.nih.gov:8995' +
                         '/solr/bioproject/select?' +
                         'wt=json&indent=true&json.wrf=?',
        autoLoad: true,  // load data as soon as running
        rownumbers: true,  //show "rownumbers" column
        // per column configuration info
        colModel: [
          { name: 'Project_Acc', // name of the column
            label: 'Project Acc', // the solr field mapped to this column
            width: 60, // width of column in px
            align: 'left', // how to align the column
            sortable: true // can it be sorted
          }, 
          { name: 'Organism_Name',
            label: 'Organism Name'
          }
        ],
      };
    }
  ]);
```

The JavaScript in this application configures the app, the controller, and components.  JavaScript is used for these purposes (instead of declarative programming) for flexibility and conciseness.  To give a simple example, a grid column can be configured with properties that are functions, such as a function for formatting the html in each grid element.

The overall object is an AngularJS module, which is collection of services, directives, controllers, filters, and configuration information.  The module is defined by `angular.module()`, where the first argument is the name of the app, `TestApp`, which is referred to in the HTML using the div attribute `ng-app`. The second argument is an array of AngularJS modules that this module is dependent on. For this module, the sole dependency is `ngramApp`, which is another module containing the DataDicer libraries.

We then define a controller for this application using the function `controller()`, part of the application module. Since the architecture is hierarchical model view controller, there can be multiple controllers, but we define only one in this tutorial, `gridController`.  The second argument to controller is a list of dependencies, which in this case is `$scope`, the application object.

Inside the controller we define two properties in the application object: the query used to retrieve the data from Solr, `$scope.query`, and the grid table configuration, `$scope.gridConfig`.

`$scope.query` contains the solr query `"*:*"` in the matching property.  This particular query matches all terms in all indices.

DataDicer has a flexible query architecture that allows you to query multiple back ends.  To tell which backend to use, `$scope.gridConfig` has the property `serviceEngine`, which, when set to "solr", tells DataDicer to connect directly to Solr using the url `serviceQueryUrl`. 

The rest of the properties in `$scope.gridConfig` directly configure the grid. `autoLoad` set to true means to load the grid automatically upon startup.  rownumbers set to true means show row numbers in the grid. `colModel` is an array that contains the per column configuration, one column per array element. In this case we have two columns, with names `Project_Acc` and `Organism_Name`. Each column has a human readable label. You can set the width of the column in pixels, how text is aligned in each table element, and whether the column is sortable.

That's it!  Our first DataDicer application.