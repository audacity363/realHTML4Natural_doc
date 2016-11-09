Webserver
=========

The webserver is based on the open source project miniweb (http://www.http://miniweb.sourceforge.net/) but this version is heavily modified.

This version can parse POST Requests where data is urlencoded and not an multipart message, read cookies and set new ones and of course calling Natural programs.

There are two configuration files that miniweb can use. A general configuration file with how the server should be configured and a file which controls when which Natural program is called.

.. _webserver-config:

Server configuration
^^^^^^^^^^^^^^^^^^^^^
The path to this file is stored in the environment variable "WEBCONFIG". It can contain multiple environments. For example a configuration for test and one for production.

Example
-------

.. code-block:: xml

   <miniweb>
       <environment type="test">
           <routes>/home/tom/C/realHtml4Natural/web_server/routes.xml</routes>
           <htdocs>/home/tom/C/realHtml4Natural/web_server/htdocs/</htdocs>
           <templates>/home/tom/C/realHtml4Natural/web_server/templates/</templates>
           <natsourcepath>/natural/natsrc/</natsourcepath>
           <port>1024</port>
           <debug>true</debug>
           <naterror>nat_error.html</naterror>
           <ldaerror></ldaerror>
           <parsererror></parsererror>
           <natparms></natparms>
       </environment>
       <environment type="production">
           <routes>/home/tom/C/realHtml4Natural/web_server/routes_prod.xml</routes>
           <htdocs>/home/tom/C/realHtml4Natural/web_server/htdocs/</htdocs>
           <templates>/home/tom/C/realHtml4Natural/web_server/templates/</templates>
       </environment>
   </miniweb>

Explanation enviroment tag
--------------------------
With the environment tag you can setup multiple environments in which the server is running. It takes exactly one argument. The type attribute with the name of the environment.

Explanation enviroment childs
-----------------------------
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| entry         | explanation                                                                                                   | default Value  | required |
+===============+===============================================================================================================+================+==========+
| routes        | the path to the routes configuration file                                                                     | None           | yes      |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| htdocs        | the path to the htdocs folder                                                                                 | None           | yes      |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| templates     | the path to the template folder                                                                               | None           | yes      |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| natsourcepath | path to the Natural sources (must be the root directory)                                                      | None           | yes      |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| port          | port on which the webserver should listen                                                                     | 80             | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| debug         | decide wether an error occurs to render a custom 500 page with the error message or a generic 500 error page  | false          | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| naterror      | the template that should be rendered when Natural error occurs (debug must set to true)                       | None           | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| ldaerror      | the template that should be rendered when error occurs while parsing the LDA (debug must set to true)         | None           | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| parsererror   | the template that should be rendered when error occurs while handling the template (debug must set to true)   | None           | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+
| natparms      | parameter that will be passed through to Natural (the same as if you were calling Natural from the binary)    | None           | no       |
+---------------+---------------------------------------------------------------------------------------------------------------+----------------+----------+

Route configuration
^^^^^^^^^^^^^^^^^^^^
In the route configuration file you enter the Natural program which should run when a specific route is called.

Example
-------

.. code-block:: xml

    <realHtml>
        <route path="notizen">
            <programm>SHNOTIZ</programm>
            <alias></alias>
            <return></return>
            <library>TOMENGE</library>
        </route>
        <route path="new_notiz">
            <programm>STNOTIZ</programm>
            <library>TOMENGE</library>
        </route>
        <route path="delete_notiz">
            <programm>SDENOTIZ</programm>
            <library>TOMENGE</library>
        </route>
    </realHtml>


Route tag
---------

The route tag takes exactly one parameter. The path attribute. That is the URL which is requested on the server without the first slash.

Explanation route childs
------------------------

+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| entry    | explanation                                                                                  | default Value  | required |
+==========+==============================================================================================+================+==========+
| program  | the Natural program that will be called                                                      | None           | no       |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| library  | the library to logon                                                                         | None           | no       |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| alias    | a file in the htdocs folder that will be delivered instead of calling the Natural program    | None           | no       |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+
| return   | a HTTP code that will be returned instead of a file from htdocs or calling a Natural program | None           | no       |
+----------+----------------------------------------------------------------------------------------------+----------------+----------+


Miniweb usage
^^^^^^^^^^^^^^
-h  display this help screen
-v  log status/error info
-p  specifiy http port [default 80]
-r  specify http document directory [default htdocs]
-l  specify log file
-m  specifiy max clients [default 32]
-M  specifiy max clients per IP
-s  specifiy download speed limit in KB/s [default: none]
-n  disallow multi-part download [default: allow]
-d  disallow directory listing [default ON]
--environment    environment to load [default: none]

