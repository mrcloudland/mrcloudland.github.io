---
layout: post
title: Learning JavaScript - Part 1
---


<div class="message">
This is the 1st post of my JavaScript study notes series.
</div>

In this post, I'm going to explain the differences among three JavaScript array methods, reduce(), map() and forEach(). All three of them would require a callback function to be passed in and the function would be applied to the calling array. However, how the function is applied and the results are quite different. So let's dive into these methods one at a time.

### reduce()

Syntax
{% highlight js %}
arr.reduce(callback,[initialValue])
{% endhighlight %}

Syntax of callback function
{% highlight js %}
function(previousValue, currentValue, index, array){...}
{% endhighlight %}

The reduce() method would take in the call back function and run the values in the array against the function and return a final value. When the initialValue argument is set, it would become the previousValue of the callback function, and the first value of the array would be the currentValue. Then the function would run with these values, and get a return value. The returned value would be the previousValue for the second run, while the second value of the array becomes the currentValue, and the function runs again. The reduce method would run until the last value of the array is used (as the last currentValue, of course) and the final returned value of the callback function would become the returned value of the reduce() method. By the way, if the initialValue is not set for reduce(), then for the first iteration, the first value of the array is the previousValue while the second value is the currentValue.

### map()

Syntax
{% highlight js %}
array.map(callback[, thisArg])
{% endhighlight %}

Syntax of callback function
{% highlight js %}
function(item){...}
{% endhighlight %}

The map() method would also take in a callback function, but it will take each of the values in the array and run the value against the function, and return a new array with the returned values from the function. So unlike the reduce() method, the return value from map() method is an array, and the values in the array are independent.

### forEach()

Syntax
{% highlight js %}
array.forEach(callback[, thisArg])
{% endhighlight %}

Syntax of callback function
{% highlight js %}
function(item){...}
{% endhighlight %}

The forEach() method is quite similar to the map() method, but it's more like a loop, where each of the values of the array is taken out and applied to the callback function and no value is returned.

In summary, all these three methods take in a callback function and run the values from the array against the function. However, reduce() runs the next value with the result from the previous iteration and return a single final value, map() runs each of its values against the function and return an array containing each of the result, and finally forEach() runs each of its values against the function but it does not return any value.