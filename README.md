
## Getting Started
### Clone the repo
`$ git clone https://github.com/hyfn/toyota-mirai-admin.git`

### Install Dependencies
`$ npm install`
`$ bower install`

### Run the build system
`$ gulp`

##Tech Stack
* Build System
  * [node](http://nodejs.org)
  * [npm](http://npmjs.org)
  * [bower](http://bower.io)
  * [jade](http://jade-lang.com)
  * [gulp](http://gulpjs.com)
  * [browserify](http://browserify.org)
* Front End
  * [jQuery](http://jquery.com)
    * [metismenu](http://demo.onokumus.com/metisMenu/)
    * [DataTables](http://datatables.net)
    * [mockjax](https://github.com/jakerella/jquery-mockjax)
  * [bootstrap](http://getbootstrap.com)
    * [bootstrap-datepicker](https://github.com/eternicode/bootstrap-datepicker)
    * [bootstrap-validator](http://1000hz.github.io/bootstrap-validator)
  * [lodash](http://lodash.com)
  * [moment](http://momentjs.com)
  * [q](http://documentup.com/kriskowal/q/)
  * [mustache](https://github.com/janl/mustache.js)
  * [animate.css](http://daneden.github.io/animate.css/)
  * [font-awesome](http://fortawesome.github.io/Font-Awesome/)
  * [toastr](http://codeseven.github.io/toastr/)
  * [Inspinia Admin Template](https://wrapbootstrap.com/theme/inspinia-responsive-admin-theme-WB0R5L90S)

## Repo Structure
```
.
├── app //this is where the js, scss, and jade files live
│   │
│   ├── assets // copied to /public on build
│   │   │
│   │   ├── fonts
│   │   │
│   │   ├── images
│   │   │
│   │   └── patterns
│   │
│   ├── css // compiled sass output after build
│   │
│   ├── scripts // the application js
│   │   │
│   │   ├── components
│   │   │
│   │   ├── services
│   │   │
│   │   └── utils
│   │
│   ├── scss // sass files
│   │
│   └── views // jade files
│       │
│       ├── components
│       │
│       └── templates
│
├── compiled // compiled component templates
│
├── fixtures // mock data
│
├── public // the compiled app
│   │
│   ├── css // compiled and concatenated css
│   │
│   ├── fonts // copied from app/assets
│   │
│   ├── images // copied from app/assets
│   │
│   ├── js // bundled application js
│   │
│   └── patterns // copied from app/assets
│
└── vendor //scripts and styles that aren't in bower
    │
    ├── scripts // will be bundled on build
    │
    └── styles // will be bundled on build

```

## Gulp Tasks
The following tasks can be executed by running the following command in terminal:

`$ gulp <task>`


* `css`
  * Compiles sass files to css, and concatenates bower dependency css files and custom css files into a single *styles.css* file which is placed in */public/css/*
* `jshint`
  * runs jshint on all javascript files in the */app* directory and logs warnings and errors to the console
* `browserify`
  * runs `jshint` and then bundles the application files while maintaining dependencies and places the bundled javascript in the *public/js* directory
* `templates`
  * Builds static html from jade files, including piping in demo data where necessary, and places them in the *public/* directory. 
* `compile-templates`
  * builds all static html from jade and moves it into the `compiled/` directory which will contain a hierarchical file structure with all modules and templates available for easy browsing.
* `bower-assets`
  * copies required image and font assets from bower dependencies to public
* `assets`
  * runs `bower-assets` and then copies files from `app/assets` to the `public/` directory
* `refresh-data`
  * run to refresh fixtures.
* `clean`
  * Cleans the output directories
* `watch`
  * runs `browserify`, `templates`, `css`, and `assets` and then starts a browsersync instance; also watches for file changes and rebuilds & refreshes the browser when necessary
* `build`
  * runs `clean`, `browserify`, `templates`, `css`, and `assets`

##UI Components
Each major UI component of the site is exported to a compiled html file placed in the **compiled/components** directory. These files are to be used as guidelines to further develop the site as needed. Some minor components are not compiled to their own component.

### Component List
#### Examples
* [Info Widget](compiled/examples/info-widget.html)
* [Modal](compiled/examples/modal.html)
* [Tool](compiled/examples/tool.html)

#### Full Monty
* [Basic Save Modal](compiled/components/basic-save-modal.html)
* [Customer Case History](compiled/components/customer-case-history.html)
* [Customer Header](compiled/components/customer-header.html)
* [Customer Profile](compiled/components/customer-profile.html)
* [Customer Survey Data](compiled/components/customer-survey-data.html)
* [Modal Approve Configuration Change Request](compiled/components/modal-approve-configuration-change-request.html)
* [Modal Approve Dealer Change Request](compiled/components/modal-approve-dealer-change-request.html)
* [Modal Business Partner Place On Hold](compiled/components/modal-business-partner-place-on-hold.html)
* [Modal Case History Add Note](compiled/components/modal-case-history-add-note.html)
* [Modal Complete Qual Score](compiled/components/modal-complete-qual-score.html)
* [Modal Confirm Submission](compiled/components/modal-confirm-submission.html)
* [Modal Handle Configuration Change Request](compiled/components/modal-handle-configuration-change-request.html)
* [Modal Review Qual Score](compiled/components/modal-review-qual-score.html)
* [Modal Submit Order](compiled/components/modal-submit-order.html)
* [Request List Toolbox](compiled/components/request-list-toolbox.html)
* [Requests Tasks To Complete](compiled/components/requests-tasks-to-complete.html)
 
## Application Configuration
For the most part, the application can be configured using a mixture of semantic markup for static components and manipulating a non-compiled javascript file.

### [app-config.js](app/assets/app-config.js)
The file is located in **app/assets/app-config.js** and is copied to **public/assets/app-config.js** on compile. This file allows you to make many adjustments to the application without needing to recompile the entire code base.

You have access to all global methods and variables as identified in `main.js` in this file. This means that libraries such as jQuery, lodash, moment, etc can be used when needed.

####A Simple Example:

```
// app-config.js
function appConfig() {
  var apiBase = 'api';
  return {
    environment: 'development',
    apiBase: apiBase,
    views: {
      requests: {
        customerAction: {
          route: '/customer-details-business-partner.html',
          queryParams:[
            { parameter: 'id', property: 'customerID' }
          ]
        },
        table: {
          endpoint: 'order-requests',
          searchFields: [
            {field: 'firstName', label: 'First Name' },
          ],
          columns: [ {
            title: 'Survey Date',
            data: 'surveyDate',
            className: 'text-center small',
            render: function(data, type, full, meta) {
              return moment(data).format('MM.DD.YYYY hh:mm A');
            }
          }]
        },
        tasks: [
            {
              title: 'Score Qualitative Answers',
              id: 'score-qualitative-answers',
              filter: {
                status: ['Business Partner Review'],
                salesArea: ['California']
              }, 
              roles: ['Business Partner']
            }
        ],
      },
      customerDetails: {
        calculateTotalScore: function(quantScore, qualScore) {
          switch(qualScore) {
            case 'LOW':
              return 10;
            case 'MEDIUM':
              return 30;
            case 'HIGH':
              return 50;
          }
        }
      }
    },
    mocks: [
      {
        endpoint: path.join(apiBase,'*'),
        type: 'POST',
        dataset: 'empty',
        responseTime:  1500,
        mockResponse: function(request, mockData) {
          return {status: 200, data:mockData};
        }
      },
    ],
  };
}
```

### Properties & Methods
`apiBase`
The base route you will use for your api. 

`environment`
If set to 'development', the app will use the mock api instead of sending real requests.

`views`  
An object containing configuration for each over-arching view in the app.

`views.requests`
configuration for the requests view

#### customerAction
Indicates the expected behavior when a user clicks on a customer in the requests table.

#####`route`  
the url to forward the user to

#####`queryParams`  
A list of query params and data properties to append to the url

* `param`: the parameter name
* `value`: the property of the customer object to set the parameter to
* *Example:*  Assume that the customer object contains the following properties:  
```
{
  firstName: 'John',
  lastName: 'Doe',
  customerId: 1024510,
  isAdmin: false
}
```
and we configured the action thusly:
```
customerAction: {
      route: '/dashboard.html',
      queryParams:[
        { parameter: 'id', property: 'customerID' },
        { parameter: 'admin', property: 'isAdmin'}
      ]
    }
```
The route that the user would be forwarded to would be:  
`/dashboard.html?id=1024510&admin=false`

####table  
Configuration options for the requests table and dependent interactions

* `endpoint`: The api endpoint used to load the table data from. **NOTE:** This will be concatenated with `apiBase` to create the full url.
* `searchFields`: Configuration options for the search box drop down
  * `field`: The property on customer that you wish to search for
  * `label`: The display in the select dropdown
* `columns`: Allows configuration of which data points will be shown in the table. Please refer to the DataTables API for more information.
* `tasks`
Allows configuration of Tasks To Complete
  * `title`: The displayed title on the button
  * `id`: Must be unique
  * `filter`: See [Filters](#filters)

##### `views.customerDetails`
Configuration for the customerDetails view

* `calculateTotalScore`: Called from the app when a user chooses a new qualitative score from the qualitative score widget. The customer's `quantitativeScore` and the selected `qualitativeScore` are passed into it. 

#### mocks
Used to simulate a backend api if `environment` is set to 'development'

### Semantic Markup
Each component has different data attributes that tell the application what to do when interacted with.
 
#### Examples
#####[Modal](compiled/examples/modal.html)
```

<div id="basic-submission-modal" role="dialog" class="modal inmodal" data-action="api-endpoint">
  <div class="modal-dialog">
    <div class="modal-content animated bounceInDown">
      <div class="modal-body">
        <form class="form">
          <div class="form-group">
            <textarea type="text" placeholder="Case Note Comments" name="caseNoteComments" rows="3" class="form-control"></textarea>
          </div>
          <div class="form-group">
            <textarea type="text" placeholder="Comments for the Dealer" name="dealerComments" rows="3" class="form-control"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="submit" class="btn btn-primary text-uppercase">Save</button>
        <button data-dismiss="modal" class="btn btn-white text-uppercase">Cancel</button>
      </div>
    </div>
  </div>
</div>
```
Modals will use the `data-action` attribute to determine which route to POST data too on save. **NOTE:** This action represents an endpoint and will be concatenated with `config.apiBase`.

#####[Tool](compiled/tool.html)

```
<div class="panel panel-default tool" data-field="foo">
  <div class="panel-heading filter-name border-bottom">
    <h4 class="panel-title"><a data-toggle="collapse" href="#tool-my-tool" class="collapsed">My Tool<i class="fa fa-chevron-right"></i></a></h4>
  </div>
  <div id="tool-my-tool" class="panel-collapse collapse">
    <ul class="list-group">
      <li class="list-group-item">
        <div class="checkbox">
          <label>
            <input type="checkbox" value="0"/><span>option 1</span><span class="custom_check glyphicon glyphicon-ok"></span>
          </label>
        </div>
      </li>
      <li class="list-group-item">
        <div class="checkbox">
          <label>
            <input type="checkbox" value="1"/><span>option 2</span><span class="custom_check glyphicon glyphicon-ok"></span>
          </label>
        </div>
      </li>
      <li class="list-group-item">
        <div class="checkbox">
          <label>
            <input type="checkbox" value="2"/><span>option 3</span><span class="custom_check glyphicon glyphicon-ok"></span>
          </label>
        </div>
      </li>
    </ul>
  </div>
</div>
```
Tools are used to mark filters on the Requests page. They use the `data-field` attribute to tell the application which field to filter on the customer model.


##API
###Request
####DataTables & Filters
The requests that are being sent to the API to request data for the Pre-Order Requests view, follows the documented DataTables requests with two exceptions:

1. **Filters** (see discussion below)
2. **Search**
Instead of doing an openly comparative search, I'm actually passing search in as a filter, so that we can search on a given field instead of the entire record.

#####Example Request Payload
```
{
  "start": 0,
  "length": 1,
  "order": [
    {
      "column": 0,
      "dir": "asc"
    }
  ],
  "columns": [
    {
      "data": "id"
    }
  ],
  "filters": {
    "status": [
      "Expired"
    ],
    "salesArea": [
      "California"
    ]
  }
}
```

###Response
The response must conform to the DataTables API protocol. 
####Example Response
```
{
  recordsTotal: 1000, 
  recordsFiltered: 44, 
  data: Array[..]
}
```
##Filters
Filters are passed to the server when the requests table needs to load new data.

Their properties are related to an actual field on the customer object. Each property's value is an array of values to **OR** the data against when comparing. 

### Example Filter
```
{
    status: ['Business Partner Review'],
    salesArea: ['California'],
    eligibility: ['eligible']
}
```
This filter let's the API know that we want all `eligible` customers with a status of `Business Partner Review`, in the `California` sales area. 


##Routes
To start a route, a script tag must be placed in the body like so:

```
<script type="text/javascript">
  window.onload = function() { 
    startRoute('workbench-vips'); 
  }
</script>
```
This will let the app know which view controller you want to use 

### Currently Available Routes

* `'requests'` - PreOrder Request List View
* `'customer-details'` - Customer Details View
* `'manage-vips'` - Manage VIPs View

### Creating a new route
Simply add the route and controller to main.js and including the logic in the `startRoute()` function definition



> Written with [StackEdit](https://stackedit.io/).
