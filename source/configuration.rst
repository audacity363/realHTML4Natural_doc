Configuration
=============

RealHTML4Natural needs just two environment variables to be able to run.
"RH4NLOG" and "RH4NCONF".

RH4NLOG must contain a path to a directory where logs should be written. For example: "/srv/rh4n/logs/"

.. code-block:: bash

   export RH4NLOG=/srv/rh4n/logs/

RH4NCONF must contain a path to an xml file (it does not have to exist). For example: "/srv/rh4n/config.xml"

.. code-block:: bash

   export RH4NCONF=/srv/rh4n/config.xml

Configurationfile structure
---------------------------

The configuration file has the following structure:

.. raw:: html

       <object data="_static/xml_description.svg" type="image/svg+xml"></object>

Configuration example
---------------------

.. code-block:: xml

    <realHTML4Natural>
        <environment name="env1">
            <natsrc>/srv/softwareag/natural/env1/fuser/</natsrc>
            <natparms>etid=$$ parm=env1parm</natparms>
            <route path="/route1">
                <natLibrary>MYNATLIB1</natLibrary>
                <natProgram>MYNATPROG1</natProgram>
                <active>true</active>
                <loglevel>FATAL</loglevel>
                <login>false</login>
            </route>
            <route path="/route2">
                <natLibrary>MYNATLIB1</natLibrary>
                <natProgram>MYNATPROG2</natProgram>
                <active>true</active>
                <loglevel>WARNING</loglevel>
                <login>true</login>
            </route>
            <environ>
                <name>PATH</name>
                <content>/usr/bin</content>
                <append>true</append>
            </environ>
            <environ>
                <name>VAR1</name>
                <content>my value</content>
                <append>false</append>
            </environ>
        </environment>
        <environment name="env2">
            <natsrc>/srv/softwareag/natural/env2/fuser/</natsrc>
            <natparms>etid=$$ parm=env2parm</natparms>
            <route path="/route1">
                <natLibrary>MYNATLIB2</natLibrary>
                <natProgram>MYNATPROG1</natProgram>
                <active>true</active>
                <loglevel>DEVELOP</loglevel>
                <login>false</login>
            </route>
            <route path="/route2">
                <natLibrary>MYNATLIB2</natLibrary>
                <natProgram>MYNATPROG2</natProgram>
                <active>false</active>
                <loglevel>ERROR</loglevel>
                <login>false</login>
            </route>
        </environment>
    </realHTML4Natural>

Of course you don't have to write the configuration yourself. For that there is a

Webinterface
------------

You can access the webinteface over <your server>:<your portnumber>/realHTML4Natural/config. For example "server1:8080/realHTML4Natural/config"
It should be pretty self-explanatory.

Logging
-------

The framework can log what it is currently doing. This is handy when developing the framework itself or searching for an error while working with it.
RealHTML4Natural creates logfiles in the path provided by RH4NLOG. The filename get's generated from the natural library, program and the current date.

So a natural program (TESTPROG in the library TESTLIB) called on the 23.06.2018 would generate a logfile with the name "rh4n_TESTLIB_TESTPROG_23.06.2018.log"
The file get appended with every request with logmessages in the format:

"dd.mm.yyyy HH:MM:SS.<milli sec> [<natlibrary>-><natprogram>] <pid> <loglevel> <log message>"

You can configure every individual route to a specific loglevel:

FATAL
    Something went terribly wrong and could possible not be recovered.

ERROR
    Sommething went wrong, the framework could probably correct the error but the behavior could be undifined


WARNING or WARN
    Something went wrong but the framework could correct the error

INFO
    Just infos what is going on.

DEBUG
    This is a verbose output. It can be used when you want to know what happens behind the scenes. It is not recommended for production use.

DEVELOP
    This is a very verbose output. It's normaly used when searching for bugs in the framework. This loglevel appends the log format to the format "dd.mm.yyyy HH:MM:SS.<milli sec> [<natlibrary>-><natprogram>] <pid> <loglevel> <C-function> <C-file> <C-source line> <log message>". It is not recommened for production use.


They get evaluated in the order: FATAL > ERROR > WARNING or WARN > INFO > DEBUG > DEVELOP. So when you select a log level every levels above get's logged too. 
When you for example select ERROR, FATAL and ERROR messages will get logged but not WARNING, INFO, DEBUG or DEVELOP. 

Recommendations
...............

When you are in development of a natural program which uses realHTML4Natural I recommend using WARNINGs and when putting it into production you should change it to ERROR. INFO, DEBUG and DEVELOp should only be used when you want to understand what the framework is doing or developing it by urself. DEBUG and DEVELOP produces a lot of logges and the the logfiles whill get very large very soon.
