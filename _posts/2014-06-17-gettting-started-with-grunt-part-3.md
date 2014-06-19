---
layout: post
title: Getting Started with Grunt - Part 3
---


<div class="message">
With Jinglei's help, I started my journey learning Grunt.js recently. I learned a lot from him and this blog series is a record of what was learned. This is the Part 3 of the series.
</div>

In this part, we are going to combine what we did in the first two parts, and build up a front-end of a simple website.

In order to keep the code easily maintainable and well-organized, we would put all the source code in a 'src' directory, and separating the views and stylesheets. For the views, we are going to use a template engine, so the code would be more clean and tidy, and we will use HAML here. For the stylesheets, we are going to use CSS preprocessing, for the same reasons, and we will use SASS in this case.

The initial structure:
{% highlight js %}
/*


  /
  --src/
  ----stylesheets/
  ----views/


*/
{% endhighlight %}

Firstly, lets install the dependencies in.
{% highlight js %}
npm install grunt grunt-cli grunt-contrib-haml grunt-contrib-sass grunt-contrib-watch grunt-contrib-clean load-grunt-tasks --save-dev
{% endhighlight %}
As a reminder, please also make sure you have Ruby, HAML, SASS installed for HAML and SASS compiling.

Next up, let's get our HAML and SASS files ready in their own directories, so the current directory structure becomes:
{% highlight js %}
/*


  /
  --src/
  ----stylesheets/
  ------_general.scss
  ------_content.scss
  ------style.scss
  ----views/
  ------index.haml
  --package.json


*/
{% endhighlight %}

Now we can make a directory named 'tasks', and build the Gruntfile.js at the root directory, to load our installed Grunt packages and also load the 'tasks' directory:
{% highlight js %}
module.exports = function(grunt){
  require('load-grunt-tasks')(grunt);
  require('./tasks/')(grunt);
}
{% endhighlight %}

Then we can make a folder named 'compile' inside the 'tasks' folder, and inside 'compile', let's add two grunt files, for the configurations for SASS and HAML compiling tasks. 
{% highlight js %}
/* --- /tasks/compile/sass.js --- */

module.exports = function (grunt){
  grunt.config('sass.dist',{
    files: [{
      expand: true,
      cwd: '<%= path.src %>/stylesheets/',
      src: ['*.scss'],
      dest: '<%= path.dest %>',
      ext: '.css'
    }]
  });
}
{% endhighlight %}

{% highlight js %}
/* --- /tasks/compile/sass.js --- */

module.exports = function (grunt){
  grunt.config('haml.dist',{
    files: [{
      expand: true,
      cwd: '<%= path.src %>/views/',
      src: ['*.haml'],
      dest: '<%= path.dest %>',
      ext: '.html'
    }]
  });
}

{% endhighlight %}

This time, we are going to add one more task that we didn't mention in the first two parts of this series, and you probably have noticed it in the code for the packaging installation. This task is called 'clean', and we'll use it to clean up the 'dest' directory before we do the compiling so we can ensure that the 'dest' directory is clean. By the way, let's also create the 'dest' directory inside the root directoy as well. After that, let's head back to 'compile' and create 'clean.js'. In this configuration, we set it to clean everything inside 'dest', but not the 'dest' directory itself.
{% highlight js %}
/* --- /tasks/compile/clean.js --- */

module.exports = function (grunt){
  grunt.config('clean.build',[
    '<%= path.dest %>/*', '!<%= path.dest %>'
  ]);
}
{% endhighlight %}

Next, the same as what we did in the last part, we want the compiling to be automatic whenever there's a file change. So let's create a 'helper' directory in 'tasks', and create 'watch.js' inside.

{% highlight js %}
/* --- /tasks/helper/watch.js --- */

module.exports = function(grunt){
  grunt.config('watch.sass',{
    files: ['<%= path.src %>/stylesheets/{,*/}*.scss'],
    tasks: 'sass:dist',
    options: {
      livereload: true
    }
  });

  grunt.config('watch.haml',{
    files: ['<%= path.src %>/views/{,*/}*.haml'],
    tasks: 'haml:dist',
    options: {
      livereload: true
    }
  });
}
{% endhighlight %}

As the tasks configurations are added, let's load them up in 'index.js' inside 'tasks'. Other than that, we also need to define the 'path' that we used. Finally, we can define the tasks as default.
{% highlight js %}
/* --- /tasks/index.js --- */

module.exports = function(grunt){
  grunt.config('path',{
    src: 'src',
    dest: 'dest'
  });

  grunt.loadTasks('./tasks/compile/');
  grunt.loadTasks('./tasks/helper');

grunt.registerTask('default',[  
  'clean:build'
  'sass:dist',
  'haml:dist',
  'watch' 
]); 
}
{% endhighlight %}

Now we have completed it, and we can run grunt to build the site up. The current structure with the compiled files is:
{% highlight js %}
/*


  /
  --dest/
  ----index.html
  ----style.css
  --src/
  ----stylesheets/
  ------_general.scss
  ------_content.scss
  ------style.scss
  ----views/
  ------index.haml
  --tasks/
  ----compile/
  ------clean.js
  ------haml.js
  ------sass.js
  ----helper/
  ------watch.js
  --package.json
  --Gruntfile.js


*/
{% endhighlight %}

We have completed this series on basic Grunt, and I would possibly write more about Grunt in the future to introduce some intermediate topics. Thank you, and stay tuned.