FASTA file
=================

.. code::

    def filetype plaintext;
    def encode ascii;

    [+]{
        [1]<string>(name="head line"; re_pattern=">.+"; end="\n")
        [+]{
            [1]<string>(re_pattern="[a-zA-Z]+"; end="\n")
        }
    }