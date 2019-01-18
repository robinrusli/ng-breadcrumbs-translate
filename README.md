# [ng-breadcrumbs-translate](http://ianwalter.github.io/ng-breadcrumbs/)
*A better AngularJS service to help with breadcrumb-style navigation between views. And provide translation for the breadcrumbs out of the box with [Angular Translate](https://github.com/angular-translate/angular-translate)!*

[![Code Climate](https://codeclimate.com/github/ianwalter/ng-breadcrumbs.png)](https://codeclimate.com/github/ianwalter/ng-breadcrumbs)

This project was built using [ng-boilerplate](https://github.com/ianwalter/ng-boilerplate)!

#### Step 1: Install ng-breadcrumbs

Install using Bower:

```
bower install ng-breadcrumbs-translate --save
```

Include ng-breadcrumbs-translate.min.js in your app.

#### Step 2: Set up routing

In order to use breadcrumbs you'll need to use configure your app to use Angular's routeProvider. You'll also need to 
load the ng-breadcrumbs module. You can then set a label for each route (breadcrumb) within the route options.

```javascript
  var app = angular.module('ab', ['ngRoute', 'ng-breadcrumbs', 'pascalprecht.translate'])
    .config(['$routeProvider', function($routeProvider) {
      $routeProvider
        .when('/', { templateUrl: 'assets/template/home.html', label: 'Home' })
        .when('/stock/:stock', { controller: 'StockController', templateUrl: 'assets/template/stock.html' })
        .when('/stock/:stock/detail', {
          controller: 'StockDetailController',
          templateUrl: 'assets/template/stock-detail.html',
          label: 'More Detail'
        })
        .otherwise({ redirectTo: '/' });
```


#### Step 3: Make the breadcrumbs service available to your controller

Set the breadcrumbs service in your app's main controller.

```javascript
  app.controller('HomeController', [
    '$scope',
    'breadcrumbs',
    function($scope, breadcrumbs) {
      $scope.breadcrumbs = breadcrumbs;
      ...
```


#### Step 4: Display the breadcrumbs within your app

This HTML snippet will display your breadcrumb navigation and leave the last breadcrumb (the page you're currently on)
unlinked.

```html
  <ol class="ab-nav breadcrumb">
    <li ng-repeat="breadcrumb in breadcrumbs.get() track by breadcrumb.path" ng-class="{ active: $last }">
      <a ng-if="!$last" ng-href="#{{ breadcrumb.path }}" ng-bind="breadcrumb.label" class="margin-right-xs"></a>
      <span ng-if="$last" ng-bind="breadcrumb.label"></span>
    </li>
  </ol>
```

#### Step 5: Setup translation for the labels

The translation key have to have the same as the label defined in your route

```html
  var app = angular.module('ab', ['ngRoute', 'ng-breadcrumbs', 'pascalprecht.translate'])
    .config(function ($translateProvider) {

      $translateProvider.translations('pt', {
        'Home', 'Início',
        'More Detail': 'Mais detalhes'
      });
    });

```

That's it! You should now have breadcrumb navigation that can even handle nested routes.


#### Adding dynamic route labels

To add dynamic route labels, create an options object on the breadcrumbs service or pass one as a parameter within
```breadcrumbs.get()```, for example:

```javascript
// Will replace the default label 'Stock Detail' with the dynamic label 'AAPL Details'
breadcrumbs.options = { 'Stock Detail': $routeParams.stock + ' Details' };
```

I hope you find this useful!

«–– [Lucas](http://github.com/lukax)
«–– [Ian](http://ianvonwalter.com)
