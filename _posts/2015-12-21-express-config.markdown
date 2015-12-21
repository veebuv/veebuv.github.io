---
title:  "A little express at a time!"
date:   2015-11-21 10:18:00
description: Some settings that may interest you
---

So as we all know express has two sets of settings available:
1-System Settings
2-Custom Settings

Lets start of with a famous one:

**ENV**

This variable stores the current environment for a particular node.js process

Common values : development,test,stage,preview,production

We can augment the env setting by adding app.set('env', 'preview')

Knowing in what mode the application runs is very important because logic related to error handling, compilation of style sheets, and rendering of the templates can differ dramatically.

**VIEW CACHE**

This flag when set to false allows for painless development because the templates are reach each time a server requests for them. If however, the flag was set to true, it allows for template compilation caching - which in all honesty is desired in the case of production.

The convenience part is, view cache is set to true when NODE_ENV is set to 'production' YAAY

**VIEW ENGINE**

By doing

{% highlight javascript %}

app.set('view engine','ejs')

{% endhighlight %}

Node knows that the view templates in the view folder have an extension of ejs in the situation that the extention type isn't passed to res.render()

**VIEWS**

The views setting has an absolute path to a directory with all the templates. This setting defaults to the absolute path of the views folder in the project’s root. You don't need to always have the setting as views, you're allowed to name it whatever you want as long as it's consistent!

{% highlight javascript %}

app.set('view',express.static(path.join(__dirname + 'views')))

{% endhighlight %}

*__dirname* basically creates a reference path to the current directory that **this** particular JS file is in.

**TRUST PROXY**

Set this guy to true if your app is working behind reverse proxy such as Varnish. It'll allow trust in the X-Forwared-* headers such as X-Forwarded-Proto (req.protocol) or X-Forwarder-For (req.ips).

{% highlight javascript %}

app.set('trust proxy',true)
app.enable('trust proxy')

     enable and disable are another way of setting values to true or false NEAT!

{% endhighlight %}

**JSON REPLACER AND JSON SPACES**

We can spice up the widely used JSON.stringify(), which takes a JS object and converts it into a string format. Widely used in API storage, the opposite JSON.parse('json string in here')

Express sets replacer to null by default.

{% highlight javascript %}
    JSON.stringify(obj,null,2)
{% endhighlight %}

Onto replacer, it is a function that takes in a key value pair, and if we return undefined, it ignores that particular KV pair in the json string. Imagine you had some data and you wanted to filter out some sensitive information perhaps, this is your go to function.

Spaces, pretty much just takes care of indentation, i believe it's generally 2 for production.

{% highlight javascript %}

app.set('json replacer', function(key, value){
  if (key === 'discount')
    return undefined;
  else
    return value;
});
app.set('json spaces', 4);

{% endhighlight %}

**CASE SENSITIVE ROUTING**

Its set to false by default and ignores alphabet cases in the routes, however you can enable it/ set it to true so that /users and /Users are no longer the same

{% highlight javascript %}

  app.enable( 'case sensitive routing')

{% endhighlight %}

**STRICT ROUTING**

This deals with cases of trailing slashes in URLS, with the *strict routing* enabled by doing

By default, this parameter is set to false, which means that the trailing slash is ignored and those routes with a trailing slash will be treated the same as their counterparts without a trailing slash

{% highlight javascript %}

  app.enable('string routing')

{% endhighlight %}

**X POWERED BY**

This sets the response header X-Powered-By to Express, for security reasons, to prevent vulnerability by keeping things ambiguous, you can disable it!

**ETAG**
ETag is a caching tool. The way it works is akin to the unique identifier for the content on a given URL. In other words, if content doesn’t change on a specific URL, the ETag will remain the same and the browser will use the cache. By default Express uses 'weak' Etag.

Strong ETag guarantees the response is byte-for-byte the same, while an identical weak ETag indicates that the response is semantically the same.

** QUERY PARSER**

A query is data sent after the question mark in a URL for example, ?name=value&name2=value2). This format needs to be parsed into JavaScript/Node.js object format before we can use it.

By default it is set to true, but you could be go ahead an actually create your own function that does the parsing if you're not happy with the qs module's function!

***ENVIRONMENTS***

Some may know that applications run on many environments - production,testing,development. Each has a different requirement from the app. Easiest example, in development you want the console.logs for any errors to be vebose so we can track down errors fast however in production you'd want a generic or user friendly error that doesn't reveal alot about the system. In development you can show the stacktrace for the error while for production you can hide it!







[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
