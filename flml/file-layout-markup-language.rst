======================================================
File Layout Markup Language Specification
======================================================

:Authors: Hailing Fang
:Version: 2.1.0
:Create Date: 20230401
:Update Date: 20260601


Introdution
======================

File Layout Markup Language (FLML) is a markup language for describing the
layout/structure of binary or plaintext text files.

It has two components: FLML sentences and FLML programming language stataments.


FLML Sentences
======================

The FLML sentences are used to describe data block consists.

A sample FLML sentence have three parts: [...], <...> and (...).

There is a example of a FLML sentence::

    [5]<int>(name="sample sentence")

The '[...]' is count part of a FLML sentence, and is necessary.

The '<...>' is element part of a FLML sentence, and is necessary too.

The '(...)' is label part of a FLML sentence, and is optional.

A complex FLML sentence use '{...}' replace the '<...>' part. The '{...}'
is used to holding other FLML sentences.

For example::

    [2] {
        [2]<int>
        [1]<double>
        [8]<uint8>
    }(name="comple sentence")


The '[...]' Part
------------------------

The '[...]' part of a FLML sentence is called count part.

Where '...' can be a expression. The value is the number of elements defined
in '<...>' of the FLML sentence. The value must be an integer.

Example::

    [2 + 3]<int>

The '...' can be '?', '*' or '+'. '?' represent 0 or 1. '*' represent a integer
that is greater than or equal 0. '+' represent a integer that is greater than 1.  

If '...' be '?', '*' or '+', a variable marker can flollow them. A variable marker
defined as ':variable_name'. The variable equal the acturally value of '?', '*' or '+'.

Example::

    [+:int_n]<int>

In the example, means that the file have 1 or more integer, for a specific file,
the actural number of integer is refered by variable 'int_n'.


The '<...>' Part
-------------------------

The '<...>' part of a FLML sentence is called element part.

The '...' is a element type. There are element types that have been defined.

=============== ============ ===================================================
Element Type    Byte Size    Note
=============== ============ ===================================================
bit             1/8          1 bit
byte            1            8 bits       
char            1            a signed 8 bits integer, can be ascii characters
int8            1            same as 'char'
uint8           1            an unsigned 8 bits integer
short           2            signed 16 bits integer
int16           2            same as 'short'
uint16          2            an unsigned 16 bits integer
int             4            a signed 32 bits integer
int32           4            same as 'int'
uint32          4            an unsigned 32 bits integer
long            8            a signed 64 bits integer
int64           8            same as 'long'
uint64          8            a unsigned 64 bits integer
float           4            a 32 bits float number
float32         4            same as 'float'
double          8            a 64 bits float number
float64         8            same as 'double'
string          vary         a string, note the string do not have a '\0'
=============== ============ ===================================================

The value of sample element can be refered by an variable by follwing a variable
marker in form as ':variable_name'. When refered by an variable the element type
should end with a ';'.

Example::

    [1]<int; :v1>
    [4]<float; :v2>
    [4]<uint16; [0]:v3>

In the example,
the first sentence use 'v1' to refer to one int, a scalar;
the second sentence use 'v2' to refer to a 4 float, a vector;
the thired sentence use 'v3' to refer to the first element of 4 elements.

The value or values stored in sample elements can be assert that it should be equals to
a specific value or values.

Example::

    [1]<int; =32>
    [3]<float; =[3.14, 0.5, 2.5]>
    [4]<uint16; []=4>
    [4]<uint16; [0]=8>

In the example,
the first sentences indicates that the value stored in element should equals 32;
the second indicates that the values stored should be 3.14, 0.5 and 2.5 individually;
the third indicatess that every value of 4 that stored should equals 4;
the fourth indicates that the first item of the 8 stored value should equals
the second element of the 8 stored elements.


The '(...)' Part
--------------------------

The '(...)' part of FLML sentence is called label part.

In this part, labels are added to recode or define more information.

There are labels that have defined.


=============== ============ ==========================================================
Label           Value Type   Note
=============== ============ ==========================================================
name            string       the name of block described by current FLML sentence
id              string       the id of the sentence, unique in a FLML file
desc            string       the descripiton of the data block the sentence repeated
choices         set          a set wrote as '{ele1, ele2, ...}', the element in set 
                             is values the elements can be
