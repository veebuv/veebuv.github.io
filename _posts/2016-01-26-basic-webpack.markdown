---
title:  "WebPack "
description: How to webpack with babel
---

**Alright lets get started**

Webpack basically allows us to modularize our front end code - the browser isn't "require" enabled, so you need to set a intermediary to account for that.

I'm going to try throw out some code and explain why and what each thing does, hopefully that helps you out.

So we need some basic modules set up to get our auto refreshing server along with our ES6 transpilers and JSX transpilers

{% highlight javascript %}

$ npm install webpack-dev-server -g
$ npm install webpack-dev-server webpack babel-core babel-loader babel-preset-es2015 babel-polyfill --save-dev

{% endhighlight %}

Why these specific modules, well they were actually all included by default in babel5 but that's changed for babel6

{% highlight javascript %}

# For ES6/ES2015 support
npm install babel-preset-es2015 --save-dev

# If you want to use JSX
npm install babel-preset-react --save-dev

# If you want to use experimental ES7 features
npm install babel-preset-stage-0 --save-dev

{% endhighlight %}

That's about it, now to run our webpack-dev-server ( why this and not like a normal host ? this has yummy reloading )

{% highlight javascript %}

$ webpack-dev-server --content-base ./ ./main.js

{% endhighlight %}

When we run the above command it takes the path to our HTML files which you can imagine is in the root folder and our JS entry point ( where all our code originates or talks to our html -> in react terms where the main App element gets rendered onto the DOM )

**Lets configure the webpack.config.js**

Even though everything is installed WebPack still needs to understand ES6 and we need to provide instructions on how to do so.

Below is some boiler plate code that we can go over :

{% highlight javascript %}

module.exports = {
  entry: [
    './src/index.js'
  ],
  output: {
    path: __dirname,
    publicPath: '/',
    filename: 'bundle.js'
  },

  //Exclude -> Skip any files inside of your project's `node_modules` directory

  module: {
    loaders: [{
      exclude: /node_modules/,
      loader: 'babel'
    }]
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  devServer: {
    historyApiFallback: true,
    contentBase: './'
  }
};


{% endhighlight %}

*Here's the run through*

- entry is similar to what we discussed, its the startup file, in this case i'm running index.js which is in the src folder
    Index.js is where im rendering my React App component to my DOM
- output tells webpack-dev-server or webpack (if you just do "webpack -w") where to serve all the compiled files to. You tell it to build a bundle.js right onto the server's root rather than host it up to the client.
- module.loaders are nothing but a list of loaders or npm packages that permit webpack to convert the source file.
- You could add a *include : (__dirname + './')* to basically load all the files with *. , .js or .jsx* extensions through babel, essentially converting them to ES5
    In this case you need to only do that to bundle.js as that's the file that contains all the condensed information from all the different components of your application

On a seperate file in the same directory path include a .babelrc file that will host your presets :

{% highlight javascript %}
{
  "presets": ["react", "es2015"]
}
{% endhighlight %}

- What the presets do is provide configurations to babel to use for transformations - like a guideline

- The code below lets the webpack-derv server know where your content's base is located, so you can ignore the *--content-base src* when you try run ur dev server in the terminal

{% highlight javascript %}

devServer: {
    contentBase: "./src"
  }
{% endhighlight %}

**All hail the package.json**

To avoid a lot of the typing each time, you can go ahead an include all the dependencies in a package.json file and just do * npm install *

Within the start script you can traverse to the webpack dev server and use that to run your dev server, this picks up the content to be targeted at through the package.json file

*Heres my package.json* :

{% highlight javascript %}

{
  "name": "WebPack",
  "version": "1.0.0",
  "description": "Simple starter package for Redux with React and Babel support",
  "main": "index.js",
  "scripts": {
    "start": "node ./node_modules/webpack-dev-server/bin/webpack-dev-server.js"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.2.1",
    "babel-loader": "^6.2.0",
    "babel-preset-es2015": "^6.1.18",
    "babel-preset-react": "^6.1.18",
    "webpack": "^1.12.9",
    "webpack-dev-server": "^1.14.0"
  },
  "dependencies": {
    "babel-preset-stage-1": "^6.1.18",
    "lodash": "^3.10.1",
    "react": "^0.14.3",
    "react-dom": "^0.14.3",
    "react-redux": "^4.0.0",
    "redux": "^3.0.4",
    "youtube-api-search": "0.0.5"
  }
}

{% endhighlight %}


[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
