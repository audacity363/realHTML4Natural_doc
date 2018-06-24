GENJSONO
========

This user exit is basically the same as :doc:`genjson` but when a group is defined as an array all entrys in this group get generated as an array of JSON objects.

The only reason why this function has it's own exit is because the most websites which calls the framework generates Java Script objects out of the array from the :doc:`genjson` response. I have to adapt the Java Script code of these pages so that they expect an array of JSON objects. 



Example LDA
^^^^^^^^^^^

This LDA is used as an example through this documentation page.

Name of the LDA: "USERLIST"

.. code-block:: cobolfree

   1 user-response
       2 userlist (1:*)
           3 id (I4)
           3 firstname (A) DYNAMIC
           3 lastname (A) DYNAMIC
           3 age (I4)
       2 error
           3 occurred (L)
           3 msg (A) DYNAMIC
   
   2 user-details
       3 firstname (A) DYNAMIC
       3 lastname (A) DYNAMIC
       3 age (I4)
       3 street (A) DYNAMIC
       3 post_code (I4)


Example call
^^^^^^^^^^^^

.. code-block:: cobolfree

   call 'interface4' 'genjson' 'USERLIST' 'user-response' 
          REALHTML_TMPFILE REALHTML_SETTINGS user-response


.. code-block:: JSON

   {
       "userlist": 
       [
           {
               "id": 1,
               "firstname": "Justus",
               "lastname": "Bahr",
               "age": 21
           },
           {
               "id": 2,
               "firstname": "Nicholas",
               "lastname": "Ostrowski",
               "age": 43
           },
           {
               "id": 3,
               "firstname": "Julia",
               "lastname": "Gebhard",
               "age": 38
           }
       ],
       "error":
       {
           "occured": false,
           "msg": ""
       }
   }


.. code-block:: cobolfree

   call 'interface4' 'genjson' 'USERLIST' 'user-details' 
          REALHTML_TMPFILE REALHTML_SETTINGS user-details


.. code-block:: JSON

   {
       "firstname": "Justus",
       "lastname": "Bahr",
       "age": 21,
       "street": "Herdingstr. 167",
       "post_code": 24238
   }



Parameter
^^^^^^^^^

+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
| position | name in example   | explanation                                                                                                        |
+==========+===================+====================================================================================================================+
|    0     | 'USERLIST'        | This is the name of the LDA which you want to use. It must be written in upper case letters and without the ending |
+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
|    1     | 'user-response'   | This is a description of the group header which you want to hand over to the framwork                              |
+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
|    2     | REALHTML_TMPFILE  | Its just the internal variable which was given by the PDA                                                          |
+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
|    3     | REALHTML_SETTINGS | Its just the internal variable which was given by the PDA                                                          |
+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
|    4     | user-response     | That's the actual head of the group you want to generate                                                           |
+----------+-------------------+--------------------------------------------------------------------------------------------------------------------+
