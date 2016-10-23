realHTML4Natural Documentation
============================================
In this Documentation you will find how the project is build, how you are getting started and a full list of command that are available in the templates.

What is realHTML4Natural?
-------------------------

It is a web framework for natural. It allows you to write your own HTML code and you don't have to use predefined controls. It does not rely on one big Natural Session (like N4Aj), every HTTP request (when the route it is defined to call a natural subprogram) is a new natural session. So it is thread save and you can make real asynchronous request in Java script.
Your skills are the limit here. It is completely independent from the Software AG.

Motivation and idea
-------------------

I have worked a lot with the Python Framework `Flask <http://flask.pocoo.org/>`_ which is using the template engine `Jinja <http://jinja.pocoo.org/>`_ and I thought: "Why can't you do this kind of web programming with Natural?" 

How does it work?
-----------------

Placeholder


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

