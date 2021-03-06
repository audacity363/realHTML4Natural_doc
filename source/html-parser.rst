Html Parser
===========

.. _template-example:

Overview
^^^^^^^^

The syntax of the templates are inspirited from the template engine `jinja <http://jinja.pocoo.org/>`_ which is written in Python.

In contrast to natural4ajax you will write here your own HTML code. In your HTML code you can use two types of blocks to refer to variables or check them if they equals to a specific condition.

Here is an simple example:

.. code-block:: jinja

   <html>
       <head>
           <title>Hello World</title>
       </head>
       <body>
           <table>
               <thead>
                   <th>id</th>
                   <th>name</th>
                   <th>lastname</th>
              </thead>
              <tbody>
              {% for user in username %}
                  <tr>
                       <td>{{ isnnumber[loop.i] }}</tr>
                       <td>{{ user }}</tr>
                       <td>{{ firstname[loop.i] }}</tr>
                  </tr>
              {% end-for %}
              </tbody>
          </table>
      </body>
  </html>

With the LDA:

.. code-block:: cobolfree

   define data local
   1 page
      2 isnnumber (N19/1:20)
      2 username (A20/1:20)
      2 firstname (A20/1:20)
   end-define

There are two types of blocks. First the variable block (:code:`{{ ... }}`) which will just be replaced with the value of the variable and second the command block (:code:`{% ... %}`). The command block reacts like a if or  for block in natural. It have a command line (the first one) where you specify the break condition and a end line where the block ends.

Variable blocks
^^^^^^^^^^^^^^^

As in the section :ref:`template-vars` explained you have available every variable that is defined under the page group in your LDA. If a variable is not known the HTML parser will exit with an error message and will be render a error page instead. Currently (Version 2.0) it is not possible to add two variable blocks in one line. The parser would just print out the complete block without parsing it. I am working on this problem.

Plain variable:
---------------

Let's assume that you have defined a variable with the name "varname" which has the type alphanumeric with the length of 20 bytes. When you just write :code:`{{ varname }}` it would be replaced with the value. But you can also handle it as an array by adding square brackets after the name: :code:`{{ varname[0] }}`. Now you would get the value saved at the position zero of the variable. If you have spaces at the end the parser will automatically trim the value to the last non space character.

Arrays:
-------

Now let's chance the variable to array with 10 occurrences. In this situation the behavior of the variable block change a little bit. It will be still replace the complete block with the value but when you don't specify a index a JSON string will be generated and printed out. But when you don't what that you can specify a index in square brackets (:code:`{{ varname[0] }}`). Even here you can access the individual characters with a second index (:code:`{{ varname[0][1] }}`).

.. _array-example:

Example
........

The variable: :code:`2 varname(A20/1:10)`

The HTML-code: :code:`{{ varname }}`

The output: :code:`['value 1', 'value 2', 'value 3', 'value 4', 'value 5', 'value 6', 'value 7', 'value 8', 'value 9', 'value 10']`


The HTML-code: :code:`{{ varname[0] }}`

The output: :code:`value 1`


The HTML-code: :code:`{{ varname[0][1] }}`

The output: :code:`a`



Did you noticed that I accessed the first occurrence of the array with the index of zero even we declared it with an offset of one? That is because the HTML parser begins counting from zero. So whatever your offset is the first occurrence will be zero.

The explanation above also applies to 2D or 3D arrays. You just add more square brackets after the variable name to access the different dimensions.


Command blocks
^^^^^^^^^^^^^^

Command blocks are control structures which influences the render process of the template. So you can iterate through arrays, or render different HTML code when a condition is true.

For
---

Can loop over each entry in a array. For example the user list from above: :ref:`template-example`. In the for loop you can access a special variable: :code:`loop.i`. This variable contains the current iteration of the loop with an index of zero. In the loop you can use the :code:`{% break %}` command to break out of the loop and the :code:`{% continue %}` command to go into the next iteration of the loop.

If
--

The if statement is based on the Python if. But this one have much less functionality (at least in version 1.0). So you can compare variables with each other or a hard coded value. With the :code:`{% else %}` command you can switch to the else branch of the if statement.

Here is an example where we just want five iterations through the array "varname" from the Array :ref:`array-example`.

Example
.......

.. code-block:: jinja

   <ul>
   {% for entry in varname %}
       {% if loop.i == 4 %}
           {% break %}
       {% else %}
           <li>{{ entry }}</li>
       {% end-if %}
   {% end-for %}
   </ul>


printVars
---------

This function is just for debugging purposes. It will print out a pretty formated list of the defined variables with there corresponding values. It is recommended to wrap this command in the HTML tag :code:`<pre>` so you can read it in your browser.

Example
.......

:code:`<pre>{% printVars() %}</pre>`

genJSON
-------

This function will generate a JSON object from the paramaters that where given. A group will create a new JSON object in the ROOT object.

Example
.......

Variables:

.. code-block:: cobolfree

   DEFINE DATA LOCAL
       1 VAR1 (A10/1:5) init <"val1", "val2", "val3", "val4", "val5">
       2 GRP1
        3 VAR2 (I4) init <63>
        3 VAR3 (A20) init <"Hello World">


.. code-block:: jinja

   {% genJSON(VAR1, GRP1) %}

Output:

.. code-block:: javascript

   {
       "VAR1": ["val1", "val2", "val3", "val4", "val5"],
       "GRP1": 
       {
           "VAR2": 63,
           "VAR3": "Hello World"
       }
   }


typeof
------

Prints out the type of the variable which is given as a argument.

Example
.......

:code:`{% typeof(varname) %}`

import
------

**Currently only in the Version 1.0 available and not in the 2.0_beta. In the next parser version it will be available again.**

Imports another template that is provided after the import statement with quotation marks. This path is relative to the template_path defined in the :ref:`webserver-config`. The new template is parsed with the variable scope of the root template. It is planed to provide a second parameter which makes it possible to call a second natural program for this template.

Example
.......

:code:`{% import "navbar.html" %}`


macro
-----

Macros are like function you define them in a block and call them later on. You can define two types of parameter required and optional. The optional ones must be defined at the end and have a default value.

Example
.......

.. code-block:: jinja

   {% macro renderButton(btn_text, btn_type="submit") %}
       <button type="{{ btn_type }}">btn_text</button>
   {% end-macro %}

   {% renderButton("Hello World", "reset") %}
   {% renderButton("Hello World") %}


Output:

.. code-block:: html

   <button type="reset">Hello World</button>
   <button type="submit">Hello World</button>

Preview Versions
^^^^^^^^^^^^^^^^

When you want to life dangerous you can get the develop version from the parser `here <https://github.com/audacity363/realHTMLparser>`_. But there is no documentation for this version (The code speaks for itself) nor can I promise that it will work after every commit. 
