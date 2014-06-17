---
layout: post
title: Getting Started with Grunt - Part 3
---


<div class="message">
With Jinglei's help, I started my journey learning Grunt.js recently. I learned a lot from him and this blog series is a record of what was learned. This is the Part 3 of the series.
</div>

In this part, we are going to combine what we did in the first two parts, and build up a front-end of a simple website.

In order to keep the code easily maintainable and well-organized, we would put all the source code in a 'src' folder, and separating the views and stylesheets. For the views, we are going to use a template engine, so the code would be more clean and tidy, and we will use HAML here. For the stylesheets, we are going to use CSS preprocessing, for the same reasons, and we will use SASS in this case.

The initial folder structure:
{% highlight %}
  /
  --src/
  ----stylesheets/
  ----views/
{% endhighlight %}

Firstly, lets install the dependencies in.
{% highlight js %}
npm install grunt grunt-cli grunt-contrib-haml grunt-contrib-sass grunt-contrib-watch load-grunt-tasks --save-dev
{% endhighlight %}
As a reminder, please also make sure you have Ruby, HAML, SASS installed for HAML and SASS compiling.

Next up, let's get our HAML and SASS files ready in their own folders, so the current folder structure becomes:
{% highlight %}
  /
  --src/
  ----stylesheets/
  ------_general.scss
  ------_content.scss
  ------style.scss
  ----views/
  ------index.haml
  --package.json
{% endhighlight %}

To be Continued...
