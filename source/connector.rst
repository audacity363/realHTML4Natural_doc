Tomcat Connector
================

Overview
^^^^^^^^
When you want to use the framework with a new or current tomcat installation you can use the `TomcatConnector <https://github.com/audacity363/realHTML_TomcatConnector>`_. It consists of three parts:

- A tomcat servlet which handles the requests
- A library which provides helper function for the servlet
- A JNI (Java Native Interface) Library which calls Natural

Tomcat Servlet
--------------
The purpose of the tomcat servlet is to handle the incoming request, get the configuration and call Natural.
You have to put the .class file which is located in the servlet folder into your webapps folder under "ROOT/WEB-INF/classes". When one of the folder does not exist you can just create it.

To get the servlet to get known from the tomcat you have to edit your web.xml configuration file. It should be located in your "conf" folder in your tomcat installation directory. Just add the following at the end:

.. code-block:: xml

    <servlet>
        <servlet-name>realHTMLServlet</servlet-name>
        <servlet-class>realHTMLServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>realHTMLServlet</servlet-name>
        <url-pattern>/realHTML4Natural/*</url-pattern>
    </servlet-mapping>

The servlet tag is there for getting to know the java class (located in the servlet-class tag) of the servlet to the tomcat. With the servlet-name tag under servlet you give the servlet a name for later configuration.

With the servlet-mapping tag you map the servlet to a specific path. In your case every url with "/realHTML4Natural/" at the beginning will call the servlet.

Servlet Library
---------------
The library you can find in the "realHTML/tomcat/connector" library. In there you find utility function for parsing the xml configuration files (see section "Connector configuration"), the loading of the JNI library and the call to the C-function in the JNI library. This jar file must be placed in the "lib" folder of your tomcat installation. 

Why is this so?

A JNI library can only loaded once in one Java VM. Because the servlets runs in threads you are allowed to load her once. The jar files in the lib directory gets loaded on the start up process of the tomcat and so gets the JNI library loaded and you can use the functions in a servlet.


JNI library
-----------
In the JNI library happens all the magic. Here the parameter for the natural gets generated, a new process gets spawned  and natural runs in this process. The library now waits until this process has exited and returns to the servlet.

Installation
^^^^^^^^^^^^^
The installation is rather simple: 

- run the Makefile with the target "all"
- copy realHTMLconnector.jar into the lib directory of your tomcat installation
- copy the \*.class file into "<path to your webapps folder>/ROOT/WEB-INF/classes/"
- copy the librealHTMLconnector.so into a folder that you choose
- edit the tomcat web.xml as show in `Tomcat Servlet`_
- edit your setenv.sh in the bin directory and add the following lines:

.. code-block:: bash

    export realHTMLconfiguration=<path to your config file for the framework>
    export NATUSER=$NATUSER:<path to your ralHTMLNatural.so>
    export LIBPATH=$LIBPATH:<path to the directory where your libnatural.so lies>

- edit your catalina.sh in the bin directory and search for a line where the variable "JAVA_OPTS" get set.
- append it with the "-Djava.library.path=<path to the directory which contains the JNI library>". For example:

.. code-block:: bash

   JAVA_OPTS="$JAVA_OPTS $JSSE_OPTS -Djava.library.path=/realHTML4Natural/bin/"

- restart tomcat


Connector configuration
^^^^^^^^^^^^^^^^^^^^^^^^
Placeholder