gthan           number       a number the value stored in elements great than
gethan          number       a number the value stored in elements great equal than
lthan           number       a number the value stored in elements less than
lethan          number       a number the value stored in elements less equal than
NA              vary         a value for the value that not available 
datatype        value type   when stored value is a string, the string should can
                             be converted to the value type, a integer or float    
re_pattern      RegExp       a regular expression should math stored string
end_with        string       the data block should append ending the string
sep_with        string       elements of data block should seperated with the string
sorted          bool         the elements' value is sorted
order           variable     to repeated the order of elements
align_with      variable     the order should corresspondent with the order refer
                             by the variable
corresspondence variable     the value stored have relation to the variable
offset          expression   a expression in '[]', to indicate data block offset from
                             the begaining of the described file.
byte_len        expression   a expression in '[]' to indicate data block byte length
element_num     expression   a expression in '[]' in indicate the element number
use_offset      bool         the value in '[...]' is offset from begaining instead of
                             number of elements
use_byte_len    bool         the value in '[...]' is byte length instead of number of
                             elements
use_element_num bool         the value in '[...]' is number of elements, the defoult
                             behavior
=============== ============ ==========================================================


The '{...}' part
--------------------------

The '{...}' part of a FLML sentence is called complex element part.

It is used to construct a complex element.

Example::

    [2]{
        [1]<int>
        [2]<char>
        [4]<float>
    }

The element is make of one int, follwed by 2 char, and then 4 float.

The '{...}' can be emplemented in another '{...}'.


Concepts and Terminology
----------------------------

A FLML describe a data block. The data block is make of element described by
'<...>' part or '{...}' part.

Diagram for the block and elements of block. 

.. image:: data-block-and-block-element-diagram.svg


A '{...}' if a FLML sentence make a complex elements. The block is make 
of one or more complex elements. 


FLML Programming Language Stataments
==========================================

The FLML programming language stataments are use to do calculation and flow control.
It works like C and Python programming language.

Variable
---------------------

The rule for naming a variable is same as it in C or Python.


Data Types
----------------------

Number and String
~~~~~~~~~~~~~~~~~~~~~~~~

Integer, float and string are support in FLML programming language. A char in ''
is a integer same as in C. The string can be concatenated by '+' operator.

Example::

    a = 12;
    b = 3.14;
    c = "hello world"
    d = 'a'


List and Dictionary
~~~~~~~~~~~~~~~~~~~~~~~~~

List and dictionary are implemented in FLML programming language.
It behave just like it in Python, even the attributes of the it are
same as it in Python.


Example::

    li = [1, 2, 'a', "hello"]
    li.append(3)
    dic = {"ab": 1, 2: "bc"}


Built in Variables/Object
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are built in variables or objects to refer to specific objects.

================= =============================== ==============================
objects           notes                              example
================= =============================== ==============================
fileself          refer to the file that          fileself.open("rb");
                  described                       fileself.seek(8, 0);
                                                  #move to files' offset 8
None              refer to nothing                a = None;
================= =============================== ==============================

Expression
----------------------

Numbers, string, functions operated by operators make an expression.

Arithmetic Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~

============== ================ ================================================
Operators      Name             Examples
============== ================ ================================================
\+             plus             3 + 4; "hello " + "world"
\-             minus            3 - 4
\*             mulitiply        2 * 3 
/              divide           4 / 3
//             integer divide   4 // 3
%              mod              4 % e
\*\*           expernation      2\*\*3
=              equal            var = var * 2
============== ================ ================================================

The operatons that '++', '--', '+=', '-=', '\*=', '/=', '//=', '%=', '\*\*=' are also
supported. 

Relational and Logical Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============== =================== =============================================
Operators      Name                Examples
============== =================== =============================================
>              greater than        4 > 3
>=             greater equal than  4 >= 3
<              less than           4 < 3 
<=             less equal than     4 <= 3
==             be equal            4 == 3
!=             not equal           4 != 3
&&             logical 'and'       0 && 1
\|\|           logical 'or'        0 \|\| 1
!              logical 'not'       !(3)
============== =================== =============================================

Bitwise Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FLML programming language support bitwise operatons, the behavior is just like that
in C.

============== ===================== ============================================
Operators      Name                  Examples
============== ===================== ============================================
&              bitwise AND           8 & 0
\|             bitwise inclusive OR  8 \| 0
^              bitwise exclusive OR  8 ^ 0
<<             left shift            2 << 2
>>             right shift           16 >> 2
~              one's complement      ~8
============== ===================== ============================================

