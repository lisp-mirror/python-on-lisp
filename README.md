https://common-lisp.net/project/python-on-lisp/index.htm

seen on https://stackoverflow.com/questions/5174199/is-there-a-simple-way-to-use-python-libraries-from-common-lisp/54796862


Python-On-Lisp: unleashing Python libraries in Common Lisp
Last updated November 24, 2006, created 2nd February 2006
Introduction

Every so often a newbie Lisper pops up on comp.lang.lisp and says "I want to make a web application. Python is really easy to get up and running but Lisp is much harder". Sometimes they get called a troll, but some people will also say they have a valid point.

Python was originally defined to be easily embedded, so you can link it into a C/C++ program as a dynamically linked library (DLL). Lisp's common foreign function interface (CFFI) lets you call into linked libraries from lisp. Python-on-lisp puts these two pieces together, so that you can call python from lisp. This is a two way bridge, so python can return its results back to lisp. Crucially, this makes python's rather extensive libraries available from within common lisp.
Download

You can get pythononlisp-0.2.tar.gz, an ASDF-installable tarball.

This is an Alpha release, I cannot be held responsible for any damage or loss that occurs from running this Lisp program. Always start a new Lisp session in case Lisp crashes, and make a backup of any important files in the directory it resides in.
Requirements
To use python-on-lisp, you need:

    Python (>=version 2.2)
    CFFI (>= version 0.9.2)
    ASDF-INSTALL, for easy installation

This package has been tested successfully on the following CL implementations, among others:

    WinXP: Clisp 2.37, Allegro 7.0, Lispworks 4.4
    WinXP, Gentoo Linux: SBCL 0.9.12, 0.9.13, acl80
    OSX: SBCL 0.9.18 (fink), Python2.5 (fink)

Getting Started

Python-on-lisp is defined to use the asdf-install system, making installation fairly automatic. More details are contained in the documentation and source code of the download bundle. But quick instructions are as follows:

    Installing it. Make sure you have python and asdf-install installed on your system. Download the tarball pythononlisp.tar.gz, load asdf-install, and install pythononlisp in the usual way:

      (asdf-install:install "/path/to/pythononlisp.tar.gz") 

    This will also install dependencies like cffi
    Loading it. To load it, you should do

      (asdf:operate 'asdf:load-op :pythononlisp) 

    If you want to load pythononlisp without asdf, load cffi, then do:

    (LOAD "packages.lisp") and (LOAD "pythononlisp.lisp")

    Using it.

      (py::py "print \"Hello from python\"")

      (py::py get-web-page "http://www.google.com")

How it works
For efficiency, python-on-lisp also lets you define virtual modules, which Python code can call back to as if they were Python functions, which means the time taken to call the Lisp is as long as it takes to unwrap a pointer and run the actual code. For convenience, but with a slight loss of efficiency, python-on-lisp can redirect the Python output (errors, printed text, etc) to a Lisp string, and Python programs can eval Lisp code, which is a more flexible, but slower, kind of callback.

You could call the project PyEval, like Lisp's eval function.

Python-On-Lisp will look for the DLL or object code file for Python in the default location for your platform. On Windows and OSX this should just work, as well as on Debian Linux. However, if you have installed it in an unusual place, or are using an unusual platform, you may need to specify this information yourself by amending the call to (CFFI:DEFINE-FOREIGN-LIBRARY python-library ...) in pythonlisp.lisp

For further instructions, consult the documentation in the package itself.

Known Bugs and Tips

    [Bug]In Clisp, If you get into a REPL loop while inside a callback function that has been called from Python, aborting will crash Clisp. This doesn't seem to happen when inside the py-repl. [Now seems to work okay in Clisp, so this bug might be fixed]
    [Tip]If you use a Python webserver, it may be run with serve_forever. This is just a while(1) loop calling handle_request (on the same object), so if you need to break out of the loop without any hassle, just call handle_request a few times, one after the other, and it will drop back into Lisp after that many web requests.
    [Tip]To keep things as fast as possible, only use Python for network access and for things like obscure libraries that Lisp doesn't have. Arbitrary Python code may run slower than Lisp, though the Python library code is very well optimised. 

For more known bugs, see pol.lisp (below).

I'm hoping to get CVS up and running so all changes are logged, etc.
Mailing Lists

    Python-on-lisp-devel
    for developers
    Python-on-lisp-cvs
    CVS log feed.
    Python-on-lisp-announce
    for announcements. 

SVN

You can browse our SVN repository or download the current development tree via anonymous svn, as described here
Developers
The lead developer on this project is Jeremy Smith, who has received some help from Alexis Gallagher with making the package asdf-installable and compatible across platforms.
Jeremy Smith, June 7th 2006. 
