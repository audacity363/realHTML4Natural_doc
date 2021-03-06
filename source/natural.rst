Natural
=======

.. _pda:

Parameter Data Area
^^^^^^^^^^^^^^^^^^^

A parameter data area like this has to been defined. You don't have to take this exact names but the type and length has to match.

.. code-block:: cobolfree

   1 HTTP_KEYS (A/1:*)
   1 HTTP_VALS (A/1:*)
   1 HTTP_VALS_LENGTH (I4/1:*)
   1 HTTP_PARM_COUNT (I4)
   1 HTTP_REQ_TYPE (A) DYNAMIC

   1 REALHTML_TMPFILE (A) DYNAMIC
   1 REALHTML_SETTINGS (A) DYNAMIC


Explanation parameter data area
-------------------------------

+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| variable name     | explanation                                                                                                                                                          |
+===================+======================================================================================================================================================================+
| HTTP_KEYS         | the variables HTTP_KEYS and HTTP_VALS belongs together. These two array form together a Key/Value Pair. "HTTP_VALS" includes the values from the GET or POST request |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| HTTP_VALS         | the value to the key in occurrence n                                                                                                                                 |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| HTTP_VALS_LENGTH  | the real length of the value in occurrence n                                                                                                                         |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| HTTP_PARM_COUNT   | includes own much entry the arrays HTTP_KEYS, HTTP_VALS and HTTP_VALS_LENGTH have                                                                                    |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| HTTP_REQ_TYPE     | which HTTP request was received                                                                                                                                      |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| REALHTML_TMPFILE  | just a internal variable. Contains the file which will be delivered to the browser                                                                                   |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| REALHTML_SETTINGS | just a long string which contains settings from the webserver configuration for the html parser                                                                      |
+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _template-vars:

Variables for the template
^^^^^^^^^^^^^^^^^^^^^^^^^^

You have to create a group named **page** in an external local data area (LDA). Every variable under this group is available in the template. You can redefine variables and create new subgroups. 

Example:
--------

.. code-block:: cobolfree 

   define data local
   1 page
       2 daten (A128/1:200)
       2 isnnummer (A29/1:200)
       2 maxdatenid (I4)
   end-define

.. _datatypes:

Supported data types
--------------------

+-----------------------------------------------------+---------------+
| type                                                | length        |
+=====================================================+===============+
| Integer (I)                                         | 1/2/4         |
+-----------------------------------------------------+---------------+
| Logical (L)                                         | \-\-\-\-      |
+-----------------------------------------------------+---------------+
| Alphanumeric (A)                                    | every/Dynamic |
+-----------------------------------------------------+---------------+
| Numeric (N) (Floating points are not supported yet) | every/Dynamic |
+-----------------------------------------------------+---------------+
| Unicode (U)                                         | every/Dynamic |
+-----------------------------------------------------+---------------+

Every type can be also defined as 1D/2D or 3D arrays.

Other
-----

You also need two alphanumeric variable with the length of 20. One of them has to contain the LDA name which you are using to store the page group and the second must contain the name of the template you want to use.

Example
.......

.. code-block:: cobolfree

  define data
      local using your_lda
      local
          1 realhtml_ldaname(A20)
          1 realhtml_template(A20)
   end-define

   move "your_lda" to realhtml_ldaname
   move "the_template.html" to realhtml_template
   end

The "flipbuf" function
^^^^^^^^^^^^^^^^^^^^^^

**This function should now be obsolete beacuse in the current version the HTTP values are with a dynamic length so you don't have trailing characters.**

The "flipbuf" function shift a specific number of digits from the beginning of the buffer to the end.

For example you want to search for a ISN number with a POST argument. The number in the POST value will be standing on the beginning of the buffer but the ISN number is sitting on the right. With the "value_length" (from the pda. See :ref:`pda`) variable you know how long the number is.

Arguments
---------

+--------+--------------+----------------------+
| number | type         | purpose              |
+========+==============+======================+
| 1      | Numeric (N)  | input buffer         |
+--------+--------------+----------------------+
| 2      | Numeric (N)  | output buffer        |
+--------+--------------+----------------------+
| 3      | Integer (I4) | length of the number |
+--------+--------------+----------------------+

Example
-------

.. code-block:: cobolfree

  define data local
     1 post_isn(N19) init <"1200000000000000000"> /*The value from the post argument
     1 post_length (I4) init <2>
     1 right_isn(N19)
  end-define

  call interface4 "flipbuf" post_isn right_isn post_length
  * Now right_isn contains the following: "0000000000000000012"
  end


The "setcookie" function
^^^^^^^^^^^^^^^^^^^^^^^^

Nothing to see here. Currently under development. This function will be used to set new cookies and send them to the browser.

The "genpage" function
^^^^^^^^^^^^^^^^^^^^^^

The "genpage" function generate the html page out of the template. 

Arguments
---------

+--------+--------------------------+-----------------------------------------------------+
| number | type                     | purpose                                             |
+========+==========================+=====================================================+
| 1      | Alphanumeric (A20)       | name of the lda                                     |
+--------+--------------------------+-----------------------------------------------------+
| 2      | Alphanumeric (A20)       | name of the template                                |
+--------+--------------------------+-----------------------------------------------------+
| 3      | Alphanumeric (A100)      | file path which the template engine should write to |
+--------+--------------------------+-----------------------------------------------------+
| 4      | Alphanumeric (A Dynamic) | settings string                                     |
+--------+--------------------------+-----------------------------------------------------+
| 5      | see :ref:`datatypes`     | the page group from your lda                        |
+--------+--------------------------+-----------------------------------------------------+

Example
-------

.. code-block:: cobolfree

   define data local
       1 lda_name (A20) init <"mylda">
       1 template_name (A20) init <"mytemplate">
       1 output_file (A100) init <"/tmp/output.html">
       1 settings (A) DYNAMIC init <"templatepath=/tmp/templates/;debug=true">
       1 page /* this would be in your external lda
           2 field-1 (A10)
           2 field-2 (A10)
   end-define

   call interface4 "genpage" lda_name template_name output_file settings page
   end

Complete skeleton
^^^^^^^^^^^^^^^^^^

The subprogram
--------------

.. code-block:: cobolfree

   define data
        parameter using reqinfos /*request information
        local using testlda
   end-define

   move "testlda" to realhtml_ldaname
   move "testtemplate.html" to realhtml_template

   /* your code goes here

   call interface4 "genpage" realhtml_ldaname realhtml_template 
                             req_file req_settings page

The LDA
-------

.. code-block:: cobolfree

   define data local
       1 realhtml_ldaname (A20)
       1 realhtml_template (A20)
       1 page
           2 field-1 (A10)
           2 field-2 (A20)
   end-define

The PDA
-------

.. code-block:: cobolfree

   DEFINE DATA PARAMETER
       1 HTTP_KEYS (A/1:*)
       1 HTTP_VALS (A/1:*)
       1 HTTP_VALS_LENGTH (I4/1:*)
       1 HTTP_PARM_COUNT (I4)
       1 HTTP_REQ_TYPE (A) DYNAMIC

       1 REALHTML_TMPFILE (A) DYNAMIC
       1 REALHTML_SETTINGS (A) DYNAMIC
   END-DEFINE

