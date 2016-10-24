realHTML4Natural Documentation
============================================
In this Documentation you will find how the project is build, how you are getting started and a full list of command that are available in the templates.

What is realHTML4Natural?
-------------------------

It is a web framework for natural. It allows you to write your own HTML code where you don't have to use predefined controls. It does not rely on one big Natural Session (like N4Aj), every HTTP request (when the route it is defined to call a natural subprogram) is a new natural session. So it is thread save and you can make real asynchronous request with Java script.
Your skills are the limit here. It is completely independent from the Software AG.

Motivation and idea
-------------------

I have worked a lot with the Python Framework `Flask <http://flask.pocoo.org/>`_ which is using the template engine `Jinja <http://jinja.pocoo.org/>`_ and I thought: "Why can't you do this kind of web programming with Natural?" So I sat down began to wrote my own framework where you have full control over the behavior of the web server and the HTML page.

How does it work?
-----------------

.. image:: explanation.png

The complete framework is based on the natural native interfaces which allows you to create a natural process from your own program without using the natural binary directly. The interface to the HTML parser is based on the interface 4 API from the natural native interfaces. With this API you can call external programs out of natural and hand over parameters. Then you can read there attributes like value, length or type. Unfortunately not there names. So I have written a program which parses the local data areas from natural and can make a connection between the parameters and the names based one the position, length and type. Based on this informations you can write a HTML template which is just HTML code with placeholder for the natural variables. The HTML parser will then parse the template an replace these placeholder with the corresponding values. But how does the web server know when to call a natural program? That is quite simple. In a configuration file you describe on which URL which program is called. When nothing is defined the server will deliver the requested file out of a htdocs folder. When no file is requested or the file is not found a "Page not found" (HTTP error 400) is delivered.

Guide:
------

.. toctree::
   :maxdepth: 2

   installation
   webserver
   natural
   html-parser
   workflow
   


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

