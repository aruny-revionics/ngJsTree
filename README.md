[![Bower version](https://badge.fury.io/bo/ng-js-tree.svg)](http://badge.fury.io/bo/ng-js-tree)
[![Build Status](https://travis-ci.org/ezraroi/ngJsTree.svg?branch=master)](https://travis-ci.org/ezraroi/ngJsTree)
[![Dependency Status](https://gemnasium.com/ezraroi/ngJsTree.svg)](https://gemnasium.com/ezraroi/ngJsTree)
[![Coverage Status](https://img.shields.io/coveralls/ezraroi/ngJsTree.svg)](https://coveralls.io/r/ezraroi/ngJsTree?branch=master)
[![Code Climate](https://codeclimate.com/github/ezraroi/ngJsTree/badges/gpa.svg)](https://codeclimate.com/github/ezraroi/ngJsTree)
[![Built with Grunt](https://cdn.gruntjs.com/builtwith.png)](http://gruntjs.com/)
[![NPM](https://nodei.co/npm/ng-js-tree.png?mini=true)](https://npmjs.org/package/ng-js-tree)

Enhancement into ngJsTree
=========================

Angular Directive for the famous [JS Tree] library.


##Dependencies


The ngJsTree depends on the following libraries:
* Angular
* JsTree


##Install


You can install the ngJsTree with bower:

```bat
bower install ng-js-tree --save
```

or with npm:
```bat
npm install ng-js-tree --save
```


or you can add the ngJsTree.min.js file to your HTML page:
```html
<script src="jquery.js"/>
<script src="angular.js"/>
<script src="jstree.min.js"/>
<script src="ngJsTree.min.js"/>
```

Add the `ngJsTree` to your module dependencies


#Documentation


You can find the JSTree documentation at [this link]

## Usage

```html
<div js-tree="treeConfig" ng-model="treeData" should-apply="ignoreModelChanges()" tree="treeInstance" tree-events="ready:readyCB;create_node:createNodeCB"></div>
```

* `treeConfig` - This is the configuration object of the JsTree, if you will not supply one, an empty one will be created (not mandatory).
* `treeData` - The array with the elements of the tree, will be used for data binding (adding / removing / updating this data will be reflected in the tree).
* `ignoreModelChanges()` - A method that returns true or false. when returning false, model changes will not be reflected in the tree (not mandatory).
* `treeInstance` - The Js Tree instance will be assigned to this variable in your controller scope (not mandatory).
* `ready:readyCB;create_node:createNodeCB` - List of Js Tree events and callbacks in your controller scope that will be called for each event (not mandatory.

##ngJSTree as DropDown

```html
 <div class="input-group" data-toggle="dropdown">
                            <input type="text"
                                   ng-readonly="true"
                                   class="form-control"
                                   ng-model="vm.selectedDisplayText">

                            <div class="input-group-btn hierarchydropdown-select">
                                <button type="button"
                                        style="height:33px;"
                                        class="btn btn-default dropdown-toggle"
                                        data-toggle="dropdown"
                                        aria-haspopup="true"
                                        aria-expanded="false">

                                    <span class="caret"></span>

                                </button>

                                <ul class="dropdown-menu scrollable-dropdown-menu" cg-busy="promise">
                                    <!--stopPropagation to stop toggle event of dropdown option-->
                                    <li ng-click="$event.stopPropagation()">
                                        <div js-tree="vm.treeConfig"
                                             should-apply="vm.applyModelChanges()"
                                             ng-model="vm.treeData"
                                             selected-node="vm.selectedNode"
                                             selected-display-text="vm.selectedDisplayText"
                                             tree="vm.treeInstance"
                                             tree-events="ready:vm.readyCB;create_node:vm.createCB"></div>
                                    </li>
                                </ul>
                            </div><!-- /btn-group -->

                        </div>
```
* `selectedNode` - This will contains selected node ID
* `selectedDisplayText` - This will store the selected node text.


### Registering for events
You can register a callback for any Js Tree event in the following way:
* add the  `tree-events` attribute and specify the name of the events to register for and a callback for each event.

Example:
```html
<div ng-controller='myCtrl'>
    <div js-tree="treeConfig" ng-model="treeData" should-apply="ignoreModelChanges()" tree="treeInstance" tree-events="ready:readyCB;create_node:createNodeCB"></div>
</div>
```

```javascript
angular.module('myApp').controller('myCtrl', function($scope,$log) {
    $scope.readyCB = function() {
        $log.info('ready called');
    };
    
    $scope.createNodeCB = function(e,item) {
        $log.info('create_node called');
    };
);
```

### Using the Js Tree API from your controller
Add the tree attribute to the jstree directive and assign it with a name of a variable in your controller that will hold the jstree instance.
```html
<div ng-controller='myCtrl'>
    <div js-tree="treeConfig" ng-model="treeData" tree="treeInstance"></div>
</div>
```

```javascript
function yourCtrl($scope)  {
    var selected_nodes = $scope.treeInstance.jstree(true).get_selected();
}
```

### Recreating the Tree
If from some reason you would like to recreate the tree, the right way to do it is update the tree configuration object. Once the directive will detect a change to the tree configuration it will destory the tree and recreate it. 
```javascript
this.treeConfig = {
    core : {
        multiple : false,
        animation: true,
        error : function(error) {
            $log.error('treeCtrl: error from js tree - ' + angular.toJson(error));
        },
        check_callback : true,
        worker : true
    },
    version : 1
};
this.reCreateTree = function() {
    this.treeConfig.version++;
}
```
* The reason i am using the version property is because it is not a JsTree config property, so it will not effect the tree.

## Development
#### Prepare your environment

* Install [Node.js](http://nodejs.org/) and NPM (should come with)
* Install global dev dependencies: `npm install -g grunt-cli karma`
* Install local dev dependencies: `npm install` while current directory is ngJsTree
* Install javascript dependencies: `bower install` while current directory is ngJsTree

#### Build
* Build the whole project: `grunt` - this will run `jshint` and `test` and will build the project


#### TDD
* Run test: `grunt watch`

License
----

MIT

[JS Tree]:http://www.jstree.com/
[this link]:http://www.jstree.com/api/
