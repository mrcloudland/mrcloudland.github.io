---
layout: post
title: Getting Started with Grunt - Part 2
---


<div class="message">
With Jinglei's help, I started my journey learning Grunt.js recently. I learned a lot from him and this blog series is a record of what was learned. This is the Part 2 of the series.
</div>

In order to make the code more organized and easilly maintainable, we need to separate the tasks into individual files.

First of all, we can create some subfolders in the 'tasks' folder, for different categories of tasks. In this case, we create a subfolders named 'compile' for our sass compiling task and other compiling tasks we might need. In the index.js file in the 'tasks' folder, we can load the tasks (to be defined) in 'compile'.
{% highlight js %}
grunt.loadTasks('./tasks/compile/');
{% endhighlight %}

Next, let's create an index.js file in 'compile', and move the configuration for our sass task from index.js in 'tasks' to index.js in 'compile':
{% highlight js %}
module.exports = function(grunt){
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

Now we've put our task in an individual file, and the tasks would work like before. From now on, when we add a new task, we need to create a new Grunt file to keep the structure clean and organized. As an example, let's add one.

After having a sass compiling tool, it would be nice if the compiling task can run automatically when the sass files are changed, and the 'watch' task would get it done.

Similar to what we did in Part 1, we can install the task package with NPM.
{% highlight js %}
npm install grunt-contrib-watch --save-dev
{% endhighlight %}

Let's create a subfolder in the 'tasks' folder and name it 'helper', or you can give it any name you like. As what we did previously, we need to load the folder in index.js inthe 'tasks' folder.
{% highlight js %}
grunt.loadTasks('./tasks/helper/');
{% endhighlight %}

Then we need to create an index.js inside the 'helper' folder, which is the default entry point there, as we didn't specify any file when we loaded tasks from that folder. Then we define the configuration in it. The 'files' parameter defines which files to be watched, while the 'tasks' parameter defines which task to run.
{% highlight js %}
grunt.config('watch.sass',{
  files: ['<%= path.src %>/stylesheets/{,*/}*.scss'],
  tasks: 'sass:dist',
});
{% endhighlight %}

Finally, let's add the 'watch' task to the task registration in index.js in 'tasks' folder, making sure it runs after the 'sass' task.
{% highlight js %}
grunt.registerTask('default',[  
  'sass:dist',
  'watch:sass' 
]); 
{% endhighlight %}

Now we have the 'sass' and 'watch' grunt tasks defined, with individually located configuration files. When we run grunt, it compile .sass file from the source folder, put the compiled .css file into the destination folder, and when there's any change to the .sass files, the compiling runs again.
