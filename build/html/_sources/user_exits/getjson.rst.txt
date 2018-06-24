.. _getjsono:

GETJSONO
========

With the user exit "getjsono" you can match a JSON structure to a natural structure.

Example LDA
^^^^^^^^^^^

Name of the LDA: "USERLIST"

.. code-block:: cobolfree

   1 input-get-user
       2 firstname (A) DYNAMIC
       2 lastname (A) DYNAMIC

JSON structure:

.. code-block:: JSON

   {
       "firstname": "Tom",
       "lastmame": "Engemann"
   }

Example call
^^^^^^^^^^^^

.. code-block:: cobolfree

   call 'interface4' varptr 'getjsono' 'input-get-user@USERLIST' REALHTML_SETTINGS 
       input-get-user

Behavior
^^^^^^^^

Variables get matches based on there name and level. When a natural variable is not found in the JSON structure it gets ignored. The same applies the other way around.

Xarrays gets automaticly expanded. This function checks the type and the length of the variable. Only if everything fit the value gets copied into the natural variable. Otherwise a return code != 0 is returned. See section :ref:`returncodes`. When a JSON structure contains a float the natural variable must be a (F8) type. 


Parameter
^^^^^^^^^

+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| position | name in example           | explanation                                                                                                                                                                                                  |
+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|     0    | varptr                    | The internal varptr variable from the framework. See :ref:`pda`.                                                                                                                                             |
+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|     1    | 'input-get-user@USERLIST' | Format string with the natural structure, library name and LDA name. The library name is optional. When is id not set the current library is the default one. <nat-structure-name>@<library-name>.<lda-name> |
+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|     2    | REALHTML_SETTINGS         | Its just the internal variable which was given by the PDA. See :ref:`pda`.                                                                                                                                   |
+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|     3    | input-get-user            | That's the actual head of the group                                                                                                                                                                          |
+----------+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

