---
title:  "Cache me out"
date:   2016-01-214 10:18:00
description: what is is caching
---

**Here's whats happening**

* By default Views in Ionic are cached with a purpose of improving performance.

* When you navigate away from a view, the element still exists in the DOM, it's controller(scope) is disconnected from thr $watch cycle
 * Watch cycle in angular checks for changes in the scope of a view and its controller and initiates view changes/controller changes accordingly
* So when you navigate to a view that is already cached, the controller(scope) simply reconnects the watch cycle and the view element that was left in the DOM becomes the 'active' view
* This has a few advantages, such as the previous scroll position being maintained, so when you click on a link on facebook and go back, caching ensures you're back where you started and not all the way at the top of the news feed

**What about controllers?**

* Here's the deal, when we 'activate' a cached view, we're not creating a new controller like the first time, we're just reattaching it to the view template/element and the watch digest
* Important to note here , this means the code within the controller WON'T be executed when we enter a cached view again
* And this can be a pain point for many as you'd expect the code within the controller body to be executed each time

{% highlight javascript %}

app.controller('myApp', function($scope) {

// Code placed here will only be run once, even if the view is loaded again!
});

{% endhighlight %}

* However sometimes you want your stuff/code to be run each time a controller's view is activated, even though this view has been loaded from a cache, to do this, we can use view lifecycle events (if you've done react, you'd conceptually be aware of this)
* [ionic-view](http://ionicframework.com/docs/api/directive/ionView/) has these documented well and what we'd listen for is the $ionicView.enter event

{% highlight javascript %}

app.controller('Controller', function($scope) {

 $scope.$on('$ionicView.enter', function() {

   // Place logic you want to run on load here

 });
});

{% endhighlight %}

**I'm sure you'd want to be able to enable and disable caching**

* Caching can be turned on and off in several ways
* Ionic in default allows for 10 views, at max, to be cached - and by the way this can be configured :o !
* Not only so but apps can also explicitly state which views can be cached and which shouldn't

**Disable caching globally**

* The $ionicConfigProvider can be used to set the maximum allowable views which can be cached, but this can also be used to disable all caching by setting it to 0.

{% highlight javascript %}

app.config(function($ionicConfigProvider) {

$ionicConfigProvider.views.maxCache(0);

});

{% endhighlight %}

**State specific disabling**

{% highlight javascript %}


$stateProvider.state('state',{

cache : false,
url : '/url',
templateUrl : 'template.html'

})


{% endhighlight %}



[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
