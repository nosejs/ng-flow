ng-flow是什么?
============

Flow.js extensions for angular.js framework, no 3rd party JS dependencies required!

ng-flow extension is based on [Flow.js](https://github.com/flowjs/flow.js) library.

Demo: http://flowjs.github.io/ng-flow/

我如何安装它?
============
1) 获取库:

**Direct Download**
Download a latest build from https://github.com/flowjs/ng-flow/releases
it contains development and minified production files in `dist/` directory,
they are also concatenated with core flow.js library.

**Using Bower**
        
        bower install ng-flow#~2
                
**Git Clone**
        
        git clone https://github.com/flowjs/ng-flow
        
**Using Yeoman**

        bower install "ng-flow#~2" --save
        grunt bower-install
                
2) Add the module to your app as a dependency:

        angular.module('app', ['flow'])
        
我如何使用它?
============

First of all wrap places there you are going to use Flow.js
````html
<div flow-init>
    ... other flow directives goes here ...
</div>
````

This directive is going to add $flow variable to current scope.
Also directive can be nested, because `$flow` variable is going to be overridden.
`$flow` is instance of [Flow](https://github.com/flowjs/flow.js#flow).


Secondly you need to assign some upload buttons:
````html
<input type="file" flow-btn />
<input type="file" flow-btn flow-directory />
````

First button is for normal uploads and second is for directory uploads.


Now you need to display uploaded files, all you need to do is to loop files array.
Files array is attached to flow object named `$flow`.
````html
<tr ng-repeat="file in $flow.files">
    <td>{{$index+1}}</td>
    <td>{{file.name}}</td>
</tr>
````

file is instance of [FlowFile](https://github.com/flowjs/flow.js#flowfile).


我如何放置文件?
============

Use `flow-drop` directive:
````html
<div class="alert" flow-drop>
    Drag And Drop your file here
</div>
````

### Prevent dropping files on a document
Use `flow-prevent-drop` directive on `body` element:
````html
<body flow-prevent-drop>

</body>
````

### How to add some styles while dropping a file?
Use `flow-drag-enter` directive:
````html
<div flow-drag-enter="style={border:'4px solid green'}" flow-drag-leave="style={}"
     ng-style="style">
</div>
````
Note: `flow-drag-leave` attribute can't be used alone, it is a part of `flow-drag-enter` directive.

### How to dynamically disable drop area?
````html
<div class="alert" flow-drop flow-drop-enabled="config.enabled">
    Drag And Drop your file here
</div>
````
See example at `samples/dataurl/`.


如何预览上传的图片?
============

Use flow-img directive:
````html
<img flow-img="$flow.files[0]" />
````

Image will be automatically updated once file is added. No need to start upload.


如何为flow.js设置选项?
============

Use config:
````javascript
var app = angular.module('app', ['flow'])
.config(['flowFactoryProvider', function (flowFactoryProvider) {
    flowFactoryProvider.defaults = {
        target: '/upload',
        permanentErrors:[404, 500, 501],
        minFileSize: 0
    };
    // You can also set default events:
    flowFactoryProvider.on('catchAll', function (event) {
      ...
    });
    // Can be used with different implementations of Flow.js
    // flowFactoryProvider.factory = fustyFlowFactory;
}]);
````

also can be configured on "flow-init" directive:
````html
<div flow-init="{target:'/uploader'}">

</div>
````


如何获取事件?
============

Events are listed inside `flow-init` directive:
````html
<div flow-init
      flow-file-success=" ... properties '$file', '$message' can be accessed ... "
      flow-file-progress=" ... property '$file' can be accessed ... "
      flow-file-added=" ... properties '$file', '$event' can be accessed ... "
      flow-files-added=" ... properties '$files', '$event' can be accessed ... "
      flow-files-submitted=" ... properties '$files', '$event' can be accessed ... "
      flow-file-retry=" ... property '$file' can be accessed ... "
      flow-file-error=" ... properties '$file', '$message' can be accessed ... "
      flow-error=" ... properties '$file', '$message' can be accessed ... "
      flow-complete=" ... "
      flow-upload-started=" ... "
      flow-progress=" ... "
      >
      <div flow-file-progress=" ... events can be also assigned inside flow-init ... "></div>

</div>
````

### How can I catch an event in a controller?
If controller is on the same scope as `flow-init` directive or in a child scope,
then we can catch events with `$on`. Events are prefixed with `flow::`.
````javascript
$scope.$on('flow::fileAdded', function (event, $flow, flowFile) {
  event.preventDefault();//prevent file from uploading
});
````
second argument is always a `flow` instance and then follows event specific arguments.

How can I assign flow to a parent scope?
============

Use `flow-name` attribute and set it to any variable in the scope.
````html
<div flow-init flow-name="obj.flow">
    ... Flow is set to obj.flow  ...
    I have uploaded files: #{{obj.flow.files.length}}
</div>
````

如何支持老旧浏览器?
============
Go to https://github.com/flowjs/fusty-flow.js
and add to your config:
````javascript
var app = angular.module('app', ['flow'])
.config(['flowFactoryProvider', function (flowFactoryProvider) {
    flowFactoryProvider.factory = fustyFlowFactory;
}]);
````

需要实例吗?
============
Clone this repository and go to "ng-flow/samples/basic/index.html".
Single image upload "ng-flow/samples/image/index.html".


贡献
============
To ensure consistency throughout the source code, keep these rules in mind as you are working:

* All features or bug fixes must be tested by one or more specs.

* With the exceptions listed below, we follow the rules contained in [Google's JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml):

  * Wrap all code at 100 characters.

  * Instead of complex inheritance hierarchies, we prefer simple objects. We use prototypical
inheritance only when absolutely necessary.

