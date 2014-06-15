---
layout: post
title: Getting Started with Grunt - Part 1
---


<div class="message">
With Jinglei's help, I started my journey learning Grunt.js recently. I learned a lot from him and this blog series is a record of what was learned. This is the Part 1 of the series.
</div>

First of all, Grunt need to be installed, as well as grunt-cli which is the command line interface of grunt.
{% highlight js %}
npm install grunt grunt-cli
{% endhighlight %}

Grunt came with many official and thrid-party packages that could be installed with npm. For instance, if you would like to use grunt for compiling CSS from SASS, you could install the grunt-contrib-sass package. The  --save-dev option would save it as a dev-dependency in the package.json file. <strong>(Update: As this package depends on SASS, and SASS depend on Ruby, make sure you have both of them installed. The package would load up SASS to perform the task)</strong>
{% highlight js %}
npm install grunt-contrib-sass --save-dev
{% endhighlight %}

After the package installation, we can create a Gruntfile.js file, which is the entry point of Grunt. In the following example, it loads up the tasks folder and look for Grunt files in it.

{% highlight js %}
module.exports = function(grunt){  
  require('./tasks/')(grunt);  
}
{% endhighlight %}

To load an installed Grunt package, we can use loadNpmTasks in a Grunt file.
{% highlight js %}
grunt.loadNpmTasks('grunt-contrib-sass');
{% endhighlight %}

There's also a package for loading all the dependent Grunt packages in the package.json file, which is called load-grunt-tasks, and can be included in a Grunt file. We include it in our Gruntfile.js file here, so the file became:
{% highlight js %}
module.exports = function(grunt){  
  require('load-grunt-tasks')(grunt);  
  require('./tasks/')(grunt);  
}
{% endhighlight %}

Now we create a index.js file inside the tasks folder and it would be the default entry point in that folder. Then we define the source and destination path for the task here.
{% highlight js %}
module.exports = function(grunt){  
  grunt.config('path',{  
    src: 'src',  
    dest: 'dest'  
  });  
}  
{% endhighlight %}

<del>Then we need to define the actual task itself, which would be the 'dist' task inside under the sass task/package with a bunch of parameters setting up the source, destination, filetype etc...</del> <strong>(Update: Then we need to define configurations for the loaded tasks. When we need different configurations for different situations under the same task, we can define multiple configurations, by setting up different 'targets', and the 'targets' can be arbitrarily named. In the following example, we have only one target for the 'sass' task, which is the 'dist' target.)</strong>
{% highlight js %}
grunt.config('sass.dist',{  
  files: [{  
    expand: true,  
    cwd: '<%= path.src %>/stylesheets/',  
    src: ['*.scss'],  
    dest: '<%= path.dest %>',  
    ext: '.css'  
  }]  
}); 
{% endhighlight %}

Now we can run the task in command line, and it would take all the .scss files from the src folder, compile them into .css files, and then put them into the dest folder. <strong>(Update: Note here that we are running the dist target in sass task. If we simply run "grunt sass", it would iterate over all tasks.)</strong>
{% highlight js %}
grunt sass:dist
{% endhighlight %}

We can also define the default task in index.js, so we don't need to specify which task to run when we execute Grunt.
{% highlight js %}
grunt.registerTask('default',[  
  'sass:dist'  
]); 
{% endhighlight %}

Now we can execute the task with just one word!
{% highlight js %}
grunt
{% endhighlight %}