Statament
----------------------

A expression with with a ';' make a statament.

There are other stataments that make FLML programming language control complete.

Conditions
~~~~~~~~~~~~~~~~~~~~~

.. code::

    if (Expression) {
        stataments or FLML sentences;
    } elif (Expression) {
        stataments or FLML sentences;
    } else {
        stataments or FLML sentences;
    }

Example::

    [1]<uint16; :num>
    [1]<int; :flag>
    if (flag == 1) {
        [3]<float>
    }
    elif (flag > 1) {
        [4]<float>
    } else {
        num_char = flag * num;
        [num_char * 2]<char>
    }

Loops
~~~~~~~~~~~~~~~~~~~

Two types of 'for' stataments supported, one behave like it in C, and another
behave like it in Python. The statament 'continue' and 'break' are support in a
loop.

.. code::

    for (init_Expression1; compare_Expression2; calcu_Expression3) {
        stataments or FLML sentences;
    }

    for (ele in ele_s) {
        stataments or FLML sentences;
    }


Example::

    for (i = 0; i < 10; i++) {
        [i**2]<char>
    }

    ele_s = [1, 2, 3];
    for (ele in ele_s) {
        var = ele * 2;
        [var]<int>
    }


The 'while' statament is supported.

.. code::

    while (Expression) {
        stataments or FLML sentences;
    }


Example::

    x = 10;
    while (x > 0) {
        [3]<float>
        x--;
    }


If a data block have n elements, where n greater than 1, the stataments in '{...}' will
execuate n times.

Example::

    y = [];
    [3]{
        [1]<int; :x>
        y.append(x)
    }

In the example, the y list will have 3 elements that stored in the data block.


goto statament
~~~~~~~~~~~~~~~~~~~~~~~

The FLML programming language support 'goto' statament, the 'goto' should follow
a integer for offset from described file's begaining.

.. code::

    goto (offset) {
        stataments or FLML sentences;
    }

Example::

    [1]<uint32; :data_start_offset>
    goto (data_start_offset) {
        [1]<int>
        [24]<double>
    }


Define Functions
----------------------

.. code::

    fun func_name(args, ...) {
        stataments;
    }


Example::

    fun test(a, b) {
        return a + b;
    }



Built in Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============== =========================== ===================================
Functions      Notes                       Example
============== =========================== ===================================
sum            sum list of integer         a = [1, 2, 3];
                                           b = sum(a);
len            get length of a list or     len([1, 2, 3]);
               dictionary.               
int            convert a string to int     int("123");
str            convert a number to string  str(123);
float          convert integer to float    float(123);
open           open a file                 open('require.txt');
============== =========================== ===================================


Other Stataments
-----------------------------

================ =============================================
statament        example
================ =============================================
assert           assert 1 == 2;
raise            raise "an error";
import           import flml_modul;
export           export an_variable;
require          require infor.txt;
def              def filetype plaintext;
deflabel         def optional "the value is bool, if True
                 the block is not required"
================ =============================================

import and export stataments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FLML programming language can import other flml file format description file as
a module/library. The export variable will visiable in importing flml file.

Example::

    #lib.flml
    export x;
    export add;

    x = 123;
    fun add(x, y) {return x + y;}

    #b.flml
    import lib;

    [lib.x]<int; :x_value>
    y = add(x_value[0], x_value[1]);


require stataments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some, a flml file would like to offer a function that parse the store data into
a data structure. When doing this, other file make need.

Example::

    #in flml1.flml file 1
    def filename file_a


    #in flml file 2
    import flml1;
    require file_a;

    fin = open(file_a);
    lines = [line.rstirp() for line in fin];



def stataments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To define some attributes of described file.

Example::

    def filetype binary;
    def encode ascii;
    def filename fname_a;

When the flml file is imported by other flml file, the define attributes is visiable
to it.

deflabel stataments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If defined labels used in '(...)' not satify user's requirements. One can define
labels, to extend meaning of FLML sentences.


Comments
============================

A sentence whose '[...]' part start with '#', the sentence is
regard as a comment.

For example::

    [# this is a comment]<int>()


A line start with '#', the line is a comment.

For example::

    # [3]<int>


Contents within '/*' and '*/' are regard as comments.

For example::

    /*
        [3]<int; float_num>
        [float_num]<float>
    */
