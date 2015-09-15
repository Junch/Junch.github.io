---
layout: post
title: "$resource post到错误的url"
summary: "正确设置$resource的参数paramDefaults, 否则url可能解析错误"
description: ""
tags: ["angular", ""]
categories: ["技巧"]
---
今天试图调整某个REST api的调用方式，修改属性采用post而不是put，结果遇到问题，卡壳了。

本来是更新一个对象，结果变成创建一个对象，搞得我莫名其妙。

$resource以前是这样声明和使用的:

{% highlight javascript %}

angular.module('winesStore')
.constant('dataUrl', '/wines/:id')
.factory('wineFactory', function ($resource, dataUrl) {
    return $resource(dataUrl, {id: '@id'}, {
        update: {method: 'PUT', cache: false, isArray: false}
    });
});

// In the controller the service is used as below
var UpdateWine = function (wine) {
    wineFactory.update({id: wine._id}, wine,
        function success(data) {
            refresh();
        },
        function (error) {
            $scope.data.error = error;
        });
};

{% endhighlight %}

我更改成下面所示，UpdateWine就无法正常工作了。wineFactory.save没有post到正确的url。

{% highlight javascript %}

angular.module('winesStore')
.constant('dataUrl', '/wines/:id')
.factory('wineFactory', function ($resource, dataUrl) {
    return $resource(dataUrl, {id: '@id'});
});

// In the controller the service is used as below
var UpdateWine = function (wine) {
    wineFactory.save(wine,
        function success(data) {
            refresh();
        },
        function (error) {
            $scope.data.error = error;
        });
};

{% endhighlight %}

原因其实不复杂。看看文档[AngularJS Documentation for $resource](https://docs.angularjs.org/api/ngResource/service/$resource)。

***If the parameter value is prefixed with @ then the value for that parameter will be extracted from the corresponding property on the data object (provided when calling an action method). For example, if the defaultParam object is {someParam: '@someProp'} then the value of someParam will be data.someProp.***

wine是从mongodb中查询获取的，包含一个`_id`的属性而不是`id`属性。在调用save这个方法的时候，由于没有找到`id`属性，$resource会post对象到url: /wines。后台就解析为添加一个对象，而不是修改一个对象。

修改也不复杂，将{id: '@id'}修改为{id: '@_id'}。 具体修改查看[Use the POST method instead of PUT method to update object](https://github.com/Junch/Nodejs/commit/98c93906ab5f78f09af98b443a4487be119bb818)。
