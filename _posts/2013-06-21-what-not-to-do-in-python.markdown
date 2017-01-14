---
layout: post
title: "What Not to Do in Python"
date: 2013-06-21 12:00:00
---

Ever run into a bug and think "Wow, I should have known that." Happens to me
too, here are some of my Python **(py < 3.0)** gotchas:


### Don't forget to close tempfile file descriptors

The `tempfile` module is useful for, well, creating temp files. But one thing
that's easy to overlook is that a call to `tempfile.mkstemp()` *opens* the file
for you and expects you to close it!

`(fd, filepath) = tempfile.mkstemp()`

The mistake I made was to take the returned filepath and open a *new* file
object using `open()`, leaving me with two open fds on the same file.
I'd remember to close the second one, but totally ignored the first.

I discovered my negligence when I decided to write a long running server process
that made use of tempfiles periodically. It would crash occasionally, with
errors like:

<pre class="terminal"><code>OSError: [Errno 24] Too many open files: '/tmp/tmprT9oIH'</code></pre>

So, here's what one *Should Do*:

{% highlight python %}
import os
import tempfile

fd, fp = tempfile.mkstemp()
with os.fdopen(fd) as f:
    # do stuff with the file
{% endhighlight %}

### Don't use `self.__class__` in `super`

So there I was one day, coding up a new subclass when something went horribly wrong.

<pre class="terminal"><code>RuntimeError: maximum recursion depth exceeded while calling a Python object</code></pre>

Huh?

Well, for some reason I thought I was being clever and started calling super like so:

`super(self.__class__, self)`

I mean, it kinda worked, at least until the day I tried to subclass more than
one level deep.

Here's an example demonstrating the problem:

{% highlight python %}
class Gramps(object):
    def __init__(self):
        pass

class Dad(Gramps):
    def __init__(self):
        super(self.__class__, self).__init__()

class Me(Dad):
    def __init__(self):
        super(self.__class__, self).__init__()

Me()
{% endhighlight %}

You see, when `Dad`'s `__init__()` is called, `self` will be a reference to a
`Me` instance, so `super(self.__class__, self)` in `Dad` will return a
reference to `Dad` and things go kaboom.

Luckily, I remembered [The Art of Subclassing](http://pyvideo.org/video/879/the-art-of-subclassing)
by [Raymond Hettinger](https://twitter.com/raymondh), which discusses this
detail. That is, when dealing with superclasses, references to `self` are still
to child instances.

So, here's what one *Should Do*:

{% highlight python %}
class Gramps(object):
    def __init__(self):
        pass

class Dad(Gramps):
    def __init__(self):
        super(Dad, self).__init__()

class Me(Dad):
    def __init__(self):
        super(Me, self).__init__()

Me()
{% endhighlight %}


### Don't forget to `close_fds` when forking server processes

I had to ask for help on this one, couldn't figure it out for the life of me.

`subprocess.Popen()` has [many parameters](http://docs.python.org/2/library/subprocess.html#subprocess.Popen),
one of them is `close_fds`, which defaults to `False`. This control determines
whether to close file descriptors or inherit them from the parent process.

I don't remember what kind of errors I was getting but basically the spawned
processes would inherit the parent's open socket, so that two process had access
to a single socket. Not what I intended.

Here's a contrived example:

{% highlight python %}
import socket
import subprocess
import sys

cmd = None

if len(sys.argv) == 2 and sys.argv[1] == "child":
    # in the child process
    port = 50008
else:
    # in the parent process
    cmd = [sys.executable, __file__, 'child']
    port = 50007

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', port))
s.listen(1)

if cmd:
    subprocess.Popen(cmd)

conn, addr = s.accept()
conn.close()
{% endhighlight %}

Run the script and check the open sockets:

<pre class="terminal"><code>$ lsof | grep "5000[87]"
Python    1166 jlas    3u    IPv4 0x2e1148bfba350f8f       0t0      TCP *:50007 (LISTEN)
Python    1167 jlas    3u    IPv4 0x2e1148bfba350f8f       0t0      TCP *:50007 (LISTEN)
Python    1167 jlas    4u    IPv4 0x2e1148bfbb046857       0t0      TCP *:50008 (LISTEN)
</code></pre>

Not cool.

Of course, here's what one *Should Do*:

`subpcrocess.Popen(cmd, close_fds=True)`
