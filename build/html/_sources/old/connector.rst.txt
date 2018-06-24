.. _tomcat-connector:

Tomcat Connector
================

Overview
^^^^^^^^
When you want to use the framework with a new or current tomcat installation you can use the `TomcatConnector <https://github.com/audacity363/realHTML_TomcatConnector>`_. It consists of three parts:

- A tomcat servlet which handles the requests
- A library which provides helper function for the servlet
- A JNI (Java Native Interface) Library which calls Natural


.. image:: _static/tomcat_connector.png

Tomcat Servlet
--------------
The purpose of the tomcat servlet is to handle the incoming request, get the configuration and call Natural.
It comes in a deployable .war file. Just copy it into your webapps folder or deploy it over the manager app. The war file contains a web.xml configuration file wich automaticly assign the url <hostname>:<port>/realHTML4Natural to the servlet.

Servlet Library
---------------
The library you can find in the `servlet/src/realHTML/tomcat/connector/ <https://github.com/audacity363/realHTML_TomcatConnector/tree/master/servlet/src/realHTML/tomcat/connector>`_ directory. In there you find utility function for parsing the xml configuration files (see section "Connector configuration"), the loading of the JNI library and the call to the C-function in the JNI library. This file is also a component of the war file.

JNI library
-----------
In the JNI library happens all the magic. Here the parameter for the natural gets generated, a new process gets spawned  and natural runs in this process. The library now waits until this process has exited and returns to the servlet.


Installation
^^^^^^^^^^^^^
The installation is rather simple: 

- run the Makefile with the target "all"

- copy realHTMLconnector.jar into the lib directory of your tomcat installation
- copy the librealHTMLconnector.so into a folder that you choose
- deploy realHTML4Natural.war from the folder "servlet"
- create a file called setenv.sh in the bin directory of the tomcat installation (if it does not exist)
- edit setenv.sh and add the following lines:

.. code-block:: bash

    export realHTMLconfiguration=<path to your config file for the framework>
    export NATUSER=$NATUSER:<path to your ralHTMLNatural.so>

and (depends on your system:)

AIX:

.. code-block:: bash

    export LIBPATH=$LIBPATH:<path to the directory where your libnatural.so lies>:<path to the directory which contains the JNI library>

LINUX:

.. code-block:: bash

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:<path to the directory where your libnatural.so lies>:<path to the directory which contains the JNI library>

- restart tomcat


Connector configuration
^^^^^^^^^^^^^^^^^^^^^^^^
The path to this file is stored in the environment variable "realHTMLconfiguration". It can contain multiple environments. For example a configuration for multiple natural environments with different parm files.

Example
-------

.. code-block:: xml

    <realHTML4Natural>
        <environment type="env1">
            <routes>/realHTML4Natural/env1/routes.xml</routes>
            <templates>/realHTML4Natural/env1/templates/</templates>
            <natparms>parm=env1parm etid=$$</natparms>
            <natsourcepath>/realHTML4Natural/natural/env1/fuser63/</natsourcepath>
        </environment>
        <environment type="env2">
            <routes>/realHTML4Natural/env2/routes.xml</routes>
            <templates>/realHTML4Natural/env2/templates/</templates>
            <natparms>parm=env2parm etid=$$</natparms>
            <natsourcepath>/realHTML4Natural/natural/env2/fuser63/</natsourcepath>
        </environment>
    </realHTML4Natural>

Explanation enviroment tag
--------------------------

With the environment tag you can setup multiple environments on which the servlet should listen. It takes exactly one argument. The "type" attribute with the name of the environment.  The url entry after "realHTML4Natural" specify the enviroment to call. For example "/realHTML4Natural/env1/..." would call the configuration under the "environment" tag with the type "env1". 

Explanation enviroment childs
-----------------------------
+---------------+-----------------------------------------------------------------------------------------------------------------+----------------+----------+
| entry         | explanation                                                                                                     | default Value  | required |
+===============+=================================================================================================================+================+==========+
| routes        | the path to the routes configuration file                                                                       | None           | yes      |
+---------------+-----------------------------------------------------------------------------------------------------------------+----------------+----------+
| templates     | the path to the template folder                                                                                 | None           | yes      |
+---------------+-----------------------------------------------------------------------------------------------------------------+----------------+----------+
| natsourcepath | path to the Natural sources (must be the root directory of the natural source)                                  | None           | yes      |
+---------------+-----------------------------------------------------------------------------------------------------------------+----------------+----------+
| natparms      | parameter that will be passed through to Natural (the same as if you were calling Natural from the command line)| None           | no       |
+---------------+-----------------------------------------------------------------------------------------------------------------+----------------+----------+

Routes configuration
^^^^^^^^^^^^^^^^^^^^^
In the route configuration file you enter the Natural program which should run when a specific route is called. This is the file that is specified in the "routes" tag in your `Connector configuration`_.

Example
-------

.. code-block:: xml

    <realHtml>
        <route path="/notizen">
            <programm>SHNOTIZ</programm>
            <library>TOMENGE</library>
            <debug>true</debug>
        </route>
        <route path="/new_notiz">
            <programm>STNOTIZ</programm>
            <library>TOMENGE</library>
        </route>
        <route path="/delete_notiz">
            <programm>SDENOTIZ</programm>
            <library>TOMENGE</library>
        </route>
    </realHtml>


Route tag
---------

The route tag takes exactly one parameter. The path attribute. That is the URL which is requested on the server after the "realHTML4Natural/<enviroment>" url.

For example: The URL "localhost:8080/realHTML4Natural/env1/notizen" would call the program SHNOTIZ in the library TOMENGE.

Explanation route childs
------------------------

+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| entry    | explanation                                                                                  | default Value  | required |
+==========+==============================================================================================+================+==========+
| program  | the Natural subprogram that will be called                                                   | None           | yes      |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| library  | the library to logon                                                                         | None           | yes      |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| debug    | create a logfile with the name "<ldaname>.log" in the /tmp directory                         | False          | no       |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+


