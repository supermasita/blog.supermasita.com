---
layout: post
title:  "PYTHON - Multiprocessing simple example"
date:   2018-10-01 07:04:16 -0300
tags: python multiprocessing computers
---
Time to get those other lazy cores to work.

The **multiprocessing** library allows you to spawns multiple operating system processes for each parallel task. Below, I have tried to make the simplest example I could think of.

## The basic loop
Lets first create this very simple script that will use loop and run a function (sleep) with a series of parameters from a list:
{% highlight python %}
from time import sleep

params_list = [1,2,3,4]
for seconds in params_list:
    sleep(seconds)
{% endhighlight %}

As expected, the total number of seconds to sleep add up, to the total of 10 seconds:
{% highlight bash_session %}
$ time python3 sleep.py

real    0m10.065s
user    0m0.039s
sys    0m0.011s
{% endhighlight %}

## Multiprocessing
We now import the library and use "Pool", which will require the function and the parameter list. The pool is set to spawn four processes ("Pool(4)")
{% highlight python %}
from multiprocessing import Pool
from time import sleep

params_list = [1,2,3,4]
with Pool(4) as pool:
    pool.map(sleep, params_list)
    pool.close()
    pool.join()
{% endhighlight %}

Now each sleep is run as a separate processes and thus their seconds do not add up:
{% highlight bash_session %}
$ time python3 sleep-multi.py

real    0m4.115s
user    0m0.066s
sys    0m0.032s
{% endhighlight %}

### Bonus 
What if my function needs more than one parameter? **starmap** allows use to tuples. Below you can see how the list of tuples are feed into the pool and used to call the function "sleep_print()":
{% highlight python %}
from multiprocessing import Pool
from time import sleep

def sleep_print(seconds, msg):
    sleep(seconds)
    print(msg)

params_list = [(1,"1 Second"),(2,"2 Seconds"),(3,"3 Seconds"),(4,"4 Seconds")]
with Pool(4) as pool:
    pool.starmap(sleep_print, params_list)
    pool.close()
    pool.join()
{% endhighlight %}

Again, the time does not add up:
{% highlight bash_session %}
$ time python3 sleep-multi-starmap.py
1 Second
2 Seconds
3 Seconds
4 Seconds

real    0m4.109s
user    0m0.070s
sys    0m0.034s
{% endhighlight %}

### Recommended reading
* RTFM: <https://docs.python.org/3.5/library/multiprocessing.html#module-multiprocessing>
* Comparison of multiprocessing and threading: <https://www.quantstart.com/articles/Parallelising-Python-with-Threading-and-Multiprocessing>
