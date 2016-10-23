Natural
=======

.. _pda:

Parameter Data Area
^^^^^^^^^^^^^^^^^^^

A parameter data area like this has to been defined. You don't have to take this exact names but the type and length has to match.

.. code-block:: cobolfree

   1 VALUES_REQ (A2024/1:20)
   1 VALUE_LENGTH (I4/1:20)
   1 KEYS_REQ (A100/1:20)
   1 PARM_COUNT (I4)
   1 REQ_TYPE (A5)
   1 REQ_FILE (A100)
   1 REQ_SETTINGS (A) DYNAMIC


Explanation parameter data area
-------------------------------

+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| variable name | explanation                                                                                                                                                         |
+===============+=====================================================================================================================================================================+
| value_req     | the variables value_req and keys_req belongs together. These two array form together a Key/Value Pair. "value_req" includes the values from the GET or POST request |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| value_length  | the real length of the value in occurrence n                                                                                                                        |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| keys_req      | the key to the value in occurrence n                                                                                                                                |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| parm_count    | includes own much entry in value_req/value_length/keys_req are set                                                                                                  |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| req_type      | which HTTP request was received                                                                                                                                     |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| req_file      | just a internal variable. Contains the file which will be delivered to the browser                                                                                  |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| req_settings  | just a long string which contains settings from the webserver configuration for the html parser                                                                     |
+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _template-vars:

Variables for the template
^^^^^^^^^^^^^^^^^^^^^^^^^^

You have to create a page group in an external local data area. Every variable under this group is available in the template. You can redefine variables and create new subgroups. 

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

Every Type can be also defined as 1D/2D and 3D arrays but then the dynamic length is not supported. This is due to a bug in the INTERFACE 4 API from the SAG.

Other
-----

You also need two Alphanumeric Variable with the length of 20. One of them has to contain the lda name which you are using to store the page group and the second must contain the name of the template you want to use.

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
     1 post_isn(N19) inti <"1200000000000000000"> /*The value from the post argument
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
       1 output_file (A100) init <"/tmp/outout.html">
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
       1 VALUES_REQ (A2024/1:20)
       1 VALUE_LENGTH (I4/1:20)
       1 REQ_KEYS (A100/1:20)
       1 PARM_COUNT (I4)
       1 REQ_TYPE (A5)
       1 REQ_FILE (A100)
       1 REQ_SETTINGS (A) DYNAMIC
   END-DEFINE
   


