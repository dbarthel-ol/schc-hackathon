
Repository made for the SCHC Hackathon (IETF 102, Montreal)

* make
  (or make repos)
  * -> automatically clones schc-test (from Dominique)
  * -> automatically clones micropython (version for Linux)

----

The sender/receiver code from schc-test had been copied/modified into
a single file `schc_test.py`

Currently, the sender and the receivers can be run on Linux and communicate
through lo (loopback), 127.0.0.1:9900 (sender) <-> 127.0.0.1:9999 (receiver)

How to run both:

* `make recv`
  * -> runs micropython with "schc_test.py recv"
  * This creates a receiver for fragments

* `make send`
  * -> runs micropython with "schc_test.py send"
  * This creates a sender for fragments which sends a large packet

Alternate with normal Python:

* `make cpy-recv`  -> same as `make recv` by with normal python3 (CPython)
* `make cpy-send`  -> same as `make send` by with normal python3 (CPython)

Alternate after `make link`

* `make link-recv`  -> same as `make recv` but inside one directory generated by `make link` (see below)
* `make link-send`  -> same as `make send` bur inside another directory generated by `make link` (see below)

----

* `make link`
  * this creates a lots of links in project-X/ (where X=sending, receiving),
     the idea is that each project-X/ repository can be synchronized with a
     different pycom 
  * By default devices are /dev/ttyACM0 /dev/ttyACM1, can change by adding a Makefile.local with 

----

Notes:
  micropython issues:
  
In micropython, bytearray accepts only one argument, whereas in cpython
you must have 2 arguments (e.g. with the encoding), if the first is a str.

```
$ ./micropython/ports/unix/micropython -c 'bytearray("aaa", "utf-8")'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: function expected at most 1 arguments, got 2

$ python3 -c 'bytearray("aaa", "utf-8")'
$ # fine
```

There are many others:
* Globally, micropython for unix defines some version of modules with other names like "uXXX", e.g. usocket instead of socket, utime for time, etc.
* micropython for Pycom however, in many cases, keeps the Python names for "uXXX" modules (even if they are incomplete), or implement missing modules
